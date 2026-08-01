---
article_id: CDV-16-A02
title: "Human-in-the-loop, Approval, dan Jalur Banding"
slug: "human-in-the-loop-approval-banding"
description: "Panduan membagi wewenang keputusan, gerbang kondisi, informasi peninjauan, pembatalan, banding, antrean, waktu tunggu, beban kerja, audit, dan jalur cadangan"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2026-03-31"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-16
primary_intent: "Assign meaningful human control over automated decisions"
reader_community: "Codev.id"
reader_address: "Teman Codev.id"
final_route: "/artikel/human-in-the-loop-approval-banding.html"
technical_review: required
sources:
  - "https://peraturan.bpk.go.id/Details/229798/uu-no-27-tahun-2022"
  - "https://peraturan.bpk.go.id/Details/122030/pp-no-71-tahun-2019"
  - "https://www.nist.gov/privacy-framework"
  - "https://www.nist.gov/itl/ai-risk-management-framework"
  - "https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf"
  - "https://csrc.nist.gov/pubs/sp/800/218/a/final"
---

# Human-in-the-loop, Approval, dan Jalur Banding

Halo, Teman Codev.id! Persetujuan manusia pada alur otomatis baru berarti jika orang yang menyetujui memiliki wewenang, informasi yang cukup, waktu yang realistis, dan jalan untuk membatalkan keputusan. Tombol **Approve** yang ditekan karena antrean menumpuk hanyalah formalitas, bukan pengawasan.

Jawaban singkatnya: petakan keputusan mana yang boleh diusulkan sistem, mana yang wajib disetujui manusia, dan mana yang harus dihentikan atau dialihkan. Pasang gerbang kondisi (confidence dan sinyal risiko), tampilkan alasan serta data yang dipakai, sediakan override dan jalur banding, lalu ukur antrean, keterlambatan, beban reviewer, dan jejak audit. Jika salah satu unsur itu belum tersedia, turunkan otonomi sistem ke mode rekomendasi atau fallback manual. Batas hukum dan domain tetap harus ditetapkan oleh ahli yang berwenang.

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

*Ilustrasi umum dari aset lokal codev.id; bukan dokumentasi proyek tertentu.* Ilustrasi ini bukan bukti performa, kepatuhan, atau hasil sistem tertentu.

## Definisi dan batas objek

**Human-in-the-loop** adalah rancangan kerja ketika manusia hadir di titik keputusan yang bermakna, bukan sekadar tercatat sebagai nama pemeriksa. Approval adalah keputusan eksplisit untuk melanjutkan tindakan tertentu. Jalur banding (appeal) memberi pihak yang terdampak kesempatan meminta peninjauan ulang dengan informasi tambahan atau pemeriksa berbeda. Override adalah kemampuan berwenang untuk mengubah atau menghentikan hasil sistem, dengan alasan yang tercatat.

