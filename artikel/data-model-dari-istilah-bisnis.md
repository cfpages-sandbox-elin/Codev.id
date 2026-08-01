---
article_id: CDV-07-A01
writing_contract_version: "native-id-v2"
title: "Data Model dari Istilah Bisnis ke Constraint"
slug: "data-model-dari-istilah-bisnis"
description: "Define terms, identifiers, entities, relationships, ownership, constraints, lifecycle states, audit needs, and unresolved ambiguity"
status: draft
publication_date: "2025-08-16"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-07
primary_intent: "Turn domain language into consistent entities and rules"
reader_community: "Codev.id"
reader_address: "Kawan Codev.id"
final_route: "/artikel/data-model-dari-istilah-bisnis.html"
technical_review: required
sources:
  - "https://peraturan.bpk.go.id/Details/229798/uu-no-27-tahun-2022"
  - "https://peraturan.bpk.go.id/Details/122030/pp-no-71-tahun-2019"
  - "https://www.nist.gov/privacy-framework"
  - "https://csrc.nist.gov/Projects/ssdf/publications"
  - "https://www.cisa.gov/known-exploited-vulnerabilities-catalog"
  - "https://developers.google.com/search/docs/crawling-indexing/site-move-with-url-changes"
---

# Data Model dari Istilah Bisnis ke Constraint

Halo, Kawan Codev.id! Data model yang baik bukan gambar tabel yang dibuat setelah semua keputusan selesai. Ia adalah terjemahan istilah bisnis menjadi objek yang dapat dikenali, hubungan yang dapat diuji, dan constraint (aturan yang mencegah keadaan tidak sah). Jika istilah “pesanan”, “pelanggan”, atau “aktif” masih punya arti berbeda di tiap tim, schema paling rapi pun hanya akan mengabadikan kekacauan itu.

Jawaban singkatnya: mulai dari kamus istilah dan kejadian bisnis, lalu tetapkan identitas, kepemilikan, relasi, batas nilai, status siklus hidup, dan kebutuhan audit. Baru setelah aturan itu disepakati, tim dapat memetakan model logis ke teknologi penyimpanan. Pilihan database berada di luar artikel ini; model harus tetap dapat dibaca sebelum ada keputusan SQL, NoSQL, atau object storage.

Bukti yang dapat mengubah keputusan adalah definisi domain yang disetujui pemilik proses, contoh transaksi nyata, kewajiban perlindungan data, serta kemampuan operasional untuk memulihkan dan menelusuri perubahan. Untuk keputusan yang menyentuh dasar pemrosesan, masa simpan, penghapusan, pemindahan data, atau kewajiban pemberitahuan, minta tinjauan hukum dan keamanan: **[NEEDS GATE-05: konfirmasi kewajiban dan dasar pemrosesan proyek]**.

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

<p><em>Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.</em></p>

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

## Definisi dan batas objek

Data model di sini adalah seperangkat definisi dan aturan, bukan produk database. “Entitas” menjawab benda atau konsep apa yang perlu dikenali—misalnya Pelanggan, Pesanan, Pembayaran, atau Tiket. “Atribut” menjelaskan entitas itu, seperti `status` atau `tanggal_dibuat`. “Relasi” menjelaskan keterkaitan, sedangkan constraint menetapkan kondisi yang wajib benar, seperti satu pembayaran hanya mengacu pada satu pesanan yang ada.

Mulailah dengan kamus istilah satu halaman. Untuk setiap istilah, tulis definisi, sinonim yang dilarang, pemilik keputusan, contoh dan kontra-contoh. Jika “akun” bagi layanan pelanggan berarti identitas orang, tetapi bagi keuangan berarti kontrak penagihan, jangan memaksa satu entitas. Pisahkan konsepnya dan jelaskan hubungan di antaranya.

Batas objek juga menentukan data apa yang tidak boleh masuk. Produk tertentu boleh memiliki schema sendiri; artikel ini tidak menggantikannya. Demikian pula, model tidak menetapkan vendor, tipe indeks, partisi, atau format file. Tim yang mengelola kanal [bisnis online](/website/bisnis-online) dapat memakai kamus ini untuk menyamakan istilah lintas fungsi, tetapi rincian implementasi tetap perlu keputusan teknis terpisah.

