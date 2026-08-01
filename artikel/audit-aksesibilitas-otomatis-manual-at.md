---
article_id: CDV-13-A05
title: "Audit Aksesibilitas: Otomatis, Manual, dan Assistive Technology"
slug: "audit-aksesibilitas-otomatis-manual-at"
description: "Panduan menggabungkan pemindaian otomatis, pemeriksaan kode, keyboard, zoom/reflow, AT, tugas pengguna, bukti, penilaian keparahan, dan uji ulang"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2026-01-31"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-13
primary_intent: "Plan an accessibility evaluation with defensible evidence"
reader_community: "Codev.id"
reader_address: "Kawan Codev.id"
final_route: "/artikel/audit-aksesibilitas-otomatis-manual-at.html"
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

# Audit Aksesibilitas: Otomatis, Manual, dan Assistive Technology

Halo, Kawan Codev.id! Audit aksesibilitas yang dapat dipertanggungjawabkan bukanlah angka dari satu pemindai. Gabungkan pemindaian otomatis untuk menemukan pola yang bisa diperiksa mesin, pemeriksaan kode dan antarmuka secara manual, pengujian keyboard serta zoom/reflow, lalu uji dengan *assistive technology* (AT)—teknologi bantu seperti pembaca layar. Tambahkan tugas pengguna, bukti yang dapat diulang, penilaian keparahan, dan uji ulang setelah perbaikan.

