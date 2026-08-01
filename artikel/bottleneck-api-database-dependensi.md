---
article_id: CDV-14-A06
title: "Mencari Bottleneck API, Database, dan Dependensi"
slug: "bottleneck-api-database-dependensi"
description: "Use traces, query/dependency timing, percentiles, workload, cache, pool/queue, errors, saturation, controlled tests, and verification"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2026-02-26"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-14
primary_intent: "Diagnose server-side latency and capacity constraints"
reader_community: "Codev.id"
reader_address: "Sobat Codev.id"
final_route: "/artikel/bottleneck-api-database-dependensi.html"
technical_review: required
sources:
  - "https://sre.google/workbook/implementing-slos/"
  - "https://opentelemetry.io/docs/"
  - "https://csrc.nist.gov/pubs/sp/800/61/r3/final"
  - "https://web.dev/articles/vitals"
  - "https://developer.chrome.com/docs/crux"
  - "https://www.rfc-editor.org/rfc/rfc9111"
---

# Mencari Bottleneck API, Database, dan Dependensi

Halo, Sobat Codev.id! Saat transaksi terasa lambat, keputusan yang paling aman bukan langsung menaikkan ukuran server atau menambah indeks. Cari dulu bagian rantai yang benar-benar menunggu: API, query database, panggilan ke layanan lain, cache, pool koneksi, atau antrean. Bottleneck adalah titik yang membatasi aliran pada kondisi pengamatan tertentu; titik itu bisa berpindah ketika beban, versi, atau pola permintaan berubah.

