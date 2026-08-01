---
article_id: CDV-06-A05
writing_contract_version: "native-id-v2"
title: "Error, Retry, dan Idempotency pada API"
slug: "error-retry-dan-idempotency-api"
description: "Design error semantics, correlation, retryability, timeout, backoff, idempotency keys, deduplication, and reconciliation tests"
status: draft
publication_date: "2025-08-07"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-06
primary_intent: "Prevent duplicate effects and ambiguous recovery"
reader_community: "Codev.id"
reader_address: "Sobat Codev.id"
final_route: "/artikel/error-retry-dan-idempotency-api.html"
technical_review: required
sources:
  - "https://spec.openapis.org/oas/v3.1.1.html"
  - "https://owasp.org/API-Security/editions/2023/en/0x11-t10/"
  - "https://csrc.nist.gov/pubs/sp/800/218/final"
---

# Error, Retry, dan Idempotency pada API
Halo, Sobat Codev.id! Retry bukan tombol “coba lagi” yang aman untuk semua error. Rancang API dengan error yang jelas, timeout, backoff, dan batas percobaan; gunakan idempotency key pada operasi yang menimbulkan efek agar permintaan ulang tidak menggandakan transaksi. Pola ini mengurangi duplikasi, bukan menjamin exactly-once delivery.