Objek yang dibahas di sini adalah pembagian hak keputusan dalam pekerjaan otomatis yang konsekuensial atau sarat pengecualian: penyaringan, prioritas, rekomendasi, atau tindakan yang dapat merugikan seseorang maupun operasi. Artikel ini tidak menyatakan bahwa kehadiran manusia otomatis membuat penggunaan aman, sah, atau layak untuk keputusan berisiko tinggi. Undang-Undang Pelindungan Data Pribadi dan PP 71/2019 memberi konteks penting tentang data pribadi dan sistem elektronik, tetapi penerapan kewajiban spesifik perlu ditentukan melalui telaah hukum dan domain. Lihat [UU No. 27 Tahun 2022](https://peraturan.bpk.go.id/Details/229798/uu-no-27-tahun-2022) dan [PP No. 71 Tahun 2019](https://peraturan.bpk.go.id/Details/122030/pp-no-71-tahun-2019) sebagai rujukan resmi, bukan pengganti nasihat profesional.

Prinsipnya, jangan menjadikan “ada reviewer” sebagai klaim hasil. Yang perlu dibuktikan adalah apakah reviewer dapat memahami kasus, menolak rekomendasi, memanggil data yang hilang, dan mengembalikan pekerjaan ke jalur aman.

## Cara kerjanya

Mulailah dari matriks hak keputusan. Untuk setiap jenis keluaran, tetapkan: sistem hanya mengusulkan, manusia menyetujui, dua pihak harus menyetujui, atau sistem sama sekali tidak boleh bertindak. Tulis pula siapa pemilik keputusan, siapa pengganti ketika ia tidak tersedia, dan kondisi yang memicu eskalasi.

Urutan operasional yang dapat diuji:

1. Sistem menerima input dan memeriksa kelengkapan, hak akses, serta tanda data yang tidak lazim.
2. Sistem menghasilkan rekomendasi beserta alasan singkat, sumber data, waktu pembuatan, dan tingkat keyakinan yang didefinisikan secara internal—bukan angka yang dianggap benar tanpa evaluasi.
3. Gerbang kondisi menerapkan aturan: rekomendasi dengan informasi kurang, konflik, atau risiko di atas ambang tidak boleh langsung dieksekusi.
4. Reviewer melihat paket bukti yang sama, memilih setujui, tolak, minta informasi, atau eskalasi. Semua pilihan memerlukan alasan yang proporsional terhadap dampaknya.
5. Pemohon atau pihak terdampak diberi kanal banding yang jelas, batas waktu respons yang realistis, dan status perkara yang dapat dilacak.
6. Jika antrean, layanan, atau data pendukung gagal, sistem berpindah ke fallback yang telah ditentukan—misalnya penanganan manual atau penundaan yang aman—bukan terus berjalan diam-diam.

Pada setiap transisi, catat input yang relevan, versi aturan atau model, identitas peran (bukan data berlebihan), keputusan, alasan, override, dan waktu. NIST menekankan bahwa keluaran yang lancar bukan jaminan benar; evaluasi perlu mewakili tugas serta skenario gagal dan disalahgunakan. Prinsip tata kelola, pengukuran, dan pengelolaan risiko dapat dibandingkan dengan [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework), [NIST AI 600-1 Generative AI Profile](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf), dan praktik pengembangan aman di [NIST SP 800-218A](https://csrc.nist.gov/pubs/sp/800/218/a/final).

## Faktor yang mengubah hasil

**Hak keputusan.** Reviewer tanpa wewenang menolak hanya menjadi stempel. Pastikan peran, delegasi, dan konflik kepentingan ditulis sebelum peluncuran.

**Informasi.** Ringkasan harus menjawab apa yang terjadi, data mana yang dipakai, apa yang hilang, dan konsekuensi tiap pilihan. Jangan menampilkan data pribadi lebih banyak dari yang diperlukan. [NIST Privacy Framework](https://www.nist.gov/privacy-framework) membantu memetakan data, kontrol, dan siklus hidup; detail dasar pemrosesan, retensi, transfer, atau penghapusan memerlukan [NEEDS DOMAIN/LEGAL REVIEW: tetapkan kewajiban spesifik, retensi, dan hak pihak terdampak sebelum produksi].

**Ambang dan pengecualian.** Confidence rendah bukan satu-satunya sinyal. Konflik antar-sumber, perubahan pola, dampak tidak dapat dipulihkan, atau permintaan banding juga harus memicu manusia. Ambang harus diuji pada contoh normal, gagal, dan penyalahgunaan; jangan mengarang angka akurasi atau ambang universal.

**Antrean dan latensi.** Tetapkan kapasitas reviewer, aturan prioritas, jam layanan, dan kapan kasus dialihkan. Ukur usia kasus tertua, waktu tunggu, rasio eskalasi, serta berapa banyak approval dilakukan tanpa membuka bukti. Jika target waktu mendorong persetujuan otomatis, target itu justru menjadi risiko desain.

**Override, banding, dan pemulihan.** Override harus dapat dibatalkan kembali, mengharuskan alasan, dan menandai dampak lanjutan. Jalur banding sebaiknya memisahkan pemeriksa awal dari pemeriksa ulang ketika itu relevan. Siapkan cara memperbaiki keputusan yang telanjur diterapkan serta memberi tahu sistem hilir.

**Audit dan ketahanan.** Log yang tidak pernah ditinjau tidak membuktikan pengawasan. Jadwalkan sampling, uji pemulihan dari backup, dan simulasi ketika model, penyedia, atau layanan antrean tidak tersedia. Rencana pengembangan aman NIST juga menempatkan pemantauan dan retirement sebagai aktivitas siklus hidup, bukan pekerjaan sekali selesai.

## Contoh keputusan praktis

Bayangkan sistem menyarankan apakah sebuah permintaan operasional perlu diprioritaskan. Untuk permintaan rutin dengan data lengkap dan dampak yang mudah dibalik, sistem boleh mengusulkan urutan; operator tetap dapat mengubahnya. Jika data utama bertentangan atau dampaknya sulit dipulihkan, sistem hanya membuat tiket eskalasi. Jika pihak terdampak mengajukan banding dengan bukti baru, keputusan awal dibekukan bila memungkinkan sampai pemeriksa ulang menyelesaikan tinjauan.

Gunakan tabel keputusan sederhana berikut sebagai artefak awal, bukan aturan universal:

| Kondisi | Hak sistem | Tindakan manusia | Fallback |
|---|---|---|---|
| Data lengkap, risiko rendah, dampak dapat dibalik | Usulan dan eksekusi terbatas sesuai mandat | Sampling dan override | Penundaan aman |
| Confidence atau kelengkapan di bawah ambang internal | Tidak boleh mengeksekusi | Periksa bukti, minta data, atau tolak | Jalur manual |
| Konflik data, pengecualian baru, dampak sulit dibalik | Hanya membuat eskalasi | Reviewer berwenang memutuskan | Hentikan tindakan |
| Banding dengan informasi tambahan | Jangan menutup kasus otomatis | Pemeriksa ulang menilai bukti dan keputusan awal | Pemulihan/komunikasi |

Kawan Codev.id, uji tabel ini dengan pertanyaan “siapa dapat menghentikan apa, sebelum kapan, dan dengan bukti apa?” Jika jawabannya bergantung pada satu orang yang selalu tersedia, kontrol itu belum tangguh.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah menaruh reviewer di akhir antrean tanpa waktu atau konteks. Periksa median dan kasus ekstrem waktu tunggu, lalu baca sampel log untuk memastikan bukti benar-benar dibuka.

Kedua, menjadikan skor confidence sebagai kebenaran. Minta evaluasi terpisah untuk kesalahan yang paling merugikan, kasus batas, dan pola penyalahgunaan. Tanpa set evaluasi yang mewakili tugas, skor tersebut hanya sinyal.

Ketiga, menyediakan tombol banding tetapi tidak menetapkan pemilik dan hasil yang mungkin. Uji satu perkara dari pengajuan sampai keputusan ulang: apakah status terlihat, bukti baru tersimpan, dan tindakan hilir bisa dipulihkan?

Keempat, menganggap backup sama dengan pemulihan. Dokumentasikan uji restore, siapa yang menjalankan, dan apa yang gagal. Kerangka privasi NIST juga mendorong pemetaan siklus hidup data; jangan menyimpan salinan yang tidak memiliki tujuan dan kontrol.

Kelima, mengabaikan fallback karena “sistem jarang down”. Lakukan simulasi kehilangan penyedia, perubahan versi, atau lonjakan antrean. Jika prosedur manual tidak memiliki formulir, otorisasi, dan rekonsiliasi, fallback itu hanya tulisan.

## Jalan pintas yang sering dipilih

Shortcut yang menggoda adalah approval massal: reviewer menekan satu tombol untuk puluhan rekomendasi agar target waktu tercapai. Mekanismenya jelas—konteks per kasus hilang, pengecualian tertutup, dan audit hanya menunjukkan klik—sehingga pengawasan berubah menjadi formalitas.

Alternatif yang lebih dapat dipertanggungjawabkan adalah membatasi batch pada kondisi homogen yang telah diuji, menampilkan ringkasan per kasus, memaksa pemeriksaan sampel berisiko, serta memindahkan pengecualian ke jalur manual. Tetapkan hak untuk membatalkan batch dan telusuri dampaknya. Sobat Codev.id, bila organisasi belum dapat menyediakan informasi, wewenang, dan kapasitas tersebut, turunkan sistem menjadi rekomendasi yang tidak mengeksekusi tindakan.

## Kesimpulan: kendali manusia harus dapat digunakan

Human-in-the-loop yang bermakna bukan label, melainkan rantai kendali: hak keputusan yang jelas, gerbang kondisi, paket informasi, override, banding, antrean yang terukur, audit, dan fallback yang benar-benar bisa dijalankan. Sebelum produksi, buat satu lembar matriks keputusan dan lakukan uji kasus normal, gagal, penyalahgunaan, serta pemulihan.

Teman Codev.id, minta penanggung jawab domain dan penasihat hukum memeriksa batas keputusan, data, dan komunikasi yang berlaku pada konteks Anda. Anda dapat memulai dari [beranda Codev.id](/) untuk menata langkah kerja berikutnya. Aturan operasionalnya sederhana: jika reviewer tidak dapat memahami, menolak, dan memulihkan keputusan tepat waktu, otomatisasi tidak boleh bertindak sendiri.

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
