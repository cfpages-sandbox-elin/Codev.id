---
article_id: CDV-15-A04
title: "Refactor, Replatform, Strangler, atau Rewrite"
slug: "refactor-replatform-strangler-atau-rewrite"
description: "Panduan memilih strategi modernisasi sistem lama dengan menimbang pemicu, ketergantungan, data, risiko, migrasi bertahap, verifikasi, pemulihan, biaya, dan batas berhenti"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2026-03-14"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-15
primary_intent: "Choose a legacy modernization strategy"
reader_community: "Codev.id"
reader_address: "Sobat Codev.id"
final_route: "/artikel/refactor-replatform-strangler-atau-rewrite.html"
technical_review: required
sources:
  - "https://www.cisa.gov/sbom"
  - "https://csrc.nist.gov/pubs/sp/800/161/r1/final"
  - "https://securityscorecards.dev/"
  - "https://csrc.nist.gov/Projects/ssdf/publications"
  - "https://www.cisa.gov/known-exploited-vulnerabilities-catalog"
  - "https://developers.google.com/search/docs/crawling-indexing/site-move-with-url-changes"
---

# Refactor, Replatform, Strangler, atau Rewrite

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

Halo, Sobat Codev.id!

Saat sistem lama sulit diubah, pilihan yang terdengar paling tegas—menulis ulang semuanya—sering bukan pilihan yang paling aman. Refactor cocok ketika perilaku bisnis masih benar tetapi struktur kode menghambat perubahan. Replatform cocok ketika kode relatif dapat dipertahankan, sementara platform atau runtime menjadi sumber risiko. Strangler (strangler pattern) cocok ketika fungsi dapat dipotong menjadi irisan kecil yang hidup berdampingan. Rewrite layak dipertimbangkan hanya jika batas penggantian, data, verifikasi, rollback, dan kapasitas tim sudah dibuktikan.

Jawaban itu bisa berubah setelah Anda memiliki baseline: dependensi yang benar-benar dipakai, alur data, kontrak antarmuka, paparan keamanan, pemilik keputusan, dan biaya menjalankan dua sistem. Jadi, jangan memilih berdasarkan usia sistem atau selera arsitektur. Pilih strategi yang mengurangi risiko terbesar pada irisan nilai berikutnya, lalu tetapkan kondisi berhenti sebelum pekerjaan dimulai.

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

*Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.*

## Definisi dan batas objek

Refactor mengubah struktur internal tanpa sengaja mengubah kontrak perilaku yang disepakati. Replatform memindahkan sistem ke platform, runtime, atau layanan operasi lain dengan perubahan kode seminimal mungkin. Strangler menempatkan lapisan perutean atau adapter sehingga satu kemampuan dapat dipindahkan bertahap; sistem lama tetap melayani bagian yang belum dipindah. Rewrite membangun implementasi baru berdasarkan kontrak yang dipilih, lalu memindahkan penggunaan dan data secara terkendali.

Keempat istilah itu bukan tingkat kualitas. Refactor tidak otomatis lebih murah, replatform tidak otomatis menyelesaikan desain buruk, dan rewrite tidak otomatis lebih bersih. Artikel ini membantu memilih mekanisme perubahan, bukan menetapkan arsitektur target atau memberi estimasi proyek. Harga, kapasitas, dan jadwal perlu dihitung pada paket kerja terpisah dengan data proyek yang nyata.

## Cara kerjanya

