---
article_id: CDV-20-A03
title: "Studi Kasus Before-after dengan Baseline yang Adil"
slug: "studi-kasus-before-after-baseline"
description: "Cara menetapkan titik pembanding, jendela waktu, kelompok pengamatan, intervensi, faktor perancu, alat ukur, sebaran hasil, relevansi bisnis, batasan, dan kondisi terkini."
status: draft
publication_date: "2026-07-13"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-20
primary_intent: "Document a project outcome without misleading attribution"
reader_community: "Codev.id"
reader_address: "Kawan Codev.id"
final_route: "/artikel/studi-kasus-before-after-baseline.html"
technical_review: required
writing_contract_version: "native-id-v2"
sources:
  - "https://www.gov.uk/service-manual/service-standard"
  - "https://www.w3.org/TR/WCAG-EM/"
  - "https://www.gov.uk/guidance/the-technology-code-of-practice"
---

# Studi Kasus Before-after dengan Baseline yang Adil

Halo, Kawan Codev.id! Sebuah studi kasus before-after layak dipublikasikan bila pembaca dapat melihat apa kondisi awalnya, apa yang berubah, siapa yang dibandingkan, dan apa saja yang mungkin ikut memengaruhi hasil. Tanpa itu, angka setelah peluncuran hanya menunjukkan dua keadaan yang berbeda—bukan otomatis bukti bahwa perangkat lunak menjadi penyebab tunggal.

Jawaban singkatnya: tetapkan baseline sebelum menilai intervensi, pakai jendela waktu dan kohor yang sebanding, catat versi alat ukur, lalu tampilkan sebaran hasil serta batasannya. Klaim hasil kasus baru dapat diterbitkan setelah ada bukti proyek yang memverifikasi ruang lingkup, peran, persetujuan, dan hasilnya. **[NEEDS GATE-09: kontrak/penawaran saat ini serta bukti penyedia terverifikasi untuk peran, ruang lingkup, consent, hasil, dan handover.]**

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

Before-after bukan lomba mencari angka terbesar. Ia adalah catatan keputusan yang memungkinkan pembaca menguji apakah perbandingan itu masuk akal. Misalnya, peningkatan penyelesaian sebuah tugas setelah perubahan antarmuka belum membuktikan perubahan tersebut berhasil bila periode setelahnya bertepatan dengan kampanye, perubahan aturan layanan, atau masuknya jenis pengguna baru.

