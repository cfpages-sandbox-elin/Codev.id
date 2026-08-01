---
article_id: CDV-04-A03
writing_contract_version: "native-id-v2"
title: "Siklus Request Back-end: Validasi sampai Respons"
slug: "siklus-request-back-end"
description: "Trace authentication context, validation, authorization, business rules, data transaction, side effects, error, logging, and response"
status: draft
publication_date: "2025-06-10"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-04
primary_intent: "Understand server-side responsibilities for a web workflow"
reader_community: "Codev.id"
reader_address: "Sobat Codev.id"
final_route: "/artikel/siklus-request-back-end.html"
technical_review: required
sources:
  - "https://docs.aws.amazon.com/prescriptive-guidance/latest/architectural-decision-records/adr-process.html"
  - "https://www.rfc-editor.org/rfc/rfc9110"
  - "https://www.w3.org/TR/WCAG22/"
  - "https://www.rfc-editor.org/rfc/rfc9111"
  - "https://web.dev/articles/vitals"
---

# Siklus Request Back-end: Validasi sampai Respons

Halo, Sobat Codev.id! Request back-end bukan sekadar menerima data lalu mengembalikan JSON. Siklus yang sehat memeriksa siapa pengirimnya, apakah input dapat diproses, apakah ia berhak melakukan aksi, lalu menjalankan aturan bisnis dan transaksi data secara aman sebelum mengirim respons yang bisa ditindaklanjuti. Jika satu tahap dilewati, gejalanya bisa berupa data ganda, kebocoran informasi, atau pesan gagal yang membingungkan.

