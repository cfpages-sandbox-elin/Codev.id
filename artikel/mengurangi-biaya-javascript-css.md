---
article_id: CDV-14-A04
title: "Mengurangi Biaya JavaScript dan CSS"
slug: "mengurangi-biaya-javascript-css"
description: "Inventory code and third parties, measure transfer/parse/execute/render, remove or defer work, split responsibly, and guard regressions"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2026-02-19"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-14
primary_intent: "Diagnose and reduce front-end execution/render cost"
reader_community: "Codev.id"
reader_address: "Kawan Codev.id"
final_route: "/artikel/mengurangi-biaya-javascript-css.html"
technical_review: required
sources:
  - "https://sre.google/workbook/implementing-slos/"
  - "https://opentelemetry.io/docs/"
  - "https://csrc.nist.gov/pubs/sp/800/61/r3/final"
  - "https://web.dev/articles/vitals"
  - "https://developer.chrome.com/docs/crux"
  - "https://www.rfc-editor.org/rfc/rfc9111"
---

Halo, Kawan Codev.id!

# Mengurangi Biaya JavaScript dan CSS

Biaya JavaScript dan CSS bukan sekadar ukuran berkas yang diunduh. Pengguna membayar dengan waktu tunggu saat kode diunduh, diurai (parse), dijalankan, lalu dipakai untuk menggambar dan merespons interaksi. Karena itu, mengecilkan semua bundle sekaligus bukan keputusan awal yang aman. Mulailah dengan menemukan bagian yang benar-benar menghambat interaksi atau render pada halaman yang bermasalah, kemudian kurangi pekerjaan di jalur tersebut tanpa menghapus fungsi yang masih dibutuhkan.

