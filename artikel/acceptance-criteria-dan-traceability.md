---
article_id: CDV-01-A06
title: "Acceptance Criteria dan Traceability dari Kebutuhan ke Tes"
slug: "acceptance-criteria-dan-traceability"
description: "Write outcome-based acceptance criteria and a requirement-to-design-to-test traceability map"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2025-04-11"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-01
primary_intent: "Make requirements verifiable and trace them to evidence"
reader_community: "Codev.id"
reader_address: "Teman Codev.id"
final_route: "/artikel/acceptance-criteria-dan-traceability.html"
technical_review: required
sources:
  - "https://www.gov.uk/service-manual/agile-delivery"
  - "https://www.w3.org/TR/WCAG22/"
---

# Acceptance Criteria dan Traceability dari Kebutuhan ke Tes

Halo, Teman Codev.id! Acceptance criteria yang baik bukan daftar fitur yang terdengar meyakinkan. Ia adalah kondisi hasil yang dapat diamati, disepakati, dan diperiksa. Traceability lalu menghubungkan kebutuhan itu ke keputusan desain, implementasi, dan bukti tes. Dengan dua hal tersebut, tim dapat menjawab “apa yang harus benar?” dan “bukti mana yang menunjukkan bahwa itu benar?” tanpa mengandalkan ingatan atau asumsi.

Jawaban singkatnya: tulis kebutuhan sebagai outcome pengguna, ubah tiap outcome menjadi kriteria yang terukur melalui perilaku dan kondisi, lalu beri identitas yang sama pada jejak kebutuhan–desain–kode atau konfigurasi–tes–hasil. Detail alur pengguna, kualitas, dan bukti harus diturunkan dari riset serta keputusan pemilik produk; untuk konteks proyek ini masih diperlukan [NEEDS GATE-01: hasil riset pengguna, asumsi yang disetujui, dan penetapan pemilik keputusan sebelum kriteria dianggap final].

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

Kebutuhan menjelaskan masalah, pengguna, atau hasil yang ingin dicapai. Acceptance criteria (kriteria penerimaan) menjelaskan kondisi yang membuat satu item dapat diterima. Ia bukan langkah internal implementasi dan bukan pengganti seluruh test strategy. Traceability adalah peta hubungan antarartefak: ID kebutuhan menuju kriteria, keputusan desain, perubahan implementasi, kasus tes, dan hasilnya.

Pemisahan ini penting. “Tambahkan ekspor laporan” masih terlalu luas: siapa yang mengekspor, data apa yang boleh terlihat, format apa yang dibutuhkan, dan apa yang terjadi ketika tidak ada data? Kriteria yang baik mengubah pertanyaan tersebut menjadi perilaku yang bisa diperiksa. Sebaliknya, pilihan seperti nama tabel atau pustaka UI biasanya merupakan keputusan desain, bukan acceptance criteria.

