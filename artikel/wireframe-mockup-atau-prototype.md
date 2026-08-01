---
article_id: CDV-02-A03
title: "Wireframe, Mockup, atau Prototype: Pilih Sesuai Risiko"
slug: "wireframe-mockup-atau-prototype"
description: "Memilih wireframe, mockup visual, prototype klik, atau prototype berkode berdasarkan ketidakpastian, biaya, dan bukti keputusan"
writing_contract_version: "native-id-v2"
status: draft
publication_date: "2025-04-22"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-02
primary_intent: "Select the right design fidelity for a decision"
reader_community: "Codev.id"
reader_address: "Sobat Codev.id"
final_route: "/artikel/wireframe-mockup-atau-prototype.html"
technical_review: required
sources:
  - "https://www.gov.uk/service-manual/agile-delivery"
  - "https://www.w3.org/TR/WCAG22/"
  - "https://www.w3.org/TR/WCAG-EM/"
  - "https://www.w3.org/WAI/test-evaluate/preliminary/"
---

# Wireframe, Mockup, atau Prototype: Pilih Sesuai Risiko

Halo, Sobat Codev.id! Meminta “desain dulu” tanpa menjelaskan keputusan yang ingin diuji sering membuat biaya berpindah ke artefak yang salah. Wireframe yang terlalu rinci dapat menghabiskan waktu untuk warna sebelum alur dipahami; mockup yang cantik dapat menyamarkan tugas yang tidak bisa diselesaikan; prototype interaktif dapat terlihat seperti produk jadi padahal belum membuktikan kebutuhan.

Jawaban singkatnya: pilih tingkat fidelity (kemiripan artefak dengan produk akhir) berdasarkan ketidakpastian terbesar. Gunakan wireframe untuk menyepakati struktur dan prioritas, mockup untuk menilai bahasa visual, prototype klik untuk menguji alur dan tugas, dan prototype berkode untuk mengurangi risiko teknis atau perilaku yang tidak dapat disimulasikan dengan aman. Bila beberapa risiko sama-sama tinggi, naikkan fidelity bertahap dan tetapkan bukti yang harus terkumpul pada setiap tahap. Riset pengguna, kepemilikan keputusan, dan pemeriksaan aksesibilitas tetap menentukan; artefak tidak menggantikan validasi proyek.

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

## Jawaban singkat dan salah paham utama

Empat istilah ini bukan urutan wajib dan bukan ukuran kualitas desainer. Wireframe biasanya cukup ketika pertanyaan Anda “informasi apa muncul, dalam urutan apa, dan bagaimana pengguna berpindah?”. Mockup menjawab “seperti apa tampilan, hierarki visual, dan nada mereknya?”. Prototype klik menjawab “apakah orang dapat mengikuti alur yang direncanakan?”. Prototype berkode dipilih ketika interaksi, data, respons perangkat, atau integrasi teknis menjadi sumber risiko utama.

