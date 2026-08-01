---
article_id: CDV-10-A05
writing_contract_version: "native-id-v2"
title: "User Acceptance Test dan Release Evidence"
slug: "user-acceptance-test-dan-release-evidence"
description: "Trace requirements to scenarios, roles/data, expected results, defects, retest, exceptions, evidence, and explicit acceptance decision"
status: draft
publication_date: "2025-11-20"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-10
primary_intent: "Verify agreed outcomes before business acceptance"
reader_community: "Codev.id"
reader_address: "Sobat Codev.id"
final_route: "/artikel/user-acceptance-test-dan-release-evidence.html"
technical_review: required
sources:
  - "https://csrc.nist.gov/pubs/sp/800/218/final"
  - "https://www.w3.org/TR/WCAG-EM/"
  - "https://spec.openapis.org/oas/v3.1.1.html"
  - "https://www.w3.org/TR/WCAG22/"
  - "https://www.w3.org/WAI/test-evaluate/preliminary/"
  - "https://web.dev/articles/vitals"
  - "https://developer.chrome.com/docs/crux"
---

# User Acceptance Test dan Release Evidence

Halo, Sobat Codev.id!

User Acceptance Test (UAT) bukan sekadar daftar centang bahwa tombol dapat diklik. UAT adalah pemeriksaan bersama: apakah alur yang disepakati benar-benar menghasilkan keluaran yang dibutuhkan pemilik bisnis, pada kondisi dan data yang sudah didefinisikan. Release evidence adalah jejak yang membuat keputusan itu dapat ditelusuri—siapa mencoba apa, dengan hasil apa, cacat mana yang masih terbuka, dan atas dasar apa seseorang menyetujui atau menahan rilis.

