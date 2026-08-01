---
article_id: CDV-04-A05
title: "Form, Login, dan Workflow Bisnis di Web"
slug: "form-login-dan-workflow-web"
description: "Panduan memetakan status, validasi, identitas, otorisasi, notifikasi, audit trail, privasi, aksesibilitas, pemulihan, dan penerimaan workflow web."
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2025-06-20"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-04
primary_intent: "Specify a secure, usable transactional workflow"
reader_community: "Codev.id"
reader_address: "Teman Codev.id"
final_route: "/artikel/form-login-dan-workflow-web.html"
technical_review: required
sources:
  - "https://docs.aws.amazon.com/prescriptive-guidance/latest/architectural-decision-records/adr-process.html"
  - "https://html.spec.whatwg.org/"
  - "https://www.rfc-editor.org/rfc/rfc9110"
  - "https://www.w3.org/TR/WCAG22/"
  - "https://www.w3.org/TR/WCAG-EM/"
  - "https://www.w3.org/WAI/test-evaluate/preliminary/"
  - "https://web.dev/articles/vitals"
  - "https://developer.chrome.com/docs/crux"
  - "https://www.rfc-editor.org/rfc/rfc9111"
---

# Form, Login, dan Workflow Bisnis di Web

Halo, Teman Codev.id!

Form dan login bukan sekadar kumpulan kolom lalu tombol kirim. Untuk website bisnis yang menerima permintaan, membuat akun, atau memproses persetujuan, Anda perlu memetakan keadaan (state), aturan validasi, identitas, kewenangan, notifikasi, rekam audit, privasi, aksesibilitas, pemulihan, dan kriteria penerimaan. Jawaban praktisnya: gambar alur dari tindakan pengguna sampai status akhir, tulis aturan pada tiap transisi, lalu uji alur berhasil, gagal, dan dipulihkan sebelum memilih implementasi.

