---
article_id: CDV-05-A01
title: "Responsive Web, PWA, atau Native App"
slug: "responsive-web-pwa-atau-native-app"
description: "Panduan memilih permukaan produk seluler berdasarkan pemasangan, penemuan, API perangkat, penggunaan offline, kinerja, distribusi, pembaruan, biaya, dan kebutuhan pengguna"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2025-06-26"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-05
primary_intent: "Choose the appropriate mobile product surface"
reader_community: "Codev.id"
reader_address: "Sobat Codev.id"
final_route: "/artikel/responsive-web-pwa-atau-native-app.html"
technical_review: required
sources:
  - "https://docs.aws.amazon.com/prescriptive-guidance/latest/architectural-decision-records/adr-process.html"
  - "https://html.spec.whatwg.org/"
  - "https://www.rfc-editor.org/rfc/rfc9110"
  - "https://www.w3.org/TR/WCAG22/"
  - "https://www.w3.org/TR/WCAG-EM/"
---

# Responsive Web, PWA, atau Native App

Halo, Sobat Codev.id!

Jika pengguna Anda banyak memakai ponsel, itu belum otomatis berarti Anda harus membuat aplikasi native. Pilihan awal yang biasanya paling aman adalah **responsive web**: situs yang menata ulang konten dan interaksi untuk berbagai ukuran layar. Tambahkan karakter **Progressive Web App (PWA)** bila pengguna perlu memasang pintasan, menerima sebagian pengalaman offline, atau memakai kemampuan peramban tertentu. Pilih **native app** ketika kebutuhan benar-benar bergantung pada integrasi perangkat, alur intensif, atau distribusi melalui toko aplikasi.

