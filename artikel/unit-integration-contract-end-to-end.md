---
article_id: CDV-10-A02
title: "Unit, Integration, Contract, dan End-to-end Tests"
slug: "unit-integration-contract-end-to-end"
description: "Menjelaskan bukti, keterbatasan, biaya perawatan, dan cara menggabungkan unit, integration, contract, serta end-to-end test di sekitar kontrak stabil dan perjalanan kritis"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2025-11-08"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-10
primary_intent: "Choose test levels for different failure boundaries"
reader_community: "Codev.id"
reader_address: "Sobat Codev.id"
final_route: "/artikel/unit-integration-contract-end-to-end.html"
technical_review: required
sources:
  - "https://csrc.nist.gov/pubs/sp/800/218/final"
  - "https://www.w3.org/TR/WCAG-EM/"
  - "https://spec.openapis.org/oas/v3.1.1.html"
  - "https://www.w3.org/TR/WCAG22/"
  - "https://www.w3.org/WAI/test-evaluate/preliminary/"
  - "https://web.dev/articles/vitals"
  - "https://developer.chrome.com/docs/crux"
  - "https://www.rfc-editor.org/rfc/rfc9111"
---

# Unit, Integration, Contract, dan End-to-end Tests

Halo, Sobat Codev.id!

Ketika sebuah tim bertanya, “Tes mana yang harus kami tambah?”, jawaban yang aman bukan memilih satu level lalu mengejar jumlah terbanyak. Unit test memberi umpan balik cepat untuk logika kecil, integration test memeriksa sambungan nyata, contract test menjaga kesepakatan antar-komponen, sedangkan end-to-end (E2E) test mengikuti perjalanan pengguna dari awal sampai hasil akhir. Keempatnya membuktikan batas kegagalan yang berbeda.

