---
article_id: CDV-11-A01
title: "Cloudflare Pages atau Workers untuk Aplikasi Web"
slug: "cloudflare-pages-atau-workers"
description: "Panduan memilih Cloudflare Pages atau Workers berdasarkan model request, build, binding, batas, observability, migrasi, dan rollback"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2025-11-29"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-11
primary_intent: "Select a Cloudflare application deployment surface"
reader_community: "Codev.id"
reader_address: "Kawan Codev.id"
final_route: "/artikel/cloudflare-pages-atau-workers.html"
technical_review: required
sources:
  - "https://developers.cloudflare.com/pages/"
  - "https://developers.cloudflare.com/workers/"
  - "https://developers.cloudflare.com/workers/configuration/versions-and-deployments/"
  - "https://sre.google/workbook/implementing-slos/"
  - "https://opentelemetry.io/docs/"
  - "https://csrc.nist.gov/pubs/sp/800/61/r3/final"
---

# Cloudflare Pages atau Workers untuk Aplikasi Web

<!-- BEGIN MANAGED IMAGE PLAN
## Image plan

- **Image ID:** `LOCAL-005`
- **Source type:** `local`
- **Placement:** after the opening has answered the main question, before the first detailed H2
- **Exact Markdown to insert:** `![Ilustrasi cloudflare](/wp-content/uploads/2022/11/cloudflare.jpg)`
- **Caption/credit:** Aset lokal proyek; jangan klaim sebagai dokumentasi proyek tertentu.
- **Selection basis:** filename/source metadata identifies `cloudflare` as relevant content media; no pixels were inspected.
- **Hard boundary:** do not infer or describe unseen visual details, project ownership, location, people, brands, condition, performance, or outcome.
- **Substitution rule:** do not replace this image. If unavailable or provenance is incomplete, insert `[NEEDS IMAGE REVIEW: LOCAL-005]` and continue drafting the prose.
END MANAGED IMAGE PLAN -->

Halo, Kawan Codev.id! Kebingungan biasanya muncul ketika satu aplikasi punya halaman statis, endpoint API, dan pekerjaan singkat di edge. Pages dan Workers sama-sama berada di Cloudflare, tetapi objek yang dikelola berbeda. Pilih Pages bila pusat masalah Anda adalah build dan publikasi aset web, dengan Functions bila hanya perlu endpoint server-side yang dekat dengan aplikasi. Pilih Workers bila request, runtime, binding, atau alur deployment perlu dikendalikan sebagai layanan edge tersendiri.

Jawaban itu berubah jika kode Anda membutuhkan runtime atau binding yang tidak tersedia pada bentuk Pages yang dipilih, atau jika prosedur rilis dan rollback menuntut versi Worker yang eksplisit. Dokumentasi resmi Cloudflare menjadi acuan perilaku produk; batas akun, harga, kompatibilitas runtime, dan konfigurasi nyata tetap harus diverifikasi sebelum keputusan final. `[NEEDS GATE-07: verifikasi limits, pricing, API, runtime compatibility, regional/data implications, dan konfigurasi akun yang akan dipakai.]`

![Ilustrasi cloudflare](/wp-content/uploads/2022/11/cloudflare.jpg)

*Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.*

## Jawaban singkat dan salah paham utama