Mulailah dengan baseline yang dapat diperiksa: daftar endpoint dan job, dependensi langsung maupun transitif, pemilik, aliran data, jadwal batch, kredensial, serta jalur pemulihan. SBOM dapat memperjelas komponen yang ada, tetapi transparansi komponen bukan bukti bahwa semuanya aman ([CISA SBOM resources](https://www.cisa.gov/sbom)). Untuk vendor atau komponen pihak ketiga, NIST menekankan pengelolaan risiko rantai pasok; skor repositori seperti OpenSSF Scorecard hanya sinyal awal, bukan pengganti due diligence ([NIST SP 800-161 Rev.1](https://csrc.nist.gov/pubs/sp/800/161/r1/final), [OpenSSF Scorecard](https://securityscorecards.dev/)).

Setelah baseline, pilih satu value slice—alur kecil yang punya pemilik, kriteria berhasil, dan cara membatalkan. Pada refactor, ubah modul di belakang kontrak dan jalankan uji regresi. Pada replatform, pertahankan kontrak sambil menguji runtime, observability, konfigurasi, dan prosedur pemulihan. Pada strangler, arahkan hanya trafik slice tersebut ke komponen baru, bandingkan hasilnya, lalu perluas cakupan. Pada rewrite, bangun kontrak dan model verifikasi terlebih dahulu; jangan menunggu sampai akhir untuk mengetahui bahwa perilaku penting hilang.

Setiap irisan memerlukan urutan cutover, rekonsiliasi data, jendela rollback, dan pemilik keputusan. Jika dua sistem hidup bersamaan, definisikan sumber kebenaran, idempotensi, penanganan pesan terlambat, serta cara mengukur selisih. Untuk perubahan URL atau struktur publik, Google menyarankan inventaris URL, pemetaan, dan pemantauan setelah pemindahan—prinsip yang sama berguna untuk memastikan rute lama tidak diam-diam hilang ([Google Search site-move guidance](https://developers.google.com/search/docs/crawling-indexing/site-move-with-url-changes)).

## Faktor yang mengubah hasil

Pertama, bedakan pendorongnya. Jika masalah utama adalah kode yang rapuh tetapi platform stabil, refactor memberi umpan balik paling pendek. Jika operasi, dukungan runtime, atau batas platform menghambat, replatform mungkin cukup. Jika perubahan harus berjalan tanpa big-bang cutover, strangler mengurangi ukuran setiap langkah dengan konsekuensi biaya coexistence. Rewrite hanya mendapat nilai tambah bila eksperimen menunjukkan model lama tidak dapat memenuhi kontrak yang diperlukan.

Kedua, ukur ketergantungan dan data. Hubungan tersembunyi antarmodul, ekspor manual, integrasi vendor, dan job terjadwal sering menjadi sumber kegagalan. Catat pemilik, frekuensi, toleransi kehilangan, dan cara rekonsiliasi untuk setiap aliran. Jangan menghapus riwayat atau akses hanya karena komponen sudah tua; [NEEDS GATE-02: inventaris data, retensi, dan pemilik harus diverifikasi sebelum penghapusan atau decommissioning].

Ketiga, nilai risiko berdasarkan paparan dan dampak, bukan label kerentanan saja. Praktik pengembangan aman NIST dan katalog CISA membantu memprioritaskan perbaikan, tetapi keputusan tetap perlu mempertimbangkan apakah komponen terekspos, sedang dieksploitasi, aman diperbaiki, memiliki rollback, dan punya pemilik ([NIST SSDF publications](https://csrc.nist.gov/Projects/ssdf/publications), [CISA Known Exploited Vulnerabilities Catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog)). Jika vendor tidak menjelaskan versi API, kuota, subprosesor, atau jadwal perbaikan, catat sebagai ketidakpastian—bukan asumsi yang sudah beres.

Keempat, tetapkan bukti dan stop rule. Bukti dapat berupa hasil regresi, rekonsiliasi jumlah dan identitas data, error rate, waktu pemulihan yang disepakati, serta persetujuan pemilik proses. Stop bila selisih data melewati ambang yang belum disetujui, rollback tidak dapat diuji, atau tidak ada orang yang berwenang memutuskan. Untuk data dan akses, [NEEDS GATE-05: kriteria rekonsiliasi, otorisasi cutover, dan prosedur rollback perlu disetujui pemilik proses]. Untuk keamanan dan kepatuhan, [NEEDS GATE-08: review sebelum cutover dan decommissioning belum tersedia dalam paket ini].

Teman Codev.id, dokumentasikan juga alasan menolak tiga strategi lain. Catatan singkat itu mencegah tim kembali ke debat selera ketika baseline berubah. Tulis asumsi yang masih terbuka, pemilik yang harus mengonfirmasi, bukti yang belum ada, dan tanggal keputusan berikutnya. Bila satu asumsi runtuh—misalnya vendor mengubah kontrak atau data ternyata tidak dapat direkonsiliasi—kembali ke tahap baseline, bukan memaksa rencana lama.

## Contoh keputusan praktis

Gunakan tabel ini sebagai hipotesis awal, bukan vonis tanpa baseline.

| Kondisi yang terbukti | Strategi awal | Bukti sebelum memperluas |
|---|---|---|
| Kontrak perilaku masih stabil, perubahan terhambat struktur internal | Refactor | Uji regresi pada satu modul, observability, dan rollback commit/deploy |
| Kode dapat dipertahankan, tetapi runtime atau platform tidak lagi memenuhi kebutuhan operasi | Replatform | Uji kompatibilitas, konfigurasi rahasia, backup-restore, dan beban pada lingkungan target |
| Perlu rilis bertahap dan fungsi dapat dipisahkan lewat kontrak | Strangler | Slice memiliki pemilik, rute dapat diarahkan balik, hasil lama-baru dapat direkonsiliasi |
| Eksperimen menunjukkan kontrak/model lama mustahil dipenuhi, dan migrasi dapat dipagari | Rewrite | Kontrak baru, fixture perilaku penting, migrasi data bertahap, rollback, serta kapasitas pemeliharaan |

Misalnya, sebuah tim menemukan endpoint penagihan bergantung pada job lama dan tiga vendor. Tanpa mengarang angka keberhasilan, langkah rasional adalah memetakan kontrak dan pemilik, membuat adapter, lalu menguji satu alur penagihan sebagai slice strangler. Bila masalah ternyata hanya struktur modul, refactor di belakang adapter dapat mengurangi pekerjaan. Bila runtime menjadi sumber insiden, replatform dapat diuji setelah kontrak stabil. Rewrite baru masuk daftar setelah eksperimen membuktikan aturan bisnis lama tidak dapat dipisahkan atau diuji.

Kawan Codev.id, perhatikan biaya coexistence: dua pipeline, dua monitoring, rekonsiliasi, dan pengetahuan operasional. Biaya itu bisa sepadan bila menghindari cutover besar, tetapi harus terlihat di rencana dan memiliki tanggal evaluasi. Jika tidak ada slice yang dapat diukur atau rollback yang realistis, berhenti dan minta review teknis sebelum memilih metode.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah memilih rewrite karena kode lama terasa memalukan. Periksa apakah kontrak, data, dan dependensi benar-benar diketahui; rasa frustrasi bukan bukti ketidakmungkinan evolusi. Kedua, menganggap SBOM atau skor repositori sebagai sertifikat keamanan. Cocokkan daftar komponen dengan paparan, pemilik, dan jalur perbaikan. Ketiga, memindahkan kode tanpa memindahkan operasi: secrets, logging, alert, backup, dan akses harus diuji sebagai bagian dari slice.

Kesalahan keempat adalah menguji hanya jalur sukses. Tambahkan kasus timeout, pesan duplikat, data terlambat, vendor tidak tersedia, dan rollback di setiap value slice. Kelima, mengabaikan URL, identitas, atau referensi silang saat migrasi. Buat inventaris dan rekonsiliasi sebelum cutover, lalu pantau rute lama dan baru. Keenam, menetapkan “selesai” saat komponen baru hidup, padahal akses dan data lama belum diputuskan; minta pemilik menyetujui retensi, ekspor, dan penghapusan.

## Jalan pintas yang sering menggoda

Jalan pintas paling umum adalah memindahkan semuanya sekaligus agar tim tidak perlu memelihara dua sistem. Itu menggabungkan ketidakpastian dependensi, data, operasi, dan perilaku menjadi satu momen yang sulit dipulihkan. Alternatif yang lebih dapat diperiksa adalah memulai dari slice bernilai, mencatat metrik pembanding, dan hanya memperluas setelah stop rule terlewati. Jika sebuah slice gagal, rollback terbatas memberi informasi; kegagalan big-bang hanya memberi tagihan dan kebingungan.

## Kesimpulan

Refactor, replatform, strangler, dan rewrite adalah pilihan berdasarkan kendala yang terbukti, bukan urutan prestise. Refactor menjaga kontrak sambil merapikan internal, replatform mengubah landasan operasi, strangler memecah perpindahan menjadi irisan yang dapat dibatalkan, dan rewrite menuntut bukti bahwa evolusi bertahap tidak memadai.

Langkah berikutnya: buat baseline dependensi-data, pilih satu value slice, tulis kontrak dan kriteria rekonsiliasi, uji rollback, lalu minta review pemilik proses dan teknis. Untuk keputusan yang menyentuh retensi, akses, keamanan, atau penghapusan, selesaikan [NEEDS GATE-02], [NEEDS GATE-05], dan [NEEDS GATE-08] sebelum cutover. Mulai dari halaman [Codev.id](/) bila Anda memerlukan konteks layanan, tetapi jangan menganggap artikel ini sebagai persetujuan proyek. Aturan operasionalnya sederhana: tidak ada perluasan tanpa bukti slice dan rollback yang dapat dijalankan.
