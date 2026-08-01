---
article_id: CDV-06-A04
writing_contract_version: "native-id-v2"
title: "Versioning dan Deprecation API Tanpa Memutus Klien"
slug: "versioning-dan-deprecation-api"
description: "Define compatibility, change classification, discovery, usage evidence, migration guide, overlap window, communication, and removal gate"
status: draft
publication_date: "2025-08-03"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-06
primary_intent: "Evolve an API while protecting consumers"
reader_community: "Codev.id"
reader_address: "Teman Codev.id"
final_route: "/artikel/versioning-dan-deprecation-api.html"
technical_review: required
sources:
  - "https://spec.openapis.org/oas/v3.1.1.html"
  - "https://www.rfc-editor.org/info/rfc9700/"
  - "https://owasp.org/API-Security/editions/2023/en/0x11-t10/"
  - "https://csrc.nist.gov/pubs/sp/800/218/final"
---

# Versioning dan Deprecation API Tanpa Memutus Klien

Halo, Teman Codev.id! Mengubah API tanpa memutus klien bukan berarti menambahkan angka versi lalu berharap semua konsumen segera berpindah. Cara yang lebih aman adalah menetapkan kontrak kompatibilitas, mengklasifikasikan perubahan, menemukan pemakai nyata, memberi jalur migrasi, menjalankan masa overlap, lalu baru menghapus versi lama setelah ada bukti.

Versi baru sebaiknya diperkenalkan ketika kontrak lama tidak lagi bisa dipertahankan tanpa risiko yang tidak dapat diterima. Perubahan yang masih kompatibel dapat tetap berada di versi yang sama, sedangkan perubahan yang mengubah bentuk respons, makna status, aturan autentikasi, atau perilaku yang diandalkan klien perlu diperlakukan sebagai perubahan kontrak. Jawaban ini berubah bila Anda belum mengetahui siapa pemakai endpoint, pola pemakaiannya, atau ancaman pada alur akses; dalam keadaan itu, keputusan penghapusan harus ditahan dan ditandai untuk tinjauan teknis.

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

*Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.*

## Definisi dan batas objek

Versioning adalah cara memberi identitas dan aturan pada kontrak API yang berubah. Deprecation adalah pemberitahuan bahwa kontrak atau endpoint masih tersedia untuk sementara, tetapi tidak lagi menjadi pilihan yang dipelihara dan memiliki rencana penghentian. Keduanya berbeda dari sekadar mengganti implementasi di belakang endpoint.

