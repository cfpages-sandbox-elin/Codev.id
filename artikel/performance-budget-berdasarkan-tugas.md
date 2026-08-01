---
article_id: CDV-14-A01
title: "Performance Budget Berdasarkan Tugas Pengguna"
slug: "performance-budget-berdasarkan-tugas"
description: "Define critical tasks, representative devices/networks, resource and timing budgets, ownership, test point, exception, and regression gate"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2026-02-08"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-14
primary_intent: "Set measurable performance constraints before build"
reader_community: "Codev.id"
reader_address: "Kawan Codev.id"
final_route: "/artikel/performance-budget-berdasarkan-tugas.html"
technical_review: required
sources:
  - "https://sre.google/workbook/implementing-slos/"
  - "https://opentelemetry.io/docs/"
  - "https://csrc.nist.gov/pubs/sp/800/61/r3/final"
  - "https://web.dev/articles/vitals"
  - "https://developer.chrome.com/docs/crux"
  - "https://www.rfc-editor.org/rfc/rfc9111"
---

# Performance Budget Berdasarkan Tugas Pengguna

Halo, Kawan Codev.id! Performance budget yang berguna bukan angka “halaman harus selesai dalam sekian detik”. Budget yang dapat dipakai tim dimulai dari tugas pengguna: apa yang harus berhasil dilakukan, pada perangkat dan jaringan yang mewakili pengguna, dengan batas sumber daya dan waktu yang disepakati sebelum build dimulai.

Urutannya sederhana: tulis tugas kritis, pilih kondisi uji yang realistis, tetapkan batas untuk ukuran/permintaan serta momen interaksi, lalu beri pemilik, titik pengukuran, aturan pengecualian, dan gerbang regresi. Angka awal adalah hipotesis kerja, bukan jaminan ranking, konversi, energi, atau satu waktu muat universal. Ambang dan alat perlu ditinjau saat konteks, browser, atau data pengguna berubah [NEEDS GATE-08 REVIEW].

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

## Jawaban singkat dan salah paham utama

Mulailah dengan kalimat tugas, misalnya “pengunjung menemukan produk, membuka detail, lalu mengirim permintaan”. Dari kalimat itu, pilih momen yang menentukan keberhasilan: konten utama terlihat, kontrol dapat dipakai, dan konfirmasi terkirim. Setiap momen mendapat target yang dapat diukur serta kondisi uji yang ditulis.

Kesalahpahaman paling berbahaya adalah menganggap budget sebagai satu angka untuk semua halaman. Pengguna yang membaca artikel, mengisi formulir, dan memeriksa status transaksi memiliki jalur dan toleransi berbeda. Budget juga bukan laporan setelah rilis; ia adalah batas desain dan implementasi. Jika batas terlewati, keputusan harus terjadi sebelum perubahan masuk, bukan setelah keluhan muncul.

