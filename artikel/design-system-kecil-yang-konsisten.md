---
article_id: CDV-02-A05
title: "Design System Kecil yang Tetap Konsisten"
slug: "design-system-kecil-yang-konsisten"
description: "Define tokens, components, states, content rules, accessibility notes, versioning, and ownership without overbuilding"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2025-05-01"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-02
primary_intent: "Decide what a maintainable design system needs"
reader_community: "Codev.id"
reader_address: "Kawan Codev.id"
final_route: "/artikel/design-system-kecil-yang-konsisten.html"
technical_review: required
sources:
  - "https://www.gov.uk/service-manual/agile-delivery"
  - "https://www.w3.org/TR/WCAG22/"
  - "https://www.w3.org/TR/WCAG-EM/"
  - "https://www.w3.org/WAI/test-evaluate/preliminary/"
---

# Design System Kecil yang Tetap Konsisten
Halo, Kawan Codev.id! Ketika layar tim mulai tidak seragam, Anda tidak perlu langsung membangun perpustakaan komponen raksasa. Design system kecil yang konsisten cukup berisi keputusan berulang yang terdokumentasi, mudah dipakai ulang, dan bisa diuji. Mulailah dari token, komponen inti, status, aturan konten, catatan aksesibilitas, versi, dan pemilik keputusan.

Jawaban itu berubah bila riset pengguna menunjukkan alur berbeda, produk memiliki risiko kepatuhan tinggi, atau beberapa tim merilis perubahan bersamaan. [NEEDS PROJECT RESEARCH AND DECISION OWNER: validasi kebutuhan dan prioritas belum tersedia dalam paket ini.]

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
![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)
Gambar ini merupakan aset lokal untuk ilustrasi dan bukan dokumentasi proyek tertentu.

## Definisi dan batas objek

Design system kecil adalah seperangkat keputusan bersama, bukan sekadar file desain atau koleksi tombol. Token menyimpan nilai berulang seperti warna, jarak, tipografi, radius, dan elevasi. Komponen menerapkan token pada pola antarmuka. Dokumentasi menjelaskan kapan pola dipakai, kapan tidak, serta siapa yang menyetujui perubahan.

