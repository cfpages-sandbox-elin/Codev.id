---
article_id: CDV-06-A03
writing_contract_version: "native-id-v2"
title: "Autentikasi dan Otorisasi API Bukan Hal yang Sama"
slug: "autentikasi-dan-otorisasi-api"
description: "Memetakan identitas pemanggil, cakupan token atau sesi, kepemilikan sumber daya, kebijakan izin, akun layanan, audit, dan pengujian penolakan"
status: draft
publication_date: "2025-07-29"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-06
primary_intent: "Separate API identity from resource/action permission"
reader_community: "Codev.id"
reader_address: "Kawan Codev.id"
final_route: "/artikel/autentikasi-dan-otorisasi-api.html"
technical_review: required
sources:
  - "https://spec.openapis.org/oas/v3.1.1.html"
  - "https://www.rfc-editor.org/info/rfc9700/"
  - "https://www.w3.org/TR/webauthn-3/"
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

# Autentikasi dan Otorisasi API Bukan Hal yang Sama

Halo, Kawan Codev.id! Saat sebuah API menolak atau menerima permintaan, ada dua pertanyaan berbeda: “Siapa yang memanggil?” dan “Apa yang boleh dilakukan pemanggil itu?” Pertanyaan pertama adalah autentikasi (authentication), sedangkan pertanyaan kedua adalah otorisasi (authorization). Token yang valid hanya menjawab identitas atau kredensial; token itu belum otomatis memberi hak membaca, mengubah, atau menghapus setiap sumber daya.

