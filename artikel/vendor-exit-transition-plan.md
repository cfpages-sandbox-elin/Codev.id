---
article_id: CDV-18-A06
title: "Vendor Exit dan Transition Plan Tanpa Kehilangan Operasi"
slug: "vendor-exit-transition-plan"
description: "Plan notice, roles, source/artifacts, accounts, credentials, data export/retention, knowledge transfer, open risks, support overlap, access revocation, verification, and closure"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2026-06-07"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-18
primary_intent: "Change provider or internal owner without losing continuity"
reader_community: "Codev.id"
reader_address: "Kawan Codev.id"
final_route: "/artikel/vendor-exit-transition-plan.html"
technical_review: required
sources:
  - "https://csrc.nist.gov/pubs/sp/800/218/final"
  - "https://spec.openapis.org/oas/v3.1.1.html"
  - "https://sre.google/workbook/implementing-slos/"
  - "https://opentelemetry.io/docs/"
  - "https://csrc.nist.gov/pubs/sp/800/61/r3/final"
  - "https://csrc.nist.gov/pubs/sp/800/161/r1/final"
  - "https://www.cisa.gov/securebydesign"
  - "https://www.gov.uk/guidance/the-technology-code-of-practice"
---

# Vendor Exit dan Transition Plan Tanpa Kehilangan Operasi

Halo, Kawan Codev.id! Vendor exit tidak aman jika hanya berupa email pemberitahuan dan tanggal akses diputus. Agar operasi tetap berjalan, perlakukan perpindahan penyedia atau owner internal sebagai perubahan terkendali: tetapkan kondisi akhir, pemilik keputusan, paket artefak yang harus diserahkan, masa dukungan tumpang tindih, serta bukti verifikasi sebelum akses lama dicabut.

Jawaban singkatnya: mulai dengan inventaris layanan dan kewajiban operasional, sepakati rencana transisi tertulis, lalu jalankan serah terima bertahap dengan titik kembali (rollback) yang jelas. Hak terminasi, masa pemberitahuan, dan retensi data tetap bergantung pada kontrak serta tinjauan legal; artikel ini tidak menentukan hak tersebut. [NEEDS CONTRACT/LEGAL REVIEW: termination, notice, and retention terms]

Urutan praktisnya adalah notice dan peran, inventaris source serta artefak, ekspor dan verifikasi data, pemindahan akun dan kredensial, transfer pengetahuan, dukungan overlap, pengujian penerimaan, kemudian revokasi akses dan penutupan. Satu checklist tidak cukup bila tidak ada pemilik yang menandatangani bukti pada setiap tahap.

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

## Definisikan kebutuhan sebelum meminta harga

Jangan mulai dengan pertanyaan “berapa biaya mengambil alih sistem ini?”. Mulailah dari keadaan operasi yang harus tetap tersedia setelah vendor lama berhenti. Tulis fungsi layanan, jam kritis, dependensi, antarmuka, data yang berpindah, batas pekerjaan, dan hasil penerimaan. Misalnya, “pesanan dapat diproses dan direkonsiliasi oleh owner baru” lebih dapat diuji daripada “sistem sudah di-handover”.

Buat lembar inventaris dengan kolom berikut:

- layanan dan proses bisnis yang didukung;
- owner bisnis, owner teknis, pengambil keputusan, serta jalur eskalasi;
- repository source, konfigurasi, pipeline, dokumentasi, tiket, log, dashboard, dan kontrak pihak ketiga;
- akun, secret, sertifikat, domain, subscription, dan lokasi penyimpanannya;
- format ekspor data, periode retensi yang disetujui, serta cara menguji kelengkapan ekspor;
- risiko terbuka, asumsi, tanggal dependensi, dan kondisi yang memicu rollback.

