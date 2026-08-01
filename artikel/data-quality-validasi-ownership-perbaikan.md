---
article_id: CDV-07-A04
writing_contract_version: "native-id-v2"
title: "Data Quality: Validasi, Ownership, dan Perbaikan"
slug: "data-quality-validasi-ownership-perbaikan"
description: "Define quality dimensions, validation, stewardship, monitoring, issue workflow, correction evidence, and downstream communication"
status: draft
publication_date: "2025-08-30"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-07
primary_intent: "Establish measurable data-quality controls"
reader_community: "Codev.id"
reader_address: "Teman Codev.id"
final_route: "/artikel/data-quality-validasi-ownership-perbaikan.html"
technical_review: required
sources:
  - "https://peraturan.bpk.go.id/Details/229798/uu-no-27-tahun-2022"
  - "https://peraturan.bpk.go.id/Details/122030/pp-no-71-tahun-2019"
  - "https://www.nist.gov/privacy-framework"
  - "https://csrc.nist.gov/Projects/ssdf/publications"
  - "https://www.cisa.gov/known-exploited-vulnerabilities-catalog"
  - "https://developers.google.com/search/docs/crawling-indexing/site-move-with-url-changes"
---

# Data Quality: Validasi, Ownership, dan Perbaikan

Halo, Teman Codev.id! Data yang tampak lengkap belum tentu layak dipakai untuk keputusan. Duplikasi pelanggan, kolom wajib yang kosong, atau laporan yang saling bertentangan harus ditangani dengan tiga hal sekaligus: definisi mutu yang terukur, pemilik yang berwenang memperbaiki, dan jejak bukti sampai perubahan dikomunikasikan ke pengguna berikutnya.

Jawaban singkatnya: tetapkan dimensi kualitas untuk setiap objek data, jalankan validasi otomatis dan pemeriksaan sampel, tunjuk *data owner* (pemilik keputusan) serta *steward* (pengelola harian), lalu kelola temuan sebagai tiket dengan bukti sebelum-sesudah. Kondisi yang mengubah keputusan adalah tujuan penggunaan, sumber kebenaran, risiko pribadi, serta kemampuan melakukan rollback; tanpa itu, angka skor kualitas hanya memberi rasa aman palsu.

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

*Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.*

## Definisi dan batas objek

Data quality adalah tingkat kesesuaian data dengan kebutuhan penggunaan yang disepakati, bukan klaim bahwa semua nilai selalu benar. Untuk satu tabel pelanggan, misalnya, “lengkap” berarti kolom yang diwajibkan oleh proses terisi; “unik” berarti satu orang tidak muncul sebagai beberapa entitas; “tepat waktu” berarti tersedia sebelum keputusan; dan “konsisten” berarti definisi status sama di laporan yang berbeda. Dimensi lain yang berguna adalah validitas format, akurasi terhadap sumber rujukan, dan keterlacakan perubahan.

