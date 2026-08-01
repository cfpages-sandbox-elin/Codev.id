---
article_id: CDV-01-A01
title: "Brief Penemuan Masalah Sebelum Membuat Software"
slug: "brief-penemuan-masalah-software"
description: "Panduan satu halaman untuk merumuskan masalah, pengguna, proses saat ini, batasan, bukti, dan ukuran keberhasilan sebelum membuat perangkat lunak."
status: draft
publication_date: "2025-03-23"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-01
primary_intent: "Frame a software problem before proposing a solution"
reader_community: "Codev.id"
reader_address: "Teman Codev.id"
final_route: "/artikel/brief-penemuan-masalah-software.html"
technical_review: required
writing_contract_version: "native-id-v2"
sources:
  - "https://www.gov.uk/service-manual/agile-delivery"
  - "https://www.w3.org/TR/WCAG22/"
---

# Brief Penemuan Masalah Sebelum Membuat Software

Halo, Teman Codev.id! Ketika proses kerja terasa lambat atau pelanggan terus mengeluh, respons yang menggoda adalah langsung meminta aplikasi. Padahal, sebelum memilih fitur atau meminta penawaran, Anda perlu membuat brief satu halaman yang menjelaskan masalahnya: siapa yang terdampak, bagaimana proses berjalan sekarang, batasnya, bukti yang tersedia, dan seperti apa perbaikan yang dapat dinilai.

Brief ini bukan proposal teknis dan bukan janji bahwa software pasti jawabannya. Fungsinya adalah mengubah keluhan seperti “admin kewalahan” menjadi pertanyaan yang bisa diperiksa: tugas apa yang tersendat, pada tahap mana, bagi pengguna mana, seberapa sering, dan apa akibatnya. Jawabannya dapat berubah setelah wawancara pengguna, pengamatan alur kerja, atau data penggunaan diperoleh. Keputusan untuk meneruskan, menunda, atau mengubah arah tetap perlu pemilik keputusan proyek yang jelas. **[NEEDS GATE-01: konfirmasi pemilik keputusan serta riset pengguna/proyek sebelum brief dipakai untuk menyetujui solusi.]**

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

Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.

## Mulai dari gejala, bukan tebakan penyebab

Mulailah dengan gejala yang dapat diceritakan kembali tanpa menyalahkan orang atau teknologi. Catat kejadian, pengguna yang terkena, langkah kerja saat itu, frekuensi, lokasi data atau kanal kerja, serta perubahan yang baru terjadi. “Pesanan sering terlambat tercatat” lebih berguna daripada “kami butuh aplikasi pesanan”, karena kalimat pertama masih membuka ruang untuk memeriksa proses, peran, dan informasi yang hilang.

Tulis bagian masalah dalam dua atau tiga kalimat: keadaan saat ini, dampaknya, dan batas pengamatan. Contohnya: “Staf menerima permintaan dari tiga kanal. Pada jam sibuk, sebagian permintaan tidak tercatat pada hari yang sama. Catatan ini berasal dari keluhan internal; jumlah kejadian dan penyebabnya belum diverifikasi.” Batas terakhir penting agar dugaan tidak berubah menjadi fakta hanya karena sudah masuk dokumen.

Kawan Codev.id, tanyakan: *apa yang benar-benar dilihat atau dicatat, dan apa yang masih berupa cerita?* Jika tidak ada bukti, tulis sebagai asumsi yang harus diuji. Jangan menambahkan target penghematan, jumlah pengguna, atau dampak finansial bila belum ada data yang dapat ditelusuri.

## Saringan risiko langsung

Sebelum memetakan solusi, periksa apakah masalah menyangkut akses yang tidak semestinya, data pribadi, transaksi keliru, layanan yang tidak dapat dipakai, atau operasi yang perlu dihentikan sementara. Pada kondisi seperti itu, brief harus memuat tindakan perlindungan segera—misalnya membatasi akses atau memakai prosedur manual yang telah disetujui—serta siapa yang berwenang mengambil keputusan. Brief bukan alasan untuk bereksperimen pada data atau layanan yang berisiko.