Kawan Codev.id, minta setiap deliverable disertai keputusan yang hendak dibuat. Jika keputusan belum jelas, artefak paling mahal pun hanya menjadi bahan presentasi. Dalam pendekatan agile, asumsi, perjalanan pengguna, perilaku fungsional, kualitas, dan kriteria penerimaan merupakan pertanyaan berbeda yang perlu dipelajari dan diuji secara bertahap ([UK Government Service Manual](https://www.gov.uk/service-manual/agile-delivery)).

Jangan menyamakan “bisa diklik” dengan “sudah berfungsi”. Prototype klik hanya mensimulasikan jalur yang dibuat; ia belum membuktikan performa, keamanan, integrasi, atau kesiapan produksi. Batas artikel ini berhenti pada pemilihan dan pemeriksaan artefak desain. Implementasi web atau aplikasi produksi memerlukan pekerjaan dan persetujuan teknis tersendiri.

## Definisi dan batas objek

**Wireframe** adalah sketsa struktur: blok konten, navigasi, kontrol utama, dan urutan tugas. Detail visual sengaja minimum agar perubahan arsitektur murah. **Mockup** menambahkan komposisi visual seperti tipografi, warna, gambar pengganti, dan states yang ingin disepakati; tetap saja ia bukan bukti bahwa kode, data, atau responsivitas sudah berjalan.

**Prototype klik** menghubungkan layar atau state sehingga peserta dapat menjalankan skenario tertentu. Nilainya terletak pada percakapan dan observasi terhadap tugas, bukan pada jumlah layar. **Prototype berkode** menjalankan sebagian perilaku dengan teknologi nyata atau tiruan yang lebih dekat ke sistem. Gunakan bila simulasi statis tidak cukup untuk menjawab risiko, lalu beri label jelas bagian mana yang masih palsu, manual, atau belum terhubung.

Keempat objek memiliki batas. Tidak satu pun otomatis mewakili semua pengguna, mengesahkan permintaan pasar, atau menyatakan kepatuhan hukum. Untuk aksesibilitas, WCAG menjelaskan kriteria dan konsep yang perlu diterapkan serta diuji pada konten dan implementasi ([WCAG 2.2](https://www.w3.org/TR/WCAG22/)). Pemilihan artefak membantu menemukan masalah lebih awal, bukan memberi sertifikat.

## Cara kerjanya

Mulailah dari keputusan, bukan dari software desain. Tulis satu kalimat: “Setelah melihat artefak ini, siapa memutuskan apa berdasarkan bukti apa?” Lalu petakan ketidakpastian: struktur informasi, bahasa visual, urutan tugas, perilaku teknis, atau aksesibilitas. Pilih fidelity terendah yang masih dapat menghasilkan bukti untuk ketidakpastian tersebut.

Urutan kerja yang masuk akal adalah:

1. **Selaraskan tujuan dan skenario.** Tentukan pengguna yang relevan, tugas, batas proyek, pemilik keputusan, dan kriteria “cukup untuk lanjut”.
2. **Susun wireframe.** Periksa label, prioritas, navigasi, dan kondisi kosong atau gagal sebelum memoles tampilan.
3. **Tambahkan mockup seperlunya.** Uji apakah hierarki visual dan bahasa membantu tugas; jangan mengunci komponen yang masih diperdebatkan.
4. **Rangkai prototype klik.** Pilih beberapa alur berisiko tinggi, termasuk kembali, batal, validasi, dan error. Catat observasi, bukan hanya komentar suka atau tidak suka.
5. **Naikkan ke prototype berkode bila ada pertanyaan teknis.** Batasi ruang lingkup, data, dan integrasi; pisahkan kode eksplorasi dari fondasi produksi.
6. **Dokumentasikan keputusan.** Simpan asumsi, temuan, perubahan, dan kriteria penerimaan agar tim dapat menelusuri mengapa fidelity dinaikkan atau diturunkan.

Pada setiap langkah, tandai bukti yang belum ada. Pemeriksaan awal seperti keyboard, fokus, judul, label formulir, dan zoom dapat menemukan masalah yang terlihat dari artefak; tetapi evaluasi aksesibilitas perlu cakupan halaman dan proses yang sesuai, bukan satu pemindaian ([WAI Easy Checks](https://www.w3.org/WAI/test-evaluate/preliminary/); [WCAG-EM 1.0](https://www.w3.org/TR/WCAG-EM/)).

## Faktor yang mengubah hasil

Pertimbangkan lima kelompok berikut sebelum menyetujui paket desain.

| Risiko yang ingin dikurangi | Artefak awal yang proporsional | Bukti yang dicari |
|---|---|---|
| Struktur, prioritas, dan istilah belum sepakat | Wireframe | Orang dapat menemukan dan menjelaskan langkah utama |
| Hierarki visual atau identitas belum jelas | Mockup pada layar kunci | Stakeholder mengenali prioritas dan teks penting |
| Alur tugas atau state belum terbukti | Prototype klik | Peserta menyelesaikan skenario dan hambatan tercatat |
| Respons perangkat, data, atau integrasi meragukan | Prototype berkode terbatas | Risiko teknis terukur dengan batas dan asumsi tertulis |

Kompleksitas tidak selalu berarti harus langsung berkode. Jika masalahnya adalah label yang membingungkan, kode mahal tidak menambah bukti. Sebaliknya, animasi, input dinamis, autentikasi, pembaca layar, atau perilaku pada jaringan lambat mungkin membutuhkan percobaan yang lebih dekat ke sistem. Aksesibilitas juga mengubah pilihan: alur keyboard dan fokus perlu dipikirkan sejak struktur, sedangkan verifikasi perilaku assistive technology memerlukan evaluasi yang sesuai cakupan ([WCAG-EM 1.0](https://www.w3.org/TR/WCAG-EM/)).

Biaya revisi dan biaya salah keputusan harus dibaca bersama. Pada keputusan murah dan mudah dibalik, wireframe sering memadai. Pada keputusan yang akan mengunci kontrak teknis, prototype berkode kecil dapat lebih hemat daripada menebak. Mintalah estimasi berbasis pertanyaan dan bukti, bukan jumlah layar semata.

## Contoh keputusan praktis

Bayangkan tiga situasi bersyarat berikut; ini bukan klaim tentang proyek tertentu.

- Tim masih berbeda pendapat apakah pengguna memulai dari pencarian atau kategori. Pilih wireframe dua alternatif, lalu jalankan percakapan atau tugas singkat. Jangan membeli mockup lengkap sebelum struktur dipilih.
- Struktur sudah disepakati, tetapi pemilik merek ragu pada kontras, hierarki tombol, dan panjang judul. Buat mockup pada beberapa layar penting, termasuk kondisi error, lalu minta keputusan tertulis tentang aturan visual.
- Pengguna harus melewati beberapa langkah dengan status berhasil, batal, dan gagal. Prototype klik untuk skenario inti memberi bukti alur lebih cepat daripada gambar terpisah. Catat bagian yang belum terhubung dan jangan mempresentasikannya sebagai fitur produksi.
- Ada komponen yang bergantung pada kamera, keyboard, autentikasi, atau API. Buat spike prototype berkode yang sempit. Tetapkan data uji, perangkat, dan kriteria berhenti; hasilnya adalah pembelajaran teknis, bukan jaminan performa akhir.

Pada lembar keputusan, isi minimal: pertanyaan, fidelity yang dipilih, skenario, peserta atau peninjau, bukti yang diterima, temuan, pemilik keputusan, dan langkah berikutnya. Jika tidak ada cara mengubah hasil menjadi keputusan, tunda peningkatan fidelity.

## Kesalahan umum dan cara memeriksanya

**Memilih karena terlihat paling profesional.** Periksa apakah artefak menjawab risiko yang tertulis. Jika tidak, turunkan fidelity dan perjelas pertanyaan.

**Menganggap semua layar harus dibuat.** Minta subset yang mewakili jalur utama, variasi, dan kondisi gagal. Kelengkapan visual tanpa skenario hanya memperbesar pekerjaan.

**Menggunakan placeholder tanpa aturan.** Tulis mana teks, data, dan gambar yang sementara serta keputusan apa yang masih tertunda. Placeholder yang tidak ditandai mudah dianggap final.

**Mengukur keberhasilan dari komentar stakeholder.** Ganti “bagus” dengan tugas yang dapat diamati, kriteria penerimaan, dan catatan perubahan. Bukti harus dapat ditelusuri ke keputusan.

**Mengklaim aksesibilitas dari satu tampilan atau scanner.** Periksa struktur, fokus, keyboard, formulir, zoom, dan cakupan halaman atau proses yang relevan. WCAG tidak otomatis menjadi bukti kepatuhan hukum Indonesia; minta peninjauan yang berwenang untuk kewajiban spesifik.

Teman Codev.id, sebelum menerima file, ajukan tiga pertanyaan: risiko apa yang diuji, bagian mana yang disimulasikan, dan bukti apa yang membuat kita lanjut atau berhenti? Jawaban yang kabur adalah sinyal untuk memperbaiki brief, bukan menambah efek visual.

## Jalan pintas yang tampak praktis

Shortcut yang sering muncul adalah “langsung buat prototype klik supaya semua pihak cepat setuju”. Ini bisa gagal karena klik hanya mengikuti jalur yang disiapkan; ia dapat menutupi istilah yang salah, state yang hilang, atau kebutuhan teknis yang belum diketahui. Alternatif yang lebih aman: mulai dengan wireframe untuk dua atau tiga keputusan struktur, naikkan hanya alur berisiko ke prototype klik, lalu buat prototype berkode kecil jika simulasi tidak dapat menjawab pertanyaan teknis. Setiap perpindahan fidelity harus disertai bukti dan batas yang disepakati.

## Kesimpulan

Pilih wireframe saat ketidakpastian ada pada struktur, mockup saat bahasa visual perlu diputuskan, prototype klik saat alur dan tugas perlu diamati, dan prototype berkode saat risiko teknis atau perilaku nyata menjadi pertanyaan. Pilihan dapat bertahap; fidelity bukan medali dan prototype bukan software produksi.

Langkah berikutnya, tulis satu pertanyaan keputusan, satu skenario, dan bukti yang akan diterima sebelum meminta penawaran desain. Minta artefak diberi label asumsi, simulasi, dan bagian yang belum diuji. Jika keputusan menyentuh aksesibilitas, keamanan, integrasi, atau kewajiban hukum, jadwalkan pemeriksaan profesional yang sesuai. Untuk langkah lanjutan, lihat [beranda Codev.id](/) dan bawa lembar keputusan itu ke percakapan proyek. Aturan operasionalnya sederhana: naikkan fidelity hanya ketika bukti baru sepadan dengan risiko yang ingin dikurangi.
