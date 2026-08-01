---
article_id: CDV-06-A02
title: "REST, Webhook, Queue, atau Event"
slug: "rest-webhook-queue-atau-event"
description: "Panduan memilih pola REST, webhook, queue, atau event berdasarkan waktu tunggu, urutan, pengiriman, keterikatan, kegagalan, dan keteramatan."
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2025-07-24"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-06
primary_intent: "Choose an interaction pattern by timing and ownership"
reader_community: "Codev.id"
reader_address: "Teman Codev.id"
final_route: "/artikel/rest-webhook-queue-atau-event.html"
technical_review: required
sources:
  - "https://spec.openapis.org/oas/v3.1.1.html"
  - "https://www.rfc-editor.org/info/rfc9700/"
  - "https://owasp.org/API-Security/editions/2023/en/0x11-t10/"
  - "https://csrc.nist.gov/pubs/sp/800/218/final"
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

# REST, Webhook, Queue, atau Event

Halo, Teman Codev.id! Kebingungan memilih REST, webhook, queue, atau event biasanya muncul karena semua terlihat seperti “cara mengirim data”. Padahal keputusan utamanya bukan nama teknologinya, melainkan siapa yang menunggu, siapa yang memiliki pekerjaan, dan apa yang terjadi ketika penerima sedang tidak tersedia.

Jawaban singkatnya: gunakan REST ketika pemanggil membutuhkan jawaban untuk melanjutkan keputusan sekarang; gunakan webhook ketika pemilik suatu sistem perlu memberi tahu sistem lain setelah perubahan terjadi; gunakan queue ketika pekerjaan boleh diproses belakangan dan harus menunggu sampai konsumen siap; gunakan event ketika satu kejadian perlu diketahui oleh beberapa konsumen tanpa membuat penghasil kejadian mengenal seluruh konsumennya. Kombinasi juga sah: REST menerima perintah, lalu queue mengerjakannya dan event mengumumkan hasil.

Kondisi yang dapat mengubah pilihan itu adalah batas waktu, konsekuensi pengulangan, kebutuhan urutan, jumlah konsumen, dan kepemilikan retry. Jangan menyimpulkan bahwa pola asinkron otomatis lebih andal, atau bahwa REST selalu sinkron sampai selesai. Kontrak, otorisasi, batas beban, dan bukti pengujian tetap harus ditinjau sesuai konteks; [NEEDS GATE-03/GATE-04: validasi threat model dan alur otorisasi pada implementasi nyata].

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

*Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.*

## Definisi dan batas objek

