---
article_id: CDV-04-A06
writing_contract_version: "native-id-v2"
title: "Memilih Arketipe Website dari Tugas Pengguna"
slug: "memilih-arketipe-website-dari-tugas"
description: "Distinguish brochure, catalog, publication, lead generation, membership, marketplace, learning, donation, booking, and commerce by tasks/data/risks"
status: draft
publication_date: "2025-06-23"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-04
primary_intent: "Group website needs by workflow instead of industry labels"
reader_community: "Codev.id"
reader_address: "Teman Codev.id"
final_route: "/artikel/memilih-arketipe-website-dari-tugas.html"
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

# Memilih Arketipe Website dari Tugas Pengguna

Halo, Teman Codev.id!

Kebingungan biasanya muncul ketika satu organisasi melihat banyak pilihan website lalu memilih berdasarkan label industri: sekolah mengambil “website pendidikan”, toko memilih “website retail”, dan lembaga memilih “website organisasi”. Label itu belum menjawab pekerjaan yang harus selesai. Cara yang lebih aman adalah mulai dari tugas pengguna yang paling penting, data yang mengalir, dan risiko ketika alurnya gagal.

Jawaban singkatnya: pilih arketipe yang paling dekat dengan alur utama tersebut. **Brochure** cocok ketika pengunjung terutama membaca informasi; **catalog** ketika mereka membandingkan banyak item; **publication** ketika redaksi menerbitkan dan mengarsipkan; **lead generation** ketika tujuan akhirnya adalah permintaan kontak; **membership** ketika pengguna memiliki akun dan hak akses; **marketplace** ketika banyak penjual bertemu pembeli; **learning** ketika ada materi dan kemajuan belajar; **donation** ketika ada ajakan serta pencatatan dukungan; **booking** ketika pengguna memilih ketersediaan waktu; dan **commerce** ketika keranjang, pembayaran, serta pemenuhan pesanan menjadi alur inti. Satu situs boleh menggabungkan beberapa arketipe, tetapi tetapkan satu alur utama sebagai prioritas pertama.

