---
article_id: CDV-11-A03
writing_contract_version: "native-id-v2"
title: "CI/CD dengan Quality Gate dan Provenance Build"
slug: "cicd-quality-gate-provenance-build"
description: "Panduan jalur berulang dari sumber yang direview menuju deployment dengan pemeriksaan kualitas, jejak artefak, persetujuan, bukti, dan penanganan kegagalan"
status: draft
publication_date: "2025-12-07"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-11
primary_intent: "Build a repeatable path from reviewed source to deployment"
reader_community: "Codev.id"
reader_address: "Kawan Codev.id"
final_route: "/artikel/cicd-quality-gate-provenance-build.html"
technical_review: required
sources:
  - "https://developers.cloudflare.com/pages/"
  - "https://developers.cloudflare.com/workers/"
  - "https://developers.cloudflare.com/workers/configuration/versions-and-deployments/"
  - "https://sre.google/workbook/implementing-slos/"
  - "https://opentelemetry.io/docs/"
  - "https://csrc.nist.gov/pubs/sp/800/61/r3/final"
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

# CI/CD dengan Quality Gate dan Provenance Build

Halo, Kawan Codev.id! Jika tim masih mengunggah berkas secara manual, pertanyaan terpenting bukan “alat CI/CD apa yang dipakai?”, melainkan “bukti apa yang membuat sebuah build boleh bergerak ke lingkungan berikutnya?”. CI/CD yang dapat diulang menghubungkan perubahan sumber yang sudah ditinjau dengan pemeriksaan otomatis, artefak yang dapat ditelusuri, persetujuan, dan prosedur ketika pemeriksaan gagal.