Mulailah dari objek dan tujuan, bukan dari alat. Catat siapa memakai data, keputusan apa yang dibuat, periode keberlakuannya, dan sumber asalnya. Data pribadi membutuhkan pemetaan dan pengamanan yang lebih hati-hati. UU No. 27 Tahun 2022 merupakan undang-undang nasional utama tentang pelindungan data pribadi, sedangkan PP No. 71 Tahun 2019 mengatur penyelenggaraan sistem dan transaksi elektronik pada tingkat yang lebih luas ([UU 27/2022](https://peraturan.bpk.go.id/Details/229798/uu-no-27-tahun-2022); [PP 71/2019](https://peraturan.bpk.go.id/Details/122030/pp-no-71-tahun-2019)). Kedua sumber itu tidak otomatis menetapkan dasar pemrosesan, masa simpan, atau kewajiban sektor tertentu untuk kasus Anda; hal tersebut memerlukan penilaian yang berwenang. Untuk menyelaraskan istilah dan langkah berikutnya, gunakan [ruang kerja Codev.id](/) sebagai titik koordinasi internal.

Batas artikel ini adalah kendali mutu operasional. Ia tidak menggantikan persetujuan profesional, kebijakan privasi, atau rancangan migrasi. Koreksi yang memindahkan data antar-sistem perlu rencana rekonsiliasi dan rollback tersendiri.

## Cara kerjanya

Urutan yang dapat diaudit biasanya seperti ini.

1. **Tetapkan aturan.** Untuk setiap atribut, tulis definisi, tipe, rentang, apakah boleh kosong, sumber otoritatif, dan pemilik keputusan. Aturan “status hanya boleh aktif atau nonaktif” lebih dapat diuji daripada “status harus rapi”.
2. **Validasi di titik masuk dan berkala.** Skema, pemeriksaan rentang, referensi antar-tabel, deteksi duplikat, dan pemeriksaan perubahan mendadak menangkap masalah berbeda. Simpan jumlah baris diperiksa, jumlah gagal, dan contoh identitas yang disamarkan.
3. **Pisahkan owner dan steward.** Owner menyetujui definisi dan prioritas risiko. Steward menjalankan pemeriksaan, menghubungi sumber, dan memperbarui katalog aturan. Tim platform menjaga pipeline; pengguna bisnis mengonfirmasi arti nilai.
4. **Pantau ambang dan tren.** Dashboard sebaiknya menampilkan tingkat kelulusan per dimensi, usia temuan terbuka, sumber penyebab, dan dampak pada laporan. Ambang harus memiliki tindakan: tahan publikasi, karantina baris, atau lanjut dengan catatan.
5. **Tutup isu dengan bukti.** Tiket menyimpan nilai awal, alasan perubahan, aktor, waktu, rujukan sumber, hasil validasi ulang, dan daftar konsumen yang diberi tahu. Backup baru menjadi bukti pemulihan setelah proses restore benar-benar diuji; keberadaan berkas salinan saja tidak cukup ([NIST Privacy Framework](https://www.nist.gov/privacy-framework)).

Kawan Codev.id, ownership bukan sekadar menempelkan nama di spreadsheet. Pastikan owner punya wewenang menyetujui definisi dan steward punya jalur eskalasi ketika sumber saling bertentangan.

## Faktor yang mengubah hasil

Hasil kontrol dipengaruhi oleh beberapa kondisi yang harus ditulis eksplisit:

- **Sumber dan konteks.** Data dari formulir, perangkat, dan integrasi memiliki pola kesalahan berbeda. “Benar” harus dibandingkan dengan sumber yang ditetapkan, bukan tebakan operator.
- **Frekuensi dan jeda.** Pemeriksaan harian mungkin cukup untuk laporan mingguan, tetapi tidak untuk keputusan real-time. Ukur *freshness* dengan waktu kejadian dan waktu tersedia, bukan hanya waktu unggah.
- **Perubahan skema dan dependensi.** Pembaruan library, runtime, atau pipeline dapat mengubah parsing. Praktik pengembangan aman NIST menekankan pemeliharaan dependensi; catat versi, pemilik, uji regresi, dan rencana kembali ([NIST SSDF publications](https://csrc.nist.gov/Projects/ssdf/publications)).
- **Paparan dan dampak.** Prioritas kerentanan tidak ditentukan oleh tingkat keparahan saja. Paparan, eksploitasi yang diketahui, dampak bisnis, keamanan perbaikan, rollback, dan ownership ikut menentukan. Katalog Known Exploited Vulnerabilities CISA dapat menjadi sinyal prioritas, bukan pengganti analisis konteks ([CISA KEV Catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog)).
- **Retensi dan penghapusan.** Jangan menghapus riwayat untuk membuat metrik terlihat bersih. Tentukan kebutuhan retensi, otorisasi, dan bukti penghapusan bersama fungsi yang berwenang; jangan menganggap satu aturan berlaku untuk semua sektor.

## Contoh keputusan praktis

Bayangkan laporan penjualan memiliki 4% baris tanpa kode wilayah dan 1% duplikat. Jangan langsung menambal dengan nilai default. Buat keputusan bersyarat:

| Temuan | Bukti minimum | Tindakan | Komunikasi |
|---|---|---|---|
| Kode wilayah kosong, sumber dapat dihubungi | Rujukan sumber dan sampel terverifikasi | Karantina, lengkapi, validasi ulang | Beri tahu pemilik laporan dan waktu rilis baru |
| Duplikat dengan identitas sama | Kunci pencocokan dan keputusan merge | Tahan agregasi, simpan jejak nilai lama | Catat konsumen dan kueri yang terdampak |
| Angka berbeda antar-laporan | Definisi metrik dan timestamp masing-masing | Hentikan publikasi sampai owner memilih sumber | Terbitkan catatan perubahan definisi |
| Data pribadi ikut terkirim ke lingkungan uji | Pemetaan bidang dan tujuan penggunaan | Batasi akses atau gunakan data tersamarkan | Eskalasi untuk penilaian privasi; jangan menebak kewajiban |

Jika koreksi mengubah ID atau struktur antar-sistem, inventaris sumber, lakukan rekonsiliasi jumlah dan checksum yang disepakati, siapkan rollback, lalu uji sebagian kecil. Panduan Google tentang perpindahan URL menunjukkan pentingnya inventaris dan pemetaan lama-ke-baru; prinsip pencatatan dan rekonsiliasi yang sama membantu migrasi data ([Google site-move guidance](https://developers.google.com/search/docs/crawling-indexing/site-move-with-url-changes)).

## Kesalahan umum dan cara memeriksanya

Shortcut pertama adalah menghapus semua baris gagal agar dashboard hijau. Periksa apakah baris itu hanya dipindahkan ke tempat yang tidak dipantau dan apakah konsumen tetap menerima angka lama. Shortcut kedua adalah menunjuk satu owner untuk seluruh domain. Tanyakan apakah orang itu berwenang atas definisi, sumber, dan keputusan koreksi. Shortcut ketiga adalah menganggap validasi format sebagai akurasi. Nilai tanggal yang sesuai pola masih dapat salah secara bisnis; minta bukti pembanding.

Gunakan pemeriksaan mingguan yang sederhana: ambil sampel dari setiap sumber, hitung kegagalan per dimensi, tinjau tiga isu tertua, dan cocokkan tiket tertutup dengan log perubahan. Sobat Codev.id juga perlu meminta bukti downstream: laporan mana yang dihitung ulang, siapa yang menerima notifikasi, dan bagaimana versi sebelumnya dapat ditelusuri.

## Menjawab keberatan yang umum

“Kita cukup memperbaiki data saat ada komplain.” Pendekatan reaktif memang cepat untuk satu kasus, tetapi tidak mengukur pola, tidak menetapkan pemilik, dan dapat membuat dua laporan memakai koreksi berbeda. Alternatif yang lebih aman adalah aturan minimum untuk atribut kritis, monitoring ringan dengan ambang tindakan, dan alur tiket yang memaksa bukti sebelum penutupan. Skala kontrol boleh kecil; jejak keputusannya tidak boleh hilang.

## Kesimpulan

Data quality yang dapat dipercaya lahir dari dimensi yang didefinisikan, validasi yang berulang, ownership yang jelas, dan koreksi yang meninggalkan bukti serta pemberitahuan downstream. Mulailah dengan satu objek kritis: tulis aturan atributnya, tetapkan owner dan steward, ukur baseline, lalu buka tiket untuk temuan terbesar. Minta tinjauan teknis untuk risiko privasi, retensi, dan perubahan lintas-sistem sebelum eksekusi.

Teman Codev.id, jadikan ini aturan operasi: tidak ada status “sudah diperbaiki” tanpa nilai sebelum-sesudah, sumber rujukan, validasi ulang, dan daftar konsumen yang diberi tahu. Kontrol ini meningkatkan keterlacakan, bukan menjanjikan data sempurna; batas sumber dan keputusan profesional tetap harus dinyatakan.
