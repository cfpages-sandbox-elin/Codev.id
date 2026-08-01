---
article_id: CDV-18-A02
title: "Decision Log, Risk Register, dan Issue Log yang Berbeda"
slug: "decision-risk-issue-log"
description: "Panduan membedakan decision log, risk register, dan issue log agar pemilik, status, bukti, tanggal tinjau, eskalasi, tautan, penutupan, dan pelaporannya terlacak"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2026-05-20"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-18
primary_intent: "Keep project uncertainty and decisions traceable"
reader_community: "Codev.id"
reader_address: "Kawan Codev.id"
final_route: "/artikel/decision-risk-issue-log.html"
technical_review: required
sources:
  - "https://csrc.nist.gov/pubs/sp/800/218/final"
  - "https://www.w3.org/TR/WCAG-EM/"
  - "https://spec.openapis.org/oas/v3.1.1.html"
  - "https://sre.google/workbook/implementing-slos/"
  - "https://opentelemetry.io/docs/"
  - "https://csrc.nist.gov/pubs/sp/800/61/r3/final"
  - "https://csrc.nist.gov/pubs/sp/800/161/r1/final"
  - "https://www.cisa.gov/securebydesign"
  - "https://www.gov.uk/guidance/the-technology-code-of-practice"
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

# Decision Log, Risk Register, dan Issue Log yang Berbeda

Halo, Kawan Codev.id! Ketika konteks proyek tercecer di chat dan notulen, pertanyaan yang sama muncul berulang: apa yang sudah diputuskan, apa yang baru mungkin terjadi, dan apa yang sudah menghambat pekerjaan? Jawaban praktisnya bukan membuat satu daftar raksasa. Pisahkan tiga log berdasarkan keadaan yang hendak dikendalikan: **decision log** menyimpan pilihan yang telah diambil dan alasannya, **risk register** menyimpan ketidakpastian yang mungkin berdampak, sedangkan **issue log** menyimpan masalah yang sudah terjadi dan memerlukan tindakan.

Ketiganya boleh saling terhubung, tetapi tidak boleh saling menggantikan. Satu risiko dapat berubah menjadi issue ketika pemicunya benar-benar terjadi; sebuah issue dapat memerlukan keputusan; dan keputusan dapat menutup risiko atau membuka risiko baru. Asumsi dan action item menjadi bahan pendukung, bukan alasan untuk menghapus batas tersebut. Bentuk akhir log harus mengikuti bukti dan tata kelola proyek yang nyata—termasuk siapa yang berwenang menyetujui—bukan sekadar menyalin templat.

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

*Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.*

## Jawaban singkat dan salah paham utama

Gunakan pertanyaan pembeda berikut pada setiap entri:

| Objek | Pertanyaan pengendali | Status awal yang masuk akal | Pemilik utama |
| --- | --- | --- | --- |
| Decision log | “Pilihan apa yang disahkan, dengan alasan dan konsekuensi apa?” | Proposed atau Approved | Decision owner atau approver |
| Risk register | “Apa yang mungkin terjadi, seberapa besar dampaknya, dan bagaimana menurunkannya?” | Open atau Monitoring | Risk owner |
| Issue log | “Masalah apa yang sudah terjadi, apa dampaknya sekarang, dan siapa yang memulihkannya?” | Open atau In progress | Issue owner |

Status hanyalah sinyal; ia harus disertai bukti terakhir dan tanggal pembaruan. `Closed` tanpa catatan hasil atau verifikasi bukan penutupan, melainkan kehilangan konteks. Di sisi lain, status `Monitoring` tidak berarti risiko aman selamanya—tanggal review berikutnya tetap wajib ada.

