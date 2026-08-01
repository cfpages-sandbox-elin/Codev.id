---
article_id: CDV-19-A03
writing_contract_version: "native-id-v2"
title: "Technical SEO Audit dari Crawl sampai Canonical"
slug: "technical-seo-audit-crawl-canonical"
description: "Check response/indexability, rendering, internal links, canonicals, sitemap/robots, structured data, pagination, duplicates, logs/GSC, and prioritized evidence"
status: draft
publication_date: "2026-06-19"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-19
primary_intent: "Diagnose discoverability and duplication technically"
reader_community: "Codev.id"
reader_address: "Sobat Codev.id"
final_route: "/artikel/technical-seo-audit-crawl-canonical.html"
technical_review: required
sources:
  - "https://developers.google.com/search/docs/essentials"
  - "https://developers.google.com/search/docs/fundamentals/creating-helpful-content"
  - "https://schema.org/docs/documents.html"
  - "https://developers.google.com/search/docs/crawling-indexing/site-move-with-url-changes"
---

# Technical SEO Audit dari Crawl sampai Canonical

Halo, Sobat Codev.id!

Audit teknis yang berguna bukan sekadar menjalankan crawler lalu mengejar daftar error. Mulailah dengan gejala yang dapat diulang: halaman tertentu tidak muncul di indeks, versi yang salah dipilih sebagai canonical, hasil render berbeda dari HTML awal, atau jumlah halaman yang dirayapi membengkak karena parameter dan duplikasi. Dari gejala itu, susun bukti berurutan dari respons server sampai hubungan antar-URL.