Jawaban singkatnya: susun pipeline sebagai rangkaian gerbang, bukan tombol deploy. Trigger harus berasal dari perubahan yang terlindungi; dependency dikunci dan dibangun di lingkungan yang konsisten; tes dan pemeriksaan kualitas menghentikan alur bila gagal; artefak diberi identitas serta asal-usul (provenance); lalu manusia atau aturan rilis yang berwenang memutuskan promosi. Dokumentasi Cloudflare menjelaskan kemampuan deployment pada Pages serta versi dan deployment pada Workers, tetapi keberhasilan unggah saja tidak membuktikan seluruh alur rilis. ([Cloudflare Pages](https://developers.cloudflare.com/pages/), [versi dan deployment Workers](https://developers.cloudflare.com/workers/configuration/versions-and-deployments/))

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.

## Jawaban singkat dan salah paham utama

Quality gate adalah syarat yang harus terpenuhi sebelum tahap berikutnya dijalankan. Ia dapat berupa status review, hasil lint, unit test, pemeriksaan dependency, atau validasi konfigurasi. Provenance build adalah catatan yang menghubungkan artefak dengan commit, definisi pipeline, lingkungan build, dan hasil pemeriksaannya. Keduanya menjawab dua risiko berbeda: gate mencegah keluarnya perubahan yang belum layak, sedangkan provenance membantu membuktikan apa yang sebenarnya dibuat.

Salah paham yang sering mahal adalah menganggap “pipeline hijau” sama dengan “rilis aman”. Pipeline hanya menilai aturan yang benar-benar dikonfigurasi. Jika branch dapat ditimpa, secret tidak dipisahkan, atau artefak dibangun ulang saat deploy, tanda hijau kehilangan makna. Kondisi yang dapat mengubah keputusan adalah konfigurasi akun, izin repository, kebutuhan compliance, serta bukti operasi nyata. Untuk paket ini, detail tersebut belum tersedia sehingga perlu pemeriksaan teknis bertanda **[NEEDS GATE-07 REVIEW: verifikasi konfigurasi akun, izin, secret, runtime, dan batas operasional sebelum klaim kesiapan produksi]**.

## Definisi dan batas objek

CI (continuous integration) berarti perubahan sering digabung dan diperiksa dalam jalur otomatis. CD (continuous delivery/deployment) meneruskan artefak yang lolos menuju lingkungan rilis; “delivery” tidak selalu berarti produksi otomatis. Artikel ini membahas jalur dari source yang telah direview sampai deployment dan bukti setelahnya. Ia tidak memilih vendor CI tertentu dan bukan aturan bahwa setiap rilis harus otomatis.

Quality gate berada di titik keputusan: sebelum merge, sebelum pembuatan artefak final, dan sebelum promosi. Provenance bukan jaminan kualitas atau keamanan; ia adalah jejak yang memungkinkan tim menjawab “sumber mana, dengan dependency apa, dibangun kapan, oleh proses apa, dan diuji bagaimana?”. Pengukuran layanan juga bukan kontrak uptime. SRE menjelaskan SLO sebagai tujuan layanan dan mekanisme keputusan, bukan janji publik tanpa bukti operasi. ([Google SRE Workbook](https://sre.google/workbook/implementing-slos/))

## Cara kerjanya

Mulailah dari trigger yang dapat diaudit. Pull request menuju branch terlindungi memicu pemeriksaan; merge hanya boleh terjadi bila review dan gate yang diwajibkan berstatus lulus. Pipeline lalu mengambil dependency dari sumber yang disetujui, menggunakan lockfile atau mekanisme setara, menjalankan build yang deterministik, dan menyimpan checksum atau identitas artefak.

Urutan praktisnya dapat diringkas seperti ini:

1. **Trigger dan identitas:** catat repository, commit, branch, pelaku, dan waktu. Batasi trigger manual dengan izin yang jelas.
2. **Persiapan:** pasang toolchain yang disepakati, gunakan dependency terkunci, dan injeksikan secret hanya saat diperlukan. Secret tidak boleh ditulis ke log.
3. **Build dan pemeriksaan:** jalankan lint, static check, unit/integration test, serta pemeriksaan dependency. Gate harus gagal secara eksplisit, bukan sekadar memberi peringatan yang diabaikan.
4. **Artefak:** hasilkan satu artefak yang diberi versi atau digest. Promosi memakai artefak yang sama, bukan build ulang dengan input berbeda.
5. **Provenance dan persetujuan:** simpan metadata sumber, langkah build, hasil gate, dan siapa yang menyetujui. Pisahkan hak membuat artefak dari hak mempromosikannya bila risiko memerlukannya.
6. **Deploy dan verifikasi:** gunakan kemampuan environment dan deployment platform yang dipilih; dokumentasi Workers menjelaskan konsep versi dan deployment yang perlu disesuaikan dengan akun nyata. ([Cloudflare Workers](https://developers.cloudflare.com/workers/))
7. **Sinyal pascadeploy:** periksa log, metrik, dan trace. OpenTelemetry menyediakan dokumentasi untuk menghasilkan dan mengumpulkan sinyal telemetry, tetapi instrumentasi sendiri tidak membuktikan reliability. ([OpenTelemetry](https://opentelemetry.io/docs/))

Jika verifikasi pascadeploy menemukan dampak, hentikan promosi berikutnya, pertahankan bukti, dan aktifkan prosedur respons insiden. NIST SP 800-61 Rev. 3 memberi kerangka respons dan pembelajaran; detail peran, jalur eskalasi, dan waktu tanggap tetap harus ditetapkan oleh organisasi. ([NIST SP 800-61 Rev. 3](https://csrc.nist.gov/pubs/sp/800/61/r3/final))

## Faktor yang mengubah hasil

Pertama, **perlindungan sumber**: aturan merge, review, dan izin token menentukan apakah pipeline memeriksa kode yang benar. Kedua, **reproducibility**: versi runtime, dependency, image builder, dan konfigurasi yang berubah diam-diam dapat menghasilkan artefak berbeda. Ketiga, **pemisahan environment**: preview, staging, dan production memerlukan variabel serta secret yang tepat; jangan menyalin secret production ke log atau preview.

Keempat, **kualitas gate**: tes yang tidak mewakili jalur risiko hanya menciptakan rasa aman. Tetapkan pemilik setiap gate, ambang gagal, dan masa berlaku pengecualian. Kelima, **bukti operasi**: dashboard, alert, dan trace harus menjawab apakah layanan sehat setelah deployment. SLO membantu memilih kapan sebuah sinyal memerlukan tindakan, sementara biaya penyimpanan telemetry dan kapasitas perlu dikendalikan.

Kawan Codev.id, jangan mengisi kekosongan bukti dengan asumsi “platform pasti mengurusnya”. Periksa izin repository, retention log, lokasi data, kompatibilitas runtime, limit, dan konfigurasi akun aktual sebelum menjadikan pipeline sebagai kontrol produksi. **[NEEDS GATE-07 REVIEW]**

## Contoh keputusan praktis

Anggap tim memiliki pull request untuk perubahan aplikasi web. Bila review belum lengkap, gate merge berhenti. Bila unit test lulus tetapi dependency scan gagal, artefak produksi tidak dibuat; tim memperbaiki dependency atau mendokumentasikan pengecualian yang disetujui. Bila semua gate lulus, artefak diberi digest dan dipromosikan ke staging. Setelah sinyal pascadeploy stabil sesuai kriteria layanan, pemilik rilis dapat menyetujui promosi berikutnya.

| Kondisi | Keputusan | Bukti minimum |
|---|---|---|
| Commit belum direview | Jangan merge | Status review dan identitas commit |
| Build lulus, provenance tidak lengkap | Tahan artefak | Metadata sumber dan langkah build |
| Test lulus, health check pascadeploy gagal | Hentikan promosi; respons insiden | Log, metrik/trace, waktu kejadian |
| Semua gate lulus, tetapi konfigurasi production belum diverifikasi | Jangan klaim siap produksi | Catatan **GATE-07** dan review teknis |

Contoh ini adalah pola keputusan, bukan laporan proyek atau jaminan hasil. Ambang test, definisi sehat, serta siapa yang berwenang menyetujui harus berasal dari kebutuhan layanan Anda.

## Kesalahan umum dan cara memeriksanya

Shortcut pertama adalah menjalankan build ulang di server deploy. Tanyakan apakah digest artefak sama dengan yang diuji; jika tidak, provenance terputus. Shortcut kedua adalah menyimpan secret sebagai variabel bebas tanpa audit. Periksa siapa yang dapat membaca, memutar, dan mencabutnya. Shortcut ketiga adalah menjadikan warning sebagai gate semu. Pastikan status gagal menghentikan tahap berikutnya dan memiliki jalur pengecualian yang tercatat.

Shortcut keempat adalah menganggap rollback selalu aman. Rollback aplikasi dapat berinteraksi dengan perubahan skema data atau dependency eksternal; siapkan keputusan roll-forward, kompatibilitas, dan verifikasi sebelum eksekusi. Shortcut kelima adalah memasang telemetry tanpa tujuan. Tentukan sinyal yang diperlukan untuk SLO dan respons, lalu tinjau retensi serta biaya.

Gunakan checklist singkat pada setiap perubahan:

- Apakah commit, pipeline, dependency, dan artefak memiliki identitas yang saling terhubung?
- Gate mana yang wajib lulus, siapa pemiliknya, dan apa bukti pengecualiannya?
- Apakah deployment memakai artefak yang sama dengan yang diuji?
- Sinyal apa yang memicu penghentian, eskalasi, atau rollback/roll-forward?
- Apakah konfigurasi akun dan runtime sudah diverifikasi, bukan diasumsikan?

## Jalan pintas yang tampak praktis

“Kami tim kecil; satu tombol deploy lebih cepat.” Memang lebih cepat pada satu kejadian, tetapi sulit dijelaskan ketika hasilnya berbeda atau insiden muncul. Jalur minimum tetap dapat ringkas: branch terlindungi, build terkunci, beberapa test relevan, digest artefak, satu persetujuan, dan verifikasi kesehatan. Otomatisasi boleh bertambah setelah bukti menunjukkan gate tersebut efektif. Jangan menukar kecepatan sesaat dengan hilangnya jejak keputusan.

## Kesimpulan dan langkah berikutnya

CI/CD dengan quality gate dan provenance build adalah jalur yang membuat perubahan teruji, artefak dapat ditelusuri, dan promosi dapat dihentikan dengan alasan yang terlihat. Mulai dengan memetakan trigger, gate, metadata artefak, pemilik persetujuan, dan sinyal pascadeploy dalam satu dokumen; kemudian uji satu perubahan melalui staging memakai konfigurasi akun nyata.

Teman Codev.id, minta technical review untuk mengisi **[NEEDS GATE-07 REVIEW]** sebelum menyebut jalur ini siap produksi. Untuk langkah awal dan konteks layanan, Anda dapat kembali ke [halaman utama Codev.id](/). Aturan operasionalnya sederhana: tidak ada promosi tanpa artefak yang sama dengan yang diuji, bukti provenance yang cukup, dan keputusan yang dapat dipertanggungjawabkan.