Dalam artikel ini, REST berarti antarmuka request/response berbasis resource atau operasi HTTP. Klien mengirim permintaan, server mengembalikan jawaban, lalu klien memutuskan langkah berikutnya. REST tidak menjamin pekerjaan di server sudah selesai; status `202 Accepted`, misalnya, dapat menjadi sinyal bahwa pekerjaan baru diterima. Detail status dan payload harus ditulis dalam kontrak. [OpenAPI Specification 3.1.1](https://spec.openapis.org/oas/v3.1.1.html) membantu mendeskripsikan antarmuka, tetapi deskripsi itu tidak membuktikan perilaku implementasi atau keamanannya.

Webhook adalah callback melalui HTTP: sistem A mendaftarkan atau menyediakan endpoint, lalu sistem B memanggil endpoint itu ketika kejadian tertentu muncul. Pengirim memiliki tanggung jawab mengirim dan mencoba ulang; penerima memiliki tanggung jawab mengautentikasi, merespons cepat, dan memproses aman. Webhook tidak sama dengan event bus; biasanya ia adalah hubungan langsung antara pengirim dan endpoint tertentu.

Queue adalah perantara yang menyimpan pesan sampai konsumen mengambilnya. Pengirim tidak perlu menunggu pekerjaan selesai, sementara konsumen dapat bekerja ketika kapasitas tersedia. Durasi simpan, urutan, visibilitas pesan, dan kebijakan pengulangan bergantung pada implementasinya. Event adalah catatan bahwa sesuatu telah terjadi, misalnya `OrderPaid`; event tidak harus membawa perintah “lakukan X”. Satu event dapat dibaca beberapa konsumen dengan kebutuhan berbeda.

Batas artikel ini adalah memilih pola berdasarkan waktu dan kepemilikan interaksi. Ia tidak memilih vendor, layanan cloud, atau desain ketergantungan pihak ketiga. Untuk setiap pola, Anda tetap perlu kontrak, otorisasi, pembatasan penyalahgunaan, dan persetujuan teknis yang sesuai.

## Cara kerjanya

Urutan REST dimulai dari klien yang mengirim permintaan dengan correlation ID. Server memvalidasi otorisasi dan input, menjalankan pekerjaan yang memang termasuk dalam batas waktu, lalu mengembalikan hasil atau status yang dapat ditindaklanjuti. Karena koneksi menunggu, timeout klien tidak otomatis membatalkan pekerjaan server. Idempotency key atau pemeriksaan duplikasi diperlukan bila klien boleh mengulang permintaan.

Pada webhook, pemilik kejadian membuat payload dan menandainya dengan event ID. Pengirim mengirim ke endpoint penerima. Penerima memverifikasi tanda tangan atau kredensial yang disepakati, menyimpan event ID untuk deduplikasi, lalu mengembalikan respons cepat. Pekerjaan berat sebaiknya dipindahkan ke proses internal; jika tidak, pengirim dapat menganggap endpoint gagal dan mengirim ulang. Retry perlu backoff, batas percobaan, serta dead-letter atau jalur tinjauan manual.

Queue memisahkan producer dari consumer. Producer menulis pesan dan menerima konfirmasi bahwa pesan tersimpan; consumer mengambil pesan, memprosesnya, lalu mengakui atau mengembalikan kegagalan. Konfigurasi dapat mengizinkan pengiriman ulang, sehingga handler harus aman bila pesan yang sama datang lebih dari sekali. Jika urutan penting, aturan partisi atau kunci urutan harus dinyatakan—jangan menganggap semua queue mempertahankan urutan global.

Event menambahkan satu lapisan kepemilikan: producer hanya menyatakan fakta, sedangkan tiap consumer memutuskan reaksinya. Producer tidak perlu menunggu semua consumer, tetapi perubahan skema, kompatibilitas, dan retensi menjadi tanggung jawab bersama. Event yang terlambat atau datang tidak berurutan harus ditangani dengan versi, timestamp kejadian, dan rekonsiliasi; mekanisme persisnya perlu disepakati sebelum implementasi.

Di semua pola, observability bukan aksesori. Catat correlation ID, event atau message ID, waktu dibuat, waktu diterima, percobaan ke berapa, hasil, dan alasan kegagalan. Ukur latency dan usia pesan, bukan hanya jumlah HTTP 200. Tanpa itu, tim sulit membedakan penerima lambat, retry berulang, dan pekerjaan yang macet.

## Faktor yang mengubah hasil

Mulai dari batas waktu bisnis. Jika pengguna harus melihat saldo terbaru sebelum menyelesaikan langkah, REST lebih mudah dipahami. Jika sistem pembayaran memberi tahu status beberapa detik kemudian, webhook atau event lebih sesuai. Jika pekerjaan berupa ekspor besar yang dapat selesai nanti, REST dapat mengembalikan penerimaan pekerjaan lalu queue mengurus eksekusinya.

Berikutnya tentukan pemilik retry dan kegagalan. Pada REST, klien biasanya mengulang setelah timeout, sehingga server harus mengenali permintaan yang sama. Pada webhook, pengirim harus memiliki kebijakan retry yang dapat diamati, sementara penerima harus idempotent. Pada queue, platform atau consumer mengendalikan pengambilan ulang. Pada event, setiap consumer memiliki retry sendiri; satu consumer yang gagal tidak boleh diam-diam menghentikan consumer lain.

Urutan juga menentukan. Notifikasi independen tidak membutuhkan urutan global. Perubahan status satu pesanan mungkin membutuhkan urutan per `order_id`, bukan urutan semua pesanan. Jika pembaca memerlukan keadaan terkini, sediakan versi atau mekanisme rekonsiliasi; jangan hanya mengandalkan urutan kedatangan.

Pertimbangkan coupling dan jumlah konsumen. Webhook langsung sederhana untuk satu hubungan yang jelas, tetapi perubahan endpoint atau kredensial berdampak pada pengirim. Event mengurangi pengetahuan penghasil terhadap konsumen, tetapi menambah kontrak skema dan pekerjaan operasional. Queue cocok ketika satu jenis pekerjaan memiliki konsumen yang bertanggung jawab; ia bukan pengganti event bila banyak tim perlu bereaksi terhadap fakta yang sama.

Terakhir, evaluasi keamanan dan bukti. Endpoint publik perlu autentikasi, otorisasi, rate limit, validasi input, dan perlindungan replay. OWASP API Security Top 10 menempatkan masalah otorisasi dan penyalahgunaan alur sebagai risiko yang harus diuji sesuai konteks, bukan diselesaikan hanya dengan memilih pola ([OWASP API Security Top 10 2023](https://owasp.org/API-Security/editions/2023/en/0x11-t10/)). RFC 9700 adalah pembaruan best current practice untuk keamanan OAuth 2.0; gunakan sebagai rujukan saat alur token memang terlibat, bukan sebagai bukti bahwa suatu endpoint sudah aman ([OAuth 2.0 Security BCP—RFC 9700](https://www.rfc-editor.org/info/rfc9700/)).

## Contoh keputusan praktis

Bayangkan tiga kebutuhan berikut. Pertama, halaman checkout perlu memastikan kupon masih valid sebelum menampilkan total. Pilih REST karena jawaban diperlukan sekarang. Tetapkan timeout, perilaku ketika layanan kupon tidak tersedia, dan idempotency untuk permintaan penebusan.

Kedua, sistem gudang perlu diberi tahu setelah pembayaran berhasil. Jika hanya satu sistem tujuan yang sudah disepakati, webhook dapat mengirim `PaymentSettled` ke endpoint gudang. Pengirim menyimpan status pengiriman; penerima memverifikasi pesan dan mendorong pekerjaan berat ke proses internal.

Ketiga, beberapa tim perlu mengirim email, memperbarui analitik, dan menyegarkan proyeksi setelah pembayaran. Terbitkan event pembayaran dan biarkan tiap consumer mengelola retry-nya. Bila email gagal, analitik tidak semestinya ikut tertahan.

Untuk pekerjaan rekonsiliasi malam hari, REST dapat membuat job, queue mengatur unit kerja, dan event mengumumkan `ReconciliationFinished`. Jadi pertanyaan yang berguna bukan “mana yang paling modern?”, melainkan “pada langkah mana keputusan harus menunggu, dan siapa yang bertanggung jawab jika proses berhenti?”.

| Pertanyaan desain | REST | Webhook | Queue | Event |
| --- | --- | --- | --- | --- |
| Siapa memulai? | Klien meminta | Pengirim memberi tahu | Producer menaruh pekerjaan | Producer menerbitkan fakta |
| Perlu jawaban segera? | Biasanya ya | Tidak untuk pekerjaan utama | Tidak | Tidak |
| Konsumen | Dikenal langsung | Endpoint tertentu | Consumer pekerjaan | Dapat banyak |
| Fokus reliabilitas | Timeout dan idempotensi | Retry pengirim dan deduplikasi | Ack, retry, dead-letter | Retensi, versi, rekonsiliasi |

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah menaruh pekerjaan panjang di request sinkron. Tanyakan: berapa lama klien boleh menunggu, dan apa yang terjadi jika respons hilang setelah server bekerja? Jika jawabannya “tidak tahu”, pisahkan penerimaan job dari pemrosesan.

Kesalahan kedua adalah menganggap HTTP 200 berarti bisnis selesai. Periksa apakah respons menyatakan hasil final atau hanya penerimaan. Simpan status pekerjaan yang dapat dilihat dan gunakan correlation ID untuk menelusuri transisi.

Kesalahan ketiga adalah retry tanpa idempotensi. Kirim ulang simulasi untuk request, webhook, dan message yang sama; pastikan efek bisnis hanya terjadi sesuai aturan yang disepakati. Uji juga pesan terlambat, duplikat, dan tidak berurutan.

Kesalahan keempat adalah menerbitkan event yang terlalu menyerupai perintah internal. Tinjau nama dan payload: apakah ia menyatakan fakta yang stabil, atau memaksa konsumen mengikuti desain producer? Dokumentasikan pemilik skema dan kebijakan perubahan.

Kesalahan kelima adalah menguji jalur sukses saja. Rencana pengujian perlu menghubungkan risiko, kebutuhan, hasil, dan cacat yang belum selesai. [NIST SP 800-218 SSDF 1.1](https://csrc.nist.gov/pubs/sp/800/218/final) menekankan praktik pengembangan aman dan keterlacakan; lulus uji otomatis hanya membuktikan pemeriksaan yang disampel pada lingkungan dan data tertentu, bukan jaminan seluruh alur aman atau andal.

Sebelum rilis, minta bukti untuk lima hal: kontrak payload dan versi; aturan timeout, retry, serta dead-letter; kepemilikan autentikasi dan otorisasi; metrik serta trace yang dapat dicari; dan prosedur rekonsiliasi ketika pesan hilang atau terlambat. [NEEDS GATE-03/GATE-04: koordinator perlu meninjau bukti implementasi dan threat model sebelum keputusan produksi].

## Jalan pintas yang tampak praktis

Shortcut yang sering dipilih adalah “pakai queue untuk semuanya supaya tidak pernah timeout”. Queue memang mengurangi waktu tunggu pemanggil, tetapi tidak menghapus timeout, duplikasi, backlog, atau kebutuhan mengetahui hasil. Pengguna tetap memerlukan status job, operator memerlukan alarm usia pesan, dan bisnis memerlukan aturan ketika pekerjaan gagal permanen.

Pilihan yang lebih aman adalah memetakan setiap langkah: REST untuk menerima perintah dan mengembalikan status penerimaan, queue untuk pekerjaan yang dapat ditunda, lalu event untuk fakta yang perlu diketahui banyak pihak. Tulis pemilik retry dan kondisi berhenti di kontrak. Jika satu pola dipaksakan ke semua langkah, coupling dan operasi biasanya hanya berpindah tempat.

## Kesimpulan dan langkah berikutnya

REST dipilih ketika keputusan harus dijawab sekarang; webhook ketika sistem tertentu perlu diberi callback; queue ketika pekerjaan boleh menunggu konsumen; event ketika fakta perlu dibagikan ke beberapa konsumen. Gabungan pola sering menjadi desain yang paling jujur terhadap alur bisnis.

Teman Codev.id, sebelum menulis kode, buat tabel kecil berisi waktu tunggu, pemilik retry, kebutuhan urutan, jumlah konsumen, dan bukti observability untuk tiap interaksi. Tinjau tabel itu bersama pemilik layanan, lalu minta pemeriksaan keamanan dan uji kegagalan yang sesuai. Sobat Codev.id dapat memakai tabel tersebut sebagai agenda review, bukan sebagai bukti bahwa semua risiko sudah tertutup. Jika Anda ingin meninjau konteks layanan yang tersedia, mulai dari [beranda Codev.id](/).

Aturan operasionalnya: jangan memilih pola dari popularitasnya; pilih berdasarkan siapa yang menunggu dan siapa yang bertanggung jawab ketika pesan terlambat, terduplikasi, atau gagal. Validasi teknis dan threat model tetap diperlukan sebelum rilis produksi.
