---
article_id: CDV-08-A05
title: "Timeout, Rate Limit, Retry, dan Fallback Dependensi"
slug: "timeout-rate-limit-retry-fallback"
description: "Classify failures, set deadlines, retry/backoff, circuit/fallback behavior, queueing, user messaging, reconciliation, and alerts"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2025-09-30"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-08
primary_intent: "Keep a product safe when a dependency slows or fails"
reader_community: "Codev.id"
reader_address: "Teman Codev.id"
final_route: "/artikel/timeout-rate-limit-retry-fallback.html"
technical_review: required
sources:
  - "https://spec.openapis.org/oas/v3.1.1.html"
  - "https://owasp.org/API-Security/editions/2023/en/0x11-t10/"
  - "https://www.cisa.gov/sbom"
 - "https://csrc.nist.gov/pubs/sp/800/161/r1/final"
  - "https://securityscorecards.dev/"
---

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

# Timeout, Rate Limit, Retry, dan Fallback Dependensi

Halo, Teman Codev.id! Ketika API pihak ketiga melambat, menolak terlalu banyak permintaan, atau berhenti merespons, keputusan aman bukan “ulang terus sampai berhasil”. Tetapkan batas waktu, bedakan jenis kegagalan, lalu pilih retry, antrean, fallback, atau penghentian berdasarkan apakah operasi itu aman diulang dan apakah hasil akhirnya bisa diverifikasi.

Urutannya praktis: deadline harus membatasi setiap percobaan dan seluruh rantai panggilan; rate limit harus dihormati dengan backoff; retry hanya untuk kegagalan sementara dan operasi idempoten (hasilnya tetap sama bila diulang); fallback harus jujur terhadap pengguna; dan setiap permintaan yang mungkin berhasil tetapi responsnya hilang harus masuk rekonsiliasi. Detail kuota, kontrak, serta perilaku vendor belum tersedia di paket ini, jadi bagian yang bergantung pada informasi tersebut memerlukan `[NEEDS VENDOR REVIEW: GATE-04/GATE-09]`.

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

*Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.*

## Jawaban singkat dan salah paham utama

Timeout adalah batas menunggu, bukan bukti bahwa operasi gagal. Rate limit adalah sinyal bahwa pengirim perlu mengurangi laju, bukan alasan untuk menambah paralelisme. Retry adalah mekanisme pemulihan terbatas, bukan loop tanpa akhir. Fallback adalah perilaku alternatif yang sudah dirancang, bukan menampilkan data lama tanpa penanda.

Miskonsepsi yang paling mahal ialah menganggap semua error setara. Timeout setelah server menerima pembayaran berbeda dari penolakan kuota sebelum permintaan diproses. Jika aplikasi langsung mengulang transaksi non-idempoten, pesanan atau tagihan ganda dapat tercipta. Sebaliknya, bila semua error dianggap permanen, gangguan singkat berubah menjadi kegagalan yang terlihat pengguna.

Mulailah dengan pertanyaan: “Apakah saya tahu status operasi di sisi dependensi?” Jika tidak, tandai status sebagai tidak diketahui, simpan idempotency key atau korelasi yang sama, dan siapkan rekonsiliasi. Jangan mengklaim sukses hanya karena koneksi terputus setelah pengiriman.

## Definisi dan batas objek

Dependensi di sini berarti layanan eksternal yang dipanggil produk: API pembayaran, pengiriman pesan, identitas, pencarian, atau sumber data lain. Timeout mencakup deadline koneksi, pembacaan, dan total operasi. Rate limit mencakup respons eksplisit seperti 429 maupun kuota yang dinyatakan dalam kontrak.