Panduan agile pemerintah Inggris menekankan pembelajaran dari pengguna, pekerjaan bertahap, dan pengujian terhadap hasil yang dibutuhkan, bukan sekadar menyelesaikan daftar aktivitas. [UK Government Service Manual—agile delivery](https://www.gov.uk/service-manual/agile-delivery) dapat menjadi rujukan proses; ia tidak menggantikan riset pada proyek Anda.

## Cara kerjanya

Mulailah dengan satu kebutuhan yang memiliki pemilik keputusan. Catat outcome, aktor, konteks, dan batasan yang sudah diketahui. Setelah itu tulis kriteria memakai pola “Given–When–Then” atau kalimat kondisi yang setara:

| Artefak | Pertanyaan yang dijawab | Contoh ID |
| --- | --- | --- |
| Kebutuhan | Hasil apa dan untuk siapa? | REQ-12 |
| Acceptance criteria | Kondisi apa yang harus tampak? | AC-12.1 |
| Desain | Keputusan antarmuka atau sistem apa? | DES-12 |
| Tes | Pemeriksaan apa yang dijalankan? | TST-12.1 |
| Bukti | Hasil, log, atau tangkapan apa yang disimpan? | EVD-12.1 |

Contoh: REQ-12 adalah “Pengguna dapat mengunduh laporan transaksi yang telah difilter.” AC-12.1: “Given pengguna memiliki akses laporan dan filter periode valid, when memilih Unduh, then sistem menghasilkan berkas yang memuat hanya transaksi pada periode tersebut dan menampilkan status selesai.” AC-12.2 menangani periode tanpa data: sistem memberi pesan yang dapat dipahami dan tidak membuat berkas kosong tanpa penjelasan. DES-12 mencatat rancangan kontrol filter dan status proses. TST-12.1 memeriksa periode berisi data; TST-12.2 memeriksa periode kosong dan akses tanpa izin. EVD-12.1 menyimpan hasil tes serta versi build yang diuji.

Gunakan satu baris traceability untuk setiap hubungan, bukan tautan samar ke tiket besar. Saat kriteria berubah, pemilik dapat melihat tes dan desain yang ikut terdampak. Saat tes gagal, tim dapat kembali ke outcome yang dipertaruhkan. Kriteria dibahas bersama sebelum pengerjaan dan diperbarui ketika pembelajaran pengguna mengubah asumsi.

## Faktor yang mengubah hasil

Isi kriteria dipengaruhi oleh empat kelompok kondisi. Pertama, pengguna dan tugas: istilah, urutan, kebutuhan bantuan, serta kondisi gagal harus berasal dari observasi atau keputusan yang tercatat. Kedua, kualitas: aksesibilitas, keamanan, privasi, kinerja, dan pemulihan bukan tambahan belakangan bila outcome bergantung padanya. WCAG 2.2 menyediakan kriteria dan teknik aksesibilitas web yang dapat dipakai sebagai rujukan pemeriksaan, tetapi kesesuaian aktual tetap perlu diuji pada antarmuka dan konteks Anda. [W3C Web Content Accessibility Guidelines (WCAG) 2.2](https://www.w3.org/TR/WCAG22/)

Ketiga, batasan pelaksanaan seperti integrasi, data uji, dan lingkungan. Kriteria yang hanya berhasil di mesin pengembang belum menjadi bukti penerimaan. Keempat, tata kelola bukti: tentukan siapa yang menyetujui, kapan hasil dianggap cukup, dan bagaimana kegagalan dicatat. Tanpa pemilik keputusan dan riset proyek, angka ambang atau perilaku “ideal” tidak boleh diisi dengan tebakan.

Teman Codev.id, jadikan setiap perubahan sebagai pertanyaan dampak: apakah ID kebutuhan, kriteria, desain, tes, dan bukti masih menunjuk outcome yang sama? Jika tidak, hentikan penerimaan sementara dan minta keputusan eksplisit. Untuk konteks dan langkah berikutnya, Anda dapat mulai dari [beranda Codev.id](/).

## Contoh keputusan praktis

Bayangkan backlog berisi “login lebih aman”. Jangan langsung memilih mekanisme. Pecah menjadi outcome dan kondisi: pengguna yang benar dapat masuk; percobaan tidak sah ditolak; pesan tidak membocorkan informasi sensitif; dan pemulihan akun memiliki jalur yang disetujui. Tautkan masing-masing kriteria ke keputusan desain, kemudian ke tes positif, negatif, dan batas aksesibilitas.

Gunakan tabel keputusan berikut saat refinement:

| Pertanyaan | Jika jawabannya belum jelas | Tindakan |
| --- | --- | --- |
| Siapa yang menerima item? | Tidak ada pemilik | Tunda komitmen dan tetapkan pemilik |
| Perilaku dapat diamati? | Hanya kata “mudah” atau “cepat” | Ganti dengan kondisi dan bukti yang disepakati |
| Semua kriteria punya tes? | Ada baris tanpa TST | Tambah tes atau hapus klaim yang tak dibutuhkan |
| Hasil tes dapat ditelusuri? | Bukti hanya komentar lisan | Simpan artefak, versi, dan keputusan |

Jika sebuah asumsi belum diuji, labeli sebagai asumsi dan hubungkan ke pekerjaan riset. Jangan menyamarkannya sebagai kriteria final.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah menulis kriteria sebagai solusi: “gunakan tombol biru” tidak menerangkan outcome. Tanyakan, “perubahan apa yang dapat diamati pengguna?” Kedua, satu kriteria memuat terlalu banyak kondisi sehingga kegagalan tidak terlokalisasi. Pecah menjadi unit yang dapat diuji.

Ketiga, tim hanya menguji jalur sukses. Tambahkan kondisi kosong, data tidak valid, hak akses, kegagalan jaringan, dan kebutuhan keyboard atau pembaca layar bila relevan. Keempat, traceability dibuat setelah rilis. Periksa saat refinement dan pull request agar perubahan tidak kehilangan jejak. Kelima, bukti disimpan tanpa konteks. Setiap hasil perlu ID tes, versi yang diuji, lingkungan, tanggal, dan keputusan penerimaan sesuai praktik proyek.

Shortcut yang sering dipilih adalah memakai satu tiket sebagai spesifikasi sekaligus bukti: “QA sudah cek.” Itu gagal karena tiket tidak menunjukkan kondisi yang diperiksa, cakupan negatif, atau versi artefak. Alternatif yang lebih aman adalah menjaga matriks ringkas dan meminta pemilik keputusan menandatangani status setiap kriteria.

## Kesimpulan dan langkah berikutnya

Acceptance criteria menjadikan outcome dapat diamati; traceability memastikan setiap outcome memiliki jalur menuju desain, tes, dan bukti. Sebelum sprint berikutnya, pilih satu kebutuhan, beri ID, tulis dua atau tiga kondisi Given–When–Then, isi matriks sampai EVD, lalu lakukan review bersama pengguna atau pemilik produk. Tandai asumsi yang belum diteliti dan jangan menyatakan diterima sebelum bukti serta otoritas keputusannya jelas.

Aturan operasinya sederhana: tidak ada kriteria tanpa bukti yang direncanakan, dan tidak ada bukti tanpa kebutuhan yang dapat ditelusuri. Sobat Codev.id dapat memakai beranda Codev.id sebagai titik kembali untuk menata pekerjaan terkait. Artikel ini membantu membuat persyaratan dapat diverifikasi; ia tidak menggantikan strategi pengujian lengkap atau keputusan gerbang rilis profesional.
