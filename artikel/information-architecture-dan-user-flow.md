---
article_id: CDV-02-A02
title: "Information Architecture dan User Flow yang Bisa Diuji"
slug: "information-architecture-dan-user-flow"
description: "Produce a content inventory, hierarchy, labels, routes, states, and critical-task flow ready for testing"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2025-04-19"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-02
primary_intent: "Organize content and actions around user tasks"
reader_community: "Codev.id"
reader_address: "Teman Codev.id"
final_route: "/artikel/information-architecture-dan-user-flow.html"
technical_review: required
sources:
  - "https://www.gov.uk/service-manual/agile-delivery"
  - "https://www.w3.org/TR/WCAG22/"
  - "https://www.w3.org/TR/WCAG-EM/"
  - "https://www.w3.org/WAI/test-evaluate/preliminary/"
---

# Information Architecture dan User Flow yang Bisa Diuji

Halo, Teman Codev.id!

Information architecture (IA) dan user flow yang bisa diuji bukan sekadar sitemap yang rapi atau panah-panah di papan tulis. Keduanya harus menunjukkan apakah orang dapat menemukan sesuatu, memahami labelnya, berpindah ke langkah berikutnya, lalu pulih ketika salah. Cara paling aman adalah membuat inventaris konten, hierarki dan rute, daftar status, serta satu alur tugas kritis; kemudian mengujinya pada artefak yang cukup sederhana untuk diubah.