Gabungkan level berdasarkan risiko dan batas kegagalan: logika yang sering berubah diuji dekat dengan kode, antarmuka antarlayanan dijaga oleh kontrak yang dapat dipahami mesin, lalu beberapa perjalanan paling kritis dijalankan lintas sistem. Tes yang lulus hanya membuktikan asersi yang memang disampel pada build, lingkungan, dan data tersebut; ia bukan sertifikat bahwa semua perilaku aman. Prinsip penelusuran risiko, kebutuhan, hasil, dan cacat juga sejalan dengan praktik pengembangan perangkat lunak aman NIST ([NIST SP 800-218 SSDF 1.1](https://csrc.nist.gov/pubs/sp/800/218/final)).

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

*Gambar ini merupakan aset lokal untuk ilustrasi dan bukan dokumentasi proyek tertentu.*

## Definisi dan batas objek

“Level” di sini berarti lokasi dan luas batas yang diuji, bukan peringkat kualitas. Unit mengisolasi fungsi, kelas, atau modul dengan dependensi luar diganti *stub* atau *mock*. Pertanyaannya: apakah aturan lokal, transformasi data, dan cabang kesalahan bekerja untuk input yang dipilih? Ia tidak membuktikan konfigurasi jaringan, serialisasi aktual, atau perilaku layanan pihak ketiga.

Integration test menghubungkan dua atau lebih komponen—misalnya modul aplikasi dengan basis data, antrean, atau layanan HTTP—dalam lingkungan uji. Ia dapat menemukan kesalahan skema, transaksi, autentikasi, dan konfigurasi yang luput dari unit test. Sebaliknya, hasilnya masih bergantung pada versi dependensi, data awal, dan cara lingkungan itu disiapkan.

Contract test memeriksa kesepakatan yang terlihat oleh konsumen dan penyedia layanan: bentuk permintaan, respons, status, error, dan aturan kompatibilitas. Kontrak bukan sekadar dokumentasi yang indah. Spesifikasi OpenAPI 3.1.1, misalnya, menyediakan format untuk mendeskripsikan antarmuka HTTP; spesifikasi tersebut membantu alat dan tim menyepakati bentuk API, tetapi tidak dengan sendirinya membuktikan implementasi selalu mematuhinya ([OpenAPI Specification 3.1.1](https://spec.openapis.org/oas/v3.1.1.html)).

E2E menjalankan alur yang paling mendekati penggunaan nyata melintasi antarmuka, layanan, penyimpanan, dan UI atau klien. Nilainya ada pada pertanyaan “bisakah tujuan pengguna tercapai?”, bukan pada banyaknya skenario. Karena batasnya lebar, E2E cenderung lebih lambat, lebih mahal dirawat, dan lebih rentan gagal karena data atau lingkungan. Ia juga sulit menunjukkan komponen mana yang menjadi sumber masalah.

Batas penting lain adalah pemeriksaan spesialis. Aksesibilitas, keamanan, performa, dan eksplorasi bukan otomatis “tercakup” hanya karena ada E2E. Evaluasi aksesibilitas perlu memeriksa halaman dan proses yang relevan—termasuk keyboard, fokus, semantik, formulir, error, zoom, autentikasi, media, serta perilaku dengan teknologi asistif—bukan mengandalkan satu pemindai ([WCAG-EM 1.0](https://www.w3.org/TR/WCAG-EM/), [WCAG 2.2](https://www.w3.org/TR/WCAG22/), dan [WAI Easy Checks](https://www.w3.org/WAI/test-evaluate/preliminary/)).

## Cara kerjanya

Mulailah dari risiko atau tujuan pengguna, lalu turunkan bukti yang diperlukan. Untuk aturan diskon, unit test dapat menguji kombinasi input dan keputusan tanpa menyalakan layanan lain. Untuk penyimpanan pesanan, integration test menguji transaksi dan pemetaan data dengan komponen yang benar-benar digunakan. Untuk API pembayaran, contract test menjaga agar perubahan respons yang dikonsumsi klien terdeteksi sebelum dirilis. Untuk perjalanan “buat pesanan sampai menerima konfirmasi”, satu E2E terpilih memeriksa bahwa sambungan penting benar-benar bertemu.

Urutannya tidak harus berupa tangga kaku. Saat kontrak API berubah, tes kontrak konsumen dan penyedia memberi sinyal yang lebih terarah daripada menunggu E2E gagal. Saat unit test menemukan cacat aturan, perbaikan dapat dilakukan sebelum lingkungan terintegrasi dibangun. Setelah setiap hasil, simpan jejak sederhana: kebutuhan atau risiko, level tes, build dan data yang dipakai, hasil, serta cacat yang masih terbuka. Jejak ini membuat “lulus” dapat ditafsirkan dan diulang, bukan hanya angka di dashboard.

Gunakan *test double* hanya untuk batas yang memang ingin diisolasi. Mock yang terlalu mirip implementasi dapat menyembunyikan perbedaan perilaku; sebaliknya, mengganti semua dependensi dengan layanan sungguhan membuat unit test lambat dan sulit didiagnosis. Pilih batas dengan pertanyaan: “Komponen mana yang sedang saya buktikan, dan kegagalan mana yang sengaja saya keluarkan?”

Pada kontrak, sepakati pemilik perubahan dan aturan kompatibilitas. Uji contoh permintaan dan respons yang benar, error yang dijanjikan, serta perubahan yang dianggap merusak. Tetap jalankan integration test untuk hal yang tidak dapat dipastikan dari bentuk pesan, seperti otorisasi, urutan transaksi, batas waktu, atau konfigurasi runtime.

E2E sebaiknya memulai dari perjalanan kritis yang dapat diamati hasilnya. Buat data uji terisolasi, titik awal yang jelas, dan diagnostik ketika gagal. Jika alur sama diuji ulang oleh puluhan variasi yang sebenarnya hanya memeriksa aturan lokal, pindahkan variasi itu ke unit atau integration test dan sisakan E2E sebagai bukti lintas-batas.

## Faktor yang mengubah hasil

Beberapa kondisi mengubah level yang paling ekonomis dan bukti yang dapat dipercaya.

**Batas perubahan.** Modul stabil dengan logika kompleks membutuhkan unit test yang tajam. API yang dikonsumsi banyak klien membutuhkan kontrak yang ditinjau bersama. Sistem yang sering mengubah orkestrasi atau konfigurasi membutuhkan integration test yang merepresentasikan lingkungan tersebut.

**Kritisnya perjalanan.** Login, pembayaran, pemulihan akun, atau ekspor data mungkin layak mendapat E2E karena kegagalannya memblokir tujuan utama. Namun status “kritis” harus ditetapkan oleh pemilik produk dan risiko, bukan diasumsikan dari nama fitur.

**Kualitas data dan lingkungan.** Test yang lulus pada data kosong belum menjawab perilaku pada konflik, duplikasi, atau izin yang berbeda. Versi layanan, jam sistem, antrean, cache, dan feature flag dapat mengubah hasil. Catat kondisi itu; jangan membandingkan dua run seolah identik jika lingkup dan versinya berbeda.

**Aksesibilitas dan performa.** Pemeriksaan aksesibilitas harus mencakup proses yang benar-benar ditempuh pengguna dan kombinasi input yang relevan, bukan hanya halaman awal. Untuk performa, Core Web Vitals adalah metrik yang dikelola penyedianya dan dapat berkembang; ukur dengan lingkup, versi, kondisi, dan caveat yang jelas ([Core Web Vitals](https://web.dev/articles/vitals)). Data lapangan dari Chrome UX Report (CrUX) menggambarkan pengalaman pengguna yang termasuk dalam dataset, bukan jaminan untuk setiap pengunjung ([dokumentasi CrUX](https://developer.chrome.com/docs/crux)).

Cache juga dapat membuat hasil tampak berbeda. Aturan freshness, validasi ulang, dan penyimpanan perantara mengikuti semantik HTTP caching; karena itu uji cache harus menjelaskan permintaan, header, perantara, dan kondisi invalidasinya ([RFC 9111](https://www.rfc-editor.org/rfc/rfc9111)). Jangan mengubah satu pengukuran lab menjadi klaim pasti tentang ranking, konversi, energi, atau pengalaman semua pengguna.

`[NEEDS GATE-06 REVIEW: tidak ada ambang universal untuk komposisi level tes atau persentase coverage; keputusan akhir harus ditetapkan dari risiko, kontrak, dan perjalanan kritis proyek.]`

## Contoh keputusan praktis

Bayangkan layanan katalog yang menyediakan API untuk aplikasi web. Berikut contoh pemetaan bersyarat; nama komponen dan risikonya harus diganti dengan konteks proyek Anda.

| Pertanyaan yang ingin dibuktikan | Level utama | Bukti yang masih perlu dilengkapi |
| --- | --- | --- |
| Apakah aturan validasi SKU menolak input yang salah? | Unit | Kasus batas dan pesan yang terlihat konsumen |
| Apakah penyimpanan dan pembacaan katalog konsisten? | Integration | Skema aktual, transaksi, dan data uji |
| Apakah perubahan respons tidak mematahkan klien? | Contract | Contoh konsumen-penyedia dan kebijakan kompatibilitas |
| Apakah pengguna dapat mencari lalu menyimpan item? | E2E | Data terisolasi, akun uji, dan hasil yang dapat diamati |
| Apakah keyboard, fokus, dan error dapat digunakan? | Evaluasi aksesibilitas khusus | Cakupan proses, perangkat bantu, dan pemeriksaan manual |
| Apakah respons tetap wajar pada kondisi lapangan? | Pengukuran performa khusus | Lingkup, versi, sampel, dan kondisi pengukuran |

Kawan Codev.id, perhatikan bahwa baris terakhir bukan level keempat yang “lebih tinggi”. Ia adalah jenis bukti berbeda yang dapat berjalan di beberapa level. Demikian pula, pemindaian statis atau pengujian keamanan tidak berubah menjadi E2E hanya karena dijalankan pada pipeline yang sama.

Untuk memilih urutan kerja, buat matriks kecil dengan tiga kolom: risiko, batas kegagalan, dan bukti yang paling murah untuk menguranginya. Tambahkan kolom “bukti yang belum ada”. Jika risiko berada di bentuk API, mulai dari kontrak; jika berada di orkestrasi, tambahkan integration; jika berada di tujuan pengguna, pilih E2E. Tinjau kembali matriks saat arsitektur atau perjalanan utama berubah.

## Kesalahan umum dan cara memeriksanya

**Mengejar coverage sebagai jaminan.** Coverage menunjukkan bagian kode yang tersentuh oleh instrumen tertentu, bukan kualitas asersi atau skenario yang tidak diuji. Periksa apakah setiap risiko memiliki hasil yang dapat ditafsirkan dan apakah cacat terbuka terlihat.

**Menjadikan E2E sebagai satu-satunya pagar.** Suite besar yang gagal tidak memberi lokasi masalah yang jelas. Tanyakan apakah kegagalan dapat direproduksi lebih cepat pada unit, integration, atau kontrak, lalu tambahkan tes di batas yang tepat.

**Menganggap kontrak sama dengan implementasi.** Bentuk respons dapat benar sementara aturan izin, urutan kejadian, atau efek sampingnya salah. Sertakan integration test dan pemeriksaan perilaku yang memang tidak tersurat dalam kontrak.

**Mengandalkan satu pemindai aksesibilitas.** Jalankan pemeriksaan keyboard, fokus, struktur, formulir, zoom, dan teknologi asistif pada cakupan proses yang relevan. Hasil pemindai adalah petunjuk, bukan sertifikasi menyeluruh ([WCAG-EM 1.0](https://www.w3.org/TR/WCAG-EM/)).

**Membandingkan angka performa tanpa kondisi.** Catat build, perangkat, jaringan, cache, sampel, dan alat. Untuk data lapangan, jelaskan siapa yang terwakili oleh dataset CrUX; untuk cache, jelaskan apakah respons berasal dari cache atau origin. Tanpa konteks itu, perubahan kecil mudah disalahartikan sebagai sebab-akibat.

Checklist review sebelum menyatakan siap:

- Apakah setiap perjalanan kritis memiliki satu bukti lintas-sistem yang dapat diulang?
- Apakah perubahan API memicu pemeriksaan kontrak konsumen dan penyedia?
- Apakah integration test menggunakan dependensi dan data yang mewakili risiko?
- Apakah kegagalan dapat ditelusuri ke kebutuhan, build, lingkungan, dan cacat terbuka?
- Apakah pemeriksaan aksesibilitas dan performa memiliki cakupan serta caveat sendiri?

## Jalan pintas yang perlu dihindari

Shortcut yang sering dipilih adalah “cukup unit test dengan coverage tinggi; E2E terlalu mahal.” Ia memang menghemat waktu eksekusi, tetapi tidak menyentuh sambungan yang justru sering gagal: skema API, konfigurasi, izin, transaksi, dan perjalanan lintas-layanan. Kebalikannya—menulis E2E untuk semua variasi—membuat umpan balik lambat dan perawatan mahal.

Alternatif yang lebih dapat dipertanggungjawabkan adalah menetapkan batas kegagalan per risiko. Letakkan variasi aturan di unit, kesepakatan pesan di contract, sambungan nyata di integration, dan hanya perjalanan bernilai tinggi di E2E. Simpan hasil beserta kondisi run dan cacat yang belum terselesaikan. Bila keputusan menyangkut aksesibilitas, keamanan, atau performa produksi, minta review spesialis sesuai cakupannya; level tes tidak menggantikan persetujuan tersebut.

## Kesimpulan

Unit, integration, contract, dan E2E bukan pilihan saling meniadakan. Unit membuktikan logika terisolasi, integration membuktikan sambungan nyata, contract membuktikan kesepakatan antarmuka, dan E2E membuktikan perjalanan kritis lintas sistem. Pilih kombinasi berdasarkan risiko, bukan target coverage atau piramida yang dianggap universal.

Sobat Codev.id, langkah berikutnya adalah membuat matriks risiko–batas–bukti untuk satu perjalanan utama, menautkan setiap hasil ke build dan data uji, lalu meminta review teknis atas celah yang tersisa. Mulai dari kontrak dan integration yang paling dekat dengan perubahan, tambah E2E hanya untuk tujuan pengguna yang benar-benar kritis, dan nyatakan batas bukti secara jujur ketika lingkungan atau gate belum terverifikasi. Untuk menyelaraskan ruang lingkup dan langkah berikutnya, Anda dapat mulai dari [Codev.id](/).

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