Retry dan fallback adalah keputusan pada klien atau lapisan perantara; keduanya tidak membuat dependensi sepenuhnya andal. Artikel ini tidak menetapkan angka timeout, jumlah percobaan, SLA, harga, kuota, atau kewajiban hukum vendor. Angka tersebut harus berasal dari kontrak dan uji lingkungan Anda. OpenAPI membantu mendeskripsikan antarmuka, tetapi keberadaan spesifikasi tidak membuktikan implementasi atau perilaku keamanan aktual. ([OpenAPI Specification 3.1.1](https://spec.openapis.org/oas/v3.1.1.html))

## Cara kerjanya

Di tepi aplikasi, buat satu deadline keseluruhan dan turunkan sisa waktunya ke setiap panggilan. Bila sisa deadline terlalu pendek, hentikan panggilan berikutnya dan kembalikan status yang dapat ditindaklanjuti. Catat dependency, endpoint, metode, correlation ID, idempotency key, percobaan ke berapa, dan alasan berhenti—tanpa merekam token atau data rahasia.

Klasifikasikan hasil menjadi empat kelompok:

1. **Dapat dicoba ulang:** gangguan jaringan sementara, 502/503, atau rate limit yang memberi petunjuk waktu tunggu. Gunakan exponential backoff dengan jitter agar klien tidak menyerbu bersamaan.
2. **Jangan diulang otomatis:** validasi 4xx, kredensial tidak sah, atau permintaan yang tidak idempoten tanpa kunci deduplikasi.
3. **Status tidak diketahui:** timeout setelah pengiriman, putus koneksi saat membaca, atau respons hilang. Simpan pekerjaan untuk pemeriksaan status, bukan retry buta.
4. **Kegagalan kebijakan:** circuit breaker terbuka setelah ambang kegagalan internal tercapai. Gagal cepat mencegah thread dan koneksi habis.

Rate limit sebaiknya dipatuhi pada beberapa lapisan: per pengguna, tenant, endpoint, dan dependency. Jeda dari header vendor boleh dipakai hanya setelah diverifikasi maknanya. Kontrol terhadap penyalahgunaan API perlu diperlakukan sebagai bagian dari desain, bukan sekadar penanganan 429; OWASP menempatkan risiko otorisasi dan konsumsi sumber daya dalam API Security Top 10. ([OWASP API Security Top 10 2023](https://owasp.org/API-Security/editions/2023/en/0x11-t10/))

Antrean memisahkan pekerjaan yang boleh tertunda dari permintaan interaktif. Pesan pengguna dapat berbunyi “sedang diproses” dengan ID pelacakan, sementara worker mengulang sesuai kebijakan. Worker harus idempoten dan memiliki dead-letter queue untuk pekerjaan yang melewati batas, bukan mengulang selamanya.

## Faktor yang mengubah hasil

Pertama, sifat operasi: membaca katalog biasanya lebih mudah diulang daripada membuat pembayaran. Kedua, deadline bisnis: pencarian halaman boleh fallback, tetapi keputusan otorisasi mungkin harus berhenti. Ketiga, kapasitas lokal: retry agresif dapat menghabiskan thread pool, koneksi, dan anggaran rate limit sendiri.

Keempat, kontrak vendor. Pastikan status kode, format error, batas kuota, idempotency, dan mekanisme pemeriksaan status tertulis. Inventaris komponen dan pemasok membantu mengetahui siapa yang harus dihubungi ketika jalur kritis berubah; SBOM meningkatkan transparansi komponen, tetapi tidak membuktikan bahwa dependensi aman. ([CISA SBOM resources](https://www.cisa.gov/sbom))

Kelima, bukti operasional. Uji timeout, 429, respons lambat, respons terpotong, dan pemulihan setelah circuit ditutup di lingkungan yang aman. Catat apakah fallback menjaga konsistensi dan apakah rekonsiliasi menemukan operasi yang sebenarnya berhasil. Rencana manajemen risiko rantai pasok dapat membantu menilai pemasok, tetapi bukan pengganti verifikasi kontrak dan uji integrasi. ([NIST SP 800-161 Rev. 1](https://csrc.nist.gov/pubs/sp/800/161/r1/final))

Untuk kuota, subprosesor, perubahan API, dan kerentanan terkini, tinggalkan `[NEEDS VENDOR REVIEW: GATE-04/GATE-09]` sampai pemilik layanan memeriksa dokumentasi dan kontrak yang berlaku. Jangan mengisi kekosongan itu dengan asumsi dari vendor lain.

## Contoh keputusan praktis

Bayangkan worker mengirim notifikasi ke layanan eksternal. Jika koneksi gagal sebelum request terkirim, retry terbatas dengan backoff masuk akal. Jika request terkirim tetapi respons timeout, simpan status “tidak diketahui”, lalu panggil endpoint status menggunakan ID yang sama atau masukkan rekonsiliasi manual. Jika layanan menjawab 429, hormati jeda yang tervalidasi dan kurangi konkurensi. Jika circuit terbuka, antrekan pekerjaan dan berikan pengguna status tertunda—bukan pesan sukses.

Gunakan tabel keputusan berikut sebagai titik awal, bukan angka baku:

| Sinyal | Tindakan awal | Bukti yang harus dicatat |
|---|---|---|
| 400/401/403 | hentikan retry otomatis | kode bisnis, endpoint, versi kredensial |
| 429 | backoff, batasi konkurensi | header rate limit, tenant, waktu tunggu |
| 502/503 atau koneksi putus sebelum kirim | retry terbatas bila idempoten | nomor percobaan dan deadline tersisa |
| timeout setelah kirim | status tidak diketahui, rekonsiliasi | idempotency key dan correlation ID |
| circuit terbuka | gagal cepat atau antrekan | alasan pembukaan dan waktu pemulihan |

Teman Codev.id, tuliskan keputusan ini dalam runbook per dependency. Sertakan siapa pemiliknya, kapan fallback boleh dipakai, dan bagaimana operator menutup pekerjaan yang buntu.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah satu timeout global untuk seluruh rantai. Periksa apakah setiap hop memiliki deadline turunan sehingga satu layanan lambat tidak menghabiskan waktu layanan lain. Kedua, retry tanpa jitter. Amati lonjakan serentak setelah pemulihan dan tambahkan variasi jeda.

Ketiga, menjadikan cache sebagai kebenaran tanpa label usia data. Periksa timestamp dan tampilkan keterbatasannya. Keempat, menyamakan “diterima antrean” dengan “berhasil di vendor”. Pisahkan status queued, sent, confirmed, failed, dan unknown di model data.

Kelima, log berisi token, payload sensitif, atau respons mentah. Tinjau redaksi log dan batasi akses. Keenam, alarm hanya pada jumlah error. Pantau latency, timeout rate, 429, ukuran antrean, circuit state, usia pekerjaan tertua, dan selisih hasil rekonsiliasi. Skor repositori pihak ketiga hanyalah sinyal; due diligence tetap diperlukan. ([OpenSSF Scorecard](https://securityscorecards.dev/))

## Jalan pintas yang tampak praktis tetapi berisiko

“Tambahkan tiga retry di semua request” terdengar sederhana. Namun angka yang sama mengabaikan jenis operasi, deadline, dan kapasitas. Pada pembayaran, pengulangan dapat menggandakan efek; pada endpoint baca, tiga retry mungkin tetap menghabiskan waktu pengguna. Alternatif yang lebih aman adalah kebijakan per operasi: idempotency key untuk efek samping, retry hanya pada kelas error yang disetujui, circuit breaker, antrean untuk pekerjaan asinkron, dan rekonsiliasi untuk status tidak diketahui.

## Kesimpulan dan langkah berikutnya

Timeout, rate limit, retry, dan fallback harus dipasang sebagai satu sistem keputusan: deadline membatasi waktu, klasifikasi menentukan boleh tidaknya retry, backoff mengendalikan laju, circuit breaker melindungi kapasitas lokal, fallback mengelola ekspektasi, dan rekonsiliasi menutup celah status tidak diketahui.

Kawan Codev.id, buat satu lembar kontrak untuk setiap dependency berisi operasi, klasifikasi error, deadline, aturan retry, status antrean, jalur fallback, metrik, dan pemilik rekonsiliasi. Tautkan pembaca ke [beranda Codev.id](/) bila perlu menelusuri konteks integrasi lain, tetapi jangan anggap dokumentasi umum sebagai bukti perilaku vendor Anda. Minta tinjauan teknis atas kontrak dan uji kegagalan sebelum mengaktifkan kebijakan di produksi. Aturan operasionalnya: jangan menyatakan sukses sampai ada bukti konfirmasi; bila buktinya hilang, simpan status tidak diketahui dan rekonsiliasi.
