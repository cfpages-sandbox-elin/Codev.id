---
article_id: CDV-13-A03
writing_contract_version: "native-id-v2"
title: "Form, Error, dan Autentikasi yang Aksesibel"
slug: "form-error-autentikasi-aksesibel"
description: "Cover labels/instructions, required state, validation timing, error summary, focus, status, redundant entry, accessible authentication, and support"
status: draft
publication_date: "2026-01-23"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-13
primary_intent: "Make data entry and account access perceivable and recoverable"
reader_community: "Codev.id"
reader_address: "Sobat Codev.id"
final_route: "/artikel/form-error-autentikasi-aksesibel.html"
technical_review: required
sources:
  - "https://www.w3.org/TR/WCAG22/"
  - "https://csrc.nist.gov/pubs/sp/800/218/final"
  - "https://www.w3.org/TR/WCAG-EM/"
  - "https://spec.openapis.org/oas/v3.1.1.html"
  - "https://www.w3.org/WAI/test-evaluate/preliminary/"
---

# Form, Error, dan Autentikasi yang Aksesibel

Halo, Sobat Codev.id! Form yang aksesibel bukan sekadar menambahkan label atau membuat tombol terlihat. Pengguna harus tahu apa yang diminta, kapan input dianggap salah, dan bagaimana pulih tanpa kehilangan konteks. Pada login, pemulihan akun, checkout, dan verifikasi, urutan itu menentukan apakah orang dapat menyelesaikan tugas secara mandiri.