Sertakan tanggal notice sesuai kontrak, tetapi jangan menganggap tanggal itu sebagai tanggal pemutusan akses. Pisahkan “siap menerima” dari “vendor lama boleh dicabut”. Untuk setiap item, tunjuk seorang accountable (penanggung jawab akhir) dan seorang verifier (pemeriksa bukti). Jika keduanya orang yang sama, catat konflik tersebut dan minta pemeriksaan kedua.

Kawan Codev.id, kebutuhan juga harus menyatakan apa yang tidak termasuk. Contohnya, migrasi data historis dapat menjadi pekerjaan terpisah dari pemindahan operasi harian; perubahan fitur baru bukan otomatis bagian dari exit. Kejelasan ini mengurangi sengketa scope tanpa mengarang kuantitas, durasi, atau harga yang belum diukur.

## Buat penawaran benar-benar sebanding

Minta setiap calon penerima transisi mengisi template yang sama. Pecah penawaran menjadi fase discovery, akses dan ekspor, transfer pengetahuan, dukungan overlap, pengujian, serta closure. Untuk tiap fase, minta inklusi, eksklusi, asumsi, input dari organisasi, artefak keluaran, kriteria selesai, dan risiko yang masih tersisa. Panduan pengadaan teknologi pemerintah Inggris juga menekankan keputusan yang dapat dipertanggungjawabkan dan pengelolaan risiko sepanjang siklus hidup, bukan sekadar memilih angka awal ([Technology Code of Practice](https://www.gov.uk/guidance/the-technology-code-of-practice)).

Bandingkan setidaknya empat dimensi:

| Dimensi | Pertanyaan pembanding | Bukti yang diminta |
|---|---|---|
| Kontinuitas | Siapa memonitor layanan dan menerima eskalasi selama overlap? | Roster, jadwal, dan jalur kontak |
| Kepemilikan | Artefak apa yang benar-benar dapat diakses owner baru? | Daftar repository, akun, konfigurasi, dan hak akses |
| Verifikasi | Bagaimana membuktikan data dan fungsi tidak rusak? | Skrip/checklist uji, hasil, dan penanggung jawab |
| Risiko dan biaya | Apa yang terjadi bila akses, data, atau pengetahuan terlambat? | Asumsi, tarif kondisi khusus, dan rencana rollback |

Harga pembangunan terendah belum tentu biaya siklus hidup terendah; manajemen risiko rantai pasok NIST meminta organisasi memahami ketergantungan dan bukti pemasok secara lebih menyeluruh ([NIST SP 800-161 Rev. 1](https://csrc.nist.gov/pubs/sp/800/161/r1/final)). Jangan memberi bobot pada logo sertifikasi atau daftar klien sebagai bukti bahwa tim yang ditawarkan mampu menjalankan scope ini. Minta bukti yang langsung terkait artefak dan peran yang akan mereka ambil.

## Dokumen yang membuktikan hal berbeda

Serah terima sering gagal karena semua berkas disebut “dokumentasi” tanpa membedakan fungsinya. Pisahkan paket berikut:

1. **Source dan konfigurasi:** repository, riwayat perubahan, manifest dependensi, infrastructure-as-code, konfigurasi runtime, konfigurasi build/deploy. Spesifikasi OpenAPI dapat membantu merekam kontrak antarmuka, tetapi keberadaan spesifikasi tidak membuktikan integrasi masih berjalan ([OpenAPI Specification 3.1.1](https://spec.openapis.org/oas/v3.1.1.html)).
2. **Data dan bukti ekspor:** skema, pemetaan field, checksum atau rekonsiliasi jumlah, catatan kegagalan, serta lokasi salinan kerja dan salinan retensi. Retensi final tetap mengikuti kontrak, kebijakan organisasi, dan nasihat legal.
3. **Operasi:** runbook, dashboard, definisi alert, jadwal batch, dependensi eksternal, dan riwayat insiden. Telemetri menghasilkan sinyal untuk diamati, bukan jaminan reliabilitas; dokumentasikan apa yang diukur dan siapa yang merespons ([OpenTelemetry documentation](https://opentelemetry.io/docs/)).
4. **Akses:** daftar akun manusia dan layanan, pemilik, metode pemulihan, secret/sertifikat, expiry, serta bukti rotasi. Kirim kredensial melalui kanal yang disetujui, bukan menaruhnya di dokumen umum.
5. **Pengetahuan dan keputusan:** rekaman sesi, diagram yang diberi tanggal, keputusan arsitektur, daftar pertanyaan terbuka, known errors, dan batas dukungan. Sesi transfer pengetahuan harus menghasilkan tugas demonstrasi, bukan hanya slide.

Catat provenance: siapa membuat artefak, kapan diekspor, dari lingkungan mana, dan siapa memverifikasi. NIST SSDF menekankan ketertelusuran aktivitas dan hasil pengembangan; prinsip itu berguna saat menentukan apakah bukti build atau perubahan dapat dipercaya ([NIST SP 800-218](https://csrc.nist.gov/pubs/sp/800/218/final)).

## Pertanyaan wajib kepada penyedia

Ajukan pertanyaan yang memaksa jawaban operasional, bukan janji umum:

- Dalam 48 jam pertama setelah notice, siapa hadir, akses apa yang diperlukan, dan artefak apa yang langsung dikunci?
- Siapa pemilik setiap repository, cloud account, domain, pipeline, subscription, dan data export sebelum serta sesudah transisi?
- Bagaimana Anda menemukan secret yang tertanam, akun bersama, sertifikat yang akan kedaluwarsa, dan dependensi yang tidak terdokumentasi?
- Format ekspor apa yang dapat dibaca tanpa tool milik vendor? Bagaimana rekonsiliasi jumlah, checksum, dan sampel pemulihan dilakukan?
- Sesi demonstrasi apa yang harus dapat dilakukan owner baru tanpa bantuan vendor lama?
- Sinyal layanan apa yang dipantau selama overlap, siapa on-call, dan berapa lama jalur eskalasi tetap aktif? SLO adalah tujuan pengambilan keputusan layanan, bukan janji uptime universal ([Google SRE Workbook—Implementing SLOs](https://sre.google/workbook/implementing-slos/)).
- Risiko apa yang Anda belum dapat tutup, apa pemicunya, dan apa rencana rollback-nya?
- Bukti apa yang Anda serahkan untuk menyatakan fase selesai, dan siapa yang berwenang menolak penerimaan?

Jawaban harus masuk ke risk register dengan owner dan tanggal tinjauan. Jangan menerima “nanti kami dokumentasikan” tanpa format, lokasi, dan kriteria kelengkapan.

## Tanda bahaya dan biaya yang sering tersembunyi

Red flag pertama adalah vendor hanya menjanjikan transfer file, tetapi tidak menyebut akun, pipeline, alert, atau keputusan yang melekat pada layanan. Red flag kedua adalah ekspor diberikan dalam format yang hanya dapat dibuka oleh tool vendor. Red flag ketiga adalah penawaran mengasumsikan akses admin tersedia padahal owner akun tidak diketahui.

Biaya tersembunyi biasanya muncul sebagai waktu menunggu persetujuan akses, pekerjaan menemukan konfigurasi yang hilang, rework karena data tidak dapat direkonsiliasi, lisensi sementara untuk menjalankan lingkungan paralel, serta dukungan di luar jam yang disepakati. Tulis masing-masing sebagai asumsi yang dapat diuji; jangan mengubahnya menjadi angka sebelum data proyek tersedia. [NEEDS PROJECT EVIDENCE: effort, duration, license, and support cost]

Teman Codev.id, jangan menutup risiko hanya karena tanggal exit sudah dekat. Jika source belum dapat dibangun, owner belum menguasai pemulihan, atau data ekspor belum diuji, gunakan keputusan “lanjut terbatas”, “tunda pencabutan”, atau “rollback” dengan persetujuan pengambil risiko. CISA menempatkan keamanan sejak desain dan sepanjang siklus hidup, sehingga akses dan dependensi perlu dibenahi sebelum sistem dilepas dari pengelolaan lama ([CISA Secure by Design](https://www.cisa.gov/securebydesign)).

## Penerimaan, serah terima, dan keputusan akhir

Gunakan transisi bertahap, bukan satu tanggal serah terima:

1. **Notice dan persiapan:** konfirmasi ruang lingkup, peran, kanal eskalasi, freeze perubahan berisiko, dan daftar akses.
2. **Ekspor dan penguasaan:** salin source, konfigurasi, data, log yang disepakati, dan artefak operasi; owner baru memverifikasi integritas dan aksesnya.
3. **Demonstrasi dan overlap:** owner baru menjalankan deploy, pemulihan, rotasi secret, monitoring, dan respons insiden dengan vendor lama sebagai pendamping. Rekam hasil serta gap.
4. **Penerimaan:** accountable memeriksa kriteria fungsi dan bukti. Pengujian hanya membuktikan assertion, environment, build, dan data yang diuji; tidak ada ambang cakupan universal yang boleh diasumsikan. [NEEDS GATE-06 REVIEW: acceptance depth and release decision]
5. **Revokasi dan closure:** setelah penerimaan tertulis, cabut akses vendor lama, rotasi secret, tutup akun dan subscription yang tidak diperlukan, arsipkan bukti, serta tinjau risiko sisa.

Selama overlap, definisikan indikator layanan dan jalur respons. Instrumentasi dan alert tidak menggantikan latihan pemulihan; prosedur insiden perlu diuji dan dipelajari secara berkala ([NIST SP 800-61 Rev. 3](https://csrc.nist.gov/pubs/sp/800/61/r3/final)). Jangan klaim operasi 24/7 atau uptime tertentu tanpa bukti operasi dan kontrak. [NEEDS GATE-07 REVIEW: actual support coverage and service objective evidence]

Simpan paket closure: keputusan penerimaan, daftar item yang ditolak, risk register terbuka, bukti revokasi, log ekspor, notulen transfer, dan kontak untuk periode dukungan pasca-transisi bila memang disetujui. Hak penghapusan atau retensi data harus ditentukan melalui kontrak dan review berwenang, bukan diasumsikan dari selesainya checklist. [NEEDS GATE-09 REVIEW: procurement, responsibility allocation, and contract evidence]

## Jalan pintas yang tampak menarik

Jalan pintas yang sering dipilih adalah memutus vendor lama segera setelah source dan dump data diterima. Itu menghemat koordinasi sesaat, tetapi menghilangkan orang yang dapat menjelaskan alarm, keputusan arsitektur, atau pemulihan ketika verifikasi pertama gagal. Alternatif yang lebih aman adalah overlap terbatas dengan exit criteria: owner baru harus menyelesaikan demonstrasi dan menyetujui risiko terbuka sebelum akses lama dicabut. Jika overlap tidak mungkin, minta persetujuan eksplisit atas risiko, pemilik mitigasi, dan rencana eskalasi.

## Langkah berikutnya

Jawaban atas “Vendor Exit dan Transition Plan Tanpa Kehilangan Operasi” adalah membuat bukti perpindahan menjadi objek penerimaan, bukan sekadar memindahkan file. Dalam satu sesi kerja, buat inventaris layanan dan akun, tetapkan accountable serta verifier, lalu minta penawaran transisi dengan format yang sama. Setelah itu jadwalkan ekspor yang dapat direkonsiliasi, demonstrasi pemulihan, overlap, dan revokasi berurutan.

Mulai dari [halaman utama Codev.id](/) bila Anda perlu menyelaraskan konteks layanan sebelum menyusun paket kerja. Jangan menutup exit sampai bukti fungsi, data, akses, dukungan, dan risiko sisa ditandatangani pihak yang berwenang. Aturan operasinya sederhana: akses lama dicabut setelah penerimaan dan rotasi terverifikasi—bukan hanya setelah kalender mencapai tanggal exit.
