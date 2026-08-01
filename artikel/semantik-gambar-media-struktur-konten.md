---
article_id: CDV-13-A04
title: "Semantik, Gambar, Media, dan Struktur Konten"
slug: "semantik-gambar-media-struktur-konten"
description: "Define headings/landmarks, link purpose, alternative text decision, captions/transcripts, tables, language, reading order, and responsive presentation"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2026-01-27"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-13
primary_intent: "Make content understandable across modalities"
reader_community: "Codev.id"
reader_address: "Teman Codev.id"
final_route: "/artikel/semantik-gambar-media-struktur-konten.html"
technical_review: required
sources:
  - "https://www.w3.org/TR/WCAG22/"
  - "https://www.w3.org/TR/WCAG-EM/"
  - "https://www.w3.org/WAI/test-evaluate/preliminary/"
---

# Semantik, Gambar, Media, dan Struktur Konten

Halo, Teman Codev.id!

Halaman yang tampak rapi belum tentu dapat dipahami ketika judul dibaca melalui pembaca layar, gambar tidak tampil, video diputar tanpa suara, atau layar dipersempit. Keputusan utamanya adalah memperlakukan semantik, media, dan urutan konten sebagai satu sistem: struktur HTML harus menjelaskan hubungan, setiap media harus punya alternatif yang setara bila dibutuhkan, dan tampilan responsif tidak boleh mengubah urutan makna.

