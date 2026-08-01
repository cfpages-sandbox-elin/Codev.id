---
article_id: CDV-18-A05
writing_contract_version: "native-id-v2"
title: "Checklist Handover Software dan Bukti Kepemilikan"
slug: "checklist-handover-software"
description: "Inventory repositories/source, builds, environments, accounts/access, domains/services, data/backup, architecture, runbooks, tests, licenses, decisions, training, support, and acceptance"
status: draft
publication_date: "2026-06-02"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-18
primary_intent: "Receive everything needed to own and operate delivered software"
reader_community: "Codev.id"
reader_address: "Sobat Codev.id"
final_route: "/artikel/checklist-handover-software.html"
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

# Checklist Handover Software dan Bukti Kepemilikan

Halo, Sobat Codev.id!

Handover software yang sehat bukan sekadar menerima tautan repository dan kata sandi. Anda baru benar-benar bisa memiliki dan mengoperasikan sistem ketika sumber, cara build, lingkungan, akses, data, bukti pengujian, serta keputusan penerimaan dapat diperiksa dan dijalankan ulang. Checklist ini membantu memeriksa setiap barang dan bukti itu sebelum serah terima ditandatangani.

Kumpulkan paket handover, lakukan uji akses dengan akun organisasi Anda, dan catat penerimaan atau kekurangannya. Detail kontrak yang menentukan hak atas kode, akun, atau lisensi tetap berlaku; checklist ini tidak memindahkan hak yang tidak diberikan kontrak. Untuk butir yang menyangkut keamanan, kepatuhan, atau penerimaan formal, minta tinjauan teknis yang berwenang.

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

Gambar ini merupakan aset lokal untuk ilustrasi dan bukan dokumentasi proyek tertentu.

## Definisikan kebutuhan sebelum meminta serah terima

Mulailah dari daftar yang ingin Anda kuasai, bukan dari format folder yang disukai penyedia. Tetapkan nama aplikasi dan versinya, komponen yang termasuk, lingkungan (development, staging, production), integrasi, data yang boleh disentuh, serta kriteria penerimaan. Untuk tiap item, tulis pemilik, lokasi, versi, tanggal ekspor, dan cara memverifikasi.

Paket minimum biasanya mencakup:

- repository seluruh source, konfigurasi contoh, riwayat perubahan, dan instruksi build;
- artefak rilis, pipeline CI/CD, manifest dependensi, serta cara rollback;
- inventaris environment, domain, DNS, cloud, akun layanan, secret, sertifikat, dan masa kedaluwarsanya;
- skema data, migrasi, backup, prosedur pemulihan, retensi, dan bukti uji restore;
- diagram arsitektur, kontrak API (misalnya spesifikasi OpenAPI), runbook operasi, dan kontak eskalasi;
- hasil pengujian, daftar defect terbuka, lisensi pihak ketiga, keputusan desain, materi pelatihan, dan dukungan pascapenerimaan.

Jangan meminta secret melalui dokumen terbuka. Serahkan akses melalui pengelola identitas organisasi, lalu cabut akun sementara setelah uji. Sobat Codev.id, bukti “sudah dikirim” berbeda dari bukti “tim baru dapat menjalankan”.

## Buat paket serah terima yang dapat diverifikasi

