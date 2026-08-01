---
article_id: CDV-18-A01
title: "RACI Proyek Digital: Keputusan, Pekerjaan, dan Konsultasi"
slug: "raci-proyek-digital"
description: "Panduan membagi peran keputusan, pekerjaan, konsultasi, dan informasi pada proyek digital"
writing_contract_version: "native-id-v2"
status: draft
publication_date: "2026-05-17"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-18
primary_intent: "Clarify accountability across client, product, vendor, and specialists"
reader_community: "Codev.id"
reader_address: "Teman Codev.id"
final_route: "/artikel/raci-proyek-digital.html"
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
---

# RACI Proyek Digital: Keputusan, Pekerjaan, dan Konsultasi

Halo, Teman Codev.id! Ketika keputusan tertahan karena semua orang merasa “ikut bertanggung jawab”, masalahnya biasanya bukan kurang rapat, melainkan tidak ada peta peran. RACI memisahkan empat hal: **Responsible** mengerjakan, **Accountable** memegang keputusan akhir, **Consulted** memberi masukan dua arah, dan **Informed** menerima kabar satu arah.

Buat satu matriks per deliverable, bukan satu tabel besar yang kabur. Satu aktivitas idealnya memiliki tepat satu A; boleh beberapa R, C, atau I sesuai kebutuhan. Pembagian ini tetap tunduk pada kontrak, kompetensi profesional, dan bukti proyek. Jika pemilik keputusan, data, atau kewajiban hukum belum ditetapkan, tandai `[NEEDS OWNER REVIEW]` sebelum pekerjaan berjalan.

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

*Gambar ini merupakan aset lokal untuk ilustrasi dan bukan dokumentasi proyek tertentu.*

## Definisikan kebutuhan sebelum meminta harga

Mulai dari daftar keputusan dan hasil, lalu tetapkan peran. Contoh baris awal: tujuan bisnis (A: sponsor klien; R: product manager), kebutuhan pengguna (A: product owner; R: analis), dan batas data pribadi (A: pemilik proses; R: spesialis privasi, C: keamanan). Jangan menganggap vendor otomatis menjadi A hanya karena ia membangun sistem. A harus pihak yang berwenang menerima risiko dan mengubah prioritas.