Untuk keputusan teknis, tautkan persyaratan, hasil pemeriksaan, dan cacat yang belum selesai. NIST SSDF menekankan praktik pengembangan aman yang dapat ditelusuri, sementara WCAG-EM dan OpenAPI menunjukkan bahwa evaluasi maupun kontrak antarmuka perlu menyatakan ruang lingkup dan objek yang diperiksa; lulus pada satu sampel, build, atau lingkungan tidak otomatis membuktikan seluruh sistem lulus ([NIST SSDF](https://csrc.nist.gov/pubs/sp/800/218/final), [W3C WCAG-EM](https://www.w3.org/TR/WCAG-EM/), [OpenAPI Specification 3.1.1](https://spec.openapis.org/oas/v3.1.1.html)). Karena itu, keputusan “rilis” sebaiknya menunjuk bukti spesifik dan daftar pengecualian, bukan hanya centang hijau.

## Definisi dan batas objek

**Decision log** adalah rekaman keputusan yang sudah diambil atau sedang menunggu otorisasi. Isinya minimal: pertanyaan keputusan, opsi yang dipertimbangkan, keputusan dan alasan, pengambil keputusan, tanggal, asumsi, dampak, bukti pendukung, serta tanggal peninjauan bila keputusan dapat dibuka kembali. Ia bukan daftar tugas dan bukan notulen lengkap.

**Risk register** menangkap peristiwa yang belum terjadi. Tulis penyebab atau pemicu, peristiwa risikonya, dampak, pemilik risiko, respons (hindari, kurangi, alihkan, atau terima), indikator, target tanggal review, dan kondisi eskalasi. Nilai atau label probabilitas hanya berguna jika tim menyepakati arti dan cara memperbaruinya; jangan mengarang ambang universal.

**Issue log** mencatat fakta bahwa hambatan telah terjadi: deskripsi, kapan terdeteksi, dampak aktual, reproduksi atau bukti, pemilik pemulihan, dependensi, target penyelesaian, eskalasi, dan verifikasi penutupan. Bila masalahnya merupakan insiden layanan atau keamanan yang memerlukan proses respons khusus, gunakan catatan insidennya juga. Log ini tidak menggantikan rekaman insiden; NIST SP 800-61 Rev. 3 membahas siklus respons dan pembelajaran insiden secara tersendiri ([NIST Incident Response](https://csrc.nist.gov/pubs/sp/800/61/r3/final)).

**Assumption** adalah pernyataan yang dipakai sebagai benar untuk sementara, misalnya “spesifikasi mitra tersedia pada tanggal tertentu”. Beri pemilik dan tanggal validasi. Jika ternyata salah, ubah menjadi risiko atau issue dengan tautan ke entri asal. **Action** adalah pekerjaan untuk menjalankan keputusan atau respons; ia harus menunjuk induknya, pemilik, tenggat, dan bukti selesai. Dengan begitu, satu tindakan tidak mengaburkan apakah masalahnya masih ada.

## Cara kerjanya

Mulailah dengan satu intake sederhana dari rapat, chat, tiket, hasil uji, atau pengamatan operasi. Tanyakan: ini pilihan, kemungkinan, kejadian aktual, asumsi, atau tindakan? Berikan ID yang tidak berubah, lalu tautkan entri terkait. Contoh: `R-014` menunjuk risiko ketergantungan API; setelah gangguan benar-benar muncul, buat `I-006` dan tautkan kembali ke `R-014`; keputusan memakai fallback dicatat sebagai `D-022`; action untuk menguji fallback diberi ID tersendiri.

Tetapkan pemilik yang dapat menggerakkan langkah berikutnya, bukan sekadar orang yang menemukan entri. Untuk decision, pemilik menyiapkan opsi dan approver mengesahkan. Untuk risk, risk owner memantau pemicu dan respons. Untuk issue, issue owner mengoordinasikan pemulihan. Nama, peran, atau jalur eskalasi harus mengikuti struktur proyek yang disetujui; jika belum ada, tandai kebutuhan penetapan, jangan menebak jabatan.

Gunakan status yang pendek dan definisinya tertulis, misalnya `Proposed → Approved → Superseded` untuk keputusan; `Open → Monitoring → Triggered/Accepted → Closed` untuk risiko; dan `Open → In progress → Blocked/Resolved → Verified/Closed` untuk issue. Setiap perpindahan menyimpan siapa, kapan, alasan, dan tautan bukti. Keputusan yang digantikan tetap dipertahankan agar pembaca memahami riwayat, bukan dihapus.

Sediakan kolom `evidence` dan `links` yang menunjuk artefak nyata: versi kontrak API, hasil uji, tiket, log perubahan, atau notulen persetujuan. OpenAPI membantu membuat kontrak antarmuka eksplisit; dokumentasi OpenTelemetry menjelaskan bahwa instrumentasi menghasilkan sinyal observabilitas, bukan jaminan keandalan. SLO dari Google SRE adalah tujuan layanan dan alat keputusan, bukan janji uptime kontraktual ([OpenTelemetry](https://opentelemetry.io/docs/), [Google SRE—SLOs](https://sre.google/workbook/implementing-slos/)). Maka alert atau dashboard harus ditautkan sebagai bukti kondisi, sementara keputusan kapasitas dan biaya tetap dicatat sebagai keputusan terpisah.

Jadwalkan dua waktu yang berbeda: `due date` untuk action atau respons yang harus selesai, dan `review date` untuk menilai kembali risiko atau keputusan. Atur eskalasi berbasis kondisi: dampak melewati batas yang disetujui, pemicu risiko terdeteksi, tenggat terlewati, atau bukti tidak dapat diverifikasi. Laporan mingguan cukup merangkum entri baru, perubahan status, item melewati tenggat, risiko tertinggi, keputusan yang menunggu approver, dan tautan bukti. Pembaca harus bisa kembali ke sumber tanpa menggali seluruh chat.

## Faktor yang mengubah hasil

Kedalaman log bergantung pada konsekuensi dan umur keputusan. Eksperimen singkat dapat memakai satu halaman ringkas; perubahan arsitektur, data sensitif, atau pemasok kritis memerlukan jejak persetujuan dan bukti lebih ketat. NIST SP 800-161 membingkai risiko rantai pasok sebagai sesuatu yang perlu diidentifikasi dan dikelola sepanjang siklus hidup, bukan hanya saat membeli ([NIST SP 800-161 Rev. 1](https://csrc.nist.gov/pubs/sp/800/161/r1/final)). Karena itu, risiko vendor sebaiknya memiliki pemilik, asumsi layanan, bukti serah-terima, dan tanggal review kontrak.

Konteks operasional juga mengubah pemisahan. Sistem yang belum beroperasi mungkin memiliki risiko kapasitas; sistem yang sudah mengalami error memiliki issue; dan respons untuk gangguan yang sedang berlangsung perlu mengikuti catatan insiden. Sinyal telemetry tanpa definisi layanan, ambang keputusan, dan kemampuan respons hanya menambah data. Jangan menyatakan layanan “24/7” atau tingkat uptime tertentu tanpa bukti operasi dan kontrak yang berlaku.

Keamanan, aksesibilitas, dan keberlanjutan bukan label otomatis. CISA Secure by Design mendorong pertimbangan keamanan sejak awal, sedangkan Technology Code of Practice Inggris menekankan hasil, kepemilikan, dan pengelolaan sepanjang siklus layanan ([CISA Secure by Design](https://www.cisa.gov/securebydesign), [UK Technology Code of Practice](https://www.gov.uk/guidance/the-technology-code-of-practice)). Gunakan keduanya sebagai rujukan prinsip, bukan sebagai hukum Indonesia atau sertifikat bahwa proyek tertentu sudah patuh. Kriteria lokal, persetujuan profesional, dan bukti implementasi tetap harus diperiksa.

## Contoh keputusan praktis

Bayangkan tim akan merilis perubahan endpoint pembayaran. Temuan “dokumen kontrak belum disetujui” adalah issue bila tanggal rilis sudah terhambat; sebelum itu, ketidakpastian ketersediaan persetujuan adalah risk. Setelah opsi fallback dipilih, entri decision menjelaskan alasan, dampak pada pengguna, dan syarat pembukaan kembali.

| Entri | Isi minimum yang membuatnya dapat ditindaklanjuti |
| --- | --- |
| `R-014` | Pemicu: persetujuan mitra terlambat; dampak: rilis tertahan; owner; respons: minta konfirmasi tertulis; review date; kondisi eskalasi. |
| `I-006` | Fakta: persetujuan belum diterima pada tanggal rilis; bukti tautan email/tiket; owner pemulihan; due date; status verifikasi. |
| `D-022` | Opsi: tunda atau aktifkan fallback; approver; alasan; batas penggunaan; bukti uji; keputusan dapat ditinjau setelah persetujuan masuk. |
| `A-031` | Tindakan menguji fallback; owner; due date; artefak hasil; tautan ke `D-022`. |

Jika hasil uji hanya mencakup build dan data tertentu, tulis batas itu di decision log. Jangan mengubah hasil pengujian menjadi klaim bahwa semua pengguna, integrasi, atau kondisi produksi telah terbukti aman. Pada titik ini, Kawan Codev.id, pertanyaan yang paling berguna adalah: “Bukti apa yang membuat approver bersedia menerima sisa ketidakpastian, dan kapan keputusan itu harus dibuka kembali?”

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah satu spreadsheet dengan kolom campur aduk. Periksa setiap baris: apakah pembaca langsung tahu ini kemungkinan atau kejadian? Jika tidak, pisahkan jenisnya dan buat ID relasi. Kedua, semua entri diberi owner “tim”. Ganti dengan satu pemilik yang punya kewenangan, lalu catat siapa approver atau pihak yang dikonsultasikan.

Ketiga, status berubah tanpa tanggal, alasan, atau bukti. Audit sampel beberapa entri: dapatkah orang yang tidak hadir rapat memahami apa yang berubah? Keempat, `due date` dipakai sebagai `review date`; tambahkan keduanya agar risiko tidak dianggap selesai hanya karena tindakan sudah jatuh tempo. Kelima, entri ditutup saat action dicentang, padahal hasil belum diverifikasi. Minta artefak penutupan dan orang yang memverifikasi.

Keenam, dashboard dijadikan bukti tunggal. Instrumentasi dan alert memberi sinyal yang perlu ditafsirkan; ia tidak menggantikan keputusan, respons, atau pembelajaran insiden ([OpenTelemetry](https://opentelemetry.io/docs/)). Ketujuh, logo sertifikasi atau harga terendah dipakai untuk menutup risiko pemasok. Bandingkan ruang lingkup, bukti serah-terima, ketergantungan, dan total biaya siklus hidup sesuai konteks proyek; jangan menganggap logo membuktikan hasil tim yang diusulkan ([NIST SP 800-161 Rev. 1](https://csrc.nist.gov/pubs/sp/800/161/r1/final)).

## Jalan pintas yang tampak efisien

“Lebih cepat kalau semua dicatat di chat saja.” Chat memang berguna sebagai intake, tetapi sulit menjawab status terkini, pemilik, tenggat, dan bukti setelah percakapan bergeser. “Satu daftar sudah cukup,” juga terdengar efisien, tetapi mencampur masa depan yang belum pasti dengan masalah aktual dan keputusan yang mengikat; eskalasi pun menjadi kabur.

Alternatif yang ringan adalah satu tampilan laporan yang menarik data dari tiga log terpisah. Batasi field wajib, pakai tautan ke sumber, dan lakukan review singkat dengan pemilik. Sobat Codev.id, bila tim belum menyepakati siapa yang boleh menerima risiko atau mengesahkan keputusan, berhenti pada pencatatan fakta dan minta penetapan kewenangan. Jangan memalsukan kepastian lewat status hijau.

## Penutup: aturan operasi yang bisa langsung dipakai

Decision log menjawab pilihan yang telah disahkan; risk register menjaga kemungkinan yang belum terjadi; issue log mengendalikan masalah yang sudah nyata. Assumption dan action memperkaya jejak, tetapi harus menunjuk induk dan pemiliknya. Untuk setiap entri baru, tetapkan owner, status yang didefinisikan, bukti, due atau review date, kondisi eskalasi, tautan terkait, dan kriteria verifikasi penutupan.

Langkah berikutnya: ambil satu minggu percakapan proyek, klasifikasikan setiap item ke tiga log, lalu minta pemilik dan approver memeriksa lima entri paling berisiko. Simpan catatan insiden di proses insiden yang sesuai dan minta technical review untuk keputusan yang menyentuh keamanan, aksesibilitas, kontrak, atau operasi. Untuk konteks layanan yang lebih luas, pembaca dapat membuka [halaman utama Codev.id](/) lalu kembali ke log proyek. Aturan batasnya sederhana: bila status, bukti, atau kewenangan belum dapat diverifikasi, biarkan tetap terbuka dan tandai apa yang harus diperoleh—jangan menutupnya dengan asumsi.
