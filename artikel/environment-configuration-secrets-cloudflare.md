---
article_id: CDV-11-A02
title: "Environment, Configuration, dan Secrets dari Preview ke Production"
slug: "environment-configuration-secrets-cloudflare"
description: "Define ownership, variables/secrets, bindings, test data, access, promotion, drift checks, audit, and rotation"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2025-12-04"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-11
primary_intent: "Separate environments and configuration safely"
reader_community: "Codev.id"
reader_address: "Kawan Codev.id"
final_route: "/artikel/environment-configuration-secrets-cloudflare.html"
technical_review: required
sources:
  - "https://developers.cloudflare.com/pages/"
  - "https://developers.cloudflare.com/workers/"
  - "https://developers.cloudflare.com/workers/configuration/versions-and-deployments/"
  - "https://sre.google/workbook/implementing-slos/"
  - "https://opentelemetry.io/docs/"
  - "https://csrc.nist.gov/pubs/sp/800/61/r3/final"
---

Halo, Kawan Codev.id!

# Environment, Configuration, dan Secrets dari Preview ke Production

Memindahkan aplikasi dari preview ke production bukan sekadar menekan tombol deploy. Keputusan yang aman adalah memisahkan environment, configuration, dan secrets sebagai tiga hal yang punya pemilik, akses, serta pemeriksaan berbeda. Preview boleh cepat dan datanya sintetis; production harus memiliki konfigurasi yang disetujui, rahasia yang tidak tampil di log, dan jalur pemulihan yang sudah diuji.