Kesalahan mencampur keduanya membuat endpoint terlihat aman padahal izin terlalu luas. Perbaikannya adalah memetakan identitas pemanggil, konteks token atau sesi, kepemilikan resource, lalu menerapkan kebijakan pada setiap aksi. OpenAPI dapat mendeskripsikan skema keamanan dan antarmuka, tetapi deskripsi kontrak tidak membuktikan perilaku implementasi di runtime ([OpenAPI Specification 3.1.1](https://spec.openapis.org/oas/v3.1.1.html)). Pilihan alur token juga harus mengikuti tipe klien dan model ancaman; [NEEDS CONTEXT REVIEW: tipe klien, threat model, dan desain token/sesi sebelum alur autentikasi dipilih].

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.

## Definisi dan batas objek

Autentikasi memeriksa bukti yang dibawa caller: misalnya kredensial pengguna, sesi, kunci API, atau kredensial layanan. Hasilnya sebaiknya berupa identitas yang dapat ditelusuri, seperti `user_id`, `service_id`, atau subjek lain yang jelas. “Terautentikasi” berarti sistem mengenali siapa atau apa yang mengirim permintaan, bukan berarti semua tindakannya sah.

Otorisasi mengambil identitas itu bersama konteks permintaan: endpoint, metode HTTP, organisasi, peran, scope token, resource yang dituju, dan tindakan yang diminta. Kebijakan kemudian menjawab apakah aksi tersebut diizinkan. Seorang pengguna dapat terautentikasi sebagai anggota organisasi A tetapi tetap ditolak saat mencoba mengubah dokumen milik organisasi B. Demikian juga service account untuk proses sinkronisasi mungkin boleh membaca antrean tertentu, tetapi tidak boleh menghapus akun manusia.

Batas ini penting karena autentikasi biasanya terjadi di lapisan masuk, sedangkan otorisasi harus tetap diperiksa ketika objek dan aksi sudah diketahui. API publik, partner, dan privat dapat memakai mekanisme identitas yang berbeda, namun semuanya memerlukan keputusan izin yang eksplisit. Artikel ini tidak memberi resep implementasi OAuth; integrasi OAuth dan kontrol identitas khusus memerlukan konteks tersendiri serta tinjauan profesional.

## Cara kerjanya

Urutan praktisnya dapat dipandang sebagai rantai keputusan berikut.

1. **Terima dan batasi permintaan.** Gateway atau aplikasi memeriksa format dasar, ukuran, dan kredensial tanpa mencatat rahasia. Kredensial yang gagal tidak boleh diperlakukan sebagai identitas yang sah.
2. **Bangun konteks identitas.** Setelah bukti diverifikasi, sistem membuat konteks terautentikasi: subjek, penerbit, masa berlaku, audience, dan bila relevan scope. RFC 9700 adalah pembaruan praktik terbaik keamanan OAuth 2.0; rujukannya membantu menilai pilihan alur, tetapi tidak menggantikan analisis klien dan ancaman ([RFC 9700](https://www.rfc-editor.org/info/rfc9700/)).
3. **Resolve resource.** URL seperti `/orders/123` harus dipetakan ke data nyata dan tenant yang benar. Jangan memakai `user_id` dari body sebagai bukti kepemilikan; gunakan konteks identitas dan relasi yang disimpan server.
4. **Evaluasi kebijakan.** Cocokkan subjek, resource, aksi, scope, dan kondisi bisnis. Periksa izin pada jalur baca maupun tulis, termasuk endpoint turunan, ekspor, pencarian, dan operasi batch.
5. **Terapkan keputusan dan catat secukupnya.** Izinkan hanya setelah semua syarat terpenuhi. Penolakan harus konsisten, sementara audit menyimpan subjek, aksi, resource, waktu, hasil, dan korelasi tanpa membocorkan token atau data sensitif.

Passkey dapat memperkuat bukti autentikasi pengguna melalui WebAuthn, tetapi keberhasilan upacara WebAuthn tetap hanya menghasilkan identitas yang terverifikasi. Hak atas resource masih ditentukan kebijakan API ([WebAuthn Level 3](https://www.w3.org/TR/webauthn-3/)).

## Faktor yang mengubah hasil

Beberapa kondisi sering mengubah keputusan izin meskipun identitas sama:

- **Jenis caller.** Browser pengguna, aplikasi mobile, integrasi partner, dan job internal memiliki kemampuan menyimpan rahasia serta risiko pencurian yang berbeda. Service account perlu identitas dan ruang kerja yang terpisah dari akun manusia; jangan menyamakan “proses internal” dengan administrator.
- **Scope dan peran.** Scope token seharusnya membatasi kelas aksi, bukan menjadi pengganti pemeriksaan kepemilikan. Peran `editor` pada organisasi A tidak berarti editor global.
- **Kepemilikan dan tenancy.** Setiap query perlu mengikat resource pada tenant atau pemilik yang berasal dari konteks tepercaya. ID yang dapat ditebak, endpoint batch, dan filter pencarian adalah tempat umum terjadinya akses lintas objek.
- **Siklus hidup sesi.** Kedaluwarsa, pencabutan, rotasi kredensial, dan perubahan keanggotaan dapat membuat keputusan lama tidak lagi berlaku. Tetapkan kapan konteks izin harus dihitung ulang.
- **Jalur non-utama.** Unduhan berkas, webhook, GraphQL resolver, fungsi admin, dan ekspor sering melewati middleware yang sama secara tidak lengkap. OWASP menempatkan broken object-level authorization dan broken authentication sebagai risiko utama API; pengujian harus menyasar objek serta aksi, bukan hanya status login ([OWASP API Security Top 10 2023](https://owasp.org/API-Security/editions/2023/en/0x11-t10/)).
- **Bukti dan observabilitas.** Log yang terlalu minim menghambat investigasi; log yang memuat token, password, atau payload sensitif menambah risiko. Tentukan bidang audit dan retensinya bersama pemilik data.

Kawan Codev.id, jangan menyimpulkan “aman” dari satu status `200` pada happy path. Yang perlu dibuktikan adalah kombinasi identitas, resource, dan aksi pada konteks yang berubah.

## Contoh keputusan praktis

Gunakan matriks kecil sebelum menulis middleware. Contoh berikut adalah skenario bersyarat, bukan klaim tentang proyek tertentu.

| Caller terautentikasi | Resource dan aksi | Keputusan yang diharapkan | Alasan yang harus diuji |
|---|---|---|---|
| Pengguna organisasi A | Membaca invoice milik A | Izinkan bila scope baca dan status keanggotaan berlaku | Identitas cocok dengan tenant dan aksi |
| Pengguna organisasi A | Membaca invoice milik B | Tolak | Kepemilikan berbeda meski token valid |
| Editor organisasi A | Menghapus invoice A | Tolak bila peran hanya mengizinkan ubah | Peran tidak boleh diperluas diam-diam |
| Service account sinkronisasi | Membaca antrean sinkronisasi yang ditetapkan | Izinkan terbatas | Audience, scope, dan resource dipatok |
| Service account sinkronisasi | Mengubah kebijakan akses pengguna | Tolak | Tanggung jawab layanan berbeda |
| Token kedaluwarsa atau dicabut | Aksi apa pun | Tolak dan minta autentikasi ulang sesuai kontrak | Konteks identitas tidak lagi berlaku |

Untuk setiap baris, tulis predikat kebijakan dalam bahasa yang bisa diuji: `subject`, `tenant`, `resource_owner`, `action`, `scope`, dan kondisi tambahan. Jika salah satu nilai tidak tersedia, keputusan default harus menolak sambil menghasilkan sinyal audit yang dapat ditindaklanjuti.

## Kesalahan umum dan cara memeriksanya

**“Ada bearer token berarti bebas.”** Periksa endpoint yang menerima token dengan scope kosong atau terlalu luas. Uji token pengguna A pada resource B, termasuk variasi ID, urutan halaman, dan operasi batch.

**Otorisasi hanya di gateway.** Gateway dapat memeriksa kredensial dan rute, tetapi service yang mengetahui pemilik objek tetap harus mengevaluasi izin. Buat pengujian langsung ke setiap service yang memiliki data.

**ID dari klien dianggap sebagai pemilik.** Bandingkan `owner_id` dari body atau query dengan relasi di server. Ubah nilai itu dalam pengujian dan pastikan sistem tidak berpindah tenant.

**Admin bypass tanpa jejak.** Jika ada jalur dukungan atau impersonasi, dokumentasikan siapa yang menyetujui, alasan, durasi, dan auditnya. “Internal” bukan alasan untuk menghapus kontrol.

**Tes hanya untuk respons sukses.** Tambahkan denial tests: kredensial hilang, token kedaluwarsa, scope tidak cukup, role salah, objek milik tenant lain, resource tidak ada, dan kombinasi batch campuran. Catat apakah respons, log, dan efek samping sesuai kebijakan.

NIST SSDF menekankan keterlacakan risiko, persyaratan, dan hasil verifikasi; lulus tes otomatis hanya membuktikan assertion, lingkungan, build, dan data yang disampel ([NIST SP 800-218 SSDF 1.1](https://csrc.nist.gov/pubs/sp/800/218/final)). Karena itu, simpan matriks izin, daftar skenario penolakan, hasil pengujian, serta defect yang belum selesai sebagai paket review. [NEEDS SECURITY REVIEW: konfirmasi cakupan denial test, audit event, dan residual risk pada lingkungan target].

## Jalan pintas yang tampak praktis

Shortcut yang sering dipilih adalah memakai satu API key bersama untuk semua integrasi lalu menganggap pembatasan jaringan sudah cukup. Cara ini mengaburkan caller, memperbesar dampak kebocoran satu kunci, dan menyulitkan pencabutan satu pihak tanpa memutus pihak lain. Alternatif yang lebih dapat diaudit adalah identitas per caller atau service account, scope minimum, rotasi dan pencabutan yang terukur, serta pemeriksaan resource di service pemilik data. Detail umur token, media penyimpanan, dan alur pertukaran tetap harus ditinjau berdasarkan klien dan threat model; jangan menyalin konfigurasi dari konteks lain.

## Penutup: aturan operasi

Autentikasi menjawab siapa pemanggilnya; otorisasi menjawab aksi apa yang boleh dilakukan pada resource tertentu. Pisahkan keduanya dalam model data, middleware, kebijakan, audit, dan pengujian. Sebelum rilis, minta tim mengisi matriks `subject–resource–action`, menjalankan skenario izin dan penolakan, lalu menelusurkan setiap hasil ke risiko dan defect yang masih terbuka. Untuk langkah awal dan konteks layanan, gunakan [beranda Codev.id](/) sebagai titik masuk dokumentasi yang tersedia. Teman Codev.id, bila bukti itu belum ada atau konteks klien dan ancaman belum jelas, perlakukan keputusan izin sebagai belum selesai dan minta tinjauan teknis sebelum endpoint dibuka.