Jawaban singkatnya: periksa enam lapisan—respons dan akses, indexability, rendering, internal link, canonical, lalu sitemap/robots—kemudian cocokkan dengan structured data, pagination, duplikasi, dan data log atau GSC. Hasil audit adalah hipotesis yang memiliki bukti dan prioritas tindakan, bukan jaminan indexing, ranking, atau trafik. Google sendiri menjelaskan persyaratan teknis dan praktik dasar pencarian, tetapi pemenuhan persyaratan itu tidak berarti sebuah halaman pasti diindeks atau tampil di posisi tertentu ([Google Search Essentials](https://developers.google.com/search/docs/essentials)).

Kondisi yang dapat mengubah keputusan adalah konteks perubahan: deploy baru, perubahan template, pemindahan URL, aturan akses, atau perbedaan antara bot dan pengguna biasa. Tanpa sampel URL, waktu kejadian, serta akses log/GSC, penyebab hanya dugaan. Karena itu, catat apa yang terlihat, kapan terlihat, dan alat apa yang menghasilkannya sebelum memperbaiki apa pun.

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

*Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu. Gambar ini merupakan aset lokal untuk ilustrasi dan bukan dokumentasi proyek tertentu.*

## Mulai dari gejala, bukan tebakan penyebab

Buat tiket audit dengan empat kolom awal: URL atau pola URL, gejala yang dilihat, waktu pengamatan, dan bukti mentah. “Halaman tidak ada di Google” terlalu luas; pecah menjadi “respons 200 tetapi meta robots noindex”, “HTML awal kosong sebelum JavaScript berjalan”, atau “canonical menunjuk ke URL lain”. Sampel tiga sampai lima URL yang mewakili pola lebih berguna daripada ribuan baris laporan tanpa konteks.

Pisahkan sumber gejala. Apakah masalah hanya terjadi di satu template, satu folder, atau semua halaman? Apakah muncul setelah rilis tertentu? Bandingkan URL yang berhasil dan gagal dengan metode yang sama. Simpan status code, header penting, HTML awal, hasil render, canonical yang terbaca, dan tautan internal yang menuju halaman tersebut. Dengan cara ini, perubahan berikutnya dapat diuji terhadap baseline, bukan ingatan.

## Saringan risiko langsung

Hentikan perubahan massal bila audit menemukan aturan yang dapat memblokir seluruh situs: robots.txt yang melarang folder utama, noindex pada template global, autentikasi yang menutup halaman publik, atau redirect berantai setelah perubahan URL. Jangan menghapus URL lama, mengganti semua canonical, atau memaksa pengindeksan sebelum daftar URL terdampak dan rencana pemulihan disetujui.

Jika ada indikasi akses tidak sah, dependensi rentan, atau log berisi data sensitif, batasi izin dan simpan salinan bukti secara aman. Audit discoverability tidak memberi wewenang untuk mengubah kontrol keamanan. Untuk perpindahan URL, inventaris URL lama-baru, verifikasi redirect, dan rekonsiliasi setelah perubahan mengikuti prinsip yang dijelaskan dalam [panduan Google tentang site move dengan perubahan URL](https://developers.google.com/search/docs/crawling-indexing/site-move-with-url-changes); pekerjaan migrasi penuh berada di luar batas artikel ini.

Sobat Codev.id, titik eskalasi sederhana adalah ketika Anda tidak dapat menjawab “siapa pemilik keputusan rollback?” Jika jawabannya tidak jelas, dokumentasikan temuan dan minta review teknis sebelum menyentuh konfigurasi server atau CMS.

## Kemungkinan mekanisme

Kelompokkan hipotesis agar satu gejala tidak langsung diberi satu obat.

| Lapisan | Kemungkinan mekanisme | Bukti pembeda |
| --- | --- | --- |
| Respons dan akses | Timeout, error 4xx/5xx, redirect berantai, atau akses dibatasi | Rekaman respons berulang dan log server |
| Indexability | noindex, canonical silang, atau halaman dianggap duplikat | HTML, header, dan pemeriksaan URL yang konsisten |
| Rendering | Konten utama baru muncul setelah JavaScript atau gagal mengambil aset | Perbandingan HTML awal dengan DOM hasil render |
| Struktur | Tautan internal tidak dapat diikuti atau pagination terputus | Peta tautan dari halaman sumber ke target |
| Sinyal | Sitemap, robots, canonical, dan structured data saling bertentangan | Versi file dan URL yang benar-benar diakses |

Structured data adalah format untuk menjelaskan entitas atau atribut halaman kepada mesin, bukan bukti bahwa rich result akan muncul. Gunakan dokumentasi definisi dan konteks dari [Schema.org](https://schema.org/docs/documents.html), lalu cocokkan dengan isi yang benar-benar terlihat. Jangan menambahkan properti hanya karena validator menerimanya.

## Urutan pemeriksaan dan pengujian

Kerjakan dari yang paling mudah dipulihkan ke yang paling berisiko.

1. **Respons.** Ambil sampel URL memakai pemeriksaan yang tidak mengubah state. Catat status, redirect terakhir, header content-type, dan apakah akses berbeda menurut user-agent. Ulangi pada waktu berbeda bila gejalanya intermiten.
2. **Indexability.** Baca robots.txt, meta robots, dan header X-Robots-Tag. Pastikan larangan crawl tidak disalahartikan sebagai instruksi menghapus indeks. Periksa juga apakah URL publik membutuhkan login atau menghasilkan konten kosong.
3. **Rendering.** Bandingkan HTML yang dikirim server dengan DOM setelah skrip berjalan. Cari judul, heading, teks utama, tautan, dan canonical di kedua versi. Jika konten penting hanya tersedia setelah interaksi yang gagal, catat dependensi itu sebagai risiko, bukan langsung menyimpulkan mesin pencari pasti gagal merender.
4. **Internal link.** Mulai dari navigasi, breadcrumb, dan halaman hub. Pastikan target memakai URL kanonis, anchor menjelaskan tujuan, dan tidak ada rantai redirect yang tidak perlu. Untuk langkah lanjutan tentang layanan pencarian di situs, pembaca dapat melihat [ruang lingkup layanan Codev](/konten/seo) sekali; tautan itu bukan bukti bahwa masalah audit sudah terselesaikan.
5. **Canonical.** Bandingkan deklarasi link rel="canonical", header bila ada, redirect, sitemap, dan tautan internal. Canonical adalah sinyal pilihan URL, bukan perintah yang selalu diikuti. Jika dua versi berisi materi berbeda, jangan menyatukannya hanya untuk mengurangi jumlah URL.
6. **Sitemap dan robots.** Pastikan sitemap hanya memuat URL yang memang ingin ditemukan dan dapat diakses. robots.txt mengatur perayapan, sedangkan sitemap membantu penemuan; keduanya tidak menggantikan pemeriksaan indexability. Dokumentasikan waktu perubahan file agar hasil crawl dapat dikaitkan dengan rilis.
7. **Pagination dan duplikasi.** Kelompokkan parameter, trailing slash, huruf besar-kecil, filter, dan halaman paginasi. Tanyakan apakah setiap variasi punya tujuan pencarian mandiri atau hanya salinan teknis. Pilih satu pola URL, tautkan secara konsisten, dan biarkan keputusan penggabungan mengikuti isi serta kebutuhan pengguna.
8. **Log dan GSC.** Cocokkan sampel crawler dengan request bot di log dan laporan inspeksi URL/GSC yang tersedia. Cari perbedaan antara URL yang dikunjungi, yang merespons, dan yang dipilih sebagai canonical. Bila data tidak tersedia, nyatakan kekosongan itu secara eksplisit: **[NEEDS LOG/GSC EXPORT: verifikasi pola crawl dan canonical belum dapat disimpulkan.]**

## Cara membaca hasil tanpa melompat ke kesimpulan

Tulis setiap temuan dalam format: observasi, bukti, hipotesis, dampak yang mungkin, dan tindakan yang aman. Contoh: “Tiga URL filter mengembalikan 200 dan canonical ke URL kategori; bukti berasal dari HTML pada 2 Agustus; hipotesisnya variasi filter tidak dimaksudkan sebagai halaman mandiri; tindakan sementara adalah menghentikan penambahan tautan ke variasi tersebut sambil meminta keputusan pemilik konten.” Itu berbeda dari klaim “Google menghukum filter”.

Bedakan error alat dari masalah pengguna. Crawler dapat gagal karena rate limit atau JavaScript yang tidak didukung; pengguna dapat melihat halaman yang tampak baik sementara bot menerima error. Ulangi pengujian, simpan konfigurasi, dan nyatakan tingkat keyakinan. Panduan people-first Google menekankan bahwa konten seharusnya dibuat untuk membantu pembaca, sehingga sinyal teknis perlu dibaca bersama mutu dan kegunaan halaman, bukan sebagai permainan angka semata ([Creating helpful, reliable, people-first content](https://developers.google.com/search/docs/fundamentals/creating-helpful-content)).

Kawan Codev.id, hasil “valid” pada validator hanya berarti format yang diperiksa lolos. Ia tidak membuktikan bahwa data sesuai kenyataan, bahwa canonical dipilih, atau bahwa halaman akan mendapat trafik. Mintalah pemilik domain mengonfirmasi kriteria sukses: URL mana yang harus dipertahankan, perubahan apa yang boleh dilakukan, dan kapan rollback dipicu.

## Pilihan tindakan dan titik eskalasi

Prioritaskan tindakan berdasarkan luas dampak, kemudahan rollback, dan kekuatan bukti.

- **Kontrol sementara:** bekukan rilis yang mengubah template atau aturan crawl, simpan snapshot konfigurasi, dan hentikan pembuatan URL duplikat baru.
- **Perbaikan terukur:** ubah satu lapisan pada satu waktu—misalnya canonical template—lalu uji URL perwakilan sebelum memperluas.
- **Pemantauan:** tetapkan sampel URL, tanggal pemeriksaan ulang, dan sinyal yang dipantau di log/GSC. Perubahan yang tidak terlihat di data belum dapat dinyatakan berhasil.
- **Review profesional:** libatkan pemilik infrastruktur ketika menyentuh cache, WAF, autentikasi, database, atau rollback deploy. Libatkan editor ketika keputusan canonical menggabungkan isi yang berbeda.

Jangan mengganti teknologi atau menghapus riwayat hanya karena komponen terlihat tua. Keputusan penggantian memerlukan inventaris dependensi, risiko paparan, dampak bisnis, keselamatan perbaikan, rencana rollback, dan pemilik keputusan. Audit teknis ini boleh mengangkat pertanyaan tersebut, tetapi tidak menggantikan persetujuan perubahan.

## Jalan pintas yang sering gagal

Jalan pintas yang umum adalah menyalin satu canonical ke semua halaman, mengirim seluruh URL ke sitemap, lalu menunggu indeks pulih. Cara itu dapat menyamarkan variasi isi, mempertahankan URL yang seharusnya tidak publik, dan membuat laporan tampak rapi tanpa memperbaiki tautan atau respons. Alternatif yang lebih aman adalah mengelompokkan URL, memilih sampel, memvalidasi hubungan sinyal, kemudian mengubah aturan paling sempit yang menjelaskan bukti.

## Kesimpulan: jadikan audit sebagai rantai bukti

Technical SEO Audit dari crawl sampai canonical berarti mengikuti rantai: respons dapat diakses, halaman boleh diindeks, konten dapat dirender, tautan mengarah konsisten, canonical masuk akal, lalu sitemap/robots dan structured data tidak saling bertentangan. Setelah itu, pagination, duplikasi, log, dan GSC dipakai untuk menguji apakah hipotesis bertahan di luar sampel.

Teman Codev.id, langkah berikutnya adalah membuat lembar temuan berisi URL sampel, bukti mentah, pemilik keputusan, tindakan yang dapat di-rollback, dan tanggal pemeriksaan ulang. Jika log atau GSC belum tersedia, tandai sebagai kebutuhan review—jangan mengisi celah dengan asumsi. Audit memberi dasar keputusan; ia tidak menjamin indexing, ranking, atau hasil komersial.