Batasnya penting: artikel ini tidak memilih framework front-end dan tidak menggantikan kriteria komponen aksesibel untuk proyek tertentu. Tim tetap perlu menerjemahkan keputusan ke teknologi yang mereka gunakan. Template juga tidak membuktikan bahwa kebutuhan pengguna sudah benar; riset, pemilik keputusan, dan kriteria penerimaan harus datang dari proyek. Pendekatan iteratif UK Government Service Manual menempatkan asumsi, kebutuhan, dan pembelajaran sebagai hal yang diuji bertahap, bukan diasumsikan selesai di awal (https://www.gov.uk/service-manual/agile-delivery).

## Cara kerjanya

Bangun sistem dalam urutan yang bisa ditelusuri.

1. Catat pola yang benar-benar berulang. Ambil dua atau tiga alur utama, lalu tandai nilai yang berbeda tanpa alasan jelas.
2. Tetapkan token semantik, misalnya `color.text.muted`, bukan hanya `gray-500`. Nama menjelaskan peran sehingga nilainya dapat berubah tanpa mengganti makna.
3. Pilih komponen minimum: tombol, input, pesan, navigasi, dan kartu hanya jika muncul di beberapa alur. Setiap komponen memiliki anatomi, properti, status, dan contoh penggunaan.
4. Tulis aturan konten. Jelaskan kapitalisasi, panjang label, format tanggal, nada pesan galat, serta apa yang terjadi ketika teks memanjang.
5. Definisikan status lengkap: default, hover, focus, disabled, loading, error, success, dan empty bila relevan. Status bukan dekorasi; ia memberi umpan balik dan jalur pemulihan.
6. Tambahkan catatan aksesibilitas dan cara memeriksa. WCAG 2.2 membahas perilaku yang dapat diakses, tetapi satu pemindai tidak dapat membuktikan fokus keyboard, semantik, formulir, pembesaran, dan perilaku teknologi bantu secara menyeluruh (https://www.w3.org/TR/WCAG22/).
7. Rilis perubahan kecil dengan pemilik. Nomori versi dokumentasi atau paket, tulis perubahan yang memengaruhi konsumen, dan sediakan masa transisi bila nama token berubah.

## Faktor yang mengubah hasil

Ukuran sistem mengikuti keragaman keputusan, bukan jumlah halaman. Jika hanya satu alur memakai pola, dokumentasikan sebagai pola lokal dulu. Jika banyak alur memakai pola dengan variasi, komponen bersama layak dibuat.

Risiko juga mengubah kedalaman pemeriksaan. Alur masuk, pembayaran, atau formulir kesalahan memerlukan uji keyboard, fokus yang terlihat, label, pesan galat, dan reflow. Evaluasi WCAG-EM menyarankan penentuan cakupan halaman dan proses sebelum menilai hasil (https://www.w3.org/TR/WCAG-EM/). Pemeriksaan awal WAI dapat membantu menemukan masalah dasar, tetapi bukan sertifikat kesesuaian (https://www.w3.org/WAI/test-evaluate/preliminary/).

Kapasitas tim menentukan tata kelola. Satu pemilik dapat menerima perubahan mingguan; tim lebih besar memerlukan daftar keputusan, peninjau kedua, dan catatan kompatibilitas. Apa pun ukurannya, simpan bukti: contoh sebelum-sesudah, hasil uji tugas, dan alasan menerima atau menolak variasi. [NEEDS PROJECT EVIDENCE: belum ada data alur, hasil uji, atau matriks risiko untuk menentukan cakupan aktual.]

## Contoh keputusan praktis

Bayangkan tiga layar memiliki tombol utama dengan tiga warna dan empat jarak berbeda. Jangan langsung menyatukan semuanya. Tanyakan:

| Temuan | Keputusan awal | Bukti yang masih diperlukan |
|---|---|---|
| Perbedaan hanya nilai warna | Satu token peran, dua tema bila perlu | Kontras dan konteks penggunaan |
| Perbedaan urutan atau tindakan | Komponen berbeda atau varian bernama jelas | Tugas pengguna dan tujuan bisnis |
| Status error tidak konsisten | Standarkan struktur pesan dan pemulihan | Uji dengan keyboard dan pembaca layar |
| Pola baru muncul sekali | Simpan sebagai pola lokal | Bukti pengulangan di alur lain |

Kawan Codev.id, jadikan tabel ini hipotesis kerja, bukan keputusan final. Untuk setiap komponen, tulis pemilik, konsumen, status yang didukung, dan cara pengujian. Jika perubahan mematahkan implementasi, pilih migrasi bertahap daripada mengganti semua layar sekaligus.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah membuat token untuk setiap nilai piksel. Periksa apakah nama token menjelaskan peran dan apakah dua nilai benar-benar memiliki alasan berbeda. Kesalahan kedua adalah hanya mendokumentasikan tampilan normal. Jalankan daftar status pada perangkat keyboard dan lebar layar berbeda.

Kesalahan ketiga adalah menyebut komponen “aksesibel” setelah lolos satu alat otomatis. Lakukan evaluasi berbasis cakupan halaman dan proses; gabungkan pemeriksaan otomatis, inspeksi manual, dan uji dengan pengguna bila risikonya menuntut. WCAG bukan otomatis hukum Indonesia, sehingga kewajiban hukum dan persetujuan proyek harus diperiksa terpisah.

Kesalahan keempat adalah tidak menunjuk pemilik. Setiap usulan perubahan harus memiliki pengusul, peninjau, tanggal berlaku, dan keputusan kompatibilitas. Tanpa itu, duplikasi akan kembali meski dokumentasinya rapi.

## Jalan pintas yang perlu dihindari

Shortcut yang sering dipilih adalah menyalin komponen dari pustaka populer lalu menganggap konsistensi selesai. Itu dapat gagal karena aturan konten, status, alur pemulihan, dan kebutuhan pengguna Anda berbeda. Alternatif yang lebih aman: ambil pola pustaka sebagai bahan, lalu verifikasi token, semantik, fokus, reflow, dan pesan pada alur nyata. Catat bagian yang diadopsi, diubah, atau ditolak agar keputusan dapat ditinjau ulang.

Teman Codev.id, simpan keputusan dan contoh terbaru di [ruang kerja Codev.id](/) agar anggota tim menemukan sumber yang sama. Tautkan pula aset visual yang dipakai hanya sebagai rujukan media, bukan bukti hasil.

## Kesimpulan

Design system kecil tetap konsisten bila ia memusatkan keputusan yang berulang: token semantik, komponen inti beserta semua status, aturan konten, catatan aksesibilitas, versi, dan pemilik. Mulailah dari alur yang paling sering berubah, buktikan melalui tugas dan pemeriksaan, lalu perluas hanya saat pola berulang.

Langkah berikutnya adalah membuat satu halaman keputusan untuk tiga komponen prioritas: daftar token, matriks status, aturan konten, pemeriksaan aksesibilitas, pemilik, dan rencana versi. Minta pemilik produk menyetujui cakupan setelah riset tersedia. Aturan operasinya sederhana: jangan menambah komponen bersama tanpa bukti pengulangan dan cara uji yang jelas.

Catat juga tanggal peninjauan berikutnya dan siapa yang boleh menyetujui pengecualian. Dengan begitu, sistem tetap kecil karena setiap tambahan harus membayar biayanya melalui bukti, dokumentasi, dan pemeliharaan yang jelas.

Jika bukti belum ada, tandai keputusan sebagai sementara dan jadwalkan pemeriksaan, bukan menyamarkannya sebagai standar final.
Dengan catatan itu, rapat berikutnya dapat berangkat dari data, bukan selera pribadi.