Untuk data pribadi, catat kategori data, tujuan penggunaan, pihak yang mengakses, dan jalur keluar-masuknya. UU No. 27 Tahun 2022 merupakan undang-undang nasional utama mengenai pelindungan data pribadi, sementara PP No. 71 Tahun 2019 mengatur penyelenggaraan sistem dan transaksi elektronik dalam cakupan yang lebih luas ([UU PDP](https://peraturan.bpk.go.id/Details/229798/uu-no-27-tahun-2022); [PP 71/2019](https://peraturan.bpk.go.id/Details/122030/pp-no-71-tahun-2019)). Sumber tersebut tidak otomatis menjawab dasar pemrosesan atau masa simpan proyek Anda; tandai pertanyaan itu untuk peninjauan.

## Cara kerjanya

Urutan praktisnya dapat dibuat seperti alur berikut.

1. **Tangkap kejadian.** Tulis kalimat seperti “pelanggan mengajukan pesanan” atau “operator menyetujui pengembalian”. Kejadian memberi batas waktu dan pelaku, bukan sekadar daftar kata benda.
2. **Pisahkan identitas.** Tentukan identifier yang stabil dan unik. Nomor telepon atau alamat email bisa berubah dan, tanpa keputusan eksplisit, tidak layak menjadi kunci utama. Simpan identifier internal; catat aturan untuk duplikasi dan penggabungan.
3. **Bentuk entitas dan relasi.** Tanyakan kardinalitas: satu-ke-satu, satu-ke-banyak, atau banyak-ke-banyak. Nyatakan apakah relasi wajib, opsional, dan siapa pemilik rekamannya.
4. **Tulis constraint.** Bedakan aturan format (misalnya rentang tanggal), aturan referensial (rujukan harus ada), dan aturan proses (pesanan berstatus selesai tidak boleh kembali ke draf tanpa otorisasi). Setiap aturan perlu pemilik dan cara pengujian.
5. **Modelkan siklus hidup.** Daftar status, transisi yang diizinkan, aktor, waktu, dan alasan. Jangan menyamakan `deleted`, `cancelled`, dan `archived`; ketiganya berdampak berbeda pada pelaporan dan pemulihan.
6. **Rancang audit.** Tentukan kejadian apa yang dicatat, siapa pelakunya, kapan terjadi, nilai sebelum-sesudah bila perlu, serta retensi yang telah disetujui. Log bukan pengganti kontrol akses.
7. **Uji dengan contoh dan kontra-contoh.** Jalankan transaksi normal, duplikasi, terlambat, dibatalkan, dan dipulihkan. Jika aturan tidak dapat dijelaskan dengan contoh, model belum siap diimplementasikan.

Kawan Codev.id, perlakukan model sebagai kontrak antar tim. Perubahan istilah harus melalui pemilik domain, pengembang, dan operasi; jangan mengubah arti kolom diam-diam hanya karena migrasi sedang berjalan.

## Faktor yang mengubah hasil

Ada empat kelompok kondisi yang sering mengubah constraint.

**Konteks bisnis.** Apakah satu pelanggan boleh memiliki beberapa akun? Apakah harga pada pesanan mengikuti katalog saat ini atau disalin sebagai snapshot? Jawabannya mengubah relasi dan riwayat. Tulis asumsi yang belum diputuskan, bukan menebaknya dari nama kolom.

**Konteks penggunaan.** Beban baca, kebutuhan near-real-time, akses offline, dan integrasi pihak ketiga memengaruhi toleransi keterlambatan serta strategi rekonsiliasi. Itu belum memilih teknologi penyimpanan, tetapi menentukan bukti apa yang harus tersedia ketika dua sistem berbeda.

**Konteks privasi dan keamanan.** Peta data harus menunjukkan data pribadi, tujuan, akses, retensi, penghapusan, backup, dan pemulihan. NIST Privacy Framework dapat dipakai sebagai kerangka percakapan risiko ([NIST Privacy Framework](https://www.nist.gov/privacy-framework)). Backup baru menjadi bukti kemampuan pemulihan setelah restore benar-benar diuji dan hasilnya dicatat. Jangan menulis masa simpan atau hak penghapusan sebagai angka final tanpa keputusan proyek: **[NEEDS GATE-05: verifikasi retensi, akses, penghapusan, dan transfer]**.

**Konteks perubahan dan operasi.** Dependensi runtime dan komponen yang rentan memengaruhi urutan perbaikan. NIST SSDF menekankan praktik pengembangan perangkat lunak yang aman, sedangkan katalog Known Exploited Vulnerabilities CISA membantu memprioritaskan kerentanan yang telah dieksploitasi ([NIST SSDF](https://csrc.nist.gov/Projects/ssdf/publications); [CISA KEV](https://www.cisa.gov/known-exploited-vulnerabilities-catalog)). Usia komponen saja bukan alasan mengganti atau menghapus data. Untuk migrasi, inventaris sumber-tujuan, rekonsiliasi jumlah dan nilai, serta rencana rollback harus menjadi bagian dari model operasi ([panduan migrasi URL Google](https://developers.google.com/search/docs/crawling-indexing/site-move-with-url-changes)).

## Contoh keputusan praktis

Misalkan tim menjual paket layanan. Istilah awalnya: “order”, “customer”, “invoice”, dan “active”. Tabel keputusan berikut membantu mengubahnya menjadi aturan.

| Pertanyaan | Keputusan sementara | Bukti yang harus diminta |
|---|---|---|
| Apa identitas Pesanan? | `order_id` internal stabil; nomor tampilan boleh berubah format | Contoh duplikasi dan aturan penggabungan |
| Siapa pemilik status? | Layanan pesanan memiliki transisi; keuangan hanya memberi status pembayaran | Daftar aktor dan event yang disetujui |
| Kapan harga dibekukan? | Simpan snapshot harga pada saat pesanan dibuat | Contoh perubahan katalog setelah checkout |
| Apa arti “aktif”? | Pisahkan status akun dari status langganan | Definisi pemilik proses dan kontra-contoh |
| Apa yang diaudit? | Perubahan status, nilai pembayaran, dan akses data sensitif | Kebutuhan investigasi dan kontrol akses |

Jika pembayaran berhasil tetapi webhook terlambat, pesanan tidak boleh diam-diam dianggap gagal. Model dapat menetapkan status `payment_pending`, event idempotensi, dan proses rekonsiliasi. Itu contoh constraint proses, bukan klaim bahwa semua sistem harus memakai nama status tersebut.

Untuk data pribadi, buat matriks sederhana: kategori data, tujuan, sistem asal, penerima, retensi yang disetujui, dan cara penghapusan atau pemulihan. Bila salah satu kolom belum memiliki pemilik keputusan, tahan implementasi bagian itu. Sobat Codev.id, satu tanda `[NEEDS ...]` yang terlihat lebih aman daripada aturan yang tampak pasti tetapi tidak pernah disetujui.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah menjadikan nama kolom sebagai definisi bisnis. Periksa dengan meminta dua tim menjelaskan istilah yang sama tanpa melihat schema; perbedaan jawaban menunjukkan kamus belum selesai.

Kesalahan kedua adalah memakai email sebagai identifier permanen. Uji perubahan email, penggabungan akun, dan penghapusan identitas. Pastikan referensi transaksi lama tetap dapat ditelusuri tanpa mengekspos data yang tidak perlu.

Kesalahan ketiga adalah menyimpan satu kolom `status` untuk seluruh siklus hidup. Buat tabel transisi dan coba jalur mundur, pembatalan, timeout, serta pemulihan. Setiap transisi harus punya aktor, alasan, dan event audit.

Kesalahan keempat adalah menganggap backup, log, atau enkripsi otomatis menyelesaikan privasi. Tanyakan siapa yang dapat memulihkan backup, berapa lama salinan bertahan, bagaimana aksesnya diaudit, dan apakah penghapusan mencakup salinan tersebut. Jawaban yang belum disahkan tetap menjadi pertanyaan review.

Kesalahan kelima adalah migrasi berdasarkan usia sistem atau tekanan jadwal. Inventaris semua sumber, pemetaan field, checksum atau rekonsiliasi yang relevan, cutover, dan rollback. **[NEEDS GATE-02: persetujuan dampak perubahan, rekonsiliasi, dan rollback sebelum dekomisioning]**.

## Mengapa menyalin spreadsheet bukan model

Shortcut yang umum adalah “langsung buat tabel dari spreadsheet yang ada”. Spreadsheet memang berguna sebagai contoh input, tetapi ia mencampur istilah, data, dan kebiasaan kerja. Menyalinnya langsung mengunci duplikasi, status ambigu, dan pemilik yang tidak jelas. Alternatif yang lebih aman: bekukan spreadsheet sebagai artefak sumber, ekstrak kamus istilah dan kejadian, lalu uji constraint dengan kasus normal serta pengecualian. Setelah pemilik domain menyetujui model logis, barulah tim memilih bentuk penyimpanan di pekerjaan lanjutan.

## Langkah berikutnya

Data model menerjemahkan istilah bisnis menjadi identitas, entitas, relasi, constraint, status, kepemilikan, dan jejak audit yang dapat diuji. Ia tidak memilih database dan tidak memberi izin untuk menetapkan retensi, penghapusan, transfer, atau dekomisioning tanpa keputusan yang berwenang.

Langkah berikutnya adalah membuat kamus istilah, matriks data pribadi, tabel transisi status, dan daftar constraint untuk satu alur bisnis; minta pemilik domain, keamanan, dan hukum menandai bagian yang belum memiliki bukti. Tunda implementasi pada item **[NEEDS GATE-05]** dan **[NEEDS GATE-02]** sampai review selesai. Aturan operasionalnya sederhana: jika sebuah istilah tidak punya definisi, pemilik, contoh uji, dan batas perubahan, ia belum siap menjadi constraint produksi.