Urutan praktisnya adalah: bangun konteks autentikasi, validasi bentuk dan isi, otorisasi terhadap objek yang dituju, jalankan aturan bisnis, kelola transaksi, proses side effect, petakan error, catat log, dan bentuk respons. Urutan detail dapat berubah sesuai arsitektur; keputusan itu seharusnya dicatat bersama alasan dan konsekuensinya, bukan dianggap sebagai peringkat “lebih modern”. [NEEDS GATE-02 REVIEW: pilihan stack dan pola deployment belum memiliki bukti kebutuhan proyek.] Untuk prinsip pencatatan keputusan arsitektur, lihat [panduan Architecture Decision Record AWS](https://docs.aws.amazon.com/prescriptive-guidance/latest/architectural-decision-records/adr-process.html).

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

## Definisi dan batas objek

“Request” di sini adalah perjalanan satu permintaan web dari batas masuk server sampai respons dikirim. Batas ini mencakup endpoint, middleware, service, repositori data, antrean atau pengirim notifikasi yang dipicu, serta observabilitasnya. Ia bukan spesifikasi API publik: penamaan endpoint, skema versi, dan kontrak lintas konsumen berada di ruang desain API tersendiri.

Authentication (autentikasi) menjawab “siapa”, sedangkan authorization (otorisasi) menjawab “boleh melakukan apa pada objek mana”. Validasi memastikan data memenuhi bentuk dan aturan awal; validasi bukan pengganti otorisasi. Sebaliknya, respons HTTP perlu menyatakan hasil pemrosesan secara konsisten—kode status, header, dan isi—sesuai semantik HTTP, bukan sekadar selalu `200 OK` ([RFC 9110](https://www.rfc-editor.org/rfc/rfc9110)).

Untuk product owner atau junior developer, batas ini membantu memisahkan pertanyaan: “Apakah tombol bekerja?” dari “Apakah server mencegah tindakan yang salah dan meninggalkan jejak yang bisa diperiksa?” Detail kontrol keamanan mendalam tetap memerlukan review keamanan khusus; halaman ini memetakan tanggung jawab dan urutan, bukan sertifikasi keamanan.

## Cara kerjanya

1. **Terima dan bentuk konteks.** Server menetapkan request ID, membaca metode dan target, membatasi ukuran payload, lalu mengambil kredensial dari mekanisme yang disepakati. Middleware memverifikasi sesi atau token dan menaruh identitas minimum ke konteks. Jangan menyalin seluruh klaim identitas ke log atau meneruskan token ke lapisan yang tidak membutuhkannya.

2. **Validasi input.** Periksa tipe, format, field wajib, rentang, dan hubungan antarfield. Lakukan normalisasi yang terdokumentasi, misalnya trim spasi bila memang aturan domain mengizinkannya. Validasi sintaks yang lolos belum berarti operasi boleh dilakukan; hasilnya harus berupa error terstruktur yang dapat dipetakan ke field tanpa membocorkan detail internal.

3. **Otorisasi pada objek dan aksi.** Setelah identitas diketahui dan input menghasilkan ID yang sah, service memeriksa kepemilikan, peran, status workflow, atau kebijakan organisasi. Pemeriksaan harus terjadi dekat dengan keputusan bisnis dan diulang pada jalur alternatif seperti job atau endpoint internal. UI yang menyembunyikan tombol bukan kontrol otorisasi.

4. **Jalankan aturan bisnis.** Service menggabungkan fakta dari input dan data saat ini: apakah transisi status valid, kuota tersedia, atau syarat proses terpenuhi. Aturan ini sebaiknya tidak tersebar di controller dan query acak. Bila ada trade-off antara monolit modular, layanan terpisah, atau fungsi serverless, tulis keputusan, asumsi, dan cara membatalkannya dalam catatan arsitektur; tidak ada pilihan yang otomatis paling matang.

5. **Kelola transaksi data.** Tentukan batas atomik: operasi mana harus berhasil bersama dan mana yang boleh terpisah. Di dalam transaksi, baca versi data yang relevan, cek konflik, tulis perubahan, lalu commit. Siapkan idempotensi (aman diulang) untuk request yang dapat dikirim ulang, dan gunakan constraint database sebagai pagar terakhir, bukan hanya pemeriksaan di kode.

6. **Pisahkan side effect.** Email, webhook, pengurangan stok eksternal, atau penerbitan event dapat gagal setelah data utama tersimpan. Jika side effect tidak boleh hilang, catat pekerjaan tertunda dalam pola seperti outbox lalu proses ulang dengan kunci idempotensi. Jika boleh tertunda, respons dapat menyatakan status “diterima untuk diproses”, bukan berpura-pura sudah selesai.

7. **Petakan error secara jujur.** Bedakan input tidak valid, tidak terautentikasi, tidak berwenang, konflik, resource tidak ditemukan, dan kegagalan tak terduga. Gunakan pesan yang berguna bagi pengguna, sementara detail stack trace hanya untuk log terlindungi. Jangan mengubah semua kegagalan menjadi 500 atau semua hasil menjadi 200 karena itu menghilangkan sinyal bagi klien dan operator.

8. **Log dan kirim respons.** Log terstruktur minimal mengikat request ID, hasil keputusan, durasi, dan kategori error; rahasiakan token, password, dan data pribadi yang tidak perlu. Respons menetapkan status, content type, cache policy, dan body yang konsisten. Untuk respons yang dapat disimpan, aturan cache perlu dipilih sadar terhadap kebaruan dan privasi ([RFC 9111](https://www.rfc-editor.org/rfc/rfc9111)).

## Faktor yang mengubah hasil

Hasil siklus bergantung pada model data, konkurensi, dan cara request dipicu ulang. Form pembayaran yang menerima retry memerlukan idempotency key; pencarian yang boleh stale dapat memakai cache; perubahan hak akses biasanya membutuhkan pembacaan kebijakan terbaru. Batas waktu, pembatalan koneksi, dan retry proxy juga menentukan apakah side effect mungkin berjalan dua kali.

Jenis klien mengubah kebutuhan respons. Browser mungkin membutuhkan error per-field dan status fokus yang jelas, sedangkan job internal memerlukan kode retry dan dead-letter. Untuk alur yang dipakai keyboard atau teknologi bantu, pesan error, label, dan urutan fokus harus dirancang serta diuji dalam proses, bukan disimpulkan dari satu pemindaian otomatis. WCAG 2.2 menekankan evaluasi pada cakupan halaman dan proses; [NEEDS GATE-06 REVIEW: bukti evaluasi aksesibilitas untuk alur proyek belum tersedia](https://www.w3.org/TR/WCAG22/).

Lingkungan operasi juga berpengaruh: koneksi database yang habis, antrean penuh, atau dependency lambat mengubah kategori error dan strategi fallback. Karena Core Web Vitals dan data lapangan memiliki definisi serta kondisi pengukuran tertentu, jangan mengklaim perbaikan performa hanya dari satu request lokal; dokumentasikan scope, sampel, versi, dan kondisi sebelum membandingkan ([web.dev Core Web Vitals](https://web.dev/articles/vitals)). Kawan Codev.id, perlakukan angka itu sebagai hasil pengukuran bersyarat, bukan janji.

## Contoh keputusan praktis

Bayangkan fitur “mengubah alamat pengiriman”. Jejak keputusan yang dapat direview terlihat seperti ini:

| Tahap | Pertanyaan yang harus terjawab | Jika gagal |
|---|---|---|
| Konteks | Sesi valid dan request ID tercatat? | Hentikan tanpa memproses data |
| Validasi | Kode pos, negara, dan field wajib sesuai aturan? | Kembalikan error field yang dapat diperbaiki |
| Otorisasi | Pengguna boleh mengubah alamat pesanan ini? | Tolak tanpa mengungkap kepemilikan |
| Bisnis | Pesanan belum dikunci untuk pengiriman? | Kembalikan konflik/status yang jelas |
| Transaksi | Update alamat dan audit tersimpan atomik? | Rollback dan catat kategori kegagalan |
| Side effect | Perlu sinkronisasi ke kurir? | Antrekan pekerjaan idempotent bila tertunda |
| Respons | Klien tahu perubahan selesai atau masih diproses? | Gunakan status dan body yang sesuai |

Asumsi di contoh ini hanya pola analisis, bukan bukti bahwa proyek tertentu memakai kurir atau audit trail. Dokumen desain perlu menyebutkan siapa pemilik setiap keputusan, data apa yang menjadi sumber kebenaran, dan kondisi yang memicu retry. Untuk langkah berikutnya dalam implementasi situs, Anda dapat meninjau konteks [pengembangan web](/web-development) sekali sebagai referensi umum, bukan sebagai kontrak endpoint. Sobat Codev.id, tautan itu memberi konteks layanan, bukan bukti bahwa stack tertentu cocok untuk fitur Anda.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah menaruh semua logika di controller: periksa apakah controller hanya mengorkestrasi dan service memegang aturan bisnis. Kedua, memvalidasi hanya di browser: kirim request langsung ke server dalam pengujian dan pastikan aturan tetap berlaku. Ketiga, melakukan write lalu mengirim email sinkron tanpa rencana retry: tanyakan apa yang terjadi jika email timeout setelah commit.

Kesalahan lain adalah log yang berisi token atau payload penuh. Audit sampel log dengan data sintetis dan daftar field terlarang. Periksa juga korelasi request ID dari gateway sampai worker, metrik durasi per tahap, dan alert untuk lonjakan kategori error. Untuk respons, uji jalur sukses, input salah, sesi kedaluwarsa, akses ditolak, konflik, timeout dependency, retry, dan pembatalan koneksi.

Checklist review singkat:

- Apakah setiap aksi memiliki pemeriksaan identitas dan izin pada objek yang tepat?
- Apakah validasi server mendefinisikan pesan yang aman dan dapat diperbaiki?
- Apakah transaksi dan constraint mencegah duplikasi atau partial write?
- Apakah side effect memiliki status, retry, dan idempotensi yang terlihat?
- Apakah status HTTP, cache, log, dan correlation ID konsisten di semua jalur?
- Apakah alur lengkap diuji dengan keyboard dan teknologi bantu, serta oleh reviewer yang kompeten?

## Jalan pintas yang perlu dihindari

Shortcut yang sering muncul adalah “cukup return JSON sukses; detail lain bisa menyusul”. Ini gagal ketika klien mengulang request karena timeout, ketika pengguna tidak memiliki izin, atau ketika database sudah commit tetapi notifikasi belum terkirim. Respons sukses tanpa status proses membuat klien menebak dan operator kehilangan titik diagnosis. Alternatif yang lebih dapat dipercaya adalah menetapkan state eksplisit, menyimpan jejak transaksi, memberi kunci idempotensi, dan mendokumentasikan trade-off arsitektur. Jika kebutuhan stack atau kontrol akses belum disepakati, tandai keputusan untuk review teknis—jangan mengisinya dengan asumsi.

## Kesimpulan: jadikan siklus sebagai jejak yang bisa diperiksa

Siklus request back-end yang dapat dipercaya bergerak dari konteks autentikasi, validasi, otorisasi, aturan bisnis, transaksi, side effect, error, logging, lalu respons yang jujur. Nilainya bukan pada banyaknya lapisan, melainkan pada alasan, batas atomik, dan bukti bahwa tiap jalur gagal ditangani.

Teman Codev.id, sebelum menyetujui fitur, minta satu diagram urutan dan tabel keputusan seperti contoh di atas, lalu telusuri minimal satu jalur sukses dan seluruh jalur gagal penting. Sertakan pengujian aksesibilitas proses dan review teknis untuk keputusan arsitektur yang belum memiliki bukti. Aturan operasionalnya sederhana: setiap request harus meninggalkan hasil yang dapat dipahami klien dan jejak yang dapat diperiksa operator; di luar itu, klaim performa, keamanan, atau kepatuhan memerlukan bukti proyek dan review profesional.