Core Web Vitals adalah metrik yang didefinisikan penyedianya dan dapat berkembang; ia membantu membaca pengalaman, tetapi tidak otomatis menjawab apakah tugas bisnis selesai [web.dev Core Web Vitals](https://web.dev/articles/vitals). Karena itu, metrik pengalaman dipasangkan dengan indikator tugas seperti “tombol kirim siap dipakai” atau “pesan sukses tampil”.

## Definisi dan batas objek

Performance budget adalah dokumen kendali yang menghubungkan empat hal: tugas pengguna, kondisi representatif, batas kinerja, dan respons ketika batas dilanggar. Batas dapat mencakup ukuran aset, jumlah permintaan, waktu menuju konten atau interaksi, serta reliabilitas alur. Pilih hanya yang punya hubungan sebab-akibat dengan tugas; daftar metrik yang panjang tanpa keputusan hanya menambah kebisingan.

Yang dibahas di sini adalah penetapan constraint sebelum build: siapa yang mengukur, kapan, dengan data apa, dan kapan perubahan dihentikan. Pengukuran lab dan field secara mendalam adalah pekerjaan lanjutan; data CrUX, misalnya, merepresentasikan pengalaman pengguna nyata dalam kumpulan data tertentu, bukan jaminan untuk setiap pengguna [Chrome UX Report](https://developer.chrome.com/docs/crux). Hasil kasus, harga, kapasitas, dan klaim sebelum-sesudah juga berada di luar halaman ini.

SLO (service level objective) dapat dipakai sebagai cara menyepakati tingkat layanan dan keputusan, bukan sebagai janji uptime kontraktual [Google SRE Workbook](https://sre.google/workbook/implementing-slos/). Untuk fitur web, terjemahkan prinsip itu menjadi sasaran alur: berapa proporsi percobaan yang mencapai konfirmasi dalam kondisi yang telah ditentukan. Jangan menulis persentase tanpa data operasi yang benar-benar dimiliki tim.

## Cara kerjanya

1. **Petakan tugas kritis.** Tulis pemicu, langkah, dan bukti selesai. Bedakan jalur utama dari halaman yang hanya informatif. Mintalah pemilik produk mengonfirmasi urutan ini.
2. **Pilih profil representatif.** Catat kelas perangkat, browser, ukuran layar, kondisi CPU, serta jaringan yang memang ingin dilayani. “Laptop cepat dan Wi-Fi kantor” hanya valid bila itu benar-benar populasi sasaran.
3. **Pisahkan batas sumber daya dan waktu.** Batas sumber daya mengendalikan byte, gambar, font, JavaScript, dan permintaan. Batas waktu mengendalikan kapan konten utama, kontrol, dan konfirmasi tersedia. Satu perubahan dapat memperbaiki satu batas sambil merusak yang lain.
4. **Tentukan titik ukur.** Ukur pada commit atau build yang jelas, dengan skrip dan data uji yang disimpan. Instrumentasi memberi sinyal, bukan reliabilitas dengan sendirinya; dokumentasi OpenTelemetry menjelaskan bagaimana telemetri dikumpulkan dan dikontekstualkan [OpenTelemetry documentation](https://opentelemetry.io/docs/).
5. **Tetapkan pemilik dan gerbang.** Pemilik frontend, platform, desain, dan produk perlu tahu batas yang mereka jaga. Pull request yang menambah aset besar atau langkah jaringan harus memicu pemeriksaan budget.
6. **Kelola pengecualian.** Pengecualian harus menyebut alasan tugas, durasi, pemilik, kompensasi, dan tanggal tinjau. Tanpa tanggal kedaluwarsa, pengecualian berubah menjadi budget baru yang tidak pernah disetujui.

Simpan keputusan dalam satu tabel yang dapat dibaca saat perencanaan:

| Tugas dan bukti selesai | Kondisi uji | Batas contoh yang disepakati | Titik ukur dan pemilik | Respons pelanggaran |
|---|---|---|---|---|
| Membuka detail dan melihat konten utama | Perangkat/jaringan profil A | Byte, permintaan, dan momen konten sesuai target tim | CI dan pemeriksaan release; pemilik frontend | Tahan merge, cari penyebab, atau ajukan pengecualian |
| Mengirim formulir dan menerima konfirmasi | Profil B, data valid | Kontrol siap dan konfirmasi tidak tertunda oleh pekerjaan nonkritis | Uji alur; pemilik fitur | Nonaktifkan perubahan penyebab atau pecah pekerjaan |

Angka pada kolom batas harus diisi tim berdasarkan kebutuhan dan pengukuran awal, bukan disalin dari contoh. RFC 9111 membantu menjelaskan semantik cache HTTP; gunakan pemahaman itu untuk menetapkan aturan cache yang konsisten, bukan untuk menjanjikan waktu respons tertentu [HTTP caching RFC 9111](https://www.rfc-editor.org/rfc/rfc9111).

## Faktor yang mengubah hasil

**Tugas dan konten.** Formulir dengan validasi berbeda memiliki titik selesai berbeda. Aset wajib untuk memahami produk tidak boleh diperlakukan seperti dekorasi yang dapat ditunda.

**Perangkat dan jaringan.** CPU, memori, latensi, kehilangan paket, dan kebijakan browser mengubah urutan pekerjaan. Profil harus cukup spesifik untuk mengungkap risiko, tetapi tidak begitu banyak hingga tidak pernah diuji.

**Implementasi dan platform.** Cache, kompresi, rendering sisi klien, pihak ketiga, dan API dapat menggeser bottleneck. Perubahan cache perlu ditinjau terhadap kontrol invalidasi dan umur respons sesuai kebijakan yang disepakati.

**Bukti dan operasi.** Lab terkontrol berguna untuk membandingkan commit; data field berguna untuk melihat variasi populasi. Jangan menyamakan keduanya. Rencana telemetri sebaiknya menjawab tindakan apa yang diambil ketika sinyal melewati batas. Praktik respons insiden NIST menekankan persiapan, deteksi, respons, dan pembelajaran; gunakan siklus itu untuk regresi kinerja tanpa mengklaim kepatuhan hukum tertentu [NIST SP 800-61 Rev. 3](https://csrc.nist.gov/pubs/sp/800/61/r3/final).

**Perubahan tujuan.** Ketika tugas utama, populasi pengguna, atau komponen pihak ketiga berubah, budget lama perlu ditinjau. [NEEDS GATE-08 REVIEW: ambang numerik, sampel, versi alat, dan kondisi produksi proyek ini belum diberikan.]

## Contoh keputusan praktis

Bayangkan tim merencanakan alur permintaan demo. Tugas kritisnya bukan “halaman beranda cepat”, melainkan “pengunjung menemukan formulir, mengisi tiga bidang, dan melihat konfirmasi”. Tim dapat membuat dua profil uji: perangkat dengan CPU terbatas dan jaringan latensi lebih tinggi, serta perangkat kerja dengan jaringan stabil. Profil itu adalah asumsi yang harus disetujui; bukan klaim tentang seluruh audiens.

Keputusan berikutnya: logo dan teks utama diperlukan untuk orientasi, sedangkan widget analitik tidak diperlukan untuk mengirim formulir. Aset wajib masuk budget jalur kritis; widget dimuat setelah bukti selesai atau dikeluarkan dari jalur. Jika perubahan desain menambah font baru, pemilik harus menunjukkan dampaknya pada byte, permintaan, dan momen kontrol siap.

Kawan Codev.id, gunakan aturan “gagal berarti berhenti sejenak”. Jika pemeriksaan CI melewati batas, jangan langsung menaikkan angka. Cari diff yang menyebabkannya, cek apakah profil uji masih sah, lalu pilih: optimalkan, pecah pekerjaan, tunda fitur nonkritis, atau ajukan pengecualian berjangka. Setelah pengecualian disetujui, buat tiket penghapusan dan pemiliknya.

## Kesalahan umum dan cara memeriksanya

- **Satu angka untuk semua tugas.** Tanyakan: bukti selesai tugas mana yang sebenarnya diukur?
- **Profil “rata-rata” yang tidak terdokumentasi.** Tanyakan: perangkat, browser, jaringan, data, dan versi build apa yang dipakai?
- **Metrik tanpa tindakan.** Tanyakan: siapa yang menerima alert, apa keputusan dalam satu siklus kerja, dan kapan kondisi dinyatakan pulih?
- **Menganggap telemetri sebagai bukti sebab.** Tanyakan: apakah perubahan, sampel, dan kondisi stabil sehingga perbandingan adil? Tanpa itu, tulis sebagai sinyal atau hipotesis.
- **Membiarkan pengecualian permanen.** Tanyakan: siapa pemiliknya dan kapan ditinjau ulang?
- **Menjadikan metrik sebagai janji bisnis.** Tanyakan: apakah ada data operasi dan kontrak yang mendukung? Jika tidak, jangan menyebut jaminan ranking, konversi, energi, atau uptime.

## Jalan pintas yang perlu ditolak

Shortcut yang sering dipilih adalah memakai ambang bawaan alat lalu menyatakan fitur “lulus”. Alat memang membantu konsistensi, tetapi ambang generik tidak mengetahui tugas, perangkat, atau biaya kegagalan Anda. Teman Codev.id, alternatif yang lebih aman ialah mulai dari tugas kritis, catat profil dan versi alat, gunakan ambang awal sebagai hipotesis, lalu kalibrasikan dengan pengukuran lab dan field yang stabil. Setiap perubahan ambang harus memiliki alasan dan persetujuan pemilik, bukan sekadar menghilangkan alarm.

## Kesimpulan

Performance budget berdasarkan tugas pengguna berarti membatasi hal yang menentukan keberhasilan alur—sumber daya, waktu, dan reliabilitas—pada kondisi uji yang mewakili, dengan pemilik, titik ukur, pengecualian, dan gerbang regresi yang jelas. Ia mengarahkan keputusan sebelum build; bukan janji satu waktu muat atau hasil bisnis.

Langkah berikutnya: buka backlog fitur, pilih satu tugas kritis, lalu isi tabel budget bersama pemilik produk dan teknis. Simpan profil uji, skrip, serta alasan setiap angka. Untuk konteks awal dan pekerjaan lanjutan, Anda dapat mulai dari [beranda Codev.id](/). Tinjau ulang budget ketika tugas, populasi, atau alat berubah; sampai bukti proyek tersedia, perlakukan ambang sebagai hipotesis dan minta review teknis.