Mulailah dengan satu transaksi yang dapat ditelusuri dari request masuk sampai respons keluar. Cocokkan trace dengan waktu query dan dependensi, lalu bandingkan persentil latensi (misalnya median dan ekor lambat), volume kerja, error, serta tanda saturasi. Jawaban berubah bila sampel hanya berasal dari satu pengguna, terjadi deploy di tengah pengukuran, atau trace tidak mencakup panggilan asinkron. Dokumentasikan kondisi itu sebelum menyimpulkan penyebab. Instrumentasi menyediakan sinyal, bukan jaminan reliabilitas; dokumentasi OpenTelemetry menjelaskan peran telemetry untuk mengamati sistem, sedangkan praktik SLO menggunakannya sebagai dasar keputusan layanan ([OpenTelemetry](https://opentelemetry.io/docs/), [Google SRE Workbook](https://sre.google/workbook/implementing-slos/)).

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

[NEEDS IMAGE REVIEW: LOCAL-001]

## Mulai dari gejala, bukan tebakan penyebab

Tuliskan gejala dalam bentuk yang bisa diuji: endpoint atau operasi apa yang lambat, kapan mulai, berapa banyak permintaan terdampak, dan apakah kegagalan berupa timeout, status error, atau respons yang benar tetapi terlambat. Simpan rentang waktu, versi aplikasi, perubahan konfigurasi, wilayah, serta jenis workload. “API lambat” terlalu luas; “request pencarian pada jam tertentu menunggu di panggilan database” sudah menjadi pertanyaan pemeriksaan.

Pisahkan waktu total dari waktu di setiap rentang (span). Trace yang baik menunjukkan urutan dan durasi, tetapi jangan menganggap rentang terpanjang otomatis sebagai akar masalah: ia mungkin hanya menunggu lock, koneksi, atau respons dependensi. Catat juga jumlah permintaan, ukuran payload, rasio hit/miss cache, jumlah koneksi aktif, kedalaman antrean, dan tingkat error pada jendela yang sama. Gunakan persentil, bukan rata-rata saja, agar kelompok kecil request yang sangat lambat tidak tersembunyi.

Buat tabel kecil sebelum menyentuh kode:

| Yang diamati | Pertanyaan pembeda | Bukti yang disimpan |
| --- | --- | --- |
| Latensi API | Apakah ekor lambat muncul di semua endpoint atau satu operasi? | Persentil per endpoint dan rentang waktu |
| Database | Apakah waktu habis menunggu koneksi, lock, atau eksekusi query? | Durasi query dan status pool pada trace yang sama |
| Dependensi | Apakah timeout mengikuti layanan tertentu atau jaringan? | Durasi span keluar, status, dan retry |
| Kapasitas | Apakah CPU, memori, koneksi, atau antrean mendekati batas yang berlaku? | Deret waktu resource dan kedalaman antrean |

## Saringan risiko langsung

Sebelum profiling atau pengujian, lindungi data dan jalur produksi. Jangan menyalin payload berisi token, data pribadi, atau rahasia ke log dan alat trace. Redaksi nilai sensitif, batasi akses dashboard, dan gunakan identitas sampel yang tidak dapat mengungkap isi transaksi. Jika insiden sedang berlangsung, utamakan containment yang dapat dibalik—misalnya mengurangi beban pada endpoint tertentu atau menghentikan perubahan yang baru dirilis—sesuai kewenangan runbook. Kerangka respons insiden NIST menempatkan persiapan, penanganan, dan pembelajaran dalam satu siklus; ia tidak memberi izin untuk bereksperimen tanpa kontrol ([NIST SP 800-61 Rev. 3](https://csrc.nist.gov/pubs/sp/800/61/r3/final)).

Sobat Codev.id, hentikan pekerjaan dan minta pemeriksaan yang kompeten bila pengujian berisiko mengubah data, memicu penghapusan, menghabiskan koneksi bersama, atau mengganggu layanan pelanggan. Jangan menjalankan query diagnostik berat pada basis data utama tanpa rencana beban dan jalur pemulihan. Jika gejala menyangkut autentikasi, integritas data, atau kebocoran, perlakukan sebagai insiden keamanan; diagnosis performa tidak boleh menutupi kewajiban respons insiden.

## Kemungkinan mekanisme

Kelompokkan hipotesis berdasarkan tempat waktu terpakai, bukan berdasarkan komponen yang paling mudah disalahkan.

1. **API dan aplikasi.** Serialisasi payload besar, pemanggilan berulang, retry berantai, atau pekerjaan sinkron yang seharusnya asinkron dapat memperpanjang span induk. Trace perlu menunjukkan apakah waktu itu aktif bekerja atau menunggu.
2. **Database.** Query mungkin lambat karena rencana eksekusi, lock, I/O, atau antrean koneksi. Query yang cepat ketika dijalankan sendiri belum membuktikan cepat di bawah konkurensi; cocokkan dengan waktu tunggu pool dan workload nyata.
3. **Dependensi eksternal.** Latensi layanan pembayaran, pencarian, atau API internal dapat merambat ke request utama. Periksa timeout, retry, dan bulkhead; retry menambah beban bila semua instance mengulang pada saat yang sama.
4. **Cache.** Hit dan miss menempuh jalur berbeda. Aturan caching HTTP membedakan freshness, validasi, dan penyimpanan; keberadaan header cache tidak membuktikan respons selalu dilayani dari cache ([RFC 9111](https://www.rfc-editor.org/rfc/rfc9111)).
5. **Pool dan antrean.** Thread, koneksi, worker, atau queue yang penuh membuat pekerjaan menunggu walau CPU belum maksimum. Ukur waktu antre dan waktu kerja secara terpisah.
6. **Saturasi dan error.** Resource yang jenuh, garbage collection, throttling, atau error yang memicu retry dapat muncul bersamaan. Korelasi waktu membantu, tetapi korelasi saja belum menetapkan sebab.

## Urutan pemeriksaan dan pengujian

Ikuti urutan yang paling aman dan paling banyak mengurangi ketidakpastian.

Pertama, pilih beberapa trace representatif: satu normal, satu ekor lambat, dan satu gagal bila tersedia. Samarkan atribut sensitif, lalu ikuti trace ID ke span database dan dependensi. Pastikan jam dan versi komponen dapat dibandingkan. Kedua, pecah latensi menjadi waktu kerja dan waktu tunggu: queue, pool, lock, network, serta retry. Ketiga, cocokkan dengan metrik workload—request per detik, ukuran batch, konkurensi, dan rasio cache hit/miss—serta error dan resource pada jendela yang sama.

Keempat, uji hipotesis di lingkungan terisolasi atau melalui canary yang punya rollback. Ubah satu variabel pada satu waktu: bentuk query, batas concurrency, kebijakan retry, atau konfigurasi cache. Simpan baseline, versi, dataset, durasi, dan kriteria berhenti. Pengujian terkendali lebih informatif daripada menambah kapasitas secara serentak karena perubahan ganda menghapus pembanding.

Untuk sisi pengguna, bedakan pengukuran laboratorium dari data lapangan. Core Web Vitals adalah metrik yang ditetapkan penyedianya dan dapat berkembang; CrUX menggambarkan pengalaman pengguna nyata dengan cakupan serta metode agregasi tertentu ([web.dev Core Web Vitals](https://web.dev/articles/vitals), [Chrome UX Report](https://developer.chrome.com/docs/crux)). Keduanya berguna untuk melihat dampak di browser, tetapi bukan bukti tunggal bahwa query server tertentu adalah bottleneck. Hubungkan dengan trace backend dan kondisi jaringan yang sama sebelum membuat klaim before/after.

## Cara membaca hasil tanpa melompat ke kesimpulan

Bedakan lima hal: **hasil tes** (apa yang terukur), **kriteria proyek** (apa yang dianggap dapat diterima), **sebab** (mekanisme yang didukung bukti), **konsekuensi** (pengguna atau resource yang terdampak), dan **otoritas keputusan** (siapa yang boleh mengubah sistem). Persentil p95 yang membaik setelah cache diaktifkan menunjukkan perubahan pada sampel itu; ia belum membuktikan semua workload membaik atau data tetap fresh tanpa memeriksa kebijakan invalidasi.

Tanyakan tiga pertanyaan pembanding: apakah kelompok kontrol dan perlakuan memiliki workload sebanding, apakah versi dan konfigurasi tetap, dan apakah perubahan dapat diulang? Jika tidak, tulis hasil sebagai observasi terbatas. Jangan mengubah angka dari dashboard menjadi janji uptime, ranking, penghematan biaya, atau kapasitas tanpa bukti operasional dan kriteria yang disepakati. SLO adalah objektif layanan dan mekanisme keputusan, bukan janji kontraktual otomatis ([Google SRE Workbook](https://sre.google/workbook/implementing-slos/)).

[NEEDS REVIEW: validasi sampel, kondisi, versi, dan caveat sebelum menerbitkan klaim perbandingan before/after atau ambang kinerja.]

Kawan Codev.id, bila trace berhenti di batas layanan atau datanya tidak konsisten, simpulkan “belum terlokalisasi”, bukan “pasti database”. Minta pemilik komponen menambahkan span atau metrik yang hilang, lalu ulangi pengamatan pada jendela yang dapat dibandingkan.

## Pilihan tindakan dan titik eskalasi

Tindakan sementara harus mengurangi dampak tanpa mengunci diagnosis: turunkan concurrency pada jalur yang jenuh, hentikan retry yang tidak terkendali, aktifkan circuit breaker sesuai runbook, atau alihkan workload yang aman. Pantau error, antrean, dan latensi setelah setiap perubahan; siapkan rollback.

Perbaikan permanen mengikuti mekanisme yang terverifikasi. Query yang terbukti menunggu lock memerlukan pemilik database dan tinjauan perubahan; dependensi yang terbukti lambat memerlukan kesepakatan timeout, retry, dan kontrak antarlayanan; cache yang tidak fresh memerlukan keputusan pemilik data. Bila bukti menyentuh skema, migrasi, keamanan, atau data produksi, eskalasikan kepada spesialis dan dokumentasikan persetujuan.

Jangan memakai ambang generik sebagai keputusan otomatis. Batas pool, timeout, dan antrean bergantung pada workload, arsitektur, serta konsekuensi kegagalan. Tetapkan kriteria berhenti dan siapa yang menyetujui perubahan sebelum eksperimen berikutnya. Setelah stabil, teruskan observasi dan catat apa yang dipelajari dalam proses respons insiden, bukan hanya angka latensi terakhir.

## Jalan pintas yang sering gagal

Jalan pintas yang menggoda adalah menaikkan CPU atau ukuran database karena grafik resource tampak tinggi. Langkah itu bisa menunda gejala, tetapi tidak menghapus lock, query berulang, retry storm, atau antrean koneksi. Sebaliknya, grafik CPU rendah juga tidak membebaskan sistem dari bottleneck I/O atau pool yang penuh.

Alternatif yang lebih aman: bekukan kondisi, ambil trace normal dan lambat, petakan waktu tunggu, lalu jalankan satu perubahan terukur dengan rollback. Jika hasilnya hanya membaik pada satu sampel atau browser metric tanpa korelasi backend, nyatakan batasnya dan lanjutkan pengumpulan bukti. Anda dapat kembali ke [beranda Codev.id](/) untuk konteks umum, tetapi keputusan perubahan tetap berada pada pemilik sistem dan reviewer yang berwenang.

## Langkah berikutnya

Untuk mencari bottleneck API, database, dan dependensi, ikuti rantai request yang sama, ukur kerja serta waktu tunggu pada tiap batas, lalu uji hipotesis dalam kondisi yang terdokumentasi. Hasil yang dapat dipertanggungjawabkan memerlukan trace, query/dependency timing, persentil, workload, cache, pool atau queue, error, saturasi, dan pembanding yang jelas—bukan satu grafik atau satu angka.

Buat paket pemeriksaan berikutnya berisi tiga trace tersamarkan, jendela waktu, versi, workload, metrik resource, query dan dependensi terkait, perubahan yang diuji, serta kriteria berhenti. Minta review teknis sebelum menyentuh data produksi atau menerbitkan klaim before/after. Aturan operasinya sederhana: jangan menyebut akar masalah sampai bukti menunjukkan mekanisme yang dapat diulang, dan jangan menyebut perbaikan berhasil sampai kondisi pembanding serta batas ketidakpastiannya dicatat.