Kontrak mencakup path, parameter, tipe dan makna field, kode status, format error, autentikasi, batas laju, serta efek samping yang wajar diharapkan klien. Spesifikasi OpenAPI membantu mendeskripsikan antarmuka secara terstruktur, tetapi dokumen itu tidak membuktikan bahwa implementasi benar-benar berperilaku sesuai atau aman ([OpenAPI Specification 3.1.1](https://spec.openapis.org/oas/v3.1.1.html)). Karena itu, file spesifikasi, pengujian, dan observasi produksi harus diperlakukan sebagai bukti yang saling melengkapi.

Batas artikel ini adalah evolusi kontrak dan perlindungan konsumen. Ia tidak menetapkan satu skema universal seperti URL versioning atau header versioning, tidak menentukan tenggat migrasi yang berlaku untuk semua organisasi, dan tidak menggantikan persetujuan pemilik produk, keamanan, atau kepatuhan. Kontrak aktual, kemampuan klien, dan risiko bisnis tetap menjadi penentu.

## Cara kerjanya

Mulailah dari inventaris kontrak, bukan dari nomor versi. Catat endpoint yang berubah, konsumen internal maupun eksternal, pemilik masing-masing konsumen, dependensi transitif, serta kapan dan seberapa sering endpoint dipanggil. Pisahkan data dari gateway, log aplikasi, analitik, dan daftar langganan; tandai kualitas dan rentang waktunya agar keputusan tidak dibuat dari satu sampel.

Berikut urutan kerja yang dapat diaudit:

1. **Tetapkan aturan kompatibilitas.** Nyatakan apa yang dianggap kompatibel: misalnya penambahan field opsional mungkin aman bagi parser yang mengabaikan field tak dikenal, tetapi perubahan tipe, penghapusan field, perubahan semantik, atau perbedaan kode status biasanya memerlukan migrasi. Jangan mengasumsikan perilaku parser tanpa mengujinya pada klien yang benar-benar dipakai.
2. **Klasifikasikan perubahan.** Beri label non-breaking, breaking, atau behavioral-risk. Perubahan behavioral-risk tampak kompatibel di skema, namun dapat mengubah urutan, default, paginasi, idempotensi, atau frekuensi error yang sudah dipakai klien.
3. **Terbitkan kontrak dan contoh migrasi.** OpenAPI dapat menjadi sumber spesifikasi, tetapi sertakan contoh request/response, daftar perubahan per endpoint, padanan field, perubahan error, dan langkah rollback. Bedakan contoh normatif dari ilustrasi agar konsumen tidak menyalin asumsi yang tidak dijamin.
4. **Sediakan discovery.** Tandai versi pada dokumentasi, katalog layanan, respons metadata, atau kanal komunikasi yang benar-benar dijangkau konsumen. Informasi deprecation harus dapat ditemukan sebelum klien menerima kegagalan.
5. **Jalankan overlap.** Pertahankan versi lama dan baru bersamaan selama periode yang ditentukan oleh bukti penggunaan serta siklus rilis konsumen. Selama overlap, ukur trafik per versi, error, latensi, dan keberhasilan alur bisnis; jangan hanya menghitung jumlah klien yang mengaku sudah migrasi.
6. **Tutup dengan removal gate.** Hapus versi lama hanya jika penggunaan yang tersisa dipahami, pemiliknya sudah diberi kesempatan bertindak, jalur eskalasi tersedia, dan bukti regresi pada versi baru telah ditinjau. Simpan keputusan, pengecualian, dan rencana pemulihan.

Untuk perubahan autentikasi atau otorisasi, jangan memilih alur hanya karena nyaman. RFC 9700 merupakan pembaruan praktik terbaik OAuth 2.0 pada 2025, sementara OWASP menempatkan kelemahan otorisasi dan penyalahgunaan API sebagai risiko yang perlu dikendalikan ([RFC 9700](https://www.rfc-editor.org/info/rfc9700/); [OWASP API Security Top 10 2023](https://owasp.org/API-Security/editions/2023/en/0x11-t10/)). [NEEDS GATE-03/GATE-04 REVIEW: alur akses, konteks klien, dan model ancaman belum disediakan dalam paket ini.]

## Faktor yang mengubah hasil

Beberapa kondisi membuat masa overlap perlu lebih panjang atau aturan kompatibilitas lebih ketat:

- **Jenis konsumen.** Klien yang dirilis serentak oleh satu tim berbeda dari aplikasi mobile, integrasi vendor, atau perangkat yang jarang diperbarui. Ketahui siapa yang dapat melakukan rilis dan siapa yang tidak.
- **Bentuk perubahan.** Menambah field nullable tidak sama dengan mengubah makna field lama. Perubahan default, pembulatan, zona waktu, paginasi, atau urutan hasil dapat memutus logika tanpa mengubah skema.
- **Bukti pemakaian.** Trafik rendah bukan bukti bahwa endpoint tidak penting; bisa jadi hanya dipakai saat penutupan periode atau keadaan darurat. Cari jejak panggilan, pemilik, dan kritikalitas alur.
- **Keamanan dan privasi.** Memperketat scope, mengganti kredensial, atau mengubah verifikasi dapat membuat klien gagal secara benar dari sudut keamanan. Perubahan seperti ini memerlukan rencana komunikasi dan fallback yang tidak menghidupkan kembali akses berisiko.
- **Kualitas verifikasi.** Pengujian otomatis membuktikan assertion, lingkungan, build, dan data yang disampel—bukan seluruh perilaku produksi. NIST SSDF menekankan praktik pengembangan aman dan keterlacakan risiko ke hasil verifikasi; gunakan temuan itu untuk keputusan rilis, bukan angka cakupan tunggal ([NIST SP 800-218 SSDF 1.1](https://csrc.nist.gov/pubs/sp/800/218/final)).

Teman Codev.id, tanyakan juga siapa yang berwenang menyetujui pengecualian. Satu klien penting yang belum dapat memperbarui diri dapat mengubah removal gate, tetapi pengecualian itu harus memiliki pemilik, tanggal tinjau, dan kompensasi risiko yang jelas.

## Contoh keputusan praktis

Bayangkan endpoint `GET /orders` akan mengganti `total` dari angka bulat menjadi objek dengan mata uang dan pecahan. Walau nama field tetap, kontraknya berubah. Keputusan yang masuk akal adalah mempertahankan representasi lama pada versi atau field lama, menerbitkan representasi baru dengan dokumentasi migrasi, lalu mengukur penggunaan keduanya. Jangan menghapus field lama hanya karena klien utama sudah lulus uji.

Gunakan tabel keputusan sederhana berikut saat rapat perubahan:

| Pertanyaan | Jika jawabannya “ya” | Keputusan awal |
|---|---|---|
| Apakah bentuk atau makna respons berubah? | Parser atau aturan bisnis bisa berbeda | Perlakukan sebagai breaking/behavioral-risk |
| Apakah semua konsumen teridentifikasi dan dapat merilis? | Migrasi dapat dikoordinasikan | Rancang overlap dengan bukti trafik |
| Apakah ada konsumen tanpa pemilik jelas? | Tidak ada pihak untuk menerima notifikasi | Tahan removal, lakukan discovery |
| Apakah perubahan menyentuh token, scope, atau otorisasi? | Risiko akses ikut berubah | Wajibkan tinjauan keamanan dan gate yang relevan |
| Apakah trafik versi lama nol pada satu jendela observasi? | Bisa merupakan periode tidak aktif | Konfirmasi pola musiman sebelum menghapus |

Dokumen migrasi minimal berisi diff kontrak, contoh sebelum-sesudah, langkah uji konsumen, tanggal checkpoint (bukan janji universal), kanal dukungan, dan prosedur rollback. Setiap konsumen menandai statusnya sendiri; pemilik API tidak boleh mengubah status “selesai” berdasarkan asumsi.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah menganggap major version sebagai izin untuk memutus kapan saja. Periksa setiap perubahan terhadap kontrak aktual dan catatan penggunaan. Kesalahan kedua adalah mengumumkan deprecation di dokumentasi yang jarang dibaca. Cocokkan kanal pengumuman dengan daftar pemilik dan log panggilan.

Kesalahan ketiga ialah mengukur migrasi dari status HTTP saja. Tambahkan metrik keberhasilan alur, error semantik, dan konsistensi data pada konsumen perwakilan. Kesalahan keempat adalah memperpanjang versi lama tanpa batas karena satu pengecualian. Buat pengecualian tertulis dengan pemilik dan tanggal keputusan berikutnya.

Shortcut yang paling menggoda adalah menjalankan dua implementasi tanpa kontrak dan membiarkan klien “mencoba sendiri”. Itu memindahkan biaya perubahan ke setiap konsumen, menyulitkan diagnosis perbedaan perilaku, dan dapat memperlebar permukaan serangan. Alternatif yang lebih aman adalah kontrak yang dapat dibandingkan, contoh migrasi yang diuji, observasi per versi, dan removal gate yang dapat ditolak ketika bukti belum cukup.

Kawan Codev.id, bila bukti penggunaan bertentangan—misalnya dashboard kosong tetapi tim bisnis melaporkan integrasi periodik—anggap konflik itu sebagai pekerjaan discovery, bukan alasan untuk memilih angka yang paling nyaman. Catat sumber data, periode observasi, dan pertanyaan yang harus dijawab sebelum keputusan final.

## Kesimpulan

Versioning dan deprecation API yang tidak memutus klien adalah proses pengendalian kontrak: klasifikasikan perubahan, temukan pemakai nyata, publikasikan migrasi, jalankan overlap berbasis bukti, dan hapus versi lama hanya setelah gate terpenuhi. Tidak ada durasi atau skema yang berlaku universal; kontrak konsumen dan risiko akses menentukan keputusan.

Langkah berikutnya, buat satu lembar perubahan untuk endpoint yang akan diubah: diff kontrak, daftar konsumen, bukti trafik, pemilik, checkpoint, serta kriteria removal. Minta tinjauan teknis dan keamanan untuk perubahan autentikasi atau otorisasi sebelum menetapkan tanggal penghentian. Untuk konteks proyek dan referensi pengembangan lain, mulai dari [Codev.id](/).

Aturan operasionalnya sederhana: jika Anda belum dapat menunjukkan siapa yang masih memakai versi lama dan bagaimana mereka pulih, versi itu belum siap dihapus.
