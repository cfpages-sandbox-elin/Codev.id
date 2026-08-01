---
article_id: CDV-13-A01
title: "Menyusun Rencana Konformitas WCAG 2.2"
slug: "rencana-konformitas-wcag-22"
description: "Define product/pages/states, target level, WCAG 2.2 criteria, roles, design/dev/content evidence, testing, exceptions, remediation, and statement"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2026-01-15"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-13
primary_intent: "Scope and evidence an accessibility conformance effort"
reader_community: "Codev.id"
reader_address: "Kawan Codev.id"
final_route: "/artikel/rencana-konformitas-wcag-22.html"
technical_review: required
sources:
  - "https://www.gov.uk/service-manual/agile-delivery"
  - "https://www.w3.org/TR/WCAG22/"
  - "https://csrc.nist.gov/pubs/sp/800/218/final"
  - "https://www.w3.org/TR/WCAG-EM/"
  - "https://spec.openapis.org/oas/v3.1.1.html"
  - "https://www.w3.org/WAI/test-evaluate/preliminary/"
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

# Menyusun Rencana Konformitas WCAG 2.2

Halo, Kawan Codev.id! Rencana konformitas WCAG 2.2 bukan kalimat “situs ini sudah aksesibel” dan bukan pula pekerjaan memasang satu pemindai otomatis. Dokumen itu adalah kesepakatan kerja: produk dan halaman apa yang dinilai, kondisi penggunaan mana yang termasuk, level target yang dipilih, bukti yang harus dikumpulkan, siapa yang memeriksa, dan apa yang dilakukan terhadap temuan.