Gunakan matriks sederhana: `item`, `pemilik`, `lokasi`, `versi`, `bukti`, `penguji`, `status`, dan `catatan risiko`. Minta penerima melakukan checkout bersih, menjalankan build di lingkungan yang disepakati, dan membandingkan checksum atau tag rilis bila proses Anda memerlukannya. Dokumentasi API harus cocok dengan implementasi yang diserahkan; OpenAPI adalah format deskripsi, bukan bukti bahwa endpoint telah diuji ([OpenAPI Specification](https://spec.openapis.org/oas/v3.1.1.html)).

Pisahkan artefak dari klaim. Diagram menunjukkan rancangan, bukan kapasitas yang telah terbukti. Runbook menunjukkan langkah, bukan keberhasilan pemulihan. Catat hasil observasi, log, dan tanggal pengujian. Untuk akses, minta bukti bahwa organisasi dapat mengundang, menonaktifkan, dan memulihkan akun tanpa bergantung pada akun pribadi penyedia.

## Bedakan bukti pengujian, operasi, dan kepemilikan

Bukti pengujian harus menyebut build, environment, data uji, skenario, hasil, dan defect yang belum selesai. Automated test hanya membuktikan assertion yang tersampel pada kondisi tersebut; ia tidak otomatis membuktikan kesiapan produksi. Untuk aksesibilitas, evaluasi perlu mencakup ruang lingkup dan metode yang jelas, bukan sekadar label “WCAG compliant” ([W3C WCAG-EM](https://www.w3.org/TR/WCAG-EM/)). Praktik pengembangan aman NIST juga menekankan jejak antara kebutuhan, risiko, dan hasil verifikasi ([NIST SSDF](https://csrc.nist.gov/pubs/sp/800/218/final)).

Pisahkan pula bukti kepemilikan dari pengalaman vendor: invoice atau sertifikat tidak membuktikan bahwa akun, domain, atau lisensi kini berada di organisasi Anda. Cocokkan nama pemegang akun, peran administrator, tanggal berakhir, dan hak transfer dengan kontrak. Jika kontrak atau status lisensi belum jelas, tandai `[NEEDS CONTRACT/LICENSE REVIEW]` sebelum menyatakan sistem sepenuhnya dapat dimiliki.

## Pertanyaan wajib kepada penyedia

Ajukan pertanyaan yang menghasilkan artefak dan tindakan, bukan jawaban “aman”.

1. Repository mana yang lengkap, siapa administratornya, dan bagaimana checkout bersih diverifikasi?
2. Build rilis mana yang sedang diterima; perintah build, dependensi, dan rollback-nya apa?
3. Environment dan akun layanan apa saja yang ada; siapa pemilik organisasi dan bagaimana rotasi secret dilakukan?
4. Di mana DNS, domain, sertifikat, pipeline, dan kontrak layanan dikelola? Apa tanggal kedaluwarsanya?
5. Kapan backup terakhir dipulihkan pada lingkungan uji, oleh siapa, dan dengan hasil apa?
6. Apa dependensi pihak ketiga, lisensi, kewajiban atribusi, dan komponen yang tidak boleh dipindahkan?
7. Bagaimana alarm dibuat, siapa merespons insiden, dan apa rekaman insiden yang tersedia?
8. Definisi layanan apa yang dipantau? SLO adalah tujuan dan mekanisme keputusan, bukan janji uptime kontraktual ([Google SRE Workbook](https://sre.google/workbook/implementing-slos/)).
9. Telemetri apa yang dikumpulkan, berapa retensinya, dan bagaimana akses datanya dibatasi? Instrumentasi menghasilkan sinyal, bukan jaminan reliabilitas ([OpenTelemetry](https://opentelemetry.io/docs/)).
10. Siapa yang menyetujui defect terbuka, pengecualian keamanan, dan tanggal penyelesaiannya? Untuk prosedur deteksi, respons, dan pembelajaran insiden, selaraskan runbook dengan panduan [NIST incident response](https://csrc.nist.gov/pubs/sp/800/61/r3/final).

## Tanda bahaya dan biaya yang sering tersembunyi

Waspadai “source code sudah di-zip” tanpa riwayat, build yang hanya berhasil di laptop tertentu, akun cloud atas nama pribadi, secret tertanam di repository, backup tanpa uji restore, atau dashboard tanpa pemilik dan prosedur respons. Biaya sebenarnya lalu muncul sebagai waktu menunggu akses, rework konfigurasi, lisensi darurat, atau ketergantungan pada satu orang.

Kawan Codev.id, harga proyek yang rendah tidak otomatis berarti biaya siklus hidup rendah. NIST merekomendasikan pengelolaan risiko rantai pasok yang mempertimbangkan pemasok, komponen, dan bukti sepanjang siklus hidup ([NIST SP 800-161](https://csrc.nist.gov/pubs/sp/800/161/r1/final)). Prinsip secure-by-design juga mendorong agar keamanan dan pengoperasian tidak ditinggalkan sebagai pekerjaan penerima ([CISA Secure by Design](https://www.cisa.gov/securebydesign)). Minta estimasi berbasis item yang belum diserahterimakan, bukan menerima klaim “tinggal operasional”.

## Penerimaan dan keputusan akhir

Tetapkan sesi penerimaan dengan peran jelas: penyedia mendemokan, penerima menjalankan ulang, dan penanggung jawab bisnis menyetujui ruang lingkup. Simpan berita acara yang merujuk versi rilis, matriks item, hasil uji akses/build/restore, defect terbuka, pengecualian, serta tanggal tindak lanjut. Panduan Technology Code of Practice menekankan pengelolaan layanan, data, dan kemampuan internal sebagai bagian dari keputusan teknologi ([UK Technology Code of Practice](https://www.gov.uk/guidance/the-technology-code-of-practice)).

Jangan menandatangani penerimaan hanya karena demo berjalan. Tahan penerimaan atau buat penerimaan bersyarat ketika item kritis tidak dapat diverifikasi: misalnya organisasi belum memegang administrator domain, restore belum diuji, atau defect keamanan belum memiliki keputusan tertulis. Tanpa kontrak, bukti operasi aktual, dan kriteria yang disepakati, status 24/7, uptime, kapasitas, atau garansi tidak boleh disimpulkan—`[NEEDS OPERATING EVIDENCE/CONTRACT REVIEW]`.

### Pilihan cepat yang sering dipilih

Shortcut paling menggoda adalah menerima handover dalam satu folder dan menganggap sisanya akan dipelajari sambil jalan. Cara ini gagal ketika build, akses, atau pemulihan memerlukan konteks yang hanya dimiliki penyedia. Alternatif yang lebih aman: jadwalkan uji serah-terima berurutan—akses, checkout, build, deploy nonproduksi, restore, observability, lalu simulasi insiden—dan beri status lulus, bersyarat, atau tertunda pada tiap tahap.

## Langkah berikutnya

Checklist handover software dan bukti kepemilikan berarti setiap komponen punya lokasi, pemilik organisasi, versi, bukti verifikasi, dan keputusan penerimaan. Cetak matriks item di atas, minta penyedia mengisinya, lalu lakukan satu sesi uji ulang dengan tim yang akan mengoperasikan sistem.

Teman Codev.id, simpan berita acara beserta daftar kekurangan dan tanggal penutupannya; minta tinjauan teknis untuk risiko keamanan, lisensi, dan kontrak. Aturan operasionalnya sederhana: jangan menyebut software “sudah menjadi milik dan siap dioperasikan” sebelum akses dapat dikendalikan, build dapat diulang, pemulihan dapat dibuktikan, dan batas kontraknya tertulis.

Jika Anda perlu menyelaraskan langkah ini dengan konteks layanan yang lebih luas, mulai dari [halaman utama Codev.id](/).

Metadata aset dapat diperiksa melalui tautan aset ilustrasi.
