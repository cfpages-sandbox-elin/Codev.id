---
article_id: CDV-12-A01
writing_contract_version: "native-id-v2"
title: "SLI, SLO, dan Error Budget yang Terhubung ke Pengguna"
slug: "sli-slo-error-budget-pengguna"
description: "Panduan menghubungkan perjalanan pengguna, indikator layanan, target waktu, error budget, dan aturan keputusan operasional."
status: draft
publication_date: "2025-12-23"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-12
primary_intent: "Define measurable service reliability from user outcomes"
reader_community: "Codev.id"
reader_address: "Teman Codev.id"
final_route: "/artikel/sli-slo-error-budget-pengguna.html"
technical_review: required
sources:
  - "https://sre.google/workbook/implementing-slos/"
  - "https://opentelemetry.io/docs/"
  - "https://csrc.nist.gov/pubs/sp/800/61/r3/final"
  - "https://web.dev/articles/vitals"
  - "https://developer.chrome.com/docs/crux"
  - "https://www.rfc-editor.org/rfc/rfc9111"
---

# SLI, SLO, dan Error Budget yang Terhubung ke Pengguna

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

Halo, Teman Codev.id!

Jika pemilik layanan hanya menetapkan “uptime 99,9%”, ia belum tentu tahu apakah pengguna berhasil menyelesaikan pekerjaan. Ukuran yang lebih berguna adalah menghubungkan tiga hal: SLI (Service Level Indicator) sebagai pengukuran kejadian nyata, SLO (Service Level Objective) sebagai target internal, dan error budget sebagai ruang kegagalan yang masih dapat diterima dalam periode tertentu. Ketiganya dimulai dari perjalanan pengguna, bukan dari metrik server yang mudah dikumpulkan.

Jawaban singkatnya: pilih perjalanan kritis, ukur keberhasilannya dari sudut pengguna, tetapkan target dan jendela waktu berdasarkan baseline yang benar-benar ada, lalu gunakan selisih menuju target sebagai dasar keputusan rilis, perbaikan, dan respons insiden. SLO adalah tujuan operasional dan mekanisme keputusan, bukan janji kontraktual SLA. Angka target tidak boleh diisi dari tebakan; [NEEDS BASELINE REVIEW: GATE-07/GATE-08] diperlukan sebelum target dipakai untuk komitmen atau eskalasi.

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

*Aset lokal proyek; bukan dokumentasi proyek tertentu.*

## Jawaban singkat dan salah paham utama

Mulailah dengan pertanyaan, “Apa yang harus berhasil bagi pengguna?” Misalnya, pengguna dapat masuk, mengirim formulir, atau menerima hasil pembayaran. SLI untuk perjalanan itu bisa berupa proporsi permintaan yang selesai dengan respons valid dalam batas waktu yang disepakati. SLO lalu menyatakan tingkat keberhasilan yang dituju selama jendela harian, mingguan, atau bulanan.

