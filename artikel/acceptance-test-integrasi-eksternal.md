---
article_id: CDV-08-A06
title: "Acceptance Test untuk Integrasi Eksternal"
slug: "acceptance-test-integrasi-eksternal"
description: "Test credentials/roles, normal and duplicate events, invalid data, timeout, rate limits, replay, reconciliation, observability, and disconnect"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2025-10-05"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-08
primary_intent: "Verify an integration beyond the happy path"
reader_community: "Codev.id"
reader_address: "Kawan Codev.id"
final_route: "/artikel/acceptance-test-integrasi-eksternal.html"
technical_review: required
sources:
  - "https://spec.openapis.org/oas/v3.1.1.html"
  - "https://www.rfc-editor.org/info/rfc9700/"
  - "https://www.w3.org/TR/webauthn-3/"
  - "https://owasp.org/API-Security/editions/2023/en/0x11-t10/"
  - "https://www.cisa.gov/sbom"
  - "https://csrc.nist.gov/pubs/sp/800/161/r1/final"
  - "https://securityscorecards.dev/"
---

# Acceptance Test untuk Integrasi Eksternal

Halo, Kawan Codev.id! Integrasi eksternal baru layak diterima bukan ketika satu transaksi contoh berhasil, melainkan ketika batas akses, pesan yang berulang, data yang salah, gangguan jaringan, dan pemutusan koneksi menghasilkan perilaku yang sudah disepakati. Acceptance test adalah bukti bahwa sistem Anda dan sistem penyedia dapat bekerja pada kondisi normal sekaligus menolak atau memulihkan kondisi yang tidak normal.