Yang perlu dibuktikan bukan hanya bahwa suatu halaman, aplikasi, atau proses ada. Screenshot, domain, logo, testimoni, dan halaman yang sedang aktif tidak dengan sendirinya membuktikan penulisannya, cakupan pekerjaan, kesesuaian, keamanan, maupun dampak bisnis. Praktik penilaian layanan juga menempatkan bukti kebutuhan pengguna dan pengukuran kinerja sebagai bagian dari keputusan layanan, bukan hiasan presentasi semata. [UK Government Service Standard](https://www.gov.uk/service-manual/service-standard)

Karena itu, tulis kesimpulan dengan tingkat kepastian yang sesuai. “Pada kohor dan periode yang dicatat, metrik X berubah setelah intervensi Y” lebih jujur daripada “Y menyebabkan X naik”, kecuali rancangan dan bukti proyek memang cukup untuk mendukung atribusi tersebut.

## Definisi dan batas objek

Baseline adalah rekaman kondisi sebelum intervensi dengan definisi metrik yang tetap. Rekaman ini harus menyebut peristiwa yang dihitung, sumber data, aturan pengecualian, serta waktu pengukuran. Jendela pengukuran adalah rentang tanggal atau sesi yang digunakan; kohor adalah kelompok pengguna, transaksi, atau pekerjaan yang dibandingkan. Ketiganya membuat pembaca tahu apakah yang dibandingkan benar-benar sejenis.

Intervensi adalah perubahan yang diuji: misalnya alur formulir diganti, informasi dibuat lebih jelas, atau proses internal diberi langkah baru. Jangan memperluasnya menjadi daftar seluruh aktivitas tim bila tidak semua aktivitas itu dapat dibuktikan. Hasil pula bukan satu angka tunggal; ia dapat mencakup sebaran, kasus gagal, pengecualian, dan kondisi operasi saat data direkam.

Batas pentingnya, artikel ini bukan panduan untuk memilih metrik performa atau mengejar peringkat. Ia membahas cara mendokumentasikan perubahan secara adil. Jika hasil menyangkut aksesibilitas, evaluasi perlu menyatakan cakupan, sampel, metode, dan temuan; metodologi evaluasi W3C memang menekankan ruang lingkup evaluasi yang didefinisikan dan pelaporan hasilnya. [W3C WCAG-EM](https://www.w3.org/TR/WCAG-EM/)

## Cara kerjanya

Mulailah dari satu keputusan pembaca: tugas atau hasil bisnis apa yang hendak diperbaiki? Tetapkan pemilik penerimaan yang menyetujui definisinya. Kemudian simpan snapshot baseline sebelum perubahan: tanggal, populasi, sumber data, versi aplikasi, konfigurasi instrumen, dan aturan perhitungannya. Bila sebuah alat analitik atau skrip berubah di tengah periode, versi tersebut adalah bagian dari konteks, bukan detail teknis yang boleh dibuang.

Sesudah itu, catat intervensi dalam bahasa yang dapat diperiksa: apa yang diubah, kapan tersedia, kepada siapa, dan perubahan terkait apa yang sengaja tidak dimasukkan. Pilih jendela sesudah perubahan yang cukup sebanding dengan baseline. Jangan mencampur hari kerja dengan hari libur, pengguna baru dengan pelanggan lama, atau data produksi dengan pengujian internal tanpa label.

Kawan Codev.id, hasilnya sebaiknya disajikan sebagai distribusi atau pemecahan yang relevan, bukan rerata yang menutupi variasi. Tampilkan jumlah observasi, rentang waktu, pembagian kohor, nilai yang dikecualikan beserta alasannya, dan peristiwa penting selama periode tersebut. Lalu hubungkan angka itu ke tugas nyata: apakah pengguna dapat menyelesaikan langkah yang dimaksud, atau apakah pemilik proses punya dasar untuk mengambil keputusan? Jangan melompat dari perubahan metrik ke janji pendapatan, keandalan, atau kualitas layanan tanpa bukti yang tepat.

Terakhir, minta pemilik data dan pihak yang berwenang meninjau draf. Bukti handover, kepemilikan, peran pihak-pihak terkait, dan izin untuk memakai hasil perlu tersimpan bersama bahan kasus. Prinsip pengadaan dan pengelolaan teknologi yang baik juga menekankan penilaian nilai sepanjang siklus hidup serta pengelolaan risiko, bukan semata harga awal atau tampilan portofolio. [UK Technology Code of Practice](https://www.gov.uk/guidance/the-technology-code-of-practice)

## Faktor yang mengubah hasil

Beberapa faktor berikut tidak selalu membuat studi kasus batal, tetapi harus dicatat karena dapat mengubah makna perbandingan.

| Faktor | Pertanyaan yang perlu dicatat | Dampak bila diabaikan |
| --- | --- | --- |
| Kohor | Apakah pengguna atau jenis pekerjaan sama pada dua periode? | Perubahan dapat berasal dari komposisi pengguna. |
| Waktu | Apakah ada musim, kampanye, libur, atau perubahan kebijakan? | Periode tidak lagi sebanding. |
| Instrumen | Apakah alat, event, filter, dan versinya tetap? | Selisih mungkin berasal dari cara menghitung. |
| Operasi | Apakah ada gangguan, perubahan proses manual, atau pelatihan? | Intervensi perangkat lunak bukan satu-satunya perubahan. |
| Data | Apa yang hilang, diduplikasi, atau dikecualikan? | Angka tampak rapi tetapi tidak mewakili keadaan. |

Sobat Codev.id, sebuah caveat bukan kelemahan tulisan. Caveat menjelaskan kondisi di mana pembaca boleh memakai temuan dan kondisi di mana mereka harus berhenti menyimpulkan. Bila penyebab perubahan tidak dapat dipisahkan dari faktor lain, nyatakan bahwa hasil hanya menunjukkan asosiasi dalam periode yang dicatat.

## Contoh keputusan praktis

Bayangkan sebuah tim ingin menulis bahwa perubahan alur pendaftaran memperbaiki penyelesaian tugas. Mereka memiliki data sebelum dan sesudah, tetapi pada minggu yang sama tim pemasaran juga mengubah sumber trafik. Keputusan yang aman bukan menolak semua data, melainkan mempersempit klaim dan menambah pemecahan kohor.

| Keadaan bukti | Cara menulis | Keputusan publikasi |
| --- | --- | --- |
| Baseline, kohor, instrumen, dan perubahan lain terdokumentasi; izin serta peran terverifikasi | Jelaskan perubahan pada periode dan kohor yang didefinisikan, berikut variasi serta caveat | Dapat diajukan untuk tinjauan teknis dan pemilik bukti |
| Data ada, tetapi sumber trafik atau proses lain berubah tanpa catatan yang cukup | Jelaskan bahwa perbandingan tidak dapat dipakai untuk atribusi | Tunda klaim hasil; publikasikan metode hanya bila scope dan izin aman |
| Hanya ada tangkapan layar atau cerita informal | Jangan menyebut hasil atau peran sebagai fakta | Kumpulkan bukti proyek terlebih dahulu |

Teman Codev.id, skenario ini tidak membutuhkan angka rekaan untuk berguna. Yang pembaca perlukan ialah alasan mengapa sebuah klaim dipersempit, bukti apa yang tersedia, dan tindakan apa yang harus dilakukan sebelum menyatakan keberhasilan.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah memilih baseline setelah hasil diketahui. Ini memberi ruang memilih periode yang paling menguntungkan. Perbaikannya: arsipkan definisi baseline dan jendela pengukuran sebelum membaca hasil akhir.

Kesalahan kedua adalah mengubah event, alat, atau filter lalu memperlakukan semua angka sebagai satu seri. Perbaikannya: beri nama versi instrumen dan jelaskan titik perubahan. Bila pemetaan lama dan baru tidak ekuivalen, jangan tampilkan selisih sebagai kemajuan.

Kesalahan ketiga adalah memakai satu angka rata-rata untuk menyimpulkan semua pengguna terbantu. Periksa pemecahan kohor, kasus gagal, dan data yang dikecualikan. Jika variasinya besar, tuliskan variasi itu dan hindari kesimpulan umum.

Shortcut yang sering menggoda adalah “cukup tunjukkan tampilan lama dan baru; pembaca akan mengerti.” Tampilan dapat membantu menjelaskan intervensi, tetapi tidak membuktikan siapa yang mengerjakan, apa cakupannya, atau hasil apa yang terjadi. Alternatifnya adalah paket bukti ringkas: definisi baseline, log intervensi, catatan confounder, versi instrumen, ringkasan distribusi, persetujuan penggunaan, dan status saat ini.

Gunakan pemeriksaan akhir berikut sebelum sebuah klaim dipublikasikan:

- Apakah baseline dan periode sesudahnya punya definisi metrik yang sama?
- Apakah kohor serta pengecualian dijelaskan sehingga pembaca dapat menilai kesetaraannya?
- Apakah perubahan lain selama periode dicatat, termasuk yang tidak dikendalikan?
- Apakah versi aplikasi dan alat ukur tercantum?
- Apakah hubungan ke tugas pengguna atau keputusan bisnis dijelaskan tanpa melebihkan sebab-akibat?
- Apakah pihak yang berwenang telah memverifikasi peran, scope, izin, dan hasil?

## Langkah berikutnya: siapkan paket bukti sebelum menulis

Studi kasus before-after yang adil bukan klaim bahwa semua perubahan berasal dari software. Ia adalah catatan terverifikasi tentang baseline, intervensi, kondisi pembanding, hasil yang tersebar, dan batas kesimpulan.

Mulailah dengan satu dokumen kerja yang memuat definisi metrik, jendela waktu, kohor, versi instrumen, daftar confounder, serta pemilik keputusan. Sebelum menambahkan kalimat tentang hasil proyek, minta bukti peran, scope, consent, dan hasil ditinjau oleh pemiliknya. Bila Anda perlu memeriksa konteks penerbitnya lebih dahulu, kembali ke [halaman utama Codev.id](/). Aturan operasinya sederhana: bila pembaca tidak dapat memeriksa perbandingannya, jangan minta mereka mempercayai atribusinya.