Keputusan bergantung pada sifat operasi dan bukti implementasi. GET yang read-only biasanya dapat diulang, sedangkan pembayaran atau pembuatan pesanan memerlukan kunci idempotensi, penyimpanan hasil, dan rekonsiliasi. OpenAPI membantu mendokumentasikan antarmuka, tetapi tidak membuktikan perilaku server ([OpenAPI Specification 3.1.1](https://spec.openapis.org/oas/v3.1.1.html)).

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

*Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.*

## Definisi dan batas objek

Error adalah hasil teramati yang menjelaskan apakah permintaan ditolak, gagal sementara, atau statusnya belum diketahui. Retry adalah kebijakan mengirim ulang setelah kelas kegagalan tertentu. Idempotency berarti pengulangan dengan input dan kunci yang sama menghasilkan efek bisnis seperti satu eksekusi berhasil.

Idempotensi bukan deduplikasi sempurna. Catatan kunci dapat hilang, proses bisa berhenti di tengah, dan dependensi pihak ketiga memiliki aturan sendiri. Fokus tulisan ini adalah kontrak API, pemrosesan ulang, dan rekonsiliasi internal; exactly-once delivery dan pemulihan pihak ketiga tetap di luar jaminan.

## Cara kerjanya

Tetapkan respons konsisten: status HTTP, kode error stabil, pesan aman, dan correlation ID untuk menautkan log lintas layanan. Bedakan timeout dari penolakan validasi dan kegagalan sementara; jangan menjadikan semua 500 sebagai izin retry.

Gunakan deadline total, backoff eksponensial dengan jitter (variasi acak kecil), batas percobaan, dan hormati `Retry-After` bila kontrak menyediakannya. Untuk mutasi, klien mengirim `Idempotency-Key` unik. Server menyimpan kunci, identitas pemanggil, parameter ternormalisasi, status, dan respons final. Kunci sama dengan parameter berbeda harus menjadi konflik. Lingkup tenant, TTL, dan perlindungan dari tebakan kunci perlu ditinjau karena OWASP mengingatkan bahwa penyalahgunaan API adalah risiko yang harus diuji ([OWASP API Security Top 10](https://owasp.org/API-Security/editions/2023/en/0x11-t10/)).

Jika efek eksternal terjadi sebelum respons tersimpan, status menjadi tidak pasti. Sediakan endpoint status atau pekerjaan rekonsiliasi yang mencocokkan event internal dengan catatan penyedia. Jangan menaruh kunci atau payload sensitif di URL maupun log.

## Faktor yang mengubah hasil

Sifat operasi, kelas error, deadline pengguna, kapasitas dependensi, dan atomisitas penyimpanan kunci mengubah kebijakan. Tanpa atomisitas, dua worker dapat lolos pemeriksaan bersamaan. Kunci juga harus terkait identitas dan izin pemanggil; validasi input sebaiknya selesai sebelum catatan idempotensi dibuat.

Kawan Codev.id, unit test hijau bukan bukti universal. NIST SSDF menekankan bahwa hasil pengujian hanya berlaku pada klaim, lingkungan, build, dan data yang diuji; pelihara jejak risiko, persyaratan, hasil, dan cacat terbuka ([NIST SP 800-218](https://csrc.nist.gov/pubs/sp/800/218/final)).

## Contoh keputusan praktis

| Situasi | Tindakan | Retry |
|---|---|---|
| Validasi gagal | `400` dan kode bidang | Tidak sampai input berubah |
| Kredensial tidak sah | `401/403` sesuai kontrak | Tidak otomatis |
| Rate limit | `429` dengan waktu tunggu | Ya, setelah jeda |
| Timeout membuat pesanan | Status belum diketahui | Ya, kunci sama lalu cek status |
| Kunci dan parameter konflik | `409` | Tidak; selesaikan niat dulu |

Saat koneksi putus setelah permintaan pembuatan pesanan, ulangi dengan kunci yang sama. Jika tetap tidak ada jawaban, tampilkan status “sedang dipastikan” dan panggil endpoint status; socket tertutup bukan bukti transaksi gagal.

## Kesalahan umum dan cara memeriksanya

Periksa matriks yang menyebut siapa boleh retry, jeda, dan kondisi berhenti. Uji bahwa percobaan mutasi mempertahankan kunci; uji dua request identik secara bersamaan dan crash di antara efek eksternal serta penyimpanan. Tinjau redaksi log, korelasi lintas layanan, dan akses tabel kunci. Automated test hanyalah sampel, sehingga keputusan rilis tetap memerlukan review risiko.

Mulailah pemeriksaan dari kontrak yang dibaca klien. Pastikan setiap kode error memiliki arti tunggal, contoh payload tidak membocorkan rahasia, dan correlation ID muncul pada respons maupun log. Catat siapa yang menetapkan deadline: browser, gateway, atau layanan tujuan. Deadline yang lebih pendek di lapisan luar dapat membuat pekerjaan di dalam tetap berjalan, sehingga endpoint status menjadi penting.

Untuk implementasi penyimpanan kunci, verifikasi indeks unik pada ruang lingkup yang benar dan perilaku ketika dua request datang hampir bersamaan. Uji parameter yang sama dalam urutan field berbeda setelah normalisasi, serta payload yang berbeda dengan kunci sama. Pastikan respons tersimpan tidak dapat dipakai oleh tenant lain. Jika kunci kedaluwarsa, dokumentasikan apakah pengulangan dianggap niat baru atau harus ditolak.

Jalankan simulasi dependensi lambat, rate limit, koneksi putus, dan proses mati. Ukur hanya apa yang benar-benar diuji; jangan mengubah hasil simulasi menjadi janji performa produksi. Bila hasil menyentuh keputusan uang atau akses, minta pemeriksaan spesialis dan simpan keputusan bersama bukti build yang diuji. Pembaca yang memerlukan konteks API lebih luas dapat meninjau dokumentasi layanan sebelum kembali ke matriks error endpoint.

## Jalan pintas yang perlu dihindari

“Cukup retry tiga kali dengan jeda satu detik” mengabaikan jenis error, deadline, kapasitas, dan efek samping. Alternatifnya adalah kebijakan berbasis kelas error, deadline, backoff berjitter, idempotency key untuk mutasi, dan rekonsiliasi saat status tak pasti.

## Kesimpulan dan langkah berikutnya

Teman Codev.id, sebelum merilis endpoint dokumentasikan kontrak error, matriks retry, skema kunci, serta skenario crash dan konkurensi. Minta review teknis untuk alur yang menyentuh uang, otorisasi, atau pihak ketiga. Aset pendamping tersedia di media lokal Codev.id.

Simpan dokumen itu dekat dengan spesifikasi API dan tautkan setiap skenario ke hasil pengujian. Saat dependensi berubah, ulangi uji timeout dan rekonsiliasi; jangan menganggap perilaku lama masih berlaku. Lihat [konteks layanan di Codev.id](/) ketika menelusuri keputusan sebelum menetapkan kebijakan baru.

Tambahkan juga uji pembatalan: klien berhenti menunggu, tetapi server menyelesaikan pekerjaan. Hasil yang benar adalah satu efek dan status yang dapat ditanyakan, bukan pembatalan semu. Uji pengiriman ulang dari antrean, pemulihan setelah restart, dan perbedaan jam antarworker. Simpan contoh request serta respons yang sudah disanitasi agar reviewer dapat mengulanginya tanpa data produksi. Jika kontrak belum menyebut masa berlaku kunci, jangan mengisinya dengan angka tebakan; tandai keputusan itu untuk pemilik layanan.

Pisahkan metrik teknis dari hasil bisnis. Jumlah retry tinggi dapat menunjukkan dependensi lambat, tetapi tidak menjawab apakah pelanggan menerima satu pesanan. Rekonsiliasi harus memeriksa identitas niat, status efek, dan tindakan korektif yang diizinkan. Catatan ini membantu tim berdiskusi dengan istilah yang sama saat terjadi perbedaan antara respons API dan keadaan sebenarnya. Tinjau pula contoh payload sukses dan gagal secara berdampingan agar perbedaan status mudah dikenali oleh klien.

Aturan operasionalnya: retry hanya saat kontrak mengizinkan, ulangi mutasi dengan kunci yang sama, dan perlakukan tidak adanya respons sebagai status yang perlu direkonsiliasi. Pola ini tidak menjamin exactly-once delivery.

<!-- BEGIN MANAGED IMAGE PLAN
## Image plan

- **Image ID:** `LOCAL-001`
- **Source type:** `local`
- **Placement:** after the opening has answered the main question, before the first detailed H2
- **Exact Markdown to insert:** `![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)`
- **Caption/credit:** Aset lokal proyek; jangan klaim sebagai dokumentasi proyek tertentu.
- **Selection basis:** filename/source metadata identifies `CODEV` as relevant content media; no pixels were inspected.
- **Hard boundary:** do not infer or describe unseen visual details, project ownership, location, people, brands, condition, performance, or outcome.
- **Substitution rule:** do not replace this image. If unavailable or provenance is incomplete, insert `[NEEDS IMAGE REVIEW: LOCAL-001]` and continue drafting the prose.
END MANAGED IMAGE PLAN -->