Urutan paling aman adalah menetapkan objek dan batasnya lebih dahulu, memetakan kebutuhan ke kriteria WCAG 2.2 yang relevan, lalu menggabungkan bukti desain, kode, konten, dan pengujian. Pernyataan konformitas baru boleh dipertimbangkan setelah evaluasi yang disepakati selesai; lulus pemindaian pada sebagian halaman tidak mengubah status itu. Definisi kriteria dan level tetap merujuk pada [WCAG 2.2 Recommendation](https://www.w3.org/TR/WCAG22/), sedangkan metode evaluasi dan peninjauan yang memenuhi syarat dapat mengubah keputusan akhir.

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

Ilustrasi umum dari aset lokal codev.id; bukan dokumentasi proyek tertentu.

## Jawaban singkat dan salah paham utama

Mulailah dengan satu lembar rencana yang memiliki tujuh keputusan: scope produk, daftar halaman dan alur, state yang mungkin muncul, target level, kriteria yang diprioritaskan, pemilik bukti, dan aturan rilis. Tambahkan kolom untuk hasil uji, pengecualian, perbaikan, serta retest. Dengan begitu, “konformitas” menjadi jejak keputusan yang dapat ditinjau, bukan pendapat orang terakhir yang melihat layar.

Salah paham yang sering mahal adalah menganggap WCAG sebagai daftar centang statis. Menu yang tampak benar pada halaman awal dapat gagal ketika fokus berpindah ke dialog, ketika validasi formulir menampilkan error, atau ketika pengguna memperbesar tampilan. Karena itu, rencana harus mencakup alur dan kondisi, bukan hanya URL yang mudah dikunjungi. [W3C Easy Checks](https://www.w3.org/WAI/test-evaluate/preliminary/) berguna sebagai pemeriksaan awal, tetapi tidak menggantikan evaluasi menyeluruh.

Ada pula perbedaan antara “memenuhi pedoman” dan “memenuhi hukum”. WCAG adalah standar teknis; penerapannya tidak otomatis menjadi kepatuhan hukum Indonesia. Jika kontrak, pengadaan, atau regulator menetapkan kewajiban tambahan, minta peninjauan profesional yang memahami konteks tersebut. [NEEDS PROFESSIONAL REVIEW: pemetaan hasil evaluasi ke kewajiban hukum atau kontrak proyek]

## Definisi dan batas objek

Tuliskan nama produk, versi atau build, domain, dan pemilik keputusan. Setelah itu buat inventaris halaman berdasarkan fungsi pengguna: halaman publik, area setelah autentikasi, alur transaksi, pusat bantuan, dan titik keluar. “Halaman” di sini juga mencakup tampilan yang muncul tanpa URL baru, seperti modal, menu terbuka, pesan kosong, loading, timeout, dan halaman error.

Pisahkan tiga hal yang sering tercampur:

1. **Target** adalah level konformitas yang dipilih untuk scope tertentu. Jangan menyebut level tanpa menyebut produk, versi, dan batas evaluasinya.
2. **Kriteria dan kebutuhan** menjelaskan perilaku yang ingin dicapai, misalnya urutan fokus yang dapat diikuti atau pesan error yang terhubung dengan bidangnya.
3. **Bukti** menunjukkan bagaimana keputusan itu diperiksa: spesifikasi desain, implementasi, contoh konten, rekaman uji, atau persetujuan pengecualian.

Batas ini mencegah rencana berubah menjadi janji universal. Komponen pihak ketiga, konten yang diunggah pengguna, dan alur yang hanya bisa diakses setelah login harus diberi pemilik serta aturan aksesnya. Jika sebagian produk belum dapat dinilai, nyatakan sebagai scope yang belum dievaluasi, bukan diam-diam memasukkannya ke dalam klaim.

## Cara kerjanya

**1. Temukan pekerjaan dan asumsi.** Mulai dari tugas pengguna, stakeholder, kendala teknis, dan risiko yang diketahui. Pendekatan delivery bertahap menganjurkan asumsi diuji melalui pekerjaan kecil dan umpan balik, bukan ditumpuk sampai akhir; lihat panduan [agile delivery UK Government Service Manual](https://www.gov.uk/service-manual/agile-delivery). Catat siapa yang boleh menerima risiko dan siapa yang menyetujui perubahan scope.

**2. Bangun matriks scope.** Setiap baris berisi halaman atau alur, state, peran pengguna, kriteria WCAG yang relevan, pemilik, bukti yang diharapkan, metode uji, status temuan, dan tautan ke tiket perbaikan. Satu kriteria dapat muncul di banyak state; satu state dapat memerlukan beberapa jenis bukti.

**3. Pilih target secara eksplisit.** Tetapkan level target untuk rilis ini dan jelaskan alasan bisnis atau pengguna tanpa mengubah alasan itu menjadi pengecualian otomatis. Jika target hanya berlaku pada area tertentu, tulis area tersebut di judul dokumen dan di setiap laporan.

**4. Bagikan pekerjaan lintas peran.** Desainer bertanggung jawab atas struktur, urutan, dan keadaan komponen; pengembang atas semantik, perilaku keyboard, fokus, dan perubahan dinamis; penulis konten atas label, instruksi, serta error; QA atau evaluator atas rancangan pengujian dan reproduksi; pemilik produk atas prioritas dan keputusan rilis. Pembagian ini adalah akuntabilitas, bukan bukti bahwa salah satu peran telah lulus.

**5. Uji dengan lapisan yang tepat.** Otomasi dapat menangkap assertion tertentu pada build dan data tertentu. Ia tidak membuktikan seluruh halaman, semua browser, teknologi bantu, atau pemahaman pengguna. Padukan pemeriksaan kode dan komponen, keyboard dan fokus, zoom atau reflow, formulir dan error, autentikasi, media, serta interaksi dengan teknologi bantu. [WCAG-EM 1.0](https://www.w3.org/TR/WCAG-EM/) membantu menata scope, sampling, dan pelaporan evaluasi; gunakan prosedurnya sesuai konteks proyek.

**6. Hubungkan temuan ke tindakan.** Setiap kegagalan harus memiliki contoh reproduksi, scope terdampak, pemilik, keputusan prioritas, dan hasil retest. Praktik [NIST SP 800-218 SSDF 1.1](https://csrc.nist.gov/pubs/sp/800/218/final) juga menekankan pengelolaan risiko dan bukti sepanjang siklus pengembangan, sehingga pemeriksaan aksesibilitas tidak dibiarkan sebagai aktivitas menjelang rilis. Jangan menetapkan ambang cakupan universal; sepakati aturan penerimaan proyek dan dokumentasikan alasannya.

**7. Tulis pernyataan dengan batas yang jujur.** Bila hasil evaluasi mendukungnya, pernyataan harus menyebut produk, scope, versi atau tanggal, metode, dan pengecualian yang masih terbuka. Jangan mengubah “tidak ditemukan pada sampel yang diperiksa” menjadi “bebas masalah”. Rencana konformitas adalah alat pengendali keputusan, bukan sertifikat.

## Faktor yang mengubah hasil

Pertama, **kelengkapan scope**. Evaluasi sepuluh halaman publik tidak mewakili proses pendaftaran yang memiliki beberapa state dan notifikasi. Proses yang paling penting bagi pengguna perlu masuk sampel meskipun jarang dibuka oleh tim internal.

Kedua, **perubahan state dan data**. State kosong, error, hasil pencarian, izin ditolak, dan sesi berakhir sering memakai komponen berbeda dari state normal. Uji dengan data yang memicu setiap cabang; jangan hanya menguji contoh bahagia.

Ketiga, **lingkungan penggunaan**. Browser, ukuran layar, pembesaran, metode input, dan teknologi bantu dapat memperlihatkan masalah yang tidak muncul pada laptop pengembang. Tuliskan kombinasi lingkungan yang disetujui serta alasan jika ada batasan.

Keempat, **ketergantungan dan antarmuka**. Spesifikasi API membantu menyamakan nama field, status, dan error antara layanan dan klien; [OpenAPI Specification 3.1.1](https://spec.openapis.org/oas/v3.1.1.html) dapat menjadi sumber kontrak tersebut. Namun kontrak API tidak membuktikan bahwa label, fokus, atau pengumuman perubahan di antarmuka pengguna dapat dipakai secara aksesibel.

Kelima, **mutu bukti dan waktu perubahan**. Screenshot lama tidak membuktikan build baru. Simpan identitas build, data uji, tanggal, alat, batas sampel, dan keputusan reviewer. Ketika komponen atau konten berubah, tentukan bagian mana yang harus diuji ulang.

## Contoh keputusan praktis

Berikut skenario hipotetis: sebuah portal layanan memiliki halaman publik dan area setelah login. Tim memilih target level tertentu untuk rilis pertama, tetapi belum memutuskan apakah alur pemulihan akun termasuk scope. Jangan menutup celah itu dengan asumsi; catat keputusan dan konsekuensinya.

| Objek dan state | Bukti minimum yang diminta | Keputusan kerja |
|---|---|---|
| Navigasi halaman publik dan menu terbuka | Rancangan urutan, implementasi semantik, uji keyboard dan fokus | Masukkan menu terbuka ke sampel, bukan hanya URL awal. |
| Form login: kosong, salah, terkunci, dan timeout | Contoh teks instruksi/error, hubungan label-field, uji manual dengan keyboard dan teknologi bantu | Tunda klaim untuk alur login bila state error belum dapat direproduksi dan diuji. |
| Dashboard setelah login pada layar sempit atau diperbesar | Bukti perilaku layout pada environment yang disepakati dan hasil uji interaksi | Tandai batas environment secara eksplisit; jangan menyebut “semua perangkat” tanpa data. |
| Data pilihan dari API dengan error atau hasil kosong | Kontrak field/error, perilaku komponen, serta uji pengumuman perubahan | Gunakan OpenAPI untuk menyamakan kontrak, lalu uji antarmuka secara terpisah. |

Kawan Codev.id, matriks seperti ini membantu rapat rilis berfokus pada bukti. Jika satu baris belum memiliki pemilik, metode, dan hasil retest, status yang paling jujur adalah belum siap dinilai, bukan “diasumsikan lulus”.

## Kesalahan umum dan cara memeriksanya

**“Scanner hijau berarti AA.”** Tanyakan halaman dan state apa yang dipindai, assertion apa yang dijalankan, dan temuan manual apa yang belum ditutup. Hasil otomatis hanya berlaku untuk sampel, build, environment, dan data tersebut.

**“Kami menguji satu halaman contoh.”** Cocokkan sampel dengan alur dan komponen berisiko tinggi. Periksa variasi autentikasi, error, dialog, dan konten panjang. Metode evaluasi harus menjelaskan mengapa sampel mewakili scope.

**“Komponen sudah disetujui, jadi semua penggunaan aman.”** Periksa implementasi nyata, konfigurasi, teks, dan interaksi pada setiap produk. Komponen yang sama dapat gagal ketika dipakai dengan label atau state berbeda.

**“Pengecualian disimpan di chat.”** Pindahkan ke daftar resmi dengan alasan, scope, pemilik, tanggal tinjau, mitigasi, dan rencana retest. Pengecualian tanpa tanggal cenderung menjadi permanen.

**“Pernyataan ditulis sebelum evaluasi.”** Ganti dengan draf internal yang diberi label belum final. Pernyataan publik hanya boleh menyatakan apa yang benar-benar dicakup dan ditemukan.

Gunakan pemeriksaan berikut sebelum meminta keputusan rilis:

- Apakah setiap alur penting memiliki daftar state dan data pemicu?
- Apakah target level dan scope tertulis dalam satu tempat yang disetujui?
- Apakah bukti desain, kode, konten, dan uji memiliki pemilik berbeda bila perlu?
- Apakah temuan dapat direproduksi pada build yang disebutkan?
- Apakah pengecualian memiliki alasan, mitigasi, pemilik, dan tanggal tinjau?
- Apakah evaluator yang kompeten sudah menilai batas dan metode, bukan hanya hasil alat?

## Jalan pintas yang tampak cepat tetapi rapuh

Shortcut yang sering dipilih adalah membeli scanner, menjalankannya pada halaman depan, lalu menambahkan lencana konformitas. Cara itu gagal karena scanner tidak melihat semua state, urutan tugas, makna konten, atau pengalaman teknologi bantu. Ia juga tidak menentukan siapa yang menerima risiko ketika temuan dibiarkan.

Alternatif yang lebih dapat dipertanggungjawabkan tetap ringkas: inventaris scope, pilih sampel yang mewakili proses, jalankan pemeriksaan otomatis sebagai sinyal awal, lakukan uji manual dan evaluasi spesialis, lalu hubungkan setiap hasil ke tiket dan keputusan rilis. Sobat Codev.id, jika waktu terbatas, kurangi scope secara tertulis dan jadwalkan perluasan; jangan memperluas klaim melebihi pekerjaan yang benar-benar dilakukan.

## Langkah berikutnya dan batas keputusan

Buat dokumen rencana konformitas pertama dengan kolom: produk dan build, halaman serta state, target level, kriteria relevan, pemilik bukti, environment uji, hasil, pengecualian, tiket perbaikan, retest, dan persetujuan. Minta pemilik produk menyetujui scope, lalu minta evaluator yang kompeten menilai apakah metode dan sampelnya memadai. Anda dapat memakai [beranda Codev.id](/) bila perlu menyamakan konteks layanan sebelum menetapkan peran.

Setelah evaluasi selesai, terbitkan pernyataan hanya untuk scope dan waktu yang benar-benar diperiksa, sambil menyebut pengecualian yang masih terbuka. [NEEDS PROFESSIONAL REVIEW: evaluasi WCAG-EM lengkap, kecukupan sampel, dan keputusan pernyataan konformitas proyek]

Aturan operasionalnya sederhana: tidak ada klaim konformitas tanpa scope yang disepakati, bukti yang dapat ditelusuri, hasil uji yang relevan, dan keputusan profesional atas sisa risiko. Itu menjaga rencana WCAG 2.2 tetap berguna—dan tetap jujur tentang apa yang belum diketahui.