Pages bukan sekadar “hosting murah”, dan Workers bukan otomatis pilihan yang lebih canggih. Pages mengatur alur build lalu menyajikan hasilnya sebagai aset situs; fungsi server-side di dalam proyek Pages cocok ketika perilaku dinamis masih mengikuti situs tersebut. Workers memosisikan kode sebagai handler request edge dengan konfigurasi dan binding yang dapat dikelola sendiri. Rujuk [dokumentasi Pages](https://developers.cloudflare.com/pages/) dan [dokumentasi Workers](https://developers.cloudflare.com/workers/) untuk detail kemampuan yang tersedia saat ini.

Salah paham yang mahal adalah menganggap upload berhasil berarti rilis selesai. Rilis juga mencakup artefak yang benar, environment yang tepat, binding yang terpasang, sinyal kesehatan, serta jalan kembali bila perubahan bermasalah. Karena itu, keputusan permukaan deployment harus dibuat bersama rencana observability dan rollback, bukan dari nama produknya saja.

## Definisi dan batas objek

Dalam artikel ini, “Pages” berarti proyek yang berpusat pada source aplikasi web, perintah build, dan output aset. “Functions” berarti kode server-side yang dipanggil melalui pola aplikasi Pages. “Workers” berarti layanan edge yang menerima request dan menjalankan kode sesuai konfigurasi Worker. Detail implementasi seperti framework, cara binding, dan batas resource mengikuti dokumentasi serta akun yang digunakan.

Batasnya penting: artikel ini tidak mendesain arsitektur produk Codev, memilih database, atau menetapkan kontrak ketersediaan. SLO (service level objective) adalah sasaran internal untuk mengambil keputusan operasional, bukan janji uptime; [panduan SRE Google tentang SLO](https://sre.google/workbook/implementing-slos/) menjelaskan fungsi tersebut. Demikian pula, instrumentasi OpenTelemetry menghasilkan sinyal telemetry, bukan jaminan reliabilitas ([dokumentasi OpenTelemetry](https://opentelemetry.io/docs/)).

## Cara kerjanya

Alur Pages dimulai dari perubahan source: pipeline menjalankan build, menghasilkan aset, lalu deployment menyajikan hasil pada environment yang dituju. Bila ada Functions, request tertentu diteruskan ke kode server-side itu. Pemisahan ini membuat pertanyaan utama berada pada build reproducibility, variabel environment, dan kecocokan endpoint dengan batas runtime Functions.

Alur Workers berangkat dari request yang masuk ke Worker. Kode kemudian memanggil binding yang dikonfigurasi—misalnya layanan penyimpanan atau service lain—dan mengembalikan response. Build dapat menjadi bagian dari pipeline, tetapi objek operasionalnya adalah versi Worker beserta konfigurasi dan binding-nya.

Untuk kedua jalur, tetapkan urutan yang dapat diaudit: source commit, hasil build, konfigurasi environment, verifikasi smoke test, pengamatan sinyal, lalu keputusan promosi. Dokumentasi [versions dan deployments Workers](https://developers.cloudflare.com/workers/configuration/versions-and-deployments/) berguna ketika Anda memerlukan versi yang jelas, deployment bertahap, atau rollback yang dapat ditelusuri. Jangan menganggap fitur yang terlihat di dashboard otomatis tersedia dengan cara yang sama di semua akun.

## Faktor yang mengubah hasil

**Bentuk request.** Situs dokumentasi, landing page, atau frontend hasil build biasanya memulai dari Pages. API yang memiliki routing, middleware, atau transformasi request kompleks cenderung lebih mudah diperlakukan sebagai Worker mandiri. Jika fungsi dinamis hanya pelengkap halaman, menjaga semuanya dalam satu proyek Pages dapat mengurangi koordinasi; jika fungsi menjadi pusat produk, pemisahan Worker memberi batas operasi yang lebih tegas.

**Build dan runtime.** Catat perintah build, artefak yang dihasilkan, dependensi native, dan API runtime yang dipanggil. “Berjalan di lokal” tidak membuktikan kompatibilitas edge. Uji pada environment yang setara dan simpan log versi dependency. Untuk batas waktu eksekusi, ukuran bundle, atau kuota, jangan menyalin angka dari contoh lama; lakukan pemeriksaan akun sebagai bagian dari GATE-07.

**Bindings dan rahasia.** Binding bukan sekadar environment variable. Ia menghubungkan kode dengan resource tertentu, sehingga nama, izin, dan environment harus diverifikasi bersama. Rahasia untuk preview tidak boleh diasumsikan sama dengan production. Pisahkan nilai konfigurasi, rotasi, dan prosedur pencabutan akses dari source code.

**Observability.** Tentukan indikator yang menjawab apakah request penting berhasil: error rate, latency, serta gejala dependency gagal. OpenTelemetry membantu mengirim traces, metrics, dan logs ke backend yang dipilih, tetapi Anda tetap harus menetapkan sampling, retensi, dan siapa yang merespons alarm. SLO yang disepakati memberi ambang untuk menghentikan promosi atau memulai mitigasi.

**Respons insiden dan kapasitas.** [NIST SP 800-61 Rev. 3](https://csrc.nist.gov/pubs/sp/800/61/r3/final) menempatkan persiapan, deteksi, respons, dan pembelajaran sebagai siklus. Terapkan siklus itu pada Pages maupun Workers: siapkan pemilik layanan, bukti yang dikumpulkan, jalur komunikasi, dan latihan pemulihan. Biaya dan kapasitas harus diukur dari pola request nyata, bukan dari asumsi bahwa edge selalu tanpa batas.

## Contoh keputusan praktis

Gunakan tabel ini sebagai titik awal, lalu uji asumsi pada repository dan akun Anda.

| Situasi | Permukaan awal | Alasan yang perlu diuji |
| --- | --- | --- |
| Frontend statis dengan build rutin dan sedikit endpoint | Pages, Functions bila perlu | Fokusnya artefak build; pastikan endpoint cocok dengan runtime dan binding yang tersedia. |
| API edge menjadi produk utama dengan routing dan beberapa binding | Worker mandiri | Versi, konfigurasi, dan rollback dapat dikelola sebagai layanan request. |
| Frontend dan API berubah bersama tetapi tim kecil | Pages + Functions | Satu alur build dapat mengurangi koordinasi, selama batas runtime dan observability memadai. |
| Migrasi dari aplikasi server tradisional | Mulai dari komponen yang paling stateless | Petakan dependency, sesi, background job, dan fallback; jangan memindahkan seluruh aplikasi sekaligus. |

Misalnya, tim memiliki frontend hasil build dan endpoint form sederhana. Mereka dapat memulai Pages, menambahkan Functions, lalu mengukur error dan latency endpoint tersebut. Jika kemudian routing API, binding, atau siklus rilis berkembang melampaui batas yang disepakati, ekstraksi ke Worker menjadi keputusan berbasis bukti. Ini contoh bersyarat, bukan klaim bahwa satu pola selalu unggul.

Kawan Codev.id, sebelum menyetujui pilihan, minta empat artefak: diagram alur request, daftar binding per environment, catatan hasil smoke test, dan prosedur rollback. Jika salah satunya belum ada, keputusan masih berupa hipotesis.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah memilih berdasarkan label “static” atau “serverless” tanpa memetakan request. Tulis daftar route, siapa yang memanggilnya, dependency yang disentuh, dan response yang diharapkan. Tandai route yang membutuhkan sesi, streaming, atau pekerjaan panjang untuk peninjauan kompatibilitas.

Kesalahan kedua adalah menyamakan preview dengan production. Periksa source commit, environment variable, binding, domain, dan izin pada kedua environment. Jalankan smoke test yang sama setelah promosi dan simpan hasilnya.

Kesalahan ketiga adalah tidak menguji rollback. Buat rilis kecil yang bisa dibedakan, catat versi yang sedang aktif, lalu lakukan latihan kembali ke versi sebelumnya pada jendela aman. Fitur deployment dan versi harus dibaca dari [dokumentasi resmi Cloudflare](https://developers.cloudflare.com/workers/configuration/versions-and-deployments/), sedangkan langkah aktual mengikuti pipeline Anda.

Kesalahan keempat adalah menambah telemetry tanpa aturan tindakan. Untuk setiap alarm, tulis ambang, pemilik, langkah mitigasi, dan bukti penutupan. Jika tidak ada keputusan yang dipicu oleh sinyal itu, evaluasi kembali apakah telemetry tersebut perlu dipertahankan.

## Jalan pintas yang perlu dihindari

Shortcut yang sering dipilih adalah “deploy dulu ke Pages, nanti semua masalah runtime dibereskan”. Ini dapat gagal ketika endpoint membutuhkan API atau binding yang berbeda, ketika secret preview terbawa ke production, atau ketika tidak ada versi yang bisa dikembalikan. Alternatif yang lebih aman adalah membuat matriks route–runtime–binding sebelum deploy, memilih satu alur kecil untuk uji, memasang sinyal dasar, dan mendokumentasikan rollback. Dengan begitu, migrasi tetap bertahap tanpa menyamarkan ketidakpastian.

## Kesimpulan operasional

Pilih Pages ketika build dan aset web adalah pusat layanan, lalu gunakan Functions untuk dinamika yang masih dekat dengan situs. Pilih Workers ketika request handler, binding, runtime, dan lifecycle deployment perlu berdiri sebagai layanan edge yang terkontrol. Tidak ada jawaban final sebelum Anda memverifikasi batas akun, kompatibilitas runtime, konfigurasi environment, observability, dan rollback—itulah bagian GATE-07 yang harus ditutup.

Teman Codev.id, langkah berikutnya adalah mengisi matriks route–runtime–binding untuk satu release kecil, menetapkan SLO dan alarm yang dapat ditindaklanjuti, kemudian meminta technical review atas hasil uji serta prosedur kembali. Jika Anda masih menyusun situs dan butuh bantuan implementasi, lihat [layanan web development](/web-development/); bila aplikasi juga mengandalkan monetisasi konten, [panduan web Google AdSense](/web-google-adsense/) dapat menjadi konteks lanjutan. Aturan operasionalnya sederhana: jangan mempromosikan deployment yang tidak punya versi teridentifikasi, sinyal kesehatan, dan jalan rollback yang sudah diuji.