Bukti yang dapat mengubah pilihan itu adalah pekerjaan nyata, bukan nama bidangnya: siapa yang memasukkan data, siapa yang menyetujui, berapa lama data disimpan, serta apa dampaknya jika formulir, pembayaran, atau jadwal salah. Keputusan arsitektur sebaiknya dicatat beserta alasan dan konsekuensinya; panduan Architecture Decision Record (ADR) AWS dapat menjadi contoh cara mendokumentasikan keputusan, bukan kewajiban memakai produk AWS tertentu ([panduan ADR AWS](https://docs.aws.amazon.com/prescriptive-guidance/latest/architectural-decision-records/adr-process.html)).

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

*Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.*

## Jawaban singkat dan salah paham utama

Arketipe bukan sinonim industri dan bukan peringkat kematangan. Website brochure dapat dipakai perusahaan manufaktur maupun komunitas. Sebaliknya, perusahaan yang sama dapat memerlukan catalog untuk produk, lead generation untuk layanan, dan membership untuk mitra. Yang berubah adalah tugas dan aturan data, bukan sekadar warna atau susunan halaman.

Salah paham yang mahal adalah menganggap semua kebutuhan dapat ditutup dengan menambah halaman pada brochure. Menambah halaman tidak otomatis menyediakan login, pencatatan stok, kalender ketersediaan, alur editorial, atau rekonsiliasi pembayaran. Tulis satu kalimat tugas utama: “Pengunjung datang untuk ___, lalu sistem membantu ___, dan tim menerima ___.” Jika bagian terakhir membutuhkan status, izin, atau transaksi, Anda mungkin sudah keluar dari brochure.

Catat juga keputusan yang sengaja tidak diambil. Misalnya, memilih catalog tanpa checkout berarti situs membantu penemuan dan perbandingan, tetapi penjualan berlangsung di kanal lain. Keputusan seperti ini menghindari janji yang tidak dapat dipenuhi dan memudahkan evaluasi ketika kebutuhan berubah.

## Definisi dan batas objek

Di artikel ini, “website” berarti pengalaman web dan alur pendukungnya: halaman, formulir, akun, konten, serta pertukaran data yang dibutuhkan oleh tugas pengguna. Arketipe adalah pola kerja, bukan paket teknologi. Static, server-rendered, client-rendered, CMS, custom, monolithic, modular, dan serverless semuanya merupakan opsi implementasi; tidak ada yang otomatis lebih matang. Perilaku dokumen, tautan, formulir, dan elemen interaktif tetap perlu mengikuti model web yang dipahami browser ([WHATWG HTML Living Standard](https://html.spec.whatwg.org/)).

Batasnya penting. Pengelompokan ini tidak membuat landing page per industri atau per kota, tidak menggantikan peninjauan rute `/website/*`, dan tidak menentukan stack final. Untuk keputusan platform, autentikasi, pembayaran, atau integrasi, buat dokumen keputusan yang mencatat kebutuhan, alternatif, risiko, dan cara menguji hasil. [NEEDS GATE-02 REVIEW: pilihan arsitektur dan stack final memerlukan kebutuhan nonfungsional, batas integrasi, serta persetujuan teknis proyek.]

## Cara kerjanya

Mulailah dari observasi sederhana, lalu turunkan ke data dan risiko.

1. **Petakan tugas pengguna.** Tulis tindakan berurutan: membaca, mencari, membandingkan, mengirim permintaan, masuk, belajar, memberi dukungan, memilih slot, atau membeli. Bedakan tindakan pengunjung dari pekerjaan staf setelah tindakan itu.
2. **Tentukan objek data dan pemiliknya.** Catalog berpusat pada item dan atribut. Membership menambah identitas, peran, serta status. Booking menambah slot dan aturan benturan. Marketplace menambah dua sisi pengguna dan kewajiban memisahkan data penjual dengan pembeli. Jika Anda tidak dapat menyebut objek datanya, arketipe masih berupa asumsi.
3. **Gambar status dan pengecualian.** Apa yang terjadi saat permintaan ditolak, pembayaran tertunda, slot berubah, materi diperbarui, atau akun lupa kata sandi? Alur utama yang terlihat mudah sering gagal di cabang pengecualian.
4. **Tetapkan bukti penerimaan.** Untuk tiap tugas, tentukan bukti selesai: email tercatat, status dapat dilihat, tiket memiliki pemilik, atau pesanan memiliki jejak. Jangan menyebut “otomatis” sebelum ada cara menguji dan memulihkan kegagalan.
5. **Bandingkan alternatif dan catat trade-off.** HTTP mendefinisikan semantik permintaan dan respons yang perlu dipahami ketika alur bergantung pada status, cache, atau pengulangan permintaan ([HTTP Semantics RFC 9110](https://www.rfc-editor.org/rfc/rfc9110)). Pilih implementasi setelah kebutuhan alur, keamanan, aksesibilitas, dan operasi tertulis; bukan sebelumnya.

## Faktor yang mengubah hasil

Empat faktor sering menggeser arketipe yang tampak paling sederhana.

**Frekuensi dan volume konten.** Beberapa halaman layanan yang jarang berubah masih dapat berupa brochure. Banyak penulis, jadwal terbit, arsip, dan koreksi membutuhkan pola publication. Banyak item dengan atribut yang harus konsisten lebih dekat ke catalog daripada kumpulan halaman promosi.

**Identitas dan izin.** Jika pengguna hanya mengirim pertanyaan, lead generation mungkin cukup. Jika mereka harus melihat status sendiri, mengakses materi bertahap, atau memiliki hak berbeda, membership atau learning ikut masuk. Pisahkan “ada formulir login” dari kebutuhan otorisasi yang benar-benar harus diuji.

**Waktu, uang, dan pihak yang terlibat.** Booking harus menangani ketersediaan dan perubahan jadwal. Commerce harus menelusuri keranjang, pembayaran, dan pemenuhan. Marketplace menambah koordinasi antar pihak. Donation memerlukan pencatatan dukungan dan pesan kepercayaan; jangan menjanjikan verifikasi atau pelaporan tertentu tanpa proses yang disepakati.

**Akses, kinerja, dan bukti operasi.** Keyboard, fokus, struktur semantik, formulir dan pesan kesalahan, reflow, zoom, autentikasi, serta teknologi bantu perlu dinilai pada halaman dan proses yang relevan; satu pemindai tidak dapat menyatakan seluruh situs sesuai. Gunakan WCAG 2.2 sebagai rujukan teknis ([WCAG 2.2](https://www.w3.org/TR/WCAG22/)), lalu susun cakupan dan sampel evaluasi dengan WCAG-EM ([WCAG-EM 1.0](https://www.w3.org/TR/WCAG-EM/)) serta pemeriksaan awal WAI ([WAI Easy Checks](https://www.w3.org/WAI/test-evaluate/preliminary/)). [NEEDS GATE-06 REVIEW: klaim konformansi atau kepatuhan harus menunggu evaluasi proses dan sampel yang disetujui; jangan menyamakan WCAG dengan kewajiban hukum Indonesia.]

Kinerja juga perlu dibaca sebagai pengukuran, bukan slogan. Core Web Vitals adalah metrik yang dapat berkembang dan harus dilihat lewat kondisi pengukuran yang jelas ([web.dev Core Web Vitals](https://web.dev/articles/vitals)). Data lapangan dari Chrome UX Report mempunyai cakupan dan batas sampel sendiri ([dokumentasi Chrome UX Report](https://developer.chrome.com/docs/crux)). Cache dapat membantu pengiriman ulang, tetapi aturan penyimpanan dan validasinya mengikuti semantik HTTP caching ([RFC 9111](https://www.rfc-editor.org/rfc/rfc9111)). Tidak satu pun sumber tersebut menjamin peringkat, waktu muat tertentu, energi, atau konversi.

## Contoh keputusan praktis

Gunakan tabel ini sebagai pemilah awal. “Risiko utama” adalah hal yang perlu diuji lebih dulu, bukan klaim bahwa arketipe selalu gagal di sana.

| Tugas dominan | Arketipe awal | Data yang harus jelas | Risiko yang diuji lebih dulu |
| --- | --- | --- | --- |
| Menjelaskan layanan dan kredensial | Brochure | halaman, kontak, versi informasi | informasi usang atau ajakan yang tidak jelas |
| Menemukan dan membandingkan banyak item | Catalog | item, atribut, kategori, pencarian | data tidak konsisten dan filter membingungkan |
| Menerbitkan berita, panduan, atau edisi | Publication | penulis, status naskah, tanggal, arsip | koreksi dan hak terbit tidak terlacak |
| Mengubah minat menjadi percakapan | Lead generation | formulir, persetujuan kontak, pemilik tindak lanjut | permintaan hilang atau tidak ada status |
| Memberi akses berdasarkan identitas/peran | Membership | akun, peran, status, pemulihan | akses berlebih dan proses keluar tidak jelas |
| Mempertemukan banyak penjual dan pembeli | Marketplace | profil pihak, item, pesan, status transaksi | sengketa serta batas data antar pihak |
| Menyelesaikan materi dan kemajuan | Learning | materi, enrolmen, progres, bukti selesai | progres tidak tersimpan atau akses tidak tepat |
| Mengajak dan mencatat dukungan | Donation | kampanye, nominal/status, tanda terima | pencatatan ganda dan ekspektasi pelaporan |
| Memilih waktu atau kapasitas | Booking | layanan, slot, zona waktu, pembatalan | benturan jadwal dan perubahan tidak tersampaikan |
| Menjual hingga memenuhi pesanan | Commerce | produk, keranjang, pembayaran, pengiriman | status pembayaran dan pemenuhan tidak sinkron |

Contoh bersyarat: sebuah konsultan dengan lima layanan dapat memulai dari brochure. Jika setiap layanan membutuhkan formulir yang disaring dan ditindaklanjuti tim, lead generation menjadi alur sekunder yang harus dirancang sejak awal. Sebuah toko yang hanya menampilkan spesifikasi dan mengarahkan pembelian ke distributor masih berupa catalog; ia baru menjadi commerce ketika situs sendiri memegang keranjang, pembayaran, dan status pemenuhan. Teman Codev.id, bedakan fakta proses itu dari asumsi “nanti bisa ditambahkan”.

Untuk langkah berikutnya, Anda dapat membaca [penjelasan tentang pembuatan website berbasis WordPress](/website-wordpress) bila kebutuhan konten dan pengelolaannya mengarah ke CMS. Jika tugas utama berubah karena struktur informasi dan alur sudah tidak cocok, gunakan [panduan redesign website](/redesign-website) sebagai percakapan penataan ulang, bukan sekadar mengganti tampilan.

## Kesalahan umum dan cara memeriksanya

**Memilih dari industri.** Tanyakan “tugas apa yang harus selesai dalam satu kunjungan?” lalu minta contoh input dan output. Jika jawabannya berbeda untuk pengunjung, staf, dan mitra, jangan pakai satu label industri sebagai spesifikasi.

**Menganggap fitur sama dengan alur.** Daftar “login, chat, pembayaran” belum menjelaskan siapa yang boleh melakukan apa. Gambar status, pemilik, notifikasi, dan pemulihan untuk setiap fitur.

**Mengukur keberhasilan dengan satu angka.** Angka performa tanpa perangkat, jaringan, halaman, versi, dan periode tidak cukup untuk klaim sebab-akibat. Simpan kondisi pengukuran, bandingkan cakupan yang sama, dan jadwalkan pemeriksaan regresi.

**Menganggap pemindai aksesibilitas sebagai sertifikat.** Uji keyboard dan fokus, struktur serta label, error formulir, zoom/reflow, autentikasi, media, dan alur penting pada sampel yang disepakati. Catat temuan yang belum diperbaiki dan siapa pemiliknya.

**Menduplikasi rute untuk masalah yang sama.** Sebelum membuat halaman baru, cek apakah kebutuhan sudah dijawab oleh tujuan yang ada. Jika proyek berada di wilayah tertentu, jangan mengubah artikel ini menjadi landing page kota atau industri; lakukan peninjauan konsolidasi rute secara terpisah.

## Memilih dengan bukti, bukan asumsi

Shortcut yang sering dipilih adalah “mulai dari template brochure, nanti semua fitur ditambahkan”. Ini bisa masuk akal untuk menguji pesan awal, tetapi berisiko ketika data dan status transaksi belum memiliki pemilik. Migrasi dari halaman bebas ke data terstruktur dapat mengubah URL, izin, formulir, dan cara pemulihan; biaya sebenarnya baru terlihat setelah alur penting berjalan.

Alternatif yang lebih aman adalah membatasi percobaan pada satu tugas, menuliskan objek data dan pengecualian, lalu menetapkan titik keputusan: tetap brochure, tambah lead generation, atau pindah ke arketipe lain. Catat asumsi yang belum terbukti dalam ADR dan minta tinjauan teknis sebelum pembayaran, akun berperan, atau data sensitif diaktifkan.

## Langkah berikutnya

Memilih arketipe website berarti memilih alur kerja yang menjadi pusat keputusan: tugas pengguna, data yang berpindah, dan risiko ketika alur itu gagal. Industri hanya konteks; brochure, catalog, publication, lead generation, membership, marketplace, learning, donation, booking, dan commerce dapat dipakai lintas bidang.

Langkah Anda berikutnya adalah membawa satu peta tugas, contoh input-output, daftar pengecualian, serta bukti penerimaan ke sesi review. Minta tim menyetujui arketipe utama, arketipe pendamping, dan hal yang sengaja belum dibangun. Kawan Codev.id, pertahankan keputusan itu sampai bukti penggunaan menunjukkan kebutuhan baru—jangan mengubahnya hanya karena label atau tren.

Aturan operasinya sederhana: jangan menjanjikan stack, konformansi, kinerja, atau hasil bisnis sebelum ruang lingkup, kondisi pengukuran, dan pengujian profesionalnya jelas. Bila bukti itu belum ada, tandai sebagai pekerjaan review dan biarkan arketipe tetap menjadi hipotesis yang dapat diuji.
