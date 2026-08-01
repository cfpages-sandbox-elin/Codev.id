---
article_id: CDV-03-A02
title: "Monolith, Modular, Serverless, atau Layanan Terpisah"
slug: "monolith-modular-serverless-atau-layanan-terpisah"
description: "Perbandingan ketergantungan, rilis, data, kegagalan, pengujian, tim, latensi, biaya, dan migrasi"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2025-05-13"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-03
primary_intent: "Compare software structural styles by real constraints"
reader_community: "Codev.id"
reader_address: "Sobat Codev.id"
final_route: "/artikel/monolith-modular-serverless-atau-layanan-terpisah.html"
technical_review: required
sources:
  - "https://docs.aws.amazon.com/prescriptive-guidance/latest/architectural-decision-records/adr-process.html"
  - "https://html.spec.whatwg.org/"
  - "https://www.rfc-editor.org/rfc/rfc9110"
  - "https://www.cisa.gov/sbom"
  - "https://csrc.nist.gov/pubs/sp/800/161/r1/final"
  - "https://securityscorecards.dev/"
---

# Monolith, Modular, Serverless, atau Layanan Terpisah

Halo, Sobat Codev.id! Memilih bentuk arsitektur bukan lomba mencari label paling modern. Untuk kebanyakan produk baru, monolith yang modular adalah titik awal yang paling mudah diuji dan dioperasikan; layanan terpisah atau serverless baru masuk akal ketika batas tim, pola beban, atau kebutuhan isolasi memang sudah terbukti. Keputusan itu dapat berubah setelah Anda punya data trafik, batas latensi, kompetensi tim, dan persyaratan operasional yang nyata.