Urutan yang paling dapat dipertanggungjawabkan adalah: inventarisasi kode dan pihak ketiga, ukur transfer–parse–execute–render pada kondisi yang sama, pilih pekerjaan yang bisa dihapus atau ditunda, pecah bundle hanya ketika batasnya jelas, lalu pasang pemeriksaan regresi. Hasil sebelum–sesudah harus menyebutkan halaman, versi, perangkat, jaringan, sampel, dan periode pengamatan; metrik Core Web Vitals terus berkembang dan tidak boleh dipakai untuk menjanjikan peringkat, waktu muat, energi, atau konversi tertentu ([web.dev](https://web.dev/articles/vitals), [Chrome UX Report](https://developer.chrome.com/docs/crux)).

<!-- BEGIN MANAGED IMAGE PLAN
## Image plan

- **Image ID:** `LOCAL-006`
- **Source type:** `local`
- **Placement:** after the opening has answered the main question, before the first detailed H2
- **Exact Markdown to insert:** `![Ilustrasi css](/wp-content/uploads/2022/11/css.jpg)`
- **Caption/credit:** Aset lokal proyek; jangan klaim sebagai dokumentasi proyek tertentu.
- **Selection basis:** filename/source metadata identifies `css` as relevant content media; no pixels were inspected.
- **Hard boundary:** do not infer or describe unseen visual details, project ownership, location, people, brands, condition, performance, or outcome.
- **Substitution rule:** do not replace this image. If unavailable or provenance is incomplete, insert `[NEEDS IMAGE REVIEW: LOCAL-006]` and continue drafting the prose.
END MANAGED IMAGE PLAN -->

![Ilustrasi css](/wp-content/uploads/2022/11/css.jpg)

Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.

## Mulai dari gejala, bukan tebakan penyebab

Catat gejalanya sebelum membuka alat profiling: apakah tombol lambat merespons, halaman tersendat ketika digulir, konten utama terlambat terlihat, atau hanya satu rute yang bermasalah? Tulis URL atau pola rute, waktu kejadian, perubahan rilis terakhir, jenis perangkat, jaringan, serta apakah gangguan terjadi pada semua pengguna atau segmen tertentu. “Bundle besar” adalah petunjuk, bukan diagnosis.

Buat inventaris sederhana untuk setiap halaman yang dipilih:

| Area | Yang dicatat | Pertanyaan keputusan |
| --- | --- | --- |
| Kode aplikasi | berkas, ukuran transfer, pemilik, jalur pemanggilan | Apakah fungsi ini dibutuhkan pada tampilan awal? |
| CSS | stylesheet, aturan yang dipakai, media query, dependensi | Apakah aturan ini khusus komponen atau global? |
| Pihak ketiga | domain, skrip, pemicu, data yang dikirim | Siapa pemiliknya dan apa syarat bisnisnya? |
| Render | perubahan layout, waktu tampil, respons input | Di tahap mana pengguna merasakan jeda? |

Pisahkan pengamatan dari dugaan. Contohnya, “interaksi pertama melewati target internal pada perangkat uji” adalah pengamatan dengan lingkup tertentu; “pustaka X penyebabnya” baru hipotesis yang harus diuji. Telemetri membantu membuat sinyal yang konsisten, tetapi keberadaan sinyal tidak otomatis membuktikan reliabilitas atau sebabnya. Prinsip ini sejalan dengan dokumentasi [OpenTelemetry](https://opentelemetry.io/docs/) tentang instrumentasi dan dengan pendekatan SLO sebagai sasaran pengambilan keputusan, bukan janji uptime kontraktual ([Google SRE Workbook](https://sre.google/workbook/implementing-slos/)).

## Saringan risiko langsung

Sebelum mengubah bundle produksi, pastikan Anda dapat mematikan perubahan atau mengembalikan versi sebelumnya. Batasi percobaan pada rute dan kelompok pengguna yang jelas, simpan artefak build, dan sepakati siapa yang berwenang menghentikan rollout. Jika skrip terkait pembayaran, autentikasi, persetujuan, aksesibilitas, atau fungsi keselamatan, jangan menonaktifkannya hanya karena ia besar; minta pemilik fungsi dan peninjau yang kompeten memeriksa dampaknya.

Kawan Codev.id, hentikan optimasi ketika data menunjukkan perilaku yang tidak dipahami—misalnya error meningkat setelah skrip ditunda, cache menyajikan kombinasi aset yang tidak serasi, atau perbedaan lab dan pengguna nyata terlalu lebar untuk dijelaskan. Perlakukan kejadian itu sebagai perubahan yang perlu ditangani: dokumentasikan deteksi, pembatasan dampak, pemulihan, dan pembelajaran. Kerangka [NIST SP 800-61 Rev. 3](https://csrc.nist.gov/pubs/sp/800/61/r3/final) dapat menjadi rujukan untuk alur respons insiden; artikel ini tidak menggantikan prosedur organisasi atau persetujuan profesional.

## Kemungkinan mekanisme

Kelompokkan penyebab yang mungkin agar percobaan tetap terarah:

1. **Transfer:** aset terlalu banyak, kompresi atau cache tidak efektif, atau pihak ketiga dipanggil pada semua halaman.
2. **Parse dan execute:** kode yang dikirim ke perangkat tidak diperlukan untuk tindakan awal, inisialisasi berjalan berulang, atau pekerjaan sinkron menahan thread utama.
3. **Render:** perubahan gaya memicu kerja layout dan paint lebih sering daripada yang diperlukan, atau CSS global membuat aturan sulit dipangkas.
4. **Urutan dependensi:** skrip penting menunggu skrip non-kritis, atau modul fitur yang belum terlihat ikut dimuat.
5. **Variasi pengguna:** kondisi jaringan, perangkat, versi browser, dan cache membuat dampak berbeda-beda.

Gunakan hipotesis bersyarat: “Jika skrip analitik dipindah setelah interaksi pertama, maka waktu execute di jalur awal harus turun tanpa menambah error pada pengukuran yang sama.” Jika prediksi itu tidak terjadi, kembalikan perubahan dan perbarui hipotesis. Jangan menyimpulkan bahwa pengurangan kilobyte pasti memperbaiki render; bagian yang dihapus bisa saja tidak berada di jalur kritis.

## Urutan pemeriksaan dan pengujian

Kerjakan dari observasi yang paling aman menuju perubahan yang lebih berisiko:

1. **Tetapkan baseline.** Bekukan commit, konfigurasi build, daftar pihak ketiga, rute, perangkat, jaringan, dan kondisi cache. Ambil beberapa pengukuran lab yang dapat diulang, lalu catat versi alatnya.
2. **Cocokkan dengan data lapangan.** Data pengguna nyata dari Chrome UX Report menunjukkan distribusi pengalaman, bukan reproduksi satu sesi ([dokumentasi CrUX](https://developer.chrome.com/docs/crux)). Jika data lapangan belum tersedia atau sampelnya tidak sebanding, tandai keterbatasannya.
3. **Pisahkan tahap biaya.** Di trace atau profiler, bedakan waktu unduh, parse, execute, style/layout, paint, dan respons input. Cari tugas yang dominan pada rute sasaran, bukan hanya berkas dengan nama terbesar.
4. **Uji pengurangan paling reversibel.** Hapus import yang benar-benar tidak terpanggil, hentikan pemanggilan pihak ketiga yang tidak disetujui, atau tunda pekerjaan non-kritis setelah kondisi pengguna terpenuhi. Setiap perubahan harus punya pemilik dan cara rollback.
5. **Uji pemecahan bundle.** Terapkan code splitting berdasarkan rute atau fitur yang jelas. Pastikan preloading, urutan modul, error boundary, dan navigasi ulang tetap diuji; pemecahan yang berlebihan dapat menambah request dan koordinasi.
6. **Uji cache dengan identitas aset.** Nama atau fingerprint aset harus berubah ketika isinya berubah, sementara aturan freshness dan validasi mengikuti semantik HTTP. [RFC 9111](https://www.rfc-editor.org/rfc/rfc9111) menjelaskan model cache HTTP; jangan menganggap header yang tampak “panjang” selalu aman untuk setiap strategi invalidasi.
7. **Bandingkan ulang.** Gunakan lingkup dan kondisi baseline. Simpan trace, ukuran transfer, error, dan hasil pemeriksaan fungsi bersama perubahan kodenya.

## Cara membaca hasil tanpa melompat ke kesimpulan

Buat tabel hasil yang memisahkan lima hal: apa yang diukur, kriteria internal, perubahan yang dicoba, mekanisme yang didukung data, dan keputusan pemilik produk. Contoh: transfer turun pada halaman katalog, tetapi waktu execute tetap sama; kesimpulan yang sah adalah penghematan jaringan pada kondisi uji, bukan otomatis interaksi lebih cepat.

Untuk metrik lapangan, tulis populasi, periode, versi halaman, perangkat, dan persentil yang dipakai. Jangan membandingkan skor lab dari alat atau konfigurasi berbeda lalu menyebutnya tren. Core Web Vitals adalah metrik yang definisi dan ambangnya dapat berubah; `[NEEDS GATE-08 REVIEW: verifikasi ulang definisi, ambang, versi alat, dan kecukupan sampel sebelum klaim sebelum–sesudah dipublikasikan]`.

Gunakan SLO internal sebagai pemicu tindakan: misalnya, kapan tim menyelidiki regresi, memperluas sampel, atau menghentikan rollout. SLO tidak membuktikan penyebab teknis dan bukan kontrak kinerja. Hubungkan dashboard ke log rilis dan jejak telemetri agar tim dapat menelusuri perubahan, tetapi tetap minta pemeriksaan manual untuk fungsi yang tidak tercakup otomatis.

## Pilihan tindakan dan titik eskalasi

Pilih tindakan berdasarkan temuan, bukan popularitas teknik:

- **Kontrol sementara:** matikan pemanggilan non-kritis dengan flag, batasi rollout, atau tunda fitur sampai baseline aman.
- **Perbaikan terarah:** kurangi import yang tak terpakai, pindahkan inisialisasi setelah kebutuhan nyata, dan pecah modul mengikuti batas rute.
- **Perbaikan CSS:** batasi cakupan aturan ke komponen yang memakainya, buang duplikasi setelah verifikasi visual dan aksesibilitas, lalu ukur ulang biaya style dan layout.
- **Perbaikan pihak ketiga:** minta justifikasi pemilik, jadwal pemanggilan, serta rencana penghapusan ketika tujuan bisnis berakhir.
- **Eskalasi:** libatkan pemilik keamanan, privasi, pembayaran, atau aksesibilitas jika perubahan dapat mengubah data, izin, atau pengalaman wajib.

Dokumentasikan keputusan dan kriteria rollback di tiket yang sama dengan perubahan. Instrumentasi, alert, dan review berkala membantu menjaga hasil, namun tidak membuktikan bahwa sistem selalu sehat; evaluasi kembali ketika rute, traffic, atau dependensi berubah ([OpenTelemetry](https://opentelemetry.io/docs/), [Google SRE Workbook](https://sre.google/workbook/implementing-slos/)).

## Jalan pintas yang sering gagal

Jalan pintas yang menggoda adalah menghapus seluruh library atau menunda semua JavaScript agar skor lab langsung terlihat lebih baik. Cara itu dapat memutus validasi formulir, navigasi keyboard, login, pelacakan yang diwajibkan kebijakan, atau komponen yang hanya muncul pada kondisi tertentu. Ia juga mengubah lebih banyak variabel daripada yang dapat dijelaskan, sehingga rollback dan pencarian sebab menjadi sulit.

Teman Codev.id, alternatif yang lebih aman adalah daftar dependensi berbasis fungsi: tandai pemilik, pemicu, kebutuhan waktu, data yang disentuh, dan cara menguji. Tunda hanya pekerjaan yang dapat ditunda menurut aturan produk, lalu buktikan dengan trace dan pemeriksaan fungsi. Jika suatu kode tidak dapat dibuktikan aman untuk dihapus, perlakukan ia sebagai pekerjaan review—bukan sampah yang boleh dibuang.

## Kesimpulan dan langkah berikutnya

Mengurangi biaya JavaScript dan CSS berarti mengurangi pekerjaan yang tidak perlu pada jalur pengguna, dengan bukti yang dapat diulang. Inventaris dulu, ukur transfer–parse–execute–render, ubah satu mekanisme pada satu waktu, pecah bundle sesuai batas fitur, dan lindungi hasil dengan telemetry, SLO, pengujian fungsi, serta rollback.

Langkah berikutnya: pilih satu rute yang paling sering dilaporkan lambat, simpan baseline dan daftar pihak ketiganya, lalu buat tiket eksperimen yang memuat hipotesis, sampel, kriteria berhenti, dan peninjau. Anda dapat melanjutkan pemeriksaan konteks situs melalui [halaman utama Codev.id](/), tetapi jangan menyatakan perbaikan umum sebelum data lapangan dan definisi metrik ditinjau ulang. Kawan Codev.id, aturan operasinya sederhana: tidak ada penghapusan atau penundaan permanen tanpa bukti fungsi tetap berjalan, jalur rollback yang diuji, dan review teknis untuk risiko yang tidak terukur.