Jawaban ini dapat berubah jika riset proyek menemukan istilah pengguna, aturan akses, atau tugas utama yang berbeda. Karena itu, anggap struktur awal sebagai hipotesis. Pendekatan agile menempatkan asumsi, kebutuhan pengguna, perilaku fungsional, atribut kualitas, dan bukti penerimaan sebagai hal yang perlu dibedakan dan diperiksa bertahap, bukan disimpulkan dari template ([UK Government Service Manual](https://www.gov.uk/service-manual/agile-delivery)).

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

*Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.*

## Definisi dan batas objek

IA menjawab “apa yang tersedia dan bagaimana dikelompokkan”: konten, fungsi, label, hubungan, serta rute yang dapat ditempuh. User flow menjawab “bagaimana seseorang menyelesaikan tugas”: pemicu, keputusan, input, hasil, kondisi gagal, dan jalan pulih. Sebuah halaman dapat berada di hierarki yang benar tetapi tetap menghasilkan flow buruk bila tombolnya tidak terlihat, statusnya tidak jelas, atau pengguna dipaksa mengulang.

Artefak ini bukan implementasi taksonomi CMS. Ia adalah kontrak kerja antara pemilik produk, desain, konten, dan pengembang sebelum struktur dimasukkan ke CMS. Ia juga bukan bukti bahwa permintaan pasar sudah tervalidasi atau bahwa semua kelompok pengguna sudah terwakili. Keputusan tentang prioritas tugas tetap memerlukan riset proyek dan pemilik keputusan yang jelas.

Batas ini membantu Anda menolak pertanyaan yang salah. “Apakah kategori di CMS sudah benar?” adalah pekerjaan model konten dan implementasi. “Apakah orang dapat menemukan formulir pengajuan dan tahu apa yang terjadi setelah mengirim?” adalah pertanyaan IA dan flow yang dapat diuji. Tulis keduanya di backlog berbeda agar temuan tidak hilang di antara istilah.

## Cara kerjanya

Mulai dari tugas, bukan dari menu. Tanyakan siapa yang melakukan apa, dalam konteks apa, dan hasil minimum apa yang dianggap selesai. Pilih satu atau dua tugas paling berisiko—misalnya menemukan informasi penting atau mengirim permintaan—lalu telusuri dari halaman masuk sampai konfirmasi. Jangan merancang seluruh situs sebelum tahu titik yang paling mahal bila salah.

Buat *content inventory* dengan kolom sederhana: ID, nama sementara, tujuan pengguna, pemilik, status, rute kandidat, hubungan ke konten lain, dan keputusan yang masih terbuka. “Halaman bantuan” belum cukup sebagai tujuan; tulis apakah pengguna ingin memahami syarat, membandingkan pilihan, atau memulihkan akses. Satu item boleh memiliki beberapa format, tetapi tujuan utamanya tetap satu sehingga duplikasi dapat dibahas.

Dari inventaris itu, susun hierarki berdasarkan cara orang mencari dan menggunakan informasi. Kelompokkan istilah yang dimengerti pengguna, bukan struktur organisasi internal. Uji label dalam dua arah: dapatkah orang menebak isi sebelum membuka, dan setelah membuka dapatkah mereka menjelaskan mengapa halaman itu berada di sana? Catat sinonim sebagai istilah pencarian atau redirect, bukan sebagai tiga menu yang sama-sama membingungkan.

Selanjutnya petakan rute. Untuk setiap layar atau halaman, tulis rute masuk yang realistis, tindakan utama, tindakan sekunder, dan tujuan setelah tindakan. Tambahkan status minimal: awal, memuat, berhasil, kosong, tidak ditemukan, validasi gagal, gagal jaringan, dan akses ditolak. Status bukan hiasan; status menentukan informasi apa yang perlu dibaca, tindakan apa yang aman diulang, dan kapan pengguna perlu bantuan.

Ubah rute prioritas menjadi flow berbasis tugas. Format yang mudah diuji adalah: pemicu → halaman awal → pilihan → input → validasi → hasil → pemulihan. Di tiap node, tulis kondisi yang terlihat dan keputusan yang harus dibuat. Tandai asumsi dengan jelas, misalnya “pengguna sudah memiliki nomor referensi”; jangan menyamarkannya sebagai fakta. Dari flow tersebut, turunkan kriteria penerimaan yang dapat diamati: pengguna menemukan label tertentu, mengetahui bidang wajib, menerima pesan kesalahan yang dapat ditindaklanjuti, dan mencapai hasil tanpa jalan buntu.

Kerjakan dalam irisan kecil. Buat versi rendah-fidelitas, minta orang yang tidak menyusunnya mencoba tugas, lalu ubah label, urutan, atau status berdasarkan bukti. Siklus pendek membuat keputusan terlacak dan mencegah tim menghabiskan waktu memoles layar yang fondasinya belum terbukti. Tautkan setiap temuan ke item inventaris atau node flow sehingga perubahan memiliki alasan, pemilik, dan kriteria selesai.

## Faktor yang mengubah hasil

Pertama, variasi tugas dan konteks. Pengunjung baru mungkin mencari definisi; pengguna berulang mungkin ingin akses langsung ke tindakan. Perangkat, koneksi, bahasa, dan tekanan waktu mengubah rute yang masuk akal. Jika satu flow hanya berhasil ketika pengguna mengingat istilah internal, labelnya belum siap diuji.

Kedua, keadaan sistem. Flow bahagia (happy path) tidak cukup. Data kosong, duplikasi pengiriman, sesi habis, unggahan gagal, dan izin terbatas perlu memiliki respons yang konsisten. Nyatakan apa yang tersimpan, apa yang boleh diulang, dan siapa yang dapat dihubungi. Hindari menjanjikan status “berhasil” sebelum sistem benar-benar menerima tindakan.

Ketiga, aksesibilitas. IA dan flow harus dapat diikuti dengan keyboard, fokus yang terlihat, semantik yang dipahami teknologi bantu, formulir dan pesan error yang jelas, serta tata letak yang tetap dapat digunakan saat diperbesar atau menyempit. WCAG 2.2 mencakup kebutuhan-kebutuhan tersebut, tetapi satu pemindai otomatis tidak dapat menyatakan seluruh halaman dan proses telah sesuai ([WCAG 2.2](https://www.w3.org/TR/WCAG22/)). Evaluasi perlu menetapkan cakupan halaman dan proses, metode, serta bukti; WCAG-EM menekankan pentingnya pemilihan sampel dan evaluasi terstruktur ([WCAG-EM 1.0](https://www.w3.org/TR/WCAG-EM/)).

Keempat, bukti dan kewenangan. Stakeholder dapat menyepakati nama menu, tetapi hanya pengamatan pengguna atau data tugas yang dapat menunjukkan apakah nama itu dipahami. Catat sumber setiap keputusan: observasi, aturan bisnis, kebutuhan teknis, atau asumsi yang belum diuji. Untuk pemeriksaan awal, WAI Easy Checks membantu meninjau hal-hal seperti judul, heading, fokus, dan formulir; hasilnya tetap pemeriksaan awal, bukan sertifikasi ([WAI Easy Checks](https://www.w3.org/WAI/test-evaluate/preliminary/)).

## Contoh keputusan praktis

Bayangkan tugas: “pengunjung menemukan cara mengajukan dukungan dan mengetahui kapan harus menunggu.” Jangan langsung memilih label “Layanan”. Uji beberapa hipotesis berikut pada artefak yang sama:

| Keputusan | Pertanyaan uji | Bukti yang dicari |
|---|---|---|
| Label navigasi | Apakah orang menebak isi sebelum klik? | Pilihan label dan alasan singkat mereka |
| Rute masuk | Dari beranda atau pencarian, apakah langkah pertama terlihat? | Waktu dan titik berhenti pada tugas |
| Formulir | Apakah syarat dan bidang wajib dipahami? | Input benar tanpa petunjuk lisan |
| Status gagal | Setelah error jaringan, apakah tindakan aman diulang? | Pengguna pulih tanpa kehilangan data |
| Konfirmasi | Apakah hasil dan langkah berikutnya jelas? | Pengguna dapat menyebutkan apa yang terjadi |

Jika pengguna menemukan halaman tetapi salah memilih formulir, masalahnya mungkin label atau pengelompokan, bukan tampilan tombol. Jika mereka mengisi benar tetapi tidak tahu apakah terkirim, masalahnya ada pada status dan konfirmasi. Pisahkan temuan agar solusi tidak melebar menjadi redesain total.

Teman Codev.id, gunakan keputusan berhenti: bila dua pengujian awal menghasilkan interpretasi yang bertentangan, tahan perubahan hierarki besar dan kembali ke pertanyaan tugas. Bila temuan hanya menyangkut warna atau jarak, lanjutkan flow dengan catatan bahwa detail visual diuji pada tahap berikutnya. Setiap keputusan harus memiliki pemilik dan tanggal peninjauan, tanpa mengarang angka keberhasilan sebelum pengujian dilakukan.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah menggambar menu berdasarkan struktur tim. Periksa setiap label dengan pertanyaan “tugas pengguna apa yang selesai di sini?” Jika jawabannya hanya nama divisi, cari istilah hasil atau kebutuhan pengguna.

Kedua, hanya mendokumentasikan happy path. Jalankan checklist status untuk setiap tindakan: memuat, kosong, gagal validasi, gagal jaringan, izin ditolak, dan berhasil. Pastikan pesan menyebut masalah serta tindakan pemulihan yang nyata.

Ketiga, menaruh semua konten dalam satu tingkat prioritas. Tandai tugas kritis, tugas pendukung, dan referensi. Flow kritis harus dapat ditemukan dari rute masuk yang disepakati tanpa mengandalkan pengetahuan sebelumnya.

Keempat, menganggap hasil scanner sebagai bukti selesai. Gabungkan pemeriksaan otomatis dengan keyboard, zoom atau reflow, pembacaan semantik, dan pengamatan proses. Cakupan pengujian harus menyebut halaman dan keadaan yang diperiksa; jangan menyimpulkan kepatuhan hukum Indonesia dari WCAG saja.

Kelima, mengunci rute terlalu dini. Simpan versi, alasan perubahan, dan asumsi yang belum terjawab. Bila pemilik konten atau aturan bisnis berubah, Anda dapat menelusuri node mana yang terdampak tanpa menghapus jejak keputusan.

## Jalan pintas yang perlu ditolak

Shortcut yang sering dipilih adalah “buat sitemap sekali, lalu serahkan ke pengembang.” Cara ini gagal ketika sitemap tidak memuat tujuan, status, atau pemulihan; pengembang kemudian menerjemahkan asumsi yang berbeda dan masalah baru terlihat saat implementasi mahal. Alternatif yang lebih aman adalah menyerahkan paket kecil: inventaris dengan pemilik, hierarki berlabel, rute masuk-keluar, matriks status, satu flow tugas kritis, dan kriteria penerimaan yang dapat diamati. Minta satu orang di luar tim penyusun menjalankan flow tersebut. Jika ia berhenti atau menafsirkan label berbeda, artefak masih berupa hipotesis dan perlu diperbaiki.

## Penutup

IA dan user flow yang bisa diuji adalah struktur yang menghubungkan konten, label, rute, status, dan tugas nyata—bukan gambar yang tampak lengkap. Setelah membuat paket awal, pilih satu tugas kritis, tentukan kondisi berhasil dan gagal, lalu uji dengan pengguna atau perwakilan yang relevan. Simpan temuan pada node yang tepat dan minta pemilik keputusan menyetujui perubahan.

Kawan Codev.id, langkah berikutnya adalah mengubah paket itu menjadi sesi uji singkat dengan cakupan halaman dan proses yang tertulis. Jika Anda perlu menyelaraskan artefak dengan konteks layanan yang lebih luas, gunakan [beranda Codev.id](/) sebagai titik kembali, bukan sebagai pengganti bukti tugas. Ingat batasnya: struktur ini belum membuktikan permintaan, performa implementasi, atau kepatuhan hukum. Ia hanya siap diuji; keputusan rilis tetap menunggu bukti proyek dan peninjauan teknis yang sesuai.

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