Jadi, jangan memutuskan dari kata *microservices* atau *serverless* saja. Bandingkan coupling (ketergantungan), cara deploy, kepemilikan data, pola kegagalan, beban pengujian, kemampuan tim, latensi, biaya, dan jalan migrasinya. Catat alasan serta hal yang masih belum diketahui dalam keputusan arsitektur; panduan AWS tentang Architecture Decision Record (ADR) memang menempatkan konteks, opsi, dan konsekuensi sebagai bagian keputusan yang bisa dilacak ([AWS ADR guidance](https://docs.aws.amazon.com/prescriptive-guidance/latest/architectural-decision-records/adr-process.html)).

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

*Ilustrasi umum dari aset lokal codev.id; bukan dokumentasi proyek tertentu.*

## Jawaban singkat dan salah paham utama

Monolith berarti satu unit aplikasi yang biasanya dibangun dan dirilis bersama. Modular monolith tetap satu unit deploy, tetapi kode dibagi ke modul dengan batas tanggung jawab dan antarmuka yang tegas. Layanan terpisah memecah unit deploy menjadi beberapa proses yang berkomunikasi melalui jaringan. Serverless memindahkan sebagian pengelolaan server ke penyedia; fungsi atau layanan tetap harus dipilih, diamankan, diuji, dan dipantau. Keempatnya adalah pilihan struktur, bukan jenjang kematangan.

Kesalahan paling mahal adalah menganggap pemisahan proses otomatis mengurangi coupling. Coupling dapat berpindah dari kode ke skema data, kontrak API, antrean, konfigurasi, dan prosedur rilis. Sebaliknya, monolith bukan alasan untuk membiarkan semua modul saling mengakses tabel dan variabel internal. Pilihan yang aman adalah yang membuat perubahan penting dapat diprediksi oleh tim Anda.

[NEEDS GATE-02 REVIEW: constraint proyek, beban, dan kebutuhan operasional belum tersedia.] Karena data itu belum ada di paket ini, kesimpulan di sini adalah kerangka keputusan, bukan rekomendasi stack final. Sobat Codev.id, perlakukan marker ini sebagai pekerjaan sebelum persetujuan teknis, bukan formalitas.

## Definisi dan batas objek

Bandingkan dua sumbu, bukan empat label. Sumbu pertama adalah batas kode dan data: apakah modul memiliki aturan yang jelas, dan siapa pemilik datanya? Sumbu kedua adalah batas operasi: apakah komponen harus dirilis, diskalakan, dan dipulihkan secara terpisah? Monolith dapat modular pada sumbu pertama tetapi tunggal pada sumbu kedua. Layanan terpisah memisahkan keduanya sekaligus, dengan harga koordinasi jaringan.

Serverless bukan sinonim layanan terpisah. Sebuah aplikasi serverless bisa tetap memiliki satu domain bisnis dan satu alur deploy, sedangkan layanan terpisah dapat berjalan di server yang Anda kelola. Static, server-rendered, client-rendered, CMS, dan custom juga tidak otomatis menentukan bentuk backend. Standar HTML menjelaskan perilaku dokumen dan browser, sementara semantik HTTP menjelaskan pertukaran pesan; keduanya tidak memilih struktur organisasi kode untuk Anda ([WHATWG HTML Living Standard](https://html.spec.whatwg.org/), [HTTP Semantics RFC 9110](https://www.rfc-editor.org/rfc/rfc9110)).

Batas artikel ini adalah keputusan struktur perangkat lunak dan konsekuensi operasionalnya. Ia tidak menetapkan pilihan deployment Cloudflare, tidak menggantikan threat model, dan tidak menyatakan satu gaya unggul untuk semua produk.

## Cara kerjanya

Pada monolith modular, permintaan masuk ke satu aplikasi. Router memilih modul, modul menerapkan aturan bisnis, lalu lapisan data melakukan baca/tulis. Transaksi lokal dan pengujian end-to-end relatif sederhana karena pemanggilan terjadi dalam satu proses. Batasnya hanya nyata jika akses lintas modul dipaksa melalui kontrak, bukan lewat tabel atau fungsi privat.

Pada layanan terpisah, satu permintaan dapat melewati gateway, layanan katalog, layanan pembayaran, dan penyimpanan masing-masing. Setiap lompatan menambah serialisasi, autentikasi, timeout, retry, dan kemungkinan kegagalan parsial. Kontrak API harus diperlakukan seperti antarmuka publik: versi, kompatibilitas mundur, dan observabilitas menjadi pekerjaan rutin. HTTP mendefinisikan semantik metode, status, dan pesan, tetapi tidak menjamin retry aman atau transaksi lintas layanan; aturan itu harus Anda rancang dan uji ([RFC 9110](https://www.rfc-editor.org/rfc/rfc9110)).

Pada serverless, pemicu dapat berupa HTTP, antrean, atau jadwal. Fungsi berjalan sesuai invokasi dan platform menangani sebagian provisioning. Anda tetap perlu menetapkan batas waktu, idempotensi (aman dijalankan ulang), izin, logging, dan cara menguji dependensi platform. Lonjakan singkat mungkin cocok dengan elastisitas tersebut; alur panjang, koneksi persisten, atau kebutuhan debugging lokal dapat membuat desain lebih rumit.

Di semua gaya, perubahan dimulai dari kontrak dan data. Tulis siapa pemilik aturan, siapa boleh mengubah skema, dan bagaimana kegagalan dilaporkan. Inventaris komponen pihak ketiga dengan SBOM (software bill of materials) membantu transparansi komponen, tetapi bukan bukti bahwa komponen itu aman ([CISA SBOM resources](https://www.cisa.gov/sbom)).

## Faktor yang mengubah hasil

Pertama, ukuran dan batas tim. Satu tim kecil biasanya lebih terbantu oleh satu pipeline dan satu cara debug. Layanan terpisah mulai bernilai ketika beberapa tim benar-benar dapat memiliki domain, jadwal rilis, dan tanggung jawab on-call yang berbeda. Jika semua perubahan masih menunggu orang yang sama, pemisahan hanya menambah koordinasi.

Kedua, bentuk data dan transaksi. Alur yang harus konsisten dalam satu transaksi cenderung lebih sederhana dalam monolith modular. Layanan terpisah memerlukan pola kompensasi, event, atau rekonsiliasi; jangan menganggap dua database dapat diperlakukan seperti satu transaksi lokal.

Ketiga, latensi dan kegagalan. Panggilan jaringan menambah waktu dan titik gagal; cold start, antrean, dan retry dapat memperlebar ekor latensi pada serverless. Ukur dengan beban yang mewakili pengguna Anda, bukan asumsi vendor. Tetapkan timeout dan anggaran latensi per batas layanan sebelum memecah proses.

Keempat, biaya. Hitung total biaya operasi: komputasi, penyimpanan, observabilitas, transfer, pipeline, waktu on-call, dan biaya migrasi. Model bayar-per-invokasi tidak otomatis lebih murah daripada proses yang selalu hidup; sebaliknya, server yang selalu aktif tidak otomatis boros jika utilisasinya stabil.

Kelima, rantai pasok dan vendor. NIST menekankan pengelolaan risiko rantai pasok sebagai proses evaluasi pemasok, komponen, dan hubungan yang berkelanjutan ([NIST SP 800-161 Rev. 1](https://csrc.nist.gov/pubs/sp/800/161/r1/final)). Scorecard repositori dapat menjadi sinyal awal kesehatan proyek, bukan pengganti due diligence, peninjauan lisensi, pemeriksaan maintainer, atau uji keamanan ([OpenSSF Scorecard](https://securityscorecards.dev/)).

## Contoh keputusan praktis

Gunakan tabel berikut sebagai hipotesis awal, lalu buktikan dengan prototipe kecil:

| Kondisi yang sudah terbukti | Kandidat awal | Hal yang harus dikunci |
| --- | --- | --- |
| Satu tim, domain belum stabil, transaksi saling terkait | Monolith modular | Batas modul, pemilik data, dan aturan akses lintas modul |
| Beberapa tim memiliki domain berbeda dan rilis tidak serempak | Layanan terpisah secara bertahap | Kontrak API, observabilitas, on-call, dan pemulihan kegagalan parsial |
| Beban sangat sporadis, pemicu jelas, operasi server ingin diminimalkan | Serverless pada batas yang sempit | Timeout, idempotensi, cold start, kuota, dan biaya transfer |
| Modul sudah stabil tetapi belum ada alasan operasi terpisah | Tetap monolith modular | Pipeline tunggal yang cepat serta tes kontrak antarmodul |

Misalnya, tim produk beranggotakan empat orang sedang membuat katalog dan pembayaran. Mulailah dengan modul katalog, pembayaran, dan identitas dalam satu deploy bila transaksi dan kepemilikan masih berubah. Pisahkan pembayaran hanya setelah Anda dapat menunjukkan kebutuhan isolasi, jadwal rilis berbeda, atau batas keamanan yang terdokumentasi. Untuk setiap kandidat, buat ADR berisi konteks, keputusan, alternatif yang ditolak, konsekuensi, dan tanggal peninjauan ([AWS ADR guidance](https://docs.aws.amazon.com/prescriptive-guidance/latest/architectural-decision-records/adr-process.html)). Kawan Codev.id dapat memakai [halaman utama Codev.id](/) untuk menemukan konteks layanan dan langkah lanjutan yang relevan sebelum menyusun ADR.

## Kesalahan umum dan cara memeriksanya

**“Microservices pasti lebih scalable.”** Tanyakan bagian mana yang benar-benar perlu diskalakan sendiri, bagaimana data dibagi, dan siapa yang akan menerima alarm pada pukul dua pagi. Tanpa jawaban, tetap modular mungkin lebih jujur.

**“Serverless berarti tidak ada operasi.”** Periksa log, tracing, izin, kuota, retry, dan prosedur rollback. Operasi berpindah sebagian ke platform, bukan menghilang.

**“Satu database bersama cukup untuk menghubungkan layanan.”** Catat setiap tabel yang diakses lintas batas. Jika daftar itu terus bertambah, batas layanan belum sehat; jika diputus mendadak, migrasi berisiko. Rencanakan pemilik skema dan mekanisme sinkronisasi.

**“Skor repositori membuktikan vendor aman.”** Gunakan scorecard dan SBOM untuk mengajukan pertanyaan, lalu verifikasi sumber, versi, lisensi, riwayat kerentanan, subprosesor, dan ketentuan API. Jangan mengubah sinyal menjadi jaminan.

Sebelum menyetujui desain, lakukan pemeriksaan singkat: jalankan satu perubahan lintas modul, simulasi timeout dan retry, ukur latensi p95 dengan beban realistis, pulihkan dari kegagalan dependensi, dan hitung biaya satu alur bisnis. Simpan hasil serta asumsi di ADR agar keputusan dapat ditinjau ketika konteks berubah.

## Jangan Memecah Terlalu Cepat

Shortcut yang sering dipilih adalah memecah aplikasi pada hari pertama agar “siap skala”. Ia gagal ketika batas domain belum dipahami: tim lalu membangun gateway, pipeline, dashboard, dan mekanisme sinkronisasi sebelum tahu perubahan mana yang sering terjadi. Alternatif yang lebih dapat diperiksa adalah modular monolith dengan kontrak internal, tes batas, dan telemetri sejak awal. Jika bukti baru menunjukkan kebutuhan isolasi, ekstraksi satu modul menjadi layanan adalah migrasi terarah, bukan penulisan ulang total.

## Langkah berikutnya

Monolith, modular, serverless, dan layanan terpisah bukan urutan naik kelas. Pilih bentuk yang paling kecil namun cukup untuk batas data, pola beban, kemampuan tim, dan kewajiban operasional yang sudah terbukti. Untuk proyek ini, jangan mengunci pilihan final sebelum GATE-02 menyediakan constraint tersebut. Teman Codev.id, keputusan yang bisa ditinjau ulang dengan bukti lebih sehat daripada keputusan besar yang hanya mengikuti tren.

Langkah berikutnya: tulis satu ADR untuk dua kandidat teratas, lampirkan peta modul dan pemilik data, hasil uji latensi/kegagalan, estimasi biaya operasi, serta inventaris dependensi. Minta tinjauan teknis atas asumsi yang belum terbukti. Aturan operasionalnya sederhana: pisahkan layanan ketika manfaat isolasi dapat diukur dan ada tim yang sanggup mengoperasikannya; sampai saat itu, jaga monolith tetap modular dan batasnya dapat diuji.