Caranya bukan menambahkan `alt` ke semua gambar atau menyebarkan heading sebanyak mungkin. Tentukan dulu informasi yang ingin disampaikan, siapa yang memakainya, lalu pilih elemen, teks alternatif, keterangan, transkrip, tabel, bahasa, dan urutan baca yang paling tepat. [W3C Web Content Accessibility Guidelines (WCAG) 2.2](https://www.w3.org/TR/WCAG22/) memberi kriteria untuk diuji, tetapi halaman ini tidak menyatakan bahwa satu pola otomatis membuat proyek konforman. Hasil akhirnya berubah sesuai konten nyata, komponen yang dipakai, perangkat, dan pengujian manual.

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu. Gambar ini merupakan aset lokal untuk ilustrasi dan bukan dokumentasi proyek tertentu.

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

Semantik menjawab “apa peran bagian ini”, bukan “bagaimana bagian ini terlihat”. Heading membentuk hierarki topik, landmark membantu berpindah antarwilayah, dan elemen seperti `button`, `nav`, `main`, `table`, atau `figure` memberi sinyal yang dapat dipakai teknologi bantu. Gambar dan media melengkapi pesan itu; teks alternatif, caption, atau transkrip dipilih berdasarkan fungsi, bukan berdasarkan jenis berkasnya.

Salah paham yang sering mahal adalah menganggap pemeriksaan otomatis sebagai sertifikat. Scanner dapat menemukan sebagian pola yang bisa dipastikan dari kode, tetapi tidak dapat memutuskan apakah `alt` benar-benar menyampaikan tujuan gambar, apakah urutan baca masuk akal, atau apakah transkrip cukup untuk memahami demonstrasi. [WAI Easy Checks](https://www.w3.org/WAI/test-evaluate/preliminary/) menempatkan pemeriksaan cepat sebagai langkah awal, sedangkan evaluasi yang lebih lengkap perlu mencakup halaman dan proses yang relevan. Untuk proyek tertentu, keputusan konformitas, legalitas, dan penerimaan harus ditinjau oleh pemilik keputusan: **[NEEDS GATE-05/GATE-06 REVIEW: konfirmasi ruang lingkup evaluasi, perangkat bantu, dan kriteria penerimaan proyek sebelum menyatakan halaman aksesibel atau konforman.]**

## Definisi dan batas objek

Dalam artikel ini, “struktur konten” berarti hubungan yang dapat dipahami tanpa bergantung pada posisi visual: satu judul memiliki bagian, paragraf menjelaskan konteks, daftar mengelompokkan item, dan tabel menghubungkan header dengan sel. “Media” mencakup gambar, audio, video, dan ilustrasi yang membawa atau menghias pesan. “Presentasi responsif” mencakup reflow, pembesaran, dan perubahan tata letak selama hubungan informasi tetap terjaga.

Yang tidak dibahas adalah penulisan materi promosi, strategi metadata, atau keputusan merek. Kita juga tidak menetapkan satu pustaka komponen, ambang ukuran layar, tingkat kontras tertentu, atau klaim kepatuhan hukum Indonesia. Batas ini penting: tim dapat membuat keputusan implementasi yang dapat diuji tanpa berpura-pura telah menguji semua pengguna, perangkat, atau konteks penggunaan.

Mulailah dari tugas pembaca. Apakah ia mencari definisi, membandingkan dua nilai, mengisi formulir, atau mengikuti langkah? Jawaban itu menentukan apakah konten cocok menjadi paragraf, daftar, tabel, atau rangkaian langkah. Jika kebutuhan dan pemilik keputusan belum jelas, dokumentasikan asumsi dan minta validasi sebelum mengunci struktur.

## Cara kerjanya

Gunakan alur berikut saat menyusun atau merombak halaman.

1. **Petakan tujuan dan urutan.** Tulis satu kalimat tujuan halaman, kemudian daftar tugas yang harus selesai. Susun heading mengikuti urutan tugas, bukan ukuran font. Gunakan satu `h1` yang menjelaskan halaman, lalu tingkat heading yang tidak melompat tanpa alasan. Landmark seperti `header`, `nav`, `main`, `aside`, dan `footer` membagi wilayah; gunakan label yang membedakan dua navigasi yang berbeda.
2. **Hubungkan tindakan dengan tujuan.** Teks tautan harus tetap bermakna ketika dibaca di luar paragraf, misalnya “lihat panduan struktur website” alih-alih banyak tautan “selengkapnya”. Tautan yang sama tidak perlu diulang jika pembaca sudah memiliki jalan yang jelas; sebaliknya, jangan menyamarkan tujuan dengan URL yang tidak deskriptif.
3. **Klasifikasikan setiap gambar.** Gambar informatif mendapat `alt` yang menyampaikan fakta atau fungsi yang dibutuhkan untuk tugas. Gambar dekoratif memakai `alt=""` agar tidak menambah gangguan. Gambar kompleks memerlukan penjelasan di teks sekitar atau halaman data; jangan memadatkan seluruh temuan penting ke satu kalimat `alt`.
4. **Lengkapi media bergerak dan suara.** Caption memberi konteks atau identitas yang terlihat. Subtitle membantu memahami dialog dan suara penting; transkrip menyajikan isi audio atau video sebagai teks yang dapat dicari dan dibaca ulang. Jika video hanya dekorasi, jangan menjadikannya satu-satunya pembawa instruksi.
5. **Tandai data.** Untuk perbandingan, berikan header kolom dan baris yang jelas, hubungan header-sel yang dapat diprogram, serta ringkasan singkat sebelum tabel. Jika pembaca lebih terbantu oleh urutan langkah atau daftar, jangan memaksa data kecil menjadi tabel.
6. **Tetapkan bahasa dan urutan baca.** Atribut bahasa pada dokumen dan penanda perubahan bahasa membantu pengucapan. Pastikan urutan DOM mengikuti urutan visual dan logika tugas; CSS tidak boleh memindahkan tombol penting jauh dari penjelasannya hanya demi komposisi.
7. **Uji dalam beberapa cara.** Periksa keyboard dan fokus, pembesaran serta reflow, pembaca layar, gambar yang gagal dimuat, media tanpa suara, dan viewport sempit. Catat halaman, kondisi, hasil, dan cacat yang belum selesai. [WCAG-EM 1.0](https://www.w3.org/TR/WCAG-EM/) menekankan bahwa pemilihan sampel dan cakupan evaluasi harus dinyatakan; satu halaman yang lolos tidak otomatis mewakili seluruh situs.

Teman Codev.id, pisahkan “terlihat benar” dari “terbaca dan dapat dioperasikan”. Hasil setiap pemeriksaan harus kembali ke tugas pembaca dan risiko yang ingin dikurangi.

## Faktor yang mengubah hasil

Beberapa kondisi membuat keputusan yang sama menghasilkan kebutuhan berbeda. Sobat Codev.id, jadikan kondisi ini pertanyaan saat review, bukan asumsi yang disembunyikan di balik tampilan.

| Kondisi | Keputusan yang perlu dibuat | Bukti yang disimpan |
| --- | --- | --- |
| Gambar menyampaikan angka, lokasi, atau status | Tulis nilai yang diperlukan; jangan hanya “gambar grafik” | Teks sumber atau data yang menjadi dasar `alt` |
| Gambar dekoratif di antara dua paragraf | Kosongkan `alt` dan pastikan tidak menjadi tautan tersembunyi | Pemeriksaan DOM dan pembaca layar |
| Diagram memiliki banyak relasi | Sediakan ringkasan relasi dan, bila perlu, tabel/data alternatif | Teks penjelasan yang dapat dibaca tanpa gambar |
| Video memuat instruksi visual | Subtitle untuk dialog, deskripsi audio atau narasi untuk informasi visual, dan transkrip bila tepat | Berkas media serta contoh pemutaran |
| Tabel melebar di layar sempit | Pertahankan header dan hubungan sel; gunakan reflow atau scroll yang tetap dapat dioperasikan | Uji zoom, keyboard, dan viewport sempit |
| Bahasa berganti di tengah paragraf | Tandai perubahan bahasa dan periksa pelafalan | Markup bahasa dan contoh pembacaan |
| Komponen dirakit ulang oleh CSS | Kembalikan urutan DOM ke urutan tugas | Snapshot struktur dan skenario keyboard |

Konteks juga menentukan kedalaman. Foto produk dengan satu ciri pembeda membutuhkan `alt` singkat; foto yang menjadi bukti perbandingan mungkin membutuhkan angka dan kondisi di teks pendamping. Caption tidak menggantikan `alt` secara otomatis: caption terlihat bagi semua pembaca, sedangkan `alt` memberi nama atau fungsi ketika gambar tidak tersedia. Pilih kombinasi setelah mengetahui siapa yang membutuhkan informasi tersebut.

## Contoh keputusan praktis

Bayangkan halaman berisi instruksi tiga langkah, diagram alur, dan tabel status. Keputusan yang dapat ditelusuri adalah sebagai berikut:

| Pertanyaan | Jika jawabannya “ya” | Jika jawabannya “tidak” |
| --- | --- | --- |
| Apakah diagram diperlukan untuk menyelesaikan tugas? | Beri ringkasan urutan dan hubungan dalam teks; sediakan detail yang setara | Perlakukan sebagai dekorasi atau pendukung, lalu gunakan `alt` kosong bila memang tidak informatif |
| Apakah tabel dipakai untuk membandingkan nilai? | Tetapkan header, satuan, dan keterangan sebelum tabel | Ubah menjadi daftar atau paragraf yang lebih mudah diikuti |
| Apakah video memiliki dialog atau suara yang mengubah makna? | Sediakan subtitle dan transkrip yang sesuai | Jangan menambahkan kontrol yang tidak memberi manfaat; pastikan instruksi tetap ada di teks |
| Apakah tampilan sempit memotong kolom atau tombol? | Uji reflow, zoom, dan navigasi keyboard sebelum rilis | Tetap uji pembaca layar dan gambar/media yang gagal dimuat |

Contoh ini adalah pola keputusan, bukan klaim bahwa proyek tertentu sudah mengujinya. Pada tahap penerimaan, hubungkan setiap keputusan ke halaman, komponen, kondisi uji, hasil, dan cacat terbuka. Tautkan pembaca yang sedang menyusun halaman ke [panduan konten website yang relevan](/konten/website), lalu gunakan rute [konten umum sebagai titik mulai perencanaan](/konten) bila kebutuhan belum terpetakan. Masing-masing tautan hanya dipakai sekali agar tujuan navigasinya jelas.

## Kesalahan umum dan cara memeriksanya

**“Semua gambar diberi alt yang sama.”** Tanyakan: bila gambar dihapus, fakta apa yang hilang? Jika tidak ada fakta, gunakan `alt=""`; jika ada, sebutkan fungsi atau hasil yang dibutuhkan, bukan detail visual yang tidak relevan.

**“Heading dipilih karena ukurannya.”** Matikan CSS atau lihat outline di DOM. Bila urutan topik masih logis, struktur tidak bergantung pada gaya. Bila tidak, perbaiki heading dan landmark, bukan sekadar ukuran huruf.

**“Caption sudah cukup untuk video.”** Putar tanpa suara dan minta orang mengikuti instruksi hanya dari teks. Jika dialog atau informasi visual hilang, tambahkan subtitle, transkrip, atau narasi yang sesuai.

**“Tabel responsif berarti selesai.”** Uji pembesaran, fokus keyboard, pembacaan header, dan hubungan baris-kolom. Scroll horizontal yang masih dapat dioperasikan berbeda dari kolom yang terpotong atau urutan data yang berubah.

**“Satu scanner hijau berarti aksesibel.”** Gunakan hasil otomatis untuk menemukan pola, kemudian lakukan pemeriksaan manual dan dengan teknologi bantu pada sampel yang mewakili alur penting. Laporkan lingkungan, data, dan keterbatasan; jangan mengubah hasil sampel menjadi klaim seluruh situs. Penilaian resmi harus mengikuti metode dan ruang lingkup yang disepakati, bukan ambang universal yang tidak ditetapkan proyek.

## Jangan menunda semantik sampai akhir

Shortcut yang masuk akal adalah menunda semantik dan alternatif media sampai akhir, setelah desain visual dianggap selesai. Itu sering gagal karena perubahan struktur DOM, naskah subtitle, atau tabel pada tahap akhir dapat mengubah komponen dan jadwal uji. Perbaikan yang lebih aman adalah membuat keputusan fungsi bersama konten, desain, dan pengembangan sejak contoh pertama: tetapkan heading dan urutan, tulis satu contoh `alt`, siapkan transkrip untuk satu cuplikan, lalu uji pada viewport sempit dan pembaca layar. Jika keputusan itu belum dapat dibuktikan pada konten nyata, tandai sebagai pekerjaan terbuka dan jangan menyebutnya selesai.

## Kesimpulan

Semantik, gambar, media, dan struktur konten bekerja sebagai satu kontrak: hubungan informasi harus tetap terbaca, media harus memiliki alternatif yang sesuai fungsi, dan tata letak responsif tidak boleh merusak urutan tugas. Mulailah dari tujuan pembaca, pilih elemen yang menyatakan hubungan, dokumentasikan keputusan alternatif, lalu uji otomatis, manual, dan dengan teknologi bantu pada cakupan yang disepakati.

Langkah berikutnya adalah membuat lembar pemeriksaan untuk satu alur penting: daftar heading dan landmark, tujuan setiap tautan, keputusan `alt`, caption/subtitle/transkrip, header tabel, bahasa, urutan DOM, kondisi zoom, serta hasil uji. Minta pemilik produk menyetujui kriteria penerimaan dan minta peninjau teknis mengonfirmasi batas evaluasi. Kawan Codev.id, aturan operasionalnya sederhana: jangan menyatakan konten aksesibel atau konforman hanya karena tampilannya rapi atau satu alat pemeriksa tidak menemukan masalah.