Pemindai yang bersih hanya berarti aturan yang ia cek tidak menemukan masalah pada sampel, lingkungan, build, dan data tersebut. Ia tidak membuktikan fokus keyboard, urutan pembacaan, pesan galat, alur autentikasi, atau apakah tugas nyata dapat diselesaikan. WCAG 2.2 dan metode WCAG-EM menempatkan cakupan halaman serta proses sebagai bagian dari evaluasi, bukan sekadar skor alat ([WCAG 2.2](https://www.w3.org/TR/WCAG22/), [WCAG-EM 1.0](https://www.w3.org/TR/WCAG-EM/)).

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

*Gambar ini merupakan aset lokal untuk ilustrasi dan bukan dokumentasi proyek tertentu.*

## Jawaban singkat dan salah paham utama

Mulailah dari perjalanan pengguna yang hendak dipastikan dapat dilakukan: masuk, mencari, mengisi, mengirim, atau menerima hasil. Petakan risiko dan kriteria penerimaan, jalankan beberapa lapis pemeriksaan, simpan bukti, lalu putuskan rilis berdasarkan masalah yang masih terbuka. Panduan pengiriman layanan pemerintah Inggris juga menekankan asumsi, kebutuhan pengguna, eksperimen, dan penerimaan yang dapat diuji secara bertahap ([UK Government Service Manual—agile delivery](https://www.gov.uk/service-manual/agile-delivery)).

Salah kaprah yang mahal adalah menganggap “lulus otomatis” sama dengan “aksesibel” atau “bersertifikat”. Otomatisasi membantu menemukan atribut yang dapat diamati—misalnya relasi label dan kontrol—tetapi hasilnya tetap perlu ditafsirkan. Sebaliknya, pengujian manual tanpa catatan lingkungan dan langkah reproduksi menghasilkan opini yang sulit ditindaklanjuti.

Tanyakan tiga hal sebelum menyebut audit selesai: perjalanan mana yang dicakup, bukti apa yang menunjukkan tugas berhasil atau gagal, dan siapa yang berwenang menerima risiko tersisa? Jika jawaban kedua dan ketiga belum ada, statusnya adalah temuan terbuka, bukan kelulusan. [NEEDS PROJECT REVIEW: cakupan perjalanan, lingkungan dukungan, dan kriteria keputusan rilis belum ditetapkan dalam paket ini.]

## Definisi dan batas objek

Audit di sini adalah evaluasi terencana atas halaman, komponen, alur, dan perilaku ketika dipakai dengan input serta teknologi bantu tertentu. “Otomatis” berarti aturan dijalankan alat pada build atau URL yang ditentukan. “Manual” mencakup inspeksi DOM dan kode, keyboard, fokus, zoom/reflow, kontras, formulir, media, serta skenario yang tidak dapat disimpulkan mesin. “AT” berarti perangkat lunak atau perangkat yang membantu pengguna berinteraksi, dan harus diuji pada alur yang relevan.

Objeknya bukan hanya halaman beranda. Cakupan dapat mencakup rute yang memerlukan autentikasi, dialog, pesan galat, konten dinamis, dan proses lintas langkah. WCAG-EM menyediakan kerangka pemilihan sampel dan proses evaluasi; itu bukan bukti bahwa aplikasi tertentu telah memenuhi semua kriteria. Audit juga bukan penetapan kepatuhan hukum Indonesia: status tersebut memerlukan penilaian hukum dan konteks proyek tersendiri.

## Cara kerjanya

Urutan praktisnya seperti berikut.

1. **Tentukan tujuan dan inventaris.** Tulis tugas pengguna, peran, titik mulai dan selesai, variasi data, serta rute yang masuk cakupan. Tandai asumsi dan pemilik keputusan.
2. **Bangun jejak kebutuhan.** Hubungkan risiko atau kebutuhan ke komponen, test case, hasil, dan tiket. Pada API, spesifikasi OpenAPI membantu menjelaskan kontrak operasi dan respons yang diuji, tetapi tidak membuktikan antarmuka pengguna dapat diakses ([OpenAPI Specification 3.1.1](https://spec.openapis.org/oas/v3.1.1.html)).
3. **Jalankan scan otomatis.** Simpan versi alat, konfigurasi, URL atau commit, fixture data, waktu, dan daftar temuan. Kelompokkan duplikasi; jangan mengubah peringatan menjadi klaim kepatuhan.
4. **Inspeksi manual terarah.** Periksa struktur semantik, nama dan peran kontrol, urutan fokus, indikator fokus, operasi tanpa mouse, zoom, reflow, kontras, status dinamis, validasi, dan pemulihan dari galat. WAI Easy Checks berguna sebagai titik awal, bukan pengganti evaluasi penuh ([WAI Easy Checks](https://www.w3.org/WAI/test-evaluate/preliminary/)).
5. **Uji dengan AT dan tugas.** Pilih kombinasi browser, sistem operasi, pembaca layar, pembesaran, atau input alternatif yang memang didukung. Jalankan tugas yang sama, catat langkah, hambatan, dan keluaran yang didengar atau dilihat.
6. **Nilai dan putuskan.** Beri tingkat keparahan berdasarkan dampak tugas dan jangkauan, tetapkan pemilik serta tenggat, lalu retest pada build baru. Praktik SSDF NIST menempatkan hasil pengujian dan penanganan cacat sebagai bagian dari proses pengembangan aman yang dapat ditelusuri ([NIST SP 800-218 SSDF 1.1](https://csrc.nist.gov/pubs/sp/800/218/final)).

## Faktor yang mengubah hasil

Hasil bisa berbeda ketika cakupan rute berubah, data uji tidak memicu status galat, atau fitur hanya muncul setelah login. Versi browser, sistem operasi, pembaca layar, bahasa, ukuran viewport, dan pengaturan zoom juga memengaruhi perilaku. Catat kombinasi tersebut; “berfungsi di laptop saya” bukan lingkungan yang dapat diulang.

Kualitas implementasi ikut menentukan: komponen bersama dapat menyebarkan satu cacat ke banyak halaman, sedangkan konten editorial dapat mengubah heading, tautan, atau teks alternatif tanpa perubahan kode. Proses bisnis penting pula—misalnya batas waktu sesi, CAPTCHA, unggah berkas, atau notifikasi—karena tugas yang terputus mengubah dampak temuan.

Sobat Codev.id, bedakan **temuan**, **bukti**, dan **keputusan**. Temuan menjelaskan kondisi; bukti menjelaskan cara mengulanginya; keputusan menyatakan apakah risiko diterima, diperbaiki, atau memblokir rilis. Ketiganya tidak boleh dilebur menjadi satu label “pass”.

## Contoh keputusan praktis

Bayangkan alur “masuk lalu mengirim formulir”. Scan menemukan kontrol tanpa nama yang terprogram. Inspeksi kode mengonfirmasi hubungan label memang hilang, sedangkan uji pembaca layar menunjukkan pengguna tidak dapat mengetahui tujuan kontrol. Catat selector atau lokasi komponen, langkah reproduksi, keluaran aktual, keluaran yang diharapkan, lingkungan, dan tautan commit perbaikan. Setelah perbaikan, jalankan kembali scan **dan** tugas AT; hasil scan saja tidak menutup temuan perilaku.

Gunakan matriks sederhana untuk menahan keputusan prematur:

| Bukti | Menunjukkan | Tidak menunjukkan | Keputusan sementara |
| --- | --- | --- | --- |
| Scan otomatis pada build tertentu | Aturan yang terdeteksi alat pada sampel | Cakupan penuh dan pengalaman pengguna | Lanjutkan pemeriksaan manual |
| Keyboard, zoom, dan inspeksi kode | Perilaku interaksi pada skenario yang dicoba | Semua browser/AT yang mungkin | Perluas matriks bila dukungan proyek lebih luas |
| Tugas dengan AT dan pengguna yang relevan | Hambatan nyata pada alur tertentu | Kepatuhan hukum atau semua perjalanan | Perbaiki, retest, dan minta keputusan pemilik risiko |

Jika satu perjalanan penting gagal total pada kombinasi yang dinyatakan didukung, perlakukan sebagai kandidat pemblokir rilis sampai pemilik produk mendokumentasikan keputusan. Jangan menetapkan ambang angka universal tanpa dasar proyek.

## Kesalahan umum dan cara memeriksanya

**Hanya mengirim laporan scanner.** Periksa apakah setiap temuan memiliki langkah reproduksi dan apakah alur keyboard, zoom, formulir, serta AT tercatat.

**Menguji satu halaman statis.** Cocokkan inventaris dengan rute autentikasi, dialog, status galat, dan proses multi-langkah.

**Menyamakan standar dengan pengalaman.** Gunakan kriteria WCAG sebagai referensi evaluasi, lalu validasi tugas dan konteks dukungan. Jangan menyebut hasil sebagai sertifikasi.

**Menghapus semua peringatan tanpa triase.** Simpan alasan false positive, keputusan risiko, dan tiket yang masih terbuka; jejak ini diperlukan saat retest.

**Mengklaim pengguna disabilitas telah terwakili tanpa bukti.** Dokumentasikan siapa yang dilibatkan, tugasnya, dan batas generalisasi. Bila belum ada, tandai kebutuhan riset—bukan mengisi kekosongan dengan asumsi.

## Jalan pintas yang perlu diuji

“Waktu mepet; satu scan sudah cukup untuk rilis.” Shortcut itu mungkin menemukan cacat murah lebih awal, tetapi tidak menguji mekanisme yang bergantung pada urutan fokus, status dinamis, pembesaran, atau keluaran pembaca layar. Alternatif yang lebih aman adalah paket minimum berurutan: scan pada build kandidat, keyboard dan zoom pada tugas prioritas, satu kombinasi AT yang memang didukung, pencatatan bukti, lalu retest temuan berisiko tinggi. Jika langkah minimum belum selesai, nyatakan risiko dan minta keputusan eksplisit—jangan menyebutnya audit tuntas. Untuk menyelaraskan pekerjaan dengan konteks tim, Anda dapat mulai dari [beranda Codev.id](/) dan menetapkan pemilik keputusan sebelum pengujian.

## Kesimpulan

Audit aksesibilitas yang defensible menggabungkan otomatisasi, inspeksi manual, keyboard/zoom, pengujian AT, dan tugas pengguna dalam cakupan yang jelas. Setiap hasil harus dapat diulang dan ditautkan ke kebutuhan, cacat, pemilik, serta retest; tidak ada skor pemindai yang sendirian menjadi sertifikasi.

Langkah berikutnya: buat inventaris perjalanan dan matriks lingkungan, pilih tugas prioritas, simpan bukti scan serta langkah manual, lalu minta pemilik risiko meninjau temuan yang tersisa. Kawan Codev.id, jadikan aturan operasi ini sebagai penutup setiap siklus: **tidak ada keputusan “aksesibel” tanpa cakupan, bukti, dan keputusan rilis yang terdokumentasi; penilaian teknis akhir tetap memerlukan review profesional sesuai konteks proyek.**