Masukkan pula kebutuhan aksesibilitas sebagai kebutuhan kualitas, bukan hiasan tahap akhir. [WCAG 2.2](https://www.w3.org/TR/WCAG22/) menyediakan rekomendasi untuk membuat konten web lebih mudah diakses oleh lebih banyak pengguna dengan disabilitas. Itu tidak otomatis menentukan desain produk Anda, tetapi membantu tim menanyakan sejak awal siapa yang mungkin tidak dapat menyelesaikan tugas karena cara antarmuka bekerja.

Gunakan saringan sederhana berikut sebelum pekerjaan diteruskan:

- Apakah ada pengguna, data, atau transaksi yang perlu dilindungi sekarang?
- Apakah proses manual sementara sudah cukup aman dan diketahui penanggung jawabnya?
- Siapa yang dapat menghentikan perubahan bila dampaknya memburuk?
- Kebutuhan akses apa yang perlu diuji bersama pengguna, bukan diasumsikan?

Jika satu jawaban belum jelas, tandai sebagai keputusan terbuka dalam brief dan minta pemeriksaan pihak yang kompeten. Jangan menyamakan tindakan sementara dengan perbaikan permanen.

## Kemungkinan mekanisme

Satu gejala dapat berasal dari beberapa mekanisme. Permintaan yang terlambat, misalnya, bisa berkaitan dengan informasi yang tersebar, langkah persetujuan yang tidak jelas, formulir yang membingungkan, peran yang tumpang tindih, atau volume kerja pada waktu tertentu. Software mungkin membantu salah satunya, tetapi tidak otomatis memperbaiki aturan kerja yang belum disepakati.

Karena itu, buat kolom “kemungkinan penjelasan” dan beri label sebagai hipotesis. Untuk setiap hipotesis, tulis bukti apa yang akan membedakannya dari yang lain. Bila dugaan Anda adalah “data terlewat karena harus disalin”, amati satu siklus kerja dan bandingkan catatan dari tiap kanal. Bila dugaan Anda adalah “pengguna tidak memahami urutan”, minta mereka menunjukkan langkah yang dilakukan, bukan hanya menjawab apakah mereka setuju.

Panduan [agile delivery dari UK Government](https://www.gov.uk/service-manual/agile-delivery) menempatkan pemahaman kebutuhan pengguna, pekerjaan bertahap, dan pembelajaran berkelanjutan sebagai bagian delivery layanan. Dalam brief, prinsip praktisnya adalah sederhana: pisahkan masalah yang terlihat dari mekanisme yang belum dibuktikan. Dengan begitu, tim dapat menguji hal yang paling berpengaruh terlebih dahulu tanpa mengunci diri pada satu fitur.

## Urutan pemeriksaan dan pengujian

Pemeriksaan yang baik dimulai dari yang aman, murah, dan paling informatif. Pertama, kumpulkan dokumen atau catatan yang sudah ada: contoh permintaan, aturan kerja, daftar peran, dan waktu terjadinya gangguan. Kedua, amati atau petakan satu perjalanan tugas dari pemicu sampai selesai. Ketiga, bicara dengan beberapa pengguna yang melakukan tugas itu, termasuk orang yang menerima akibatnya. Baru setelah itu tentukan apakah perlu prototipe, uji alur, atau pengukuran tambahan.

Teman Codev.id, ringkas hasilnya dalam satu halaman dengan enam bagian berikut:

| Bagian | Isi yang perlu ditulis |
| --- | --- |
| Masalah | Gejala, dampak, dan batas pengamatan saat ini |
| Pengguna | Siapa yang menjalankan, menerima, atau terdampak tugas |
| Proses saat ini | Pemicu, langkah, perpindahan informasi, dan titik tersendat |
| Batasan | Akses, waktu, kebijakan internal, integrasi, atau risiko yang telah diketahui |
| Bukti | Catatan, observasi, wawancara, atau data yang tersedia; bedakan dari asumsi |
| Keberhasilan | Perilaku atau hasil yang dapat diamati, cara mengeceknya, dan pemilik keputusan |

Jangan memulai dengan daftar fitur. Kriteria penerimaan—bukti bahwa perilaku yang disepakati benar-benar terjadi—baru berguna setelah tim memahami tugas dan batasannya. Catat juga siapa yang menyetujui interpretasi bukti tersebut; angka atau hasil uji tidak dapat berbicara sendiri tanpa konteks proyek.

## Cara membaca hasil tanpa melompat ke kesimpulan

Hasil observasi dapat memperkuat, melemahkan, atau hanya belum cukup untuk menjawab hipotesis. “Dua pengguna melewatkan kolom yang sama” adalah hasil. “Formulir adalah penyebab semua keterlambatan” adalah kesimpulan yang masih memerlukan pembandingan dengan kasus lain. Bedakan keduanya dalam brief agar pembaca berikutnya tahu bagian mana yang dapat diuji ulang.

Pisahkan pula empat hal: bukti yang diperoleh, kriteria keberhasilan yang diusulkan, konsekuensi bisnis atau layanan, dan orang yang berhak memutuskan. Jika pengguna dapat menyelesaikan tugas dalam prototipe, itu belum membuktikan proses siap diterapkan pada semua kondisi. Bila kebutuhan aksesibilitas ditemukan, catat tugas dan hambatannya; jangan mengklaim kepatuhan hanya karena daftar periksa telah dibaca.

Kegagalan yang sering terjadi adalah menganggap template sebagai validasi permintaan. Template hanya membuat pertanyaan lebih rapi. Validasi tetap datang dari riset pada konteks proyek, bukti yang relevan, dan keputusan yang dapat dipertanggungjawabkan. Untuk menentukan langkah berikutnya atau mengenal layanan Codev.id secara umum, Anda dapat kembali ke [halaman utama Codev.id](/).

## Pilihan tindakan dan titik eskalasi

Setelah brief terisi, pilih tindakan berdasarkan kepastian dan risiko, bukan semata-mata semangat membuat produk. Bila masalah berisiko tinggi atau informasi dasarnya belum ada, lakukan kontrol sementara dan pemeriksaan lebih dulu. Bila gejalanya jelas tetapi mekanismenya belum jelas, jalankan riset atau uji kecil yang dapat membedakan hipotesis. Bila kebutuhan, pengguna, batasan, dan kriteria penerimaan sudah disepakati, brief dapat menjadi masukan untuk tahap perencanaan berikutnya.

Sobat Codev.id, eskalasi diperlukan ketika keputusan memengaruhi keamanan, akses data, kewajiban organisasi, keberlangsungan layanan, atau kelompok pengguna yang tidak dapat diwakili oleh asumsi tim. Catat siapa yang harus meninjau dan keputusan apa yang dibutuhkan. Jangan mengubah brief menjadi keputusan sepihak hanya karena dokumennya singkat.

Pilihan lain adalah tidak membuat software dulu. Memperjelas alur, mengurangi kanal masuk, memperbaiki formulir, atau menetapkan pemilik proses kadang menjadi langkah yang paling tepat. Hasil itu bukan kegagalan discovery; justru itulah fungsi brief, yaitu mencegah investasi pada jawaban yang salah.

## Jalan pintas yang perlu dihindari

Jalan pintas yang umum adalah meminta tim membuat “MVP” secepat mungkin lalu berharap penggunaan nyata mengungkap masalahnya. Pendekatan ini dapat gagal bila MVP sudah mengunci istilah, alur, atau akses yang keliru; pengguna kemudian dipaksa menyesuaikan diri, sementara bukti awal sulit dibaca karena banyak hal berubah sekaligus.

Alternatifnya bukan menunda tanpa batas. Buat brief satu halaman, pilih satu atau dua ketidakpastian paling penting, lalu tentukan bukti minimum untuk mengujinya. Uji dapat berupa menelusuri proses saat ini, membandingkan contoh kasus, atau meminta pengguna menyelesaikan tugas pada rancangan awal. Kerja bertahap seperti ini memberi ruang belajar tanpa menjanjikan bahwa dugaan pertama adalah diagnosis akhir.

## Mulai dengan satu halaman yang dapat diuji

Brief penemuan masalah sebelum membuat software adalah catatan kerja satu halaman yang memisahkan gejala, pengguna, proses saat ini, batasan, bukti, dan keberhasilan yang dapat diperiksa. Isi dokumen itu hari ini dengan satu masalah nyata, lalu tandai setiap asumsi dan nama pemilik keputusan yang masih belum tersedia.

Aturan operasinya: jangan pilih solusi sebelum Anda dapat menyebutkan masalah yang diamati, orang yang terdampak, bukti yang kurang, dan cara menilai perubahan. Jika salah satu bagian menentukan keputusan penting tetapi belum dapat diverifikasi, berhenti pada batas itu dan minta review yang sesuai.

Simpan versi brief beserta tanggal pengamatan dan perubahan keputusan. Saat bukti baru muncul, perbarui bagian yang berubah alih-alih menutupinya dengan asumsi lama. Dengan cara itu, diskusi tentang software tetap berpijak pada masalah yang sedang dihadapi.