Kesalahan paling berbahaya adalah menyamakan banyaknya sinyal dengan reliabilitas. Instrumentasi hanya membuat sinyal; ia tidak otomatis membuktikan layanan andal. Dokumentasi OpenTelemetry menjelaskan cara mengumpulkan telemetry seperti metrics, logs, dan traces, tetapi kualitas keputusan tetap bergantung pada definisi kejadian dan konteksnya ([OpenTelemetry documentation](https://opentelemetry.io/docs/)).

Error budget adalah porsi ketidakberhasilan yang tersisa dari SLO. Budget ini bukan izin merusak layanan; ia alat untuk menimbang kecepatan perubahan terhadap risiko. Saat budget menipis, tim dapat menunda perubahan berisiko dan memprioritaskan pemulihan. Kerangka SRE menempatkan SLO sebagai tujuan dan keputusan operasional, bukan SLA untuk pelanggan ([Google SRE Workbook—SLOs](https://sre.google/workbook/implementing-slos/)).

## Definisi dan batas objek

SLI harus memiliki numerator dan denominator yang dapat diaudit. “Permintaan sukses” perlu didefinisikan: kode hasil, status bisnis, dan batas waktu apa yang dihitung? Denominator harus menjelaskan permintaan mana yang masuk populasi. Tanpa itu, dashboard dapat terlihat hijau karena permintaan gagal dikeluarkan dari perhitungan.

SLO terdiri dari indikator, target, jendela waktu, dan aturan pengecualian. Pengecualian harus eksplisit, misalnya traffic uji yang ditandai atau endpoint administratif yang memang disepakati. Jangan mengecualikan semua kejadian sulit agar target tercapai. SLA kontraktual, kompensasi, dan kewajiban pelanggan berada di dokumen perjanjian lain; halaman ini tidak menetapkan atau menjanjikannya.

Untuk pengalaman web, Core Web Vitals membantu memantau aspek loading, interaksi, dan stabilitas visual. Metrik tersebut didefinisikan dan dapat berkembang, sehingga ambang serta alatnya perlu diperiksa ulang ([web.dev Core Web Vitals](https://web.dev/articles/vitals)). Data lapangan dari Chrome UX Report juga memiliki cakupan dan metodologi tersendiri ([Chrome UX Report documentation](https://developer.chrome.com/docs/crux)). Jangan mengubah metrik provider menjadi janji ranking, konversi, atau waktu muat universal.

## Cara kerjanya

Urutannya praktis. Pertama, petakan perjalanan kritis dan titik selesai yang terlihat pengguna. Kedua, pasang telemetry pada batas layanan dan klien; cocokkan request ID agar satu kegagalan tidak dihitung sebagai beberapa kejadian. Ketiga, hitung SLI dari sumber yang sama secara konsisten. Keempat, pilih SLO dan jendela waktu setelah melihat baseline, variasi traffic, serta kemampuan tim merespons.

Kelima, hitung error budget secara berkala dan tampilkan tren, bukan hanya angka saat ini. Keenam, pasang alert yang memproyeksikan laju konsumsi budget sehingga tim mendapat waktu bertindak. Ketujuh, hubungkan status budget dengan keputusan: lanjutkan perubahan berisiko rendah, minta canary atau rollback, atau hentikan rilis sementara untuk perbaikan reliabilitas. Panduan respons insiden NIST menekankan persiapan, deteksi, respons, dan pembelajaran; temuan insiden seharusnya kembali memperbaiki definisi sinyal dan prosedur ([NIST SP 800-61 Rev. 3](https://csrc.nist.gov/pubs/sp/800/61/r3/final)).

Sumber data harus diberi pemilik. Log memberi konteks kejadian, metric memberi agregat dan tren, trace membantu menelusuri lintasan lintas komponen. Simpan metadata versi, wilayah, perangkat, dan status pengecualian secukupnya untuk membedakan regresi dari perubahan populasi. Untuk cache HTTP, perilaku freshness dan validasi ditentukan oleh aturan cache; jangan menyimpulkan bahwa cache selalu mempercepat semua pengguna ([RFC 9111](https://www.rfc-editor.org/rfc/rfc9111)).

## Faktor yang mengubah hasil

Hasil SLO berubah ketika perjalanan pengguna berubah. Login yang sukses tetapi halaman berikutnya gagal bukan keberhasilan penuh. Perubahan API, feature flag, dependensi pihak ketiga, dan pola traffic dapat menggeser denominator. Segmentasi menurut perangkat atau wilayah dapat menemukan kegagalan yang tertutup oleh rata-rata global.

Jendela waktu juga menentukan keputusan. Jendela pendek cepat menangkap regresi tetapi lebih berisik; jendela panjang stabil tetapi dapat terlambat memberi sinyal. Pilih sesuai ritme perubahan dan kemampuan respons, lalu dokumentasikan alasannya.

Kualitas bukti lebih penting daripada presisi desimal. Klaim sebelum-sesudah memerlukan scope, sampel, kondisi, versi, dan caveat yang stabil. Tanpa itu, penurunan latency dapat berasal dari perubahan traffic, cache, atau alat ukur, bukan perbaikan kode. Sobat Codev.id, perlakukan angka sebagai pertanyaan investigasi sampai konteksnya lengkap.

## Contoh keputusan praktis

Gunakan tabel keputusan sederhana berikut sebagai aturan awal, lalu sesuaikan dengan risiko layanan:

| Kondisi | Pembacaan | Keputusan operasional |
|---|---|---|
| Budget masih longgar dan tren stabil | Perubahan berada dalam toleransi | Lanjutkan rilis dengan observasi dan rollback siap pakai. |
| Budget terkonsumsi cepat | Laju kegagalan mengancam target | Hentikan perluasan rilis, cari penyebab, dan siapkan mitigasi. |
| SLI tidak lengkap atau denominator berubah | Bukti tidak sebanding | Jangan mengubah target; perbaiki instrumentasi dan definisi dulu. |
| Insiden selesai tetapi pola berulang | Pemulihan belum menjadi pembelajaran | Catat tindakan pencegahan, pemilik, dan tanggal verifikasi. |

Contoh bersyarat: bila perjalanan “kirim formulir” memiliki banyak respons sukses di server tetapi pengguna melaporkan konfirmasi tidak muncul, SLI perlu mencakup titik konfirmasi yang benar-benar diterima klien. Jika sumber hanya menghitung HTTP 200, target dapat tampak tercapai sementara tujuan pengguna gagal. Itu alasan pemilihan indikator harus ditinjau bersama pemilik produk dan operasi.

## Kesalahan umum dan cara memeriksanya

Shortcut pertama adalah memilih target populer tanpa baseline. Periksa catatan periode pengukuran, populasi, dan perubahan sistem sebelum menetapkan angka. Shortcut kedua adalah membuat satu SLO untuk seluruh layanan. Pecah berdasarkan perjalanan yang memiliki dampak dan toleransi risiko berbeda.

Shortcut ketiga adalah mengecualikan insiden manual atau pihak ketiga secara otomatis. Tanyakan apakah pengguna tetap merasakan kegagalan; jika ya, pengecualian itu harus dijelaskan, bukan disembunyikan. Shortcut keempat adalah mengalert setiap anomali. Alert harus memicu tindakan yang jelas; sinyal yang tidak memiliki pemilik hanya menambah kebisingan.

Checklist pemeriksaan sebelum menyetujui SLO:

- Apakah perjalanan dan kondisi sukses dapat dijelaskan dalam satu kalimat?
- Apakah numerator, denominator, sumber data, dan pengecualian dapat diaudit?
- Apakah jendela waktu selaras dengan ritme rilis dan respons?
- Siapa pemilik alert, keputusan rollback, dan tindak lanjut insiden?
- Bukti apa yang akan mengubah target atau aturan budget?

## Kesimpulan dan langkah berikutnya

SLI, SLO, dan error budget terhubung ke pengguna ketika indikator menghitung keberhasilan perjalanan nyata, target ditetapkan dari baseline yang dapat diaudit, dan budget memandu keputusan perubahan serta pemulihan. Buat satu lembar definisi untuk perjalanan paling kritis: kondisi sukses, sumber data, jendela waktu, pengecualian, pemilik, dan aturan saat budget menipis.

Kawan Codev.id, uji lembar itu pada satu periode pengukuran dan satu latihan insiden sebelum memperluas cakupan. Minta review teknis atas baseline, kualitas telemetry, dan dampak pengecualian; gunakan [beranda Codev.id](/) bila perlu kembali ke konteks layanan. Jangan mengubahnya menjadi SLA atau janji universal. Aturan operasionalnya sederhana: bila bukti tidak sebanding, perbaiki pengukuran lebih dulu—jangan mempercantik target.