Pilihan arsitektur—server-rendered, client-rendered, CMS, custom, monolitik, modular, atau serverless—adalah opsi dengan trade-off, bukan jenjang kedewasaan. Catat alasan pilihan dan konsekuensinya dalam keputusan arsitektur; panduan AWS tentang Architecture Decision Record (ADR) dapat menjadi rujukan cara mendokumentasikan keputusan, bukan kewajiban memakai produk AWS ([ADR process](https://docs.aws.amazon.com/prescriptive-guidance/latest/architectural-decision-records/adr-process.html)). [NEEDS GATE-02 REVIEW: keputusan stack dan batas tanggung jawab belum diberikan dalam paket ini.]

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

Ilustrasi umum dari aset lokal codev.id; bukan dokumentasi proyek tertentu.

## Definisi dan batas objek

“Form” adalah antarmuka untuk mengumpulkan atau mengubah data. “Login” adalah proses mengaitkan sesi dengan identitas yang telah dikenali. “Workflow bisnis” adalah rangkaian status dan aturan—misalnya draf, diajukan, ditinjau, perlu perbaikan, disetujui, atau ditolak—yang mengatur siapa boleh melakukan apa. Ketiganya saling terkait: validasi mencegah data cacat, autentikasi menjawab “siapa”, dan otorisasi menjawab “boleh melakukan tindakan ini atau tidak”.

Artikel ini menyusun spesifikasi dan pengujian alurnya. Ia tidak mengimplementasikan pembayaran, integrasi identitas pihak ketiga, atau verifikasi kontrol secara formal; dua area terakhir memiliki kepemilikan lain. Untuk konteks lebih luas tentang membangun situs, Anda dapat melanjutkan ke [layanan web development](/web-development), tetapi rute itu bukan pengganti keputusan workflow pada proyek Anda.

## Cara kerjanya

Mulailah dari peta status, bukan dari desain layar. Untuk setiap status, tulis pemiliknya, data minimum yang wajib ada, tindakan yang tersedia, dan peristiwa yang memindahkan data ke status berikutnya. Contoh sederhana: `draft → submitted → review → approved` dengan cabang `review → changes_requested` dan `review → rejected`. Tetapkan apakah pengguna boleh membatalkan, mengirim ulang, atau melihat riwayat.

Kemudian pisahkan tiga lapisan validasi. Validasi antarmuka memberi umpan balik cepat, validasi server menjadi sumber kebenaran, dan aturan bisnis memeriksa konteks seperti batas kewenangan atau kelengkapan dokumen. Jangan menganggap pesan “field wajib” sebagai kontrol keamanan; endpoint tetap harus menolak data yang tidak sah ketika dipanggil langsung. Struktur elemen form, label, tipe input, dan pesan kesalahan sebaiknya mengikuti perilaku HTML yang didefinisikan WHATWG ([HTML Living Standard](https://html.spec.whatwg.org/)).

Pada login, tentukan siklus sesi: kapan sesi dibuat, diperbarui, berakhir, dan dicabut. Rancang jalur lupa kredensial dan perubahan alamat kontak tanpa membocorkan apakah suatu akun ada. Setelah identitas dikenali, matriks otorisasi memetakan peran ke tindakan dan objek; jangan menyimpulkan izin hanya dari tombol yang disembunyikan di browser. Respons HTTP harus menyampaikan hasil secara konsisten—misalnya keberhasilan, permintaan tidak sah, atau konflik—sesuai semantik HTTP ([RFC 9110](https://www.rfc-editor.org/rfc/rfc9110)).

Setiap transisi penting memicu notifikasi yang dapat ditindaklanjuti: siapa melakukan apa, pada objek mana, dan apa langkah berikutnya. Simpan audit trail yang membedakan waktu kejadian, aktor, perubahan, dan sumber permintaan. Tentukan masa simpan, akses internal, serta cara menangani koreksi tanpa menghapus jejak yang diperlukan. Untuk data sensitif, tulis tujuan pengumpulan, minimisasi, dan penghapusan dalam spesifikasi; jangan menjanjikan kepatuhan hukum tertentu tanpa tinjauan yang berwenang.

Terakhir, definisikan penerimaan per alur. Skenario minimal mencakup berhasil, input salah, sesi kedaluwarsa, izin kurang, permintaan ganda, kegagalan jaringan, notifikasi gagal, dan pemulihan. Kriteria harus dapat diamati: status berubah sesuai aturan, pesan terbaca, dan tidak ada data yang terduplikasi.

## Faktor yang mengubah hasil

Jumlah peran dan tingkat sensitivitas data mengubah desain. Form kontak publik cukup berbeda dari pengajuan internal yang berisi dokumen. Banyaknya status meningkatkan kebutuhan akan riwayat dan aturan transisi eksplisit. Ketergantungan pada jaringan lambat membuat penyimpanan draf lokal atau pengiriman ulang perlu dipertimbangkan—dengan risiko duplikasi yang harus ditangani memakai idempotency key atau pemeriksaan server.

Perangkat dan cara akses juga menentukan hasil. Keyboard, pembaca layar, pembesaran, reflow, dan pesan kesalahan harus diuji pada keseluruhan proses, bukan satu halaman. WCAG 2.2 menekankan keberulangan evaluasi dan cakupan proses; WCAG-EM serta Easy Checks membantu menyusun pendekatan evaluasi, tetapi satu pemindai otomatis tidak dapat menyatakan seluruh proses konform ([WCAG 2.2](https://www.w3.org/TR/WCAG22/), [WCAG-EM](https://www.w3.org/TR/WCAG-EM/), [WAI Easy Checks](https://www.w3.org/WAI/test-evaluate/preliminary/)). [NEEDS GATE-06 REVIEW: kriteria aksesibilitas, target konformansi, dan review hukum proyek belum ditetapkan.]

Kinerja juga perlu dilihat sesuai konteks. Core Web Vitals adalah metrik yang dapat berubah dan harus dibaca bersama kondisi pengukuran; data lapangan dari Chrome UX Report bukan bukti otomatis bahwa alur tertentu cepat atau menghasilkan konversi ([Web Vitals](https://web.dev/articles/vitals), [Chrome UX Report](https://developer.chrome.com/docs/crux)). Tetapkan halaman, versi, perangkat, sampel, dan periode sebelum membandingkan regresi. Cache membantu mengurangi permintaan berulang, tetapi respons yang mengandung data pribadi harus memiliki aturan cache yang tepat sesuai semantik HTTP caching ([RFC 9111](https://www.rfc-editor.org/rfc/rfc9111)).

## Contoh keputusan praktis

Bayangkan formulir pengajuan layanan untuk tiga peran: pemohon, pemeriksa, dan administrator. Pemohon dapat menyimpan draf dan mengirim; pemeriksa dapat meminta perbaikan atau menyetujui; administrator dapat mencabut akses dan melihat audit. Dari sini lahir tabel keputusan berikut.

| Keadaan | Aktor | Tindakan | Pemeriksaan minimum | Hasil |
|---|---|---|---|---|
| Draf | Pemohon | Kirim | Field wajib, format, sesi aktif | Menjadi `submitted`, audit tercatat |
| Submitted | Pemeriksa | Minta perbaikan | Pemeriksa berwenang, alasan wajib | Menjadi `changes_requested`, notifikasi |
| Submitted | Pemeriksa | Setujui | Dokumen lengkap, tidak ada konflik | Menjadi `approved`, waktu dan aktor dicatat |
| Apa pun | Tanpa sesi/izin | Ubah data | Verifikasi server | Ditolak tanpa membocorkan data |

Ini contoh metode, bukan klaim bahwa proyek Anda memiliki peran tersebut. Jika bisnis hanya memiliki satu pemeriksa, matriks dapat lebih kecil; jika ada pemisahan tugas, konflik kepentingan perlu menjadi kondisi yang memblokir persetujuan. Sobat Codev.id, minta pemilik proses menandatangani peta status dan matriks izin sebelum tim membangun layar. Untuk langkah berikutnya tentang konteks bisnis online, gunakan [panduan website bisnis online](/website/bisnis-online) bila memang relevan dengan tujuan situs Anda.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah menganggap satu pesan sukses berarti workflow selesai. Periksa status yang tersimpan, audit, notifikasi, dan tampilan bagi tiap peran. Kedua, hanya mengandalkan validasi JavaScript. Nonaktifkan JavaScript atau panggil endpoint dengan data tak sah untuk memastikan server tetap menolak. Ketiga, menyamakan autentikasi dengan otorisasi. Uji akun yang sah tetapi tidak memiliki izin terhadap setiap objek dan aksi.

Keempat, membuat login yang tidak memiliki jalan pulang. Uji kedaluwarsa sesi, token pemulihan yang dipakai ulang, perubahan kontak, dan permintaan ganda. Kelima, menambahkan cache tanpa klasifikasi data. Tinjau header dan perilaku browser untuk halaman publik versus respons pribadi. Keenam, menyatakan “sudah aksesibel” setelah memeriksa kontras atau label saja. Jalankan uji keyboard, fokus, pembaca layar, zoom, reflow, dan seluruh cabang error; dokumentasikan cakupan dan batas evaluasinya.

Shortcut yang sering dipilih adalah membeli template login lalu menempelkan status bisnis di atasnya. Template dapat menghemat pekerjaan visual, tetapi tidak mengetahui aturan siapa boleh menyetujui, bagaimana konflik dicatat, atau kapan notifikasi harus diulang. Alternatif yang lebih aman adalah memakai komponen yang teruji untuk presentasi, sambil membuat state machine, matriks izin, dan kriteria penerimaan khusus proses Anda.

## Kesimpulan

Form, login, dan workflow bisnis di web harus diperlakukan sebagai satu sistem keputusan: data divalidasi di server, identitas dipisahkan dari izin, setiap transisi punya audit dan notifikasi, dan pengguna memiliki jalur pemulihan yang dapat diuji. Sebelum implementasi, minta pemilik proses meninjau peta status, matriks otorisasi, klasifikasi privasi, skenario aksesibilitas, serta rencana pengukuran kinerja. Catat keputusan arsitektur dan bukti uji pada versi yang dapat dilacak.

Kawan Codev.id, jadikan aturan operasi Anda sederhana: tidak ada status baru tanpa pemilik, tidak ada aksi tanpa pemeriksaan izin, dan tidak ada klaim penerimaan tanpa skenario uji yang dapat diamati. Pembayaran, identitas pihak ketiga, dan verifikasi kontrol tetap memerlukan spesifikasi serta review teknis terpisah.

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