Jawaban praktisnya: pasangkan setiap kontrol dengan label yang dapat diprogram, instruksi yang tetap terlihat, penanda wajib yang tidak hanya bergantung pada warna, pesan kesalahan yang spesifik di dekat field sekaligus dirangkum, dan fokus yang berpindah ke tempat yang membantu. Autentikasi perlu menawarkan cara yang dapat dipersepsi dan dikoreksi tanpa menurunkan kontrol keamanan. Apakah rancangan benar-benar berhasil tetap harus dibuktikan pada alur lengkap dengan keyboard, pembaca layar, pembesaran, dan pengguna nyata; satu pemindai otomatis tidak cukup ([WCAG 2.2](https://www.w3.org/TR/WCAG22/), [WCAG-EM 1.0](https://www.w3.org/TR/WCAG-EM/)).

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

*Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.*

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

## Definisi dan batas objek

Form aksesibel mencakup pengumpulan data, koreksi, dan umpan balik. Label menjawab “ini untuk apa”, instruksi menjawab format atau konsekuensi, sedangkan status validasi menjawab “apa yang harus dilakukan sekarang”. Autentikasi aksesibel menerapkan prinsip yang sama pada identitas dan faktor masuk: pengguna dapat mengenali kolom, memasukkan rahasia tanpa bocor, memahami kegagalan, dan meminta bantuan.

Artikel ini tidak menetapkan kebijakan identitas, tingkat jaminan, pilihan faktor, atau kontrol keamanan. Jangan menghapus pembatasan keamanan demi kenyamanan; keputusan tersebut memerlukan pemilik risiko dan tinjauan teknis. Fokus kita adalah antarmuka dan pemulihan yang dapat dipersepsi.

## Cara kerjanya

Mulailah dari tugas, bukan komponen. Untuk setiap langkah, catat siapa yang mengisi, data apa yang dibutuhkan, formatnya, dan kondisi gagal. Hubungkan label secara semantik dengan kontrol; placeholder bukan pengganti label karena hilang saat pengguna mengetik. Nyatakan field wajib dalam teks, dan berikan contoh format sebelum pengguna mengirim.

Validasi sebaiknya bertahap. Pemeriksaan format ringan dapat terjadi saat pengguna meninggalkan field, sementara pemeriksaan lintas-field dilakukan ketika informasi yang diperlukan tersedia. Jangan mengumumkan kesalahan pada setiap karakter atau memindahkan fokus secara mengejutkan. Saat submit gagal, tampilkan ringkasan yang bisa dipindai, tautkan tiap pesan ke field terkait, pertahankan nilai yang sudah benar, lalu tempatkan fokus pada ringkasan atau kesalahan pertama sesuai urutan tugas.

Pesan harus menyebut masalah dan perbaikan: “Masukkan tanggal dengan format DD-MM-YYYY”, bukan “Input tidak valid”. Berikan status sukses atau kemajuan melalui mekanisme yang dapat diumumkan teknologi bantu, tanpa mengandalkan perubahan warna. Untuk error server, jelaskan apakah pengguna dapat mencoba lagi, menunggu, atau menghubungi dukungan; jangan membocorkan apakah sebuah akun ada.

Pada autentikasi, sediakan label untuk nama pengguna, kata sandi, kode, atau perangkat. Tombol tampilkan/sembunyikan kata sandi harus dapat dioperasikan keyboard dan statusnya diumumkan. Jika kode sekali pakai kedaluwarsa, pertahankan konteks dan tawarkan kirim ulang dengan batas yang jelas. Fitur “ingat saya”, pemulihan, dan pengelola kata sandi perlu diuji sebagai alur, bukan hanya sebagai tampilan.

## Faktor yang mengubah hasil

Hasil berubah menurut perangkat, metode input, bahasa, dan kondisi jaringan. Keyboard membutuhkan urutan fokus yang logis, indikator fokus yang terlihat, dan tidak ada perangkap. Pembesaran atau reflow dapat memecah layout; pesan tidak boleh tertutup atau hanya muncul sebagai tooltip. Pembaca layar memerlukan nama, peran, nilai, dan perubahan status yang tersampaikan.

Jumlah langkah juga berpengaruh. Meminta data yang sama dua kali menambah beban dan peluang salah; bila pengulangan memang diperlukan untuk keselamatan, jelaskan alasannya dan izinkan pengisian dari data yang sudah ada ketika risikonya dapat diterima. Pada checkout, bedakan alamat penagihan dan pengiriman dengan kontrol yang jelas, lalu pastikan perubahan satu alamat tidak menghapus yang lain.

Bukti penerimaan harus mengikuti risiko. Skenario happy path tidak mewakili kegagalan jaringan, sesi habis, kode salah, atau pengguna yang kembali dengan tombol Back. [NEEDS PROJECT REVIEW: cakupan perangkat, teknologi bantu, bahasa, dan keputusan autentikasi proyek belum disediakan.] Tim dapat memakai [WCAG Easy Checks](https://www.w3.org/WAI/test-evaluate/preliminary/) untuk pemeriksaan awal, lalu evaluasi proses sesuai konteks dengan [WCAG-EM](https://www.w3.org/TR/WCAG-EM/).

## Contoh keputusan praktis

Bayangkan formulir pendaftaran empat field. Jika email kosong, ringkasan di bagian atas menyatakan “Periksa 1 kesalahan” dan tautan mengarahkan ke field email; pesan lokal menjelaskan bahwa email wajib diisi. Jika format salah, nilai tetap terlihat agar pengguna cukup memperbaiki bagian yang keliru. Setelah berhasil, status “Akun dibuat” diumumkan dan fokus menuju konten berikutnya, bukan kembali ke awal halaman.

Untuk login, jangan mengubah pesan menjadi petunjuk penyerang, tetapi jangan pula membuat pengguna buntu. Tampilkan pesan netral untuk kombinasi yang gagal, sediakan jalur “Lupa kata sandi”, dan berikan dukungan yang dapat dihubungi. Pada kode verifikasi, beri kolom yang mudah dinavigasi, dukung tempel bila kebijakan mengizinkan, dan jelaskan sisa waktu tanpa menjadikan penghitung sebagai satu-satunya informasi.

Kawan Codev.id, gunakan tabel keputusan sederhana saat meninjau rancangan. Jika perlu konteks layanan, mulai dari [beranda Codev.id](/), lalu gunakan pusat bantuan yang tersedia di proyek untuk menelusuri jalur dukungan.

| Situasi | Respons antarmuka | Bukti yang diminta |
|---|---|---|
| Field wajib kosong | Label wajib, pesan spesifik, tautan dari ringkasan | Keyboard dan pembaca layar |
| Format salah | Contoh format dan nilai dipertahankan | Uji bahasa dan pembesaran |
| Submit gagal karena jaringan | Status dapat diumumkan, opsi coba lagi | Uji offline/latensi |
| Autentikasi ditolak | Pesan netral, pemulihan tersedia | Tinjauan keamanan dan aksesibilitas |

## Kesalahan umum dan cara memeriksanya

Shortcut pertama adalah menganggap tanda bintang atau warna merah sudah cukup. Tanyakan: apakah label menyebut wajib dalam teks, dan apakah pesan tetap terbaca tanpa warna? Kedua, menaruh semua error sebagai toast yang menghilang. Ulangi submit dengan keyboard dan pastikan kesalahan dapat ditemukan kembali. Ketiga, memfokuskan ulang ke field pertama setiap kali ada error; ini memaksa pengguna menelusuri ulang. Verifikasi bahwa fokus mengikuti langkah koreksi yang paling masuk akal.

Keempat, menguji hanya dengan data valid. Siapkan matriks kasus: kosong, format salah, nilai batas, duplikasi, sesi habis, respons lambat, dan kegagalan layanan. Catat lingkungan, build, data, hasil, serta cacat yang belum terselesaikan. Praktik pengembangan aman NIST menekankan hasil uji dan risiko perlu ditelusuri sampai keputusan rilis, bukan disimpulkan dari satu angka cakupan ([NIST SSDF 1.1](https://csrc.nist.gov/pubs/sp/800/218/final)).

## Risiko jalan pintas

“Kami sudah memasang library validasi dan lulus pemindaian, jadi selesai.” Library membantu konsistensi, tetapi tidak mengetahui apakah instruksi mudah dipahami, fokus tidak mengejutkan, atau alur pemulihan dapat diselesaikan dengan teknologi bantu. Pemindaian juga hanya memeriksa aturan yang dapat dideteksi pada sampel. Alternatif yang lebih dapat dipertanggungjawabkan adalah menggabungkan pemeriksaan otomatis, uji keyboard dan pembaca layar pada alur penuh, serta penerimaan pemilik produk dengan daftar risiko terbuka. Kontrak perilaku antarmuka dan respons error juga perlu dicatat agar implementasi dan pengujian berbicara tentang hal yang sama; spesifikasi API dapat membantu mendokumentasikan respons, tetapi bukan bukti pengalaman pengguna ([OpenAPI 3.1.1](https://spec.openapis.org/oas/v3.1.1.html)).

## Kesimpulan

Form, error, dan autentikasi aksesibel membuat pengguna memahami permintaan, mengetahui apa yang gagal, dan pulih tanpa kehilangan pekerjaan. Terapkan label dan instruksi semantik, validasi yang waktunya wajar, ringkasan error yang terhubung ke field, fokus dan status yang dapat diumumkan, serta jalur autentikasi dan bantuan yang aman.

Langkah berikutnya: pilih satu alur kritis, tulis matriks kondisi berhasil-gagal, jalankan dengan keyboard, pembaca layar, pembesaran, dan jaringan buruk, lalu minta tinjauan pemilik keamanan serta pengguna yang relevan. Teman Codev.id, [NEEDS TECHNICAL REVIEW: konformitas dan keputusan keamanan proyek harus disahkan oleh tim yang berwenang.] Aturan operasionalnya sederhana: tidak ada rilis form atau login sebelum setiap kegagalan memiliki pesan, fokus, pemulihan, dan bukti uji yang dapat ditelusuri.