Jawaban singkatnya: terima rilis hanya ketika setiap kebutuhan bisnis penting memiliki skenario UAT, pemilik keputusan, data dan lingkungan yang jelas, hasil aktual yang terdokumentasi, serta keputusan eksplisit. Kelulusan UAT tidak menggantikan pengujian teknis, keamanan, kinerja, atau aksesibilitas. NIST menempatkan praktik pengembangan aman sebagai rangkaian kegiatan yang saling melengkapi, bukan satu bukti tunggal; karena itu, hasil UAT harus dibaca bersama pemeriksaan lain yang memang menjadi kewenangannya ([NIST SP 800-218 SSDF](https://csrc.nist.gov/pubs/sp/800/218/final)).

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

UAT berangkat dari kebutuhan yang dapat diamati oleh pengguna bisnis. Kebutuhan itu diterjemahkan menjadi skenario: prasyarat, peran, data uji, langkah, hasil yang diharapkan, hasil aktual, dan status. Release evidence memperluas catatan tersebut dengan versi build, lingkungan, waktu eksekusi, identitas pelaksana, lampiran, hubungan ke tiket cacat, hasil retest, pengecualian, dan keputusan akhir.

Batasnya penting. UAT menjawab “apakah solusi ini memenuhi proses dan hasil yang disepakati?” Ia tidak membuktikan bahwa seluruh kode aman, setiap endpoint tahan beban, atau semua kombinasi perangkat bebas masalah. Spesifikasi OpenAPI dapat membantu menyamakan kontrak endpoint dan contoh respons, tetapi keberadaan dokumen itu sendiri tidak membuktikan perilaku implementasi di lingkungan rilis ([OpenAPI Specification 3.1.1](https://spec.openapis.org/oas/v3.1.1.html)). Demikian pula, hasil pemindaian otomatis hanya menyatakan pemeriksaan yang diprogram dan sampel yang dijalankan.

Untuk aksesibilitas, rancang evaluasi berdasarkan cakupan halaman, alur, dan teknologi yang benar-benar dipakai. WCAG-EM menjelaskan proses penentuan cakupan, pemilihan sampel, evaluasi, dan pelaporan; satu scanner tidak dapat menggantikan pemeriksaan keyboard, fokus, semantik, formulir, pesan kesalahan, zoom, autentikasi, media, serta perilaku teknologi bantu ([WCAG-EM 1.0](https://www.w3.org/TR/WCAG-EM/); [WCAG 2.2](https://www.w3.org/TR/WCAG22/); [WAI Easy Checks](https://www.w3.org/WAI/test-evaluate/preliminary/)). UAT boleh menemukan hambatan pengguna, tetapi klaim konformansi memerlukan evaluasi dengan cakupan yang dinyatakan dan peninjauan yang sesuai.

## Cara kerjanya

Mulai dari matriks kebutuhan. Beri setiap kebutuhan ID yang stabil, pemilik bisnis, risiko jika gagal, dan kriteria penerimaan. Turunkan satu atau beberapa skenario yang dapat diulang. Skenario yang baik menyebutkan siapa yang bertindak, data apa yang digunakan, kondisi awal, langkah yang terlihat, serta hasil yang bisa diputuskan sebagai lulus atau gagal—bukan kata “berfungsi” tanpa definisi.

Tetapkan peran sebelum sesi dimulai. Pemilik proses mengonfirmasi makna hasil; tester menjalankan langkah dan merekam bukti; tim teknis menjelaskan build atau memperbaiki cacat; fasilitator menjaga lingkungan dan mencatat keputusan. Orang yang memberi persetujuan harus memiliki kewenangan yang disepakati, bukan sekadar orang yang kebetulan hadir.

Siapkan data dan lingkungan yang dapat dipertanggungjawabkan. Tandai akun, tanggal, zona waktu, konfigurasi, feature flag, dan integrasi yang dipakai. Hindari menyalin data produksi tanpa dasar dan pengamanan yang disetujui. Catat versi aplikasi dan commit/build sehingga kegagalan dapat dikaitkan dengan artefak yang tepat.

Pada eksekusi, rekam hasil aktual segera setelah langkah dilakukan. Lampiran dapat berupa tangkapan layar, ekspor transaksi, log yang sudah disanitasi, atau rekaman singkat—pilih bukti yang menjawab hasil, bukan sekadar memperbanyak berkas. Setiap kegagalan mendapat ID cacat, tingkat dampak yang disepakati, langkah reproduksi, dan hubungan balik ke skenario.

Retest harus menunjuk ke versi perbaikan dan mengulang kondisi yang relevan. Jika hasil berubah karena data, konfigurasi, atau dependensi, catat perubahan itu. Setelah retest, tandai apakah cacat ditutup, diterima sebagai pengecualian, atau masih menghalangi. NIST menekankan pentingnya menghubungkan kegiatan dan hasil agar risiko dapat ditelusuri; prinsip itu membantu release evidence tetap bermakna saat banyak tim terlibat ([NIST SP 800-218 SSDF](https://csrc.nist.gov/pubs/sp/800/218/final)).

Buat indeks bukti yang sederhana. Setiap berkas mendapat nama yang memuat ID skenario dan versi build, sementara catatan utama menyimpan tautan ke lokasi berkas, tanggal, dan pemiliknya. Jika bukti memuat data sensitif, simpan hanya salinan tersanitasi untuk pembaca keputusan dan batasi akses ke artefak mentah. Dengan begitu, auditor internal dapat mengulang penelusuran tanpa memperluas paparan data atau mengandalkan ingatan peserta sesi.

Terakhir, buat keputusan tertulis: diterima, diterima dengan pengecualian, atau ditahan. Sertakan ruang lingkup yang disetujui, cacat terbuka, pemilik mitigasi, tenggat atau pemicu peninjauan ulang, serta tanda persetujuan. Jangan mengubah “belum diuji” menjadi “lulus” hanya karena jadwal rilis mendesak.

## Faktor yang mengubah hasil

Hasil UAT berubah ketika kebutuhan atau proses bisnis berubah. Skenario pembayaran dengan mata uang, peran, atau aturan persetujuan berbeda bukan pengulangan yang setara. Perubahan tersebut harus memicu peninjauan ulang prasyarat, data, dan hasil yang diharapkan.

Lingkungan juga menentukan arti bukti. Perbedaan versi browser, layanan pihak ketiga, konfigurasi cache, feature flag, atau hak akses dapat menghasilkan perilaku berbeda. Jika bukti berasal dari lingkungan staging, nyatakan batas generalisasinya; jangan menyebutnya bukti produksi tanpa verifikasi.

Untuk klaim performa, pisahkan pengukuran laboratorium dari data lapangan. Core Web Vitals adalah metrik yang didefinisikan penyedia dan dapat berevolusi; perbandingan sebelum-sesudah memerlukan ruang lingkup, sampel, kondisi, versi, dan caveat yang stabil ([Core Web Vitals](https://web.dev/articles/vitals)). Data Chrome UX Report menggambarkan pengalaman pengguna yang dikumpulkan dari kondisi tertentu, bukan jaminan untuk setiap pengguna atau setiap rilis ([Chrome UX Report](https://developer.chrome.com/docs/crux)). Karena paket ini tidak menetapkan ambang universal, tuliskan [NEEDS GATE-06 REVIEW: ambang dan kriteria penerimaan harus disepakati pemilik proyek] bila angka atau batas performa menentukan keputusan.

Kawan Codev.id, perhatikan juga pengecualian aksesibilitas dan keamanan. UAT dapat menunjukkan bahwa alur tertentu sulit digunakan dengan keyboard atau pesan error tidak terbaca, namun keputusan konformansi dan risiko keamanan membutuhkan pemeriksa serta bukti yang berwenang. Catat temuan itu sebagai kondisi rilis, bukan menutupnya dengan label “minor” tanpa alasan.

## Contoh keputusan praktis

Bayangkan kebutuhan: petugas dapat menyetujui pengajuan, pemohon menerima status, dan auditor dapat menelusuri perubahan. Matriks keputusannya dapat diringkas seperti ini:

| Kondisi bukti | Keputusan yang masuk akal | Tindak lanjut |
| --- | --- | --- |
| Semua skenario penting lulus pada build yang akan dirilis; bukti, pelaksana, dan ruang lingkup lengkap | Diterima | Simpan paket bukti dan catat versi rilis |
| Alur utama lulus, tetapi ada cacat berdampak terbatas dengan mitigasi dan pemilik yang jelas | Diterima dengan pengecualian | Tulis alasan, batas dampak, pemilik, serta pemicu review |
| Skenario kritis gagal, data tidak dapat direproduksi, atau persetujuan tidak berwenang | Ditahan | Perbaiki prasyarat atau cacat, lalu ulangi UAT |
| UAT lulus, tetapi pemeriksaan keamanan, aksesibilitas, atau teknis wajib belum selesai | Jangan nyatakan release gate lengkap | Tunggu pemeriksaan yang memang berada di luar UAT |

Contoh ini tidak menetapkan ambang universal. Nilai “kritis”, toleransi pengecualian, dan siapa yang boleh menyetujui harus berasal dari kesepakatan proyek. Jika belum ada, sisakan keputusan terbuka dan minta pemilik produk menetapkannya sebelum tanda tangan.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah menguji happy path saja. Tambahkan pertanyaan tentang pembatalan, data kosong, duplikasi, izin berbeda, kegagalan integrasi, dan pemulihan. Kesalahan kedua adalah mencampur hasil dari beberapa build. Periksa nomor build pada setiap bukti dan pastikan retest mengarah ke versi yang benar.

Kesalahan ketiga adalah bukti tanpa konteks. Tangkapan layar tanpa peran, data, waktu, atau expected result sulit diaudit. Gunakan templat ringkas: ID kebutuhan, ID skenario, aktor, prasyarat, data, langkah, expected, actual, status, bukti, defect, retest, dan catatan pengecualian.

Kesalahan keempat adalah menyamakan UAT dengan persetujuan teknis. Tanyakan secara terpisah: apakah pemeriksaan keamanan sudah ditandatangani? Apakah cakupan aksesibilitas dan perangkat telah dievaluasi? Apakah perubahan caching atau konfigurasi memerlukan pengukuran ulang? Dokumentasi UAT tidak boleh menjawab “ya” untuk pertanyaan yang tidak diuji.

Sebelum meminta tanda tangan, buka halaman ringkasan keputusan internal [Codev.id](/). Pastikan pembaca berikutnya dapat menemukan artefak yang sama, bukan hanya pesan singkat di chat.

## Jalan pintas yang tampak cepat tetapi berisiko

Shortcut yang sering dipilih adalah meminta product owner membalas “approve” setelah melihat demo. Demo menunjukkan alur yang dipilih presenter, bukan seluruh skenario, data, pengecualian, atau versi yang akan dirilis. Balasan singkat juga tidak menyatakan cacat terbuka dan batas persetujuan.

Alternatif yang lebih aman tetap ringan: kirim matriks kebutuhan-skenario, ringkasan hasil, daftar pengecualian, tautan bukti, versi build, dan satu kalimat keputusan dengan nama serta peran pemberi persetujuan. Jika ada yang belum diuji, tulis “belum diuji” dan konsekuensinya. Transparansi ini memudahkan keputusan ditinjau ulang tanpa menafsirkan ulang percakapan lama.

## Penutup

UAT dan release evidence menjawab pertanyaan yang sama dari dua sisi: apakah hasil bisnis yang disepakati tercapai, dan dapatkah keputusan itu dibuktikan kembali? Hubungkan setiap kebutuhan ke skenario, data, pelaksana, expected dan actual result, cacat, retest, pengecualian, serta keputusan eksplisit. Setelah itu, minta pemeriksaan teknis, keamanan, aksesibilitas, dan gate rilis yang memang berada di luar UAT.

Langkah berikutnya adalah membuat satu lembar keputusan untuk build yang akan dirilis, mengisi semua kolom yang masih kosong, lalu meminta pemilik bisnis menandatangani status diterima, diterima dengan pengecualian, atau ditahan. Teman Codev.id, jangan jadikan tanda tangan sebagai pengganti bukti: operating rule-nya sederhana—tidak ada keputusan rilis yang lebih luas daripada ruang lingkup dan bukti yang benar-benar diuji.