Jawaban singkatnya: buat skenario uji berdasarkan kontrak antarmuka, siapkan akun dan peran yang benar-benar terpisah, kirim kejadian normal serta kejadian yang sengaja rusak, lalu cocokkan hasil di kedua sisi. Lulus berarti setiap skenario memiliki hasil yang dapat diamati, data dapat direkonsiliasi, dan jalur pemulihan diketahui. OpenAPI membantu mendeskripsikan antarmuka, tetapi dokumen itu tidak membuktikan implementasi atau keamanannya ([OpenAPI Specification 3.1.1](https://spec.openapis.org/oas/v3.1.1.html)). Detail penyedia seperti kuota, masa berlaku kredensial, subprosesor, dan kerentanan terkini tetap harus dikonfirmasi dalam proyek; `[NEEDS VENDOR REVIEW: terms, quotas, subprocessors, and current vulnerabilities]`.

[NEEDS IMAGE REVIEW: LOCAL-001]

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

## Jawaban singkat dan salah paham utama

Kesalahpahaman paling mahal adalah menganggap respons HTTP 200 atau status “connected” sebagai bukti integrasi selesai. Respons itu hanya menunjukkan satu pertukaran diterima pada satu kondisi. Acceptance test harus memeriksa siapa yang boleh memanggil, pesan apa yang diterima, apa yang terjadi saat pesan dikirim dua kali, dan bagaimana kedua sistem kembali memiliki catatan yang sama setelah gangguan.

Mulailah dengan kriteria lulus yang bisa disaksikan: akun dengan peran tertentu dapat melakukan aksi yang diizinkan; akun tanpa peran ditolak; payload valid menghasilkan satu efek; payload duplikat tidak menggandakan efek; payload tidak valid ditolak dengan alasan yang dapat ditindaklanjuti; timeout dan rate limit memicu perilaku retry atau fallback yang disepakati; dan setiap kejadian dapat ditelusuri melalui log. Jika salah satu hasil belum ditentukan, statusnya bukan “lulus dengan catatan”, melainkan keputusan terbuka yang harus dicatat.

## Definisi dan batas objek

Dalam artikel ini, integrasi eksternal berarti pertukaran data atau perintah antara aplikasi Anda dan layanan di luar kendali langsung tim Anda—misalnya API, webhook, penyedia identitas, layanan pembayaran, atau sistem pengiriman. Acceptance test adalah pemeriksaan bersama pada lingkungan uji atau sandbox untuk menentukan apakah integrasi memenuhi kontrak penerimaan.

Objek uji mencakup kredensial, peran, endpoint, metode autentikasi, skema data, urutan kejadian, batas waktu, kuota, mekanisme retry, idempotensi (permintaan yang diulang tidak mengulang efek), rekonsiliasi, observabilitas, dan prosedur disconnect. Objek ini tidak menggantikan sertifikasi penyedia, persetujuan keamanan formal, atau pemantauan produksi. Pemantauan kesehatan langsung memiliki ruang tersendiri, dan penerimaan rilis harus mengikuti proses rilis proyek.

Dokumen kontrak seperti spesifikasi OpenAPI dapat menjadi sumber daftar endpoint, parameter, respons, dan skema yang akan diuji. Jadikan dokumen itu sebagai hipotesis yang perlu dibuktikan dengan perilaku sandbox, bukan sebagai bukti bahwa implementasi sudah aman. Untuk konteks keputusan proyek lain, Anda dapat mulai dari [beranda Codev.id](/) lalu kembali ke matriks uji ini. Untuk OAuth, RFC 9700 adalah pembaruan praktik terbaik pada 2025; gunakan sebagai masukan peninjauan alur, bukan alasan untuk menyalin konfigurasi tanpa konteks ancaman ([OAuth 2.0 Security Best Current Practice](https://www.rfc-editor.org/info/rfc9700/)).

## Cara kerjanya

Susun acceptance test sebagai alur yang dapat diulang.

1. **Tetapkan kontrak dan prasyarat.** Catat endpoint, event, field wajib, format waktu, korelasi ID, status yang diharapkan, serta akun uji. Simpan rahasia di pengelola rahasia; jangan menaruh token aktif di tiket atau log. Minta penyedia menyebutkan sandbox, kuota, jendela pemeliharaan, dan cara mencabut akses.
2. **Buat matriks identitas.** Uji kredensial valid, kedaluwarsa, dicabut, dan salah. Untuk setiap peran, tuliskan aksi yang diizinkan dan yang harus ditolak. Bila memakai passkey atau WebAuthn, verifikasi pendaftaran dan autentikasi sesuai konteks aplikasi; spesifikasi WebAuthn menjelaskan protokol, bukan konfigurasi risiko spesifik Anda ([WebAuthn Level 3](https://www.w3.org/TR/webauthn-3/)).
3. **Kirim jalur normal.** Gunakan data minimum yang sah. Catat request ID, respons, event ID, efek di sistem Anda, dan efek yang dilaporkan penyedia. Pastikan zona waktu, pembulatan, dan status akhir sama-sama dapat dibaca.
4. **Serang batasnya secara terkendali.** Kirim event yang sama dua kali, event di luar urutan, field tambahan, field hilang, tipe data salah, ukuran berlebih, tanda tangan tidak cocok, dan timestamp kedaluwarsa. Jangan menguji volume yang dapat mengganggu penyedia tanpa persetujuan tertulis.
5. **Simulasikan kegagalan.** Tahan respons sampai timeout, kembalikan error sementara dan permanen, picu rate limit, putuskan koneksi, lalu pulihkan. Periksa apakah retry memakai batas dan backoff yang disepakati, apakah pesan tetap dapat dilacak, dan apakah operator mendapat sinyal yang jelas.
6. **Rekonsiliasi dan putuskan.** Bandingkan daftar transaksi atau event di kedua sisi berdasarkan ID idempotensi dan rentang waktu. Setelah akses dicabut atau endpoint dinonaktifkan, pastikan permintaan baru ditolak, data yang tertunda ditangani, dan jalur pengaktifan kembali terdokumentasi. Simpan bukti uji, keputusan lulus/gagal, pemilik tindak lanjut, dan tanggal kedaluwarsa akses.

## Faktor yang mengubah hasil

Hasil acceptance test bergantung pada beberapa kondisi yang harus ditulis, bukan diasumsikan.

- **Model otorisasi.** Scope yang terlalu luas, akun bersama, atau refresh token yang tidak dapat dicabut mengubah risiko. Uji deny-by-default dan pemisahan peran. OWASP menempatkan kelemahan otorisasi dan penyalahgunaan API sebagai area risiko yang perlu diuji, bukan hanya keberhasilan fungsi ([OWASP API Security Top 10 2023](https://owasp.org/API-Security/editions/2023/en/0x11-t10/)).
- **Semantik event.** Provider mungkin mengirim ulang webhook, mengubah urutan, atau mengirim event setelah respons timeout. Tetapkan kunci idempotensi, jendela deduplikasi, dan aturan untuk event terlambat. Jika aturan itu belum disediakan provider, tandai sebagai keputusan terbuka.
- **Batas operasi.** Timeout, rate limit, ukuran payload, dan kuota per tenant menentukan apakah retry aman. Bedakan error yang boleh dicoba lagi dari error yang harus masuk antrean manual. Jangan mengklaim angka kuota tanpa dokumen kontrak penyedia.
- **Kualitas data dan rekonsiliasi.** Perbedaan pembulatan, status parsial, penghapusan, atau pembatalan dapat membuat dua laporan tampak berbeda. Tentukan sumber kebenaran untuk setiap field dan prosedur menangani selisih.
- **Observabilitas.** Log harus cukup untuk menghubungkan request, event, pengguna, dan hasil tanpa membocorkan rahasia. Metrik error dan antrean membantu mendeteksi pola; itu belum sama dengan pemantauan produksi yang terus-menerus.
- **Rantai pasok.** Inventaris komponen (SBOM) meningkatkan transparansi komponen, tetapi tidak membuktikan keamanan. NIST SP 800-161 Rev.1 membahas pengelolaan risiko rantai pasok, sedangkan OpenSSF Scorecard hanya sinyal untuk menilai repositori—keduanya bukan pengganti due diligence penyedia ([CISA SBOM resources](https://www.cisa.gov/sbom), [NIST SP 800-161 Rev.1](https://csrc.nist.gov/pubs/sp/800/161/r1/final), [OpenSSF Scorecard](https://securityscorecards.dev/)).

Kawan Codev.id, faktor-faktor ini juga menjelaskan mengapa hasil sandbox tidak otomatis menjamin perilaku produksi. Versi API, kebijakan penyedia, data nyata, dan konfigurasi jaringan dapat berubah. Simpan asumsi yang belum dibuktikan sebagai risiko penerimaan, bukan sebagai fakta.

## Contoh keputusan praktis

Gunakan tabel keputusan berikut saat rapat penerimaan. Contohnya bersifat pola; nilai dan ID harus diisi dari proyek yang sedang diuji.

| Skenario | Bukti minimum | Keputusan |
| --- | --- | --- |
| Kredensial valid dan scope sesuai | Request ID, respons, efek tunggal | Lulus bila aksi dan audit trail sesuai kontrak |
| Kredensial salah atau scope kurang | Respons penolakan tanpa efek samping | Lulus bila akses ditolak dan tidak membocorkan rahasia |
| Event sama dikirim ulang | Dua request dengan event/idempotency ID sama | Lulus bila efek bisnis tetap satu dan duplikat terlacak |
| Payload invalid atau signature salah | Payload uji, alasan penolakan, log korelasi | Lulus bila ditolak sebelum efek dan dapat diperbaiki |
| Timeout atau error sementara | Bukti timeout, retry, antrean, dan hasil akhir | Tunda bila batas retry/fallback belum disepakati |
| Rate limit tercapai | Respons provider dan perilaku backoff | Lulus hanya jika tidak terjadi retry storm dan operator diberi sinyal |
| Disconnect lalu reconnect | Pencabutan akses, event tertunda, proses pemulihan | Lulus bila tidak ada event hilang tanpa status dan akses lama tidak bekerja |
| Rekonsiliasi selisih | Laporan kedua sisi, aturan sumber kebenaran | Tunda bila selisih tidak punya pemilik atau prosedur resolusi |

Misalnya, sebuah event berhasil diterima tetapi callback balasannya timeout. Jangan langsung mengulang tanpa kunci idempotensi: verifikasi dulu melalui endpoint status atau rekonsiliasi. Jika provider tidak memiliki cara memeriksa status, catat risiko bisnis dan minta keputusan pemilik produk. Teman Codev.id, keputusan “tunda” di sini lebih jujur daripada menandai lulus lalu membiarkan duplikasi ditemukan pengguna.

## Kesalahan umum dan cara memeriksanya

**Hanya menguji happy path.** Tambahkan pasangan uji untuk setiap alur: valid/invalid, baru/duplikat, cepat/timeout, di bawah/di atas kuota, tersambung/terputus. Setiap pasangan harus menunjuk ke bukti yang sama-sama dapat diperiksa.

**Memakai akun administrator untuk semua skenario.** Buat matriks peran dan minta bukti penolakan. Periksa juga pencabutan token dan rotasi kredensial; jangan menyalin token ke screenshot.

**Menganggap retry selalu aman.** Tanyakan apakah operasi idempotent, bagaimana backoff ditentukan, dan siapa yang menangani pesan yang sudah melewati batas. Uji crash di antara pengiriman request dan penyimpanan respons.

**Mengabaikan event yang datang di luar urutan.** Simpan event ID, versi, dan waktu penerimaan. Uji apakah event lama ditolak, ditunda, atau diterapkan dengan aturan yang jelas.

**Menutup tiket hanya dengan screenshot.** Bukti harus mencakup input, konfigurasi versi, respons, efek di kedua sistem, log korelasi, dan keputusan. Screenshot tanpa data pembanding tidak membuktikan rekonsiliasi.

**Mencampur acceptance dengan monitoring produksi.** Acceptance membuktikan kondisi yang diuji pada saat serah terima. Tetapkan pemilik dashboard, alert, dan prosedur insiden secara terpisah; jika belum ada, tuliskan sebagai pekerjaan sebelum go-live, bukan klaim telah selesai.

## Keberatan atau jalan pintas yang perlu diluruskan

Shortcut yang sering dipilih adalah “provider sudah tersertifikasi, jadi cukup satu transaksi contoh.” Sertifikasi atau dokumentasi provider dapat membantu memahami kontrak, tetapi tidak membuktikan konfigurasi tenant, peran lokal, mapping data, retry, atau jalur disconnect milik Anda. Alternatif yang lebih aman adalah meminta paket bukti kecil namun lengkap: matriks skenario, data uji, log korelasi, hasil rekonsiliasi, dan daftar pengecualian yang ditandatangani pemilik masing-masing sistem.

Jangan mengisi celah dengan asumsi tentang kuota, subprosesor, kerentanan terkini, atau dukungan provider. `[NEEDS VENDOR REVIEW: confirm current terms, quotas, subprocessors, and vulnerability status before acceptance]` tetap terlihat sampai ada konfirmasi yang dapat disimpan. Begitu pula, acceptance test tidak menggantikan provider certification atau monitoring produksi.

## Kesimpulan

Acceptance test untuk integrasi eksternal adalah keputusan berbasis bukti: identitas dan peran benar, event normal maupun duplikat terkendali, data invalid ditolak, timeout/rate limit/replay/disconnect memiliki jalur aman, dan catatan kedua sistem dapat direkonsiliasi. OpenAPI, panduan keamanan OAuth/WebAuthn, dan sumber rantai pasok membantu menyusun pertanyaan, tetapi perilaku proyek tetap harus dibuktikan di lingkungan yang disepakati.

Langkah berikutnya, minta tim membuat matriks skenario dan paket bukti untuk setiap endpoint atau event, lalu jadwalkan peninjauan teknis atas pengecualian dan penanda vendor yang belum terjawab. Kawan Codev.id, aturan operasinya sederhana: jangan menandai integrasi lulus sebelum hasil normal, batas, pemulihan, dan pencabutan akses dapat diamati serta direkonsiliasi; serahkan sertifikasi provider dan pemantauan produksi kepada proses yang memang memilikinya.