Jawaban itu berubah bila ada bukti tentang frekuensi penggunaan, kebutuhan offline, API perangkat, kontrol keamanan, kemampuan tim, dan biaya pemeliharaan. Jadi, jangan mulai dari pertanyaan “teknologi mana yang paling canggih?”. Mulailah dari pekerjaan pengguna yang harus selesai, kondisi jaringan yang dihadapi, dan konsekuensi bila layanan gagal. Catat pilihan, alasan, alternatif yang ditolak, serta hal yang masih perlu diuji dalam keputusan arsitektur; pendekatan seperti ini sejalan dengan tujuan *architecture decision record* (ADR) dari AWS, bukan dengan anggapan bahwa satu pendekatan selalu paling matang ([panduan ADR AWS](https://docs.aws.amazon.com/prescriptive-guidance/latest/architectural-decision-records/adr-process.html)).

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.

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

**Responsive web** adalah satu layanan web yang dapat ditemukan melalui URL dan dirender agar tetap terbaca serta dapat dioperasikan pada layar kecil maupun besar. Pengguna tidak perlu memasang paket dari toko. **PWA** tetap berangkat dari web, tetapi dapat menambahkan manifest, instalasi sebagai pintasan, dan perilaku yang dibantu *service worker* bila dukungan peramban serta desain aplikasinya memungkinkan. PWA bukan jaminan bahwa seluruh fitur native tersedia atau bahwa semua halaman otomatis bekerja tanpa jaringan.

**Native app** dibuat untuk platform tertentu dengan SDK dan pola distribusinya sendiri. Ia dapat meminta izin dan memakai API perangkat yang relevan, tetapi harus melewati proses distribusi, pembaruan, dan pemeliharaan platform tersebut. Artikel ini membandingkan permukaan produk, bukan memilih bahasa pemrograman, framework, atau vendor. Static, server-rendered, client-rendered, CMS, custom, monolithic, modular, dan serverless adalah opsi arsitektur yang bisa dipertimbangkan sesuai masalah; istilah tersebut bukan peringkat kedewasaan ([HTML Living Standard](https://html.spec.whatwg.org/)).

Batas ini penting. “Bisa dibuka di ponsel” hanya menjawab akses dasar. Ia belum menjawab apakah pengguna perlu bekerja saat offline, memakai kamera secara berulang, menerima notifikasi, mengelola data sensitif, atau menjalankan proses panjang. Begitu kebutuhan itu muncul, permukaan produk harus dibandingkan dengan risiko operasionalnya, bukan dengan daftar fitur pemasaran.

## Cara kerjanya

Pada responsive web, pengguna menemukan alamat melalui pencarian, tautan, atau kanal komunikasi; peramban meminta dokumen dan aset melalui HTTP, lalu halaman menyesuaikan tata letak berdasarkan ruang layar. HTTP mendefinisikan semantik pertukaran pesan, status, dan cache, tetapi tidak menjanjikan bahwa aplikasi Anda cepat atau selalu tersedia ([HTTP Semantics, RFC 9110](https://www.rfc-editor.org/rfc/rfc9110)). Kecepatan tetap dipengaruhi ukuran aset, server, jaringan, kode, dan pekerjaan yang dilakukan di perangkat.

Pada PWA, alurnya bertambah: manifest memberi informasi instalasi, sementara *service worker* dapat mengatur permintaan tertentu dan strategi cache. Tim harus memutuskan data apa yang aman disimpan, kapan cache dianggap kedaluwarsa, bagaimana sinkronisasi dilakukan, dan apa yang terlihat ketika jaringan putus. “Terpasang” tidak sama dengan “offline-first”; perilaku offline harus dirancang per alur dan diuji pada kondisi nyata.

Pada native app, paket dipasang melalui mekanisme platform dan berkomunikasi dengan backend melalui API. Akses kamera, lokasi, biometrik, atau notifikasi dapat lebih terintegrasi, tetapi setiap izin menambah tanggung jawab desain, pengujian, dan pemeliharaan. Pembaruan juga perlu dikoordinasikan dengan versi aplikasi yang masih terpasang. Dengan demikian, pilihan permukaan mengubah siapa yang menanggung kompleksitas: peramban dan URL memudahkan penemuan, sedangkan paket platform memberi kontrol lebih besar dengan biaya distribusi yang lebih berat.

## Faktor yang mengubah hasil

Gunakan faktor berikut sebagai pertanyaan keputusan, bukan sebagai skor universal.

| Pertanyaan | Responsive web | PWA | Native app |
|---|---|---|---|
| Bagaimana pengguna menemukan layanan? | URL, pencarian, tautan | Sama seperti web, ditambah pintasan setelah dipasang | Toko aplikasi, tautan, atau pemasangan langsung yang diizinkan platform |
| Apakah instalasi wajib? | Tidak | Opsional dan bergantung dukungan peramban | Ya untuk pengalaman penuh |
| Seberapa penting offline? | Biasanya alur online | Bisa sebagian, jika cache dan sinkronisasi dirancang | Dapat dirancang mendalam, tetap perlu strategi data dan konflik |
| Perlu API perangkat khusus? | Terbatas pada kemampuan web yang didukung | Lebih luas dari web biasa, tetap tidak identik dengan native | Umumnya paling leluasa, dengan izin dan batas platform |
| Bagaimana pembaruan berlangsung? | Di sisi layanan; pengguna memuat versi baru | Layanan dan cache harus dikelola agar tidak menyajikan versi keliru | Paket dan kebijakan toko/platform perlu dikelola |
| Beban awal yang masuk akal | Validasi kebutuhan dan alur | Tambahan desain instalasi, cache, dan pemulihan | Build, distribusi, kompatibilitas, dan operasi platform |

Tanyakan juga frekuensi penggunaan. Layanan yang dibuka sekali sebulan untuk membaca informasi biasanya tidak memperoleh manfaat jelas dari instalasi. Sebaliknya, pekerjaan harian dengan sesi panjang dapat membenarkan pintasan atau aplikasi, tetapi hanya jika retensi dan alurnya sudah terbukti. Sobat Codev.id, jadikan “berapa kali dipakai dan untuk menyelesaikan apa?” sebagai data awal, bukan asumsi.

Aksesibilitas berlaku pada ketiga permukaan. Semantik, fokus keyboard, formulir dan pesan kesalahan, reflow atau zoom, autentikasi, media, serta perilaku dengan teknologi bantu harus diperiksa pada halaman dan proses yang relevan. Satu pemindaian otomatis tidak dapat menyatakan seluruh produk sesuai; WCAG-EM menekankan penetapan ruang lingkup, sampel, dan evaluasi yang jelas ([WCAG 2.2](https://www.w3.org/TR/WCAG22/), [WCAG-EM 1.0](https://www.w3.org/TR/WCAG-EM/)). Kesesuaian WCAG juga bukan otomatis bukti kepatuhan hukum Indonesia.

## Contoh keputusan praktis

Bayangkan tiga kondisi bersyarat berikut.

1. **Katalog dan formulir permintaan.** Pengguna datang dari mesin pencari, mengisi formulir sesekali, dan tidak perlu kamera atau offline. Mulai dari responsive web. Ukur penyelesaian formulir, kegagalan validasi, dan pertanyaan dukungan sebelum menambah lapisan instalasi.
2. **Operasi lapangan dengan jaringan tidak stabil.** Pengguna mengisi data berulang, perlu melihat pekerjaan tersimpan, lalu menyinkronkan saat jaringan pulih. PWA mungkin cukup bila strategi cache, penyimpanan, konflik, dan pemulihan dapat diuji. Jika kebutuhan sensor, proses latar, atau kontrol platform melampaui kemampuan web yang tersedia, evaluasi native dengan prototipe terukur.
3. **Interaksi perangkat yang menjadi inti nilai.** Misalnya alur yang sangat bergantung pada API perangkat tertentu, notifikasi yang harus konsisten, atau sesi intensif yang telah terbukti. Native layak dipertimbangkan, tetapi keputusan tetap harus memuat biaya dua platform, dukungan versi, izin, dan rencana rilis.

Tulis ADR satu halaman: masalah pengguna, bukti penggunaan, kondisi jaringan, API yang wajib, risiko aksesibilitas, opsi yang dibandingkan, eksperimen berikutnya, dan pemicu untuk meninjau ulang. Jika datanya belum ada, tandai sebagai hipotesis. Jangan mengubah dugaan menjadi janji kinerja atau penghematan.

## Kesalahan umum dan cara memeriksanya

**“Semua harus menjadi aplikasi.”** Periksa apakah instalasi menyelesaikan hambatan yang nyata. Jika tidak, URL yang mudah dibagikan dan diperbarui bisa lebih sesuai.

**“PWA berarti offline penuh.”** Daftar alur offline satu per satu: data apa yang dibaca, ditulis, disimpan, dienkripsi, dan disinkronkan; lalu uji pemulihan setelah tab ditutup, ruang penyimpanan penuh, dan konflik.

**“Native pasti lebih cepat.”** Definisikan metrik dan perangkat uji. Ukuran paket, waktu interaksi, permintaan jaringan, dan pekerjaan backend tetap memengaruhi hasil; tanpa pengukuran, klaim itu belum terbukti.

**“Satu scanner sudah cukup untuk aksesibilitas.”** Lengkapi pemeriksaan manual, keyboard, pembesaran, pembaca layar, formulir, autentikasi, dan alur kritis dalam ruang lingkup yang ditetapkan.

**“Biaya selesai saat rilis.”** Buat daftar biaya berulang: pemantauan, dukungan versi, pembaruan konten, penanganan insiden, pengujian, dan evaluasi aksesibilitas. Bandingkan total pekerjaan, bukan hanya biaya membuat paket pertama.

## Jalan pintas yang tampak menarik tetapi berisiko

Shortcut yang sering dipilih adalah membungkus situs yang belum selesai ke dalam *webview*, lalu menyebutnya aplikasi. Cara ini tidak otomatis memberi offline yang andal, akses perangkat yang tepat, atau pengalaman instalasi yang baik. Ia juga dapat menyembunyikan masalah navigasi, fokus, ukuran sentuhan, dan pembaruan cache. Alternatif yang lebih dapat diperiksa adalah menyelesaikan alur responsive web terlebih dahulu, mengukur penggunaan, lalu membuat PWA atau native hanya untuk kebutuhan yang memiliki bukti dan kriteria penerimaan.

## Kesimpulan dan langkah berikutnya

Responsive web adalah titik awal yang masuk akal ketika penemuan lewat URL, jangkauan, dan pembaruan mudah lebih penting daripada instalasi. PWA cocok sebagai perluasan bertahap untuk pintasan, cache, atau offline terbatas yang dapat diuji. Native app dipilih ketika integrasi perangkat, alur intensif, atau kebutuhan platform memang menjadi inti nilai dan biaya operasinya dapat ditanggung.

Teman Codev.id, sebelum meminta estimasi, buat ADR berisi tiga skenario penggunaan nyata, API yang wajib, batas offline, rencana aksesibilitas, metrik keberhasilan, dan pemicu pindah permukaan. Validasi satu alur kritis pada perangkat serta jaringan yang benar-benar dipakai. Aturan operasinya sederhana: pilih permukaan paling ringan yang memenuhi kebutuhan terbukti; naikkan kompleksitas hanya setelah bukti dan kemampuan pemeliharaan siap.

Untuk langkah implementasi web dan penentuan ruang lingkup, Anda dapat melanjutkan ke [layanan web development](/web-development). Bila produk juga membutuhkan akuisisi melalui kanal web, tinjau [pengelolaan Google AdSense](/web-google-adsense) sebagai konteks terpisah—bukan alasan untuk memaksakan native app.