Cloudflare Pages dan Workers menyediakan mekanisme deployment serta konfigurasi, tetapi dokumentasi penyedia tidak mengetahui struktur akun, izin tim, data, atau kewajiban proyek Anda. Karena itu, [NEEDS GATE-07: verifikasi perilaku fitur, batas, kompatibilitas runtime, implikasi data, dan konfigurasi akun aktual] harus ditutup sebelum prosedur ini dianggap siap produksi. Dokumentasi Pages, Workers, dan versi deployment adalah rujukan perilaku platform pada saat dibaca, bukan bukti bahwa rilis Anda sudah benar ([Pages](https://developers.cloudflare.com/pages/), [Workers](https://developers.cloudflare.com/workers/), [versions and deployments](https://developers.cloudflare.com/workers/configuration/versions-and-deployments/)).

![Ilustrasi cloudflare](/wp-content/uploads/2022/11/cloudflare.jpg)

Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.

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

## Apa yang sebenarnya dipisahkan?

Environment adalah konteks tempat artefak berjalan: preview untuk perubahan terbatas, staging untuk verifikasi yang menyerupai production, dan production untuk pengguna nyata. Configuration adalah nilai non-rahasia yang mengubah perilaku, seperti nama fitur, endpoint layanan, atau pilihan mode. Secrets adalah kredensial yang memberi kuasa—token, kunci API, atau kata sandi—sehingga nilainya harus masuk melalui mekanisme rahasia, bukan source code.

Pemisahan ini menjawab tiga pertanyaan berbeda: kode apa yang dijalankan, dengan nilai apa, dan siapa yang boleh mengubahnya. Satu file `.env` yang disalin ke semua tempat gagal menjawab ketiganya. Ia juga mendorong kebiasaan berbahaya: data uji masuk ke layanan nyata, token preview mendapat hak production, lalu perubahan tidak memiliki jejak persetujuan.

Buat inventaris sederhana untuk setiap environment: pemilik bisnis, pemilik teknis, daftar variable dan tipenya, binding layanan (misalnya database atau object storage), sumber data, kelompok akses, serta cara rollback. Simpan nama dan tujuan secret, bukan nilainya. Untuk aturan akses dan konsekuensi data, minta peninjauan pemilik sistem; artikel ini tidak menggantikan persetujuan keamanan atau kewajiban hukum proyek.

## Cara kerjanya dari perubahan sampai rilis

Mulai dari deklarasi. Setiap perubahan konfigurasi harus memiliki nama yang stabil, tipe, nilai default yang aman, pemilik, dan alasan. Bedakan `PUBLIC_*` yang memang boleh dikirim ke browser dari secret yang hanya dibaca server. Bila sebuah nilai bisa mengubah tujuan pembayaran, pengiriman email, atau akses data, perlakukan sebagai perubahan berisiko meskipun formatnya hanya teks.

Berikutnya, buat sumber kebenaran per environment. Nilai preview dan staging dikelola terpisah dari production; jangan memakai satu kredensial lintas konteks. Binding juga dicatat sebagai pasangan “nama di kode–resource aktual”. Dengan begitu, penggantian resource dapat diperiksa tanpa membuka kredensial. Dokumentasi Cloudflare menjelaskan pilihan konfigurasi dan mekanisme deployment platform, tetapi pemetaan resource di akun Anda tetap perlu diverifikasi ([Workers configuration](https://developers.cloudflare.com/workers/)).

Saat artefak dipromosikan, bangun ulang keputusan, bukan rahasianya. Pipeline atau operator memeriksa commit, konfigurasi target, izin, dan hasil test; lalu mengikat artefak ke environment yang dituju. Catat siapa menyetujui, kapan, versi apa, dan pemeriksaan apa yang lulus. Deployment yang berhasil diunggah belum membuktikan migrasi data, binding, observability, atau perilaku pengguna berjalan benar. Fitur versi dan deployment Cloudflare dapat membantu mengelola rilis, namun strategi rollback harus diuji pada sistem nyata ([Cloudflare deployments](https://developers.cloudflare.com/workers/configuration/versions-and-deployments/)).

Setelah rilis, lakukan smoke test yang aman: endpoint kesehatan, autentikasi uji, operasi baca yang tidak merusak, dan jalur penting yang tidak mengirim transaksi nyata. Telemetri harus membedakan environment dan menyamarkan nilai sensitif. OpenTelemetry mendokumentasikan pola instrumentasi dan sinyal, tetapi sinyal itu sendiri tidak menjamin reliabilitas ([OpenTelemetry](https://opentelemetry.io/docs/)).

## Faktor yang mengubah hasil

Pertama, tipe data. Preview dapat memakai dataset sintetis yang menyerupai bentuk data tanpa menyalin identitas atau token. Staging mungkin memerlukan subset yang sudah disamarkan. Production tunduk pada kebijakan retensi, backup, dan akses yang disetujui pemilik data. Jika belum ada keputusan tertulis, berhenti pada data uji; jangan menganggap “internal” berarti bebas risiko.

Kedua, batas izin. Developer yang boleh membuat preview belum tentu boleh mengubah secret production. Pisahkan peran baca log, mengubah configuration, merotasi secret, dan menyetujui deployment. Gunakan akses sementara bila tersedia, serta tinjau akun yang sudah tidak aktif. Jangan mencetak environment variable ketika melakukan debugging; log harus menampilkan nama teredaksi dan hasil validasi, bukan nilainya.

Ketiga, perubahan tak terlihat. Drift terjadi ketika dashboard, perintah manual, atau hotfix mengubah production tanpa tercermin di deklarasi konfigurasi. Jadwalkan perbandingan daftar variable, binding, dan izin terhadap catatan sumber kebenaran. Setiap selisih memiliki pemilik dan keputusan: diserap ke deklarasi, dibatalkan, atau diberi pengecualian berjangka.

Keempat, kemampuan mendeteksi dampak. Tetapkan service-level objective (SLO) yang menerjemahkan kesehatan layanan menjadi keputusan operasional, bukan janji uptime kontraktual. Workbook SRE Google menempatkan SLO sebagai alat mengelola keandalan dan anggaran kesalahan, sehingga ambang alert harus terkait tindakan yang jelas ([SRE Workbook](https://sre.google/workbook/implementing-slos/)). Tanpa bukti operasi dan kontrak, jangan menulis angka ketersediaan sebagai klaim.

## Contoh keputusan praktis

Anggap sebuah fitur pembayaran memiliki tiga konteks berikut. Di preview, endpoint diarahkan ke simulator dan secret memiliki izin minimum. Di staging, binding mengarah ke akun uji terpisah; data pelanggan dibuat sintetis dan webhook memakai alamat yang tidak mengirim pesan nyata. Di production, pemilik pembayaran menyetujui endpoint, pemilik keamanan menyetujui secret, dan operator menyimpan bukti smoke test. Nilai persis, vendor, serta jadwalnya adalah keputusan proyek—bukan fakta yang boleh ditebak dari contoh ini.

Gunakan tabel keputusan saat menilai perubahan:

| Pertanyaan | Jika jawabannya “belum” | Tindakan |
|---|---|---|
| Apakah pemilik nilai dan resource tercatat? | Tidak ada akuntabilitas | Tunda promosi |
| Apakah secret berbeda dan berizin minimum? | Risiko lintas environment | Buat kredensial terpisah |
| Apakah data uji bebas identitas nyata? | Risiko kebocoran dan salah kirim | Ganti dengan data sintetis |
| Apakah smoke test dan rollback pernah dijalankan? | Jalur pulih belum terbukti | Uji di staging terlebih dahulu |
| Apakah telemetry menandai environment dan menyamarkan secret? | Diagnosis bisa menyesatkan | Perbaiki instrumentasi |

Teman Codev.id, bila satu jawaban masih “belum”, labeli rilis sebagai kandidat, bukan production-ready. Label itu membantu tim menghindari tekanan bahasa yang membuat risiko tampak selesai.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah menyamakan variable dengan secret. Periksa setiap nilai: apakah bocorannya memberi kemampuan bertindak? Jika ya, pindahkan ke penyimpanan secret dan batasi pembacaan. Kedua, memakai binding yang sama di preview dan production. Cocokkan nama binding dengan ID resource target, lalu minta orang kedua meninjau hasilnya.

Ketiga, menguji dengan data production karena “lebih realistis”. Cari bukti masking, izin, dan persetujuan tertulis; tanpa itu, gunakan dataset sintetis. Keempat, mengandalkan dashboard tanpa audit trail. Catat perubahan configuration, akses secret, persetujuan, hasil pemeriksaan, dan pengecualian. NIST menekankan bahwa respons insiden mencakup persiapan, deteksi, respons, dan pembelajaran; catatan perubahan membuat investigasi dan perbaikan dapat ditelusuri ([NIST SP 800-61 Rev. 3](https://csrc.nist.gov/pubs/sp/800/61/r3/final)).

Kelima, merotasi secret dengan mengganti nilai secara mendadak. Pastikan konsumen mendukung masa overlap yang disetujui, uji pembacaan credential baru, cabut credential lama, lalu periksa error setelah rotasi. Detail urutan dan durasi bergantung pada layanan serta kebijakan proyek; jangan mengarang interval universal.

## Jalan pintas yang tampak menarik

Jalan pintas paling umum adalah menyalin seluruh konfigurasi production ke preview agar “pasti sama”. Cara ini memang mengurangi perbedaan permukaan, tetapi juga menyalin hak akses, tujuan layanan, dan kemungkinan data nyata. Alternatif yang lebih aman adalah menyamakan skema dan nama, lalu mengisi nilai serta binding berbeda per environment. Perbedaan yang disengaja didokumentasikan; perbedaan yang tidak disengaja ditangani sebagai drift.

Kawan Codev.id, jangan menjadikan keberhasilan satu deployment sebagai bukti end-to-end. Tanyakan: apakah resource target benar, secret terotorisasi, telemetry muncul di environment yang benar, dan rollback dapat mengembalikan perilaku tanpa merusak data? Jika jawabannya belum berbukti, minta technical review sebelum promosi.

## Kesimpulan dan langkah berikutnya

Environment, configuration, dan secrets aman dari preview ke production ketika dipisahkan berdasarkan konteks, pemilik, nilai, binding, data, akses, dan bukti rilis. Cloudflare menyediakan mekanisme platform; tim Anda tetap bertanggung jawab atas pemetaan resource, izin, pengujian, drift, audit, dan rotasi.

Langkah berikutnya: buat satu lembar inventaris per environment, isi pemilik dan binding tanpa menuliskan nilai secret, lalu jalankan review silang untuk akses, smoke test, telemetry, rollback, dan rencana rotasi. Untuk konteks pekerjaan dan layanan Codev.id, Anda dapat mulai dari [halaman utama Codev.id](/) sebelum menyelaraskan dokumen dengan pemilik sistem. Simpan keputusan serta pengecualian dengan tanggal kedaluwarsa. Aturan operasionalnya sederhana: jangan promosikan konfigurasi yang tidak punya pemilik, secret yang tidak punya batas akses, atau rilis yang belum memiliki bukti pemulihan. Penutupan [NEEDS GATE-07] dan technical review proyek tetap diperlukan sebelum production.