Gunakan kolom aktivitas, output yang dapat diperiksa, A, R, C, I, tenggat keputusan, serta bukti. Untuk desain API, tim teknis dapat R, pemilik produk A, keamanan C, dan dukungan operasional I. Spesifikasi OpenAPI membantu membuat antarmuka dapat ditinjau, tetapi tidak menentukan siapa yang menyetujui perubahan ([OpenAPI Specification 3.1.1](https://spec.openapis.org/oas/v3.1.1.html)).

## Buat penawaran benar-benar sebanding

RACI membuat penawaran dapat dibandingkan karena setiap penyedia menjawab pekerjaan yang sama. Minta mereka menyatakan siapa R dan A untuk discovery, desain, integrasi, pengujian, rilis, dukungan, dan handover; minta pula asumsi akses, data uji, lingkungan, serta pekerjaan yang dikecualikan. Biaya terendah dapat memindahkan beban ke operasi dan rework; pertimbangkan biaya siklus hidup serta risiko rantai pasok, sebagaimana ditekankan NIST SP 800-161 ([NIST SP 800-161 Rev. 1](https://csrc.nist.gov/pubs/sp/800/161/r1/final)).

Jangan menerima tabel dengan semua pihak diberi A. Jika dua organisasi sama-sama harus menyetujui, tetapkan satu A untuk keputusan dan jadikan pihak lain C dengan batas waktu respons. Keputusan yang tidak dijawab diperlakukan sebagai eskalasi, bukan persetujuan diam-diam.

## Dokumen yang membuktikan hal berbeda

RACI juga menjawab pertanyaan “bukti apa yang harus diserahkan kepada siapa”. Untuk scope, simpan keputusan dan acceptance criteria. Untuk keamanan, simpan threat model, temuan, dan keputusan residual risk; secure-by-design menempatkan pertimbangan keamanan sepanjang siklus pengembangan, bukan hanya pemeriksaan akhir ([CISA Secure by Design](https://www.cisa.gov/securebydesign)). Untuk aksesibilitas, evaluasi terstruktur seperti WCAG-EM memberi kerangka pemeriksaan, tetapi tidak mengubah A menjadi milik penguji ([W3C WCAG-EM 1.0](https://www.w3.org/TR/WCAG-EM/)).

Automated test hanya membuktikan assertion yang diuji pada build, data, dan lingkungan tertentu. Karena itu R untuk test menautkan hasil ke requirement, sementara A menyetujui residual defect berdasarkan dampaknya; SSDF menekankan praktik pengembangan aman dan ketertelusuran ([NIST SP 800-218 SSDF](https://csrc.nist.gov/pubs/sp/800/218/final)).

## Pertanyaan wajib kepada penyedia

Ajukan pertanyaan yang dapat dijawab dengan nama, artefak, atau aturan: “Siapa A untuk perubahan skema data?”, “Siapa R saat kredensial bocor?”, “Berapa lama C wajib merespons?”, “Artefak apa yang kami terima sebelum rilis?”, dan “Siapa yang tetap bisa mengoperasikan sistem bila kontrak berakhir?” Pastikan jawaban masuk ke matriks dan kontrak, bukan hanya notulen. Untuk telemetry, OpenTelemetry menyediakan instrumen dan sinyal; itu bukan bukti reliabilitas tanpa target dan tindakan yang disepakati ([OpenTelemetry documentation](https://opentelemetry.io/docs/)).

Kawan Codev.id, uji jawaban itu dengan skenario sederhana: satu perubahan mendesak, satu temuan keamanan, dan satu kegagalan integrasi. Minta penyedia menyebut jalur eskalasi, pengganti ketika pemegang peran tidak tersedia, serta artefak yang membuktikan keputusan. Jika jawabannya hanya “nanti dibahas”, baris tersebut belum siap masuk rencana kerja.

## Tanda bahaya dan biaya yang sering tersembunyi

Waspadai satu aktivitas tanpa A, lebih dari satu A yang tidak punya mekanisme pemutus, vendor yang menjadi R sekaligus menyetujui hasilnya sendiri, serta C yang baru dilibatkan setelah desain beku. Red flag lain adalah janji “siap 24/7” tanpa bukti operasi atau kontrak. SLO adalah tujuan layanan dan alat keputusan, bukan otomatis janji uptime ([Google SRE Workbook—SLOs](https://sre.google/workbook/implementing-slos/)).

Untuk insiden, tetapkan R on-call, A yang berwenang mengumumkan dampak, C keamanan/legal, dan I pemangku kepentingan. Prosedur respons harus mencakup deteksi, containment, pemulihan, dan pembelajaran; rincian organisasi perlu disesuaikan dengan sistem nyata ([NIST SP 800-61 Rev. 3](https://csrc.nist.gov/pubs/sp/800/61/r3/final)).

## Penerimaan, serah terima, dan keputusan akhir

Sebelum acceptance, cocokkan setiap requirement dengan pengujiannya, temuan terbuka, keputusan risiko, dokumentasi operasi, akses, dan pemilik setelah go-live. R menjalankan pemeriksaan; C menilai aspek khusus; A menandatangani penerimaan atau menolak dengan alasan; I menerima catatan perubahan. Jangan jadikan tanda tangan vendor sebagai bukti bahwa pemilik bisnis telah menerima risiko.

Pada rapat go/no-go, tampilkan matriks versi, bukti, pengecualian, dan tanggal kedaluwarsa keputusan. Jika data, keamanan, atau aksesibilitas belum memiliki A yang kompeten, hentikan keputusan dan minta review profesional. RACI memperjelas akuntabilitas, tetapi tidak menggantikan otoritas kontrak atau tanggung jawab spesialis.

## Jalan pintas yang tampak praktis

Jalan pintas yang sering dipilih adalah menyalin template RACI lalu memberi A kepada “project manager” untuk semua baris. Itu memang cepat, tetapi menyembunyikan siapa pemilik risiko data, keputusan produk, dan persetujuan rilis. Alternatifnya, lakukan lokakarya singkat berbasis deliverable: tulis satu keputusan, sebut satu A, minta R menunjukkan bukti, lalu catat C dan I yang benar-benar diperlukan. Revisi matriks setiap perubahan scope atau organisasi.

## Kesimpulan: satu keputusan, satu pemilik

RACI proyek digital memisahkan pelaksana (R), pemilik keputusan (A), pemberi masukan (C), dan penerima kabar (I) untuk setiap hasil: scope, desain, data, keamanan, aksesibilitas, rilis, insiden, acceptance, dan perubahan. Sobat Codev.id, langkah berikutnya adalah pilih satu deliverable terdekat, isi matriks dengan nama dan bukti yang akan diserahkan, lalu minta pemilik risiko mengesahkannya.

Aturan operasionalnya sederhana: jangan mulai pekerjaan yang tidak punya R, jangan menutup pekerjaan yang tidak punya A, dan jangan menganggap C atau I sebagai pengganti persetujuan. Untuk keputusan yang menyentuh keamanan, privasi, aksesibilitas, atau kewajiban kontrak, tetap minta review kompeten; RACI membantu koordinasi, bukan menggantikan tanggung jawab tersebut.

Simpan versi matriks bersama decision log dan tautkan perubahan yang disetujui ke artefak terkait. Untuk konteks umum dan langkah berikutnya, Anda dapat kembali ke [halaman utama Codev.id](/). Tautan itu bukan pengganti pemeriksaan proyek: pemilik sistem tetap perlu memastikan akses, bukti, dan kewenangan benar-benar tersedia.

Periksa matriks pada setiap gerbang kerja: setelah kebutuhan disepakati, sebelum desain dibekukan, menjelang rilis, dan setelah insiden. Tanyakan apakah nama yang tercantum masih memiliki mandat, akses, dan waktu untuk menjalankan perannya. Bila organisasi berubah, buat versi baru dan catat alasan perubahan agar keputusan lama tidak keliru dianggap masih berlaku. Dengan kebiasaan ini, rapat beralih dari mencari siapa yang salah menjadi memastikan siapa melakukan tindakan berikutnya dan kapan buktinya tersedia.
