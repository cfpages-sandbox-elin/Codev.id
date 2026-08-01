---
article_id: CDV-06-A01
title: "Contract-first API dengan OpenAPI"
slug: "contract-first-api-dengan-openapi"
description: "Menyusun kontrak OpenAPI berversi yang memuat operasi, skema, error, keamanan, contoh, dan alur peninjauan"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2025-07-21"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-06
primary_intent: "Define an HTTP API before parallel implementation"
reader_community: "Codev.id"
reader_address: "Teman Codev.id"
final_route: "/artikel/contract-first-api-dengan-openapi.html"
technical_review: required
sources:
  - "https://spec.openapis.org/oas/v3.1.1.html"
  - "https://www.rfc-editor.org/info/rfc9700/"
  - "https://www.w3.org/TR/webauthn-3/"
  - "https://owasp.org/API-Security/editions/2023/en/0x11-t10/"
  - "https://csrc.nist.gov/pubs/sp/800/218/final"
---

# Contract-first API dengan OpenAPI

Halo, Teman Codev.id! Saat tim produsen API dan tim konsumen mulai bekerja bersamaan, shortcut “buat endpoint dulu, dokumentasi belakangan” sering memindahkan biaya ke tahap integrasi. Field berubah tanpa pemberitahuan, arti kode error berbeda, dan setiap tim membuat asumsi sendiri.

Jawaban singkatnya: tetapkan kontrak HTTP yang versioned (memiliki versi dan riwayat perubahan) sebelum implementasi paralel. OpenAPI menjadi format bersama untuk operasi, parameter, skema data, respons, keamanan, dan contoh. Kontrak itu membantu orang dan alat menyepakati bentuk antarmuka, tetapi tidak membuktikan bahwa kode sudah mengikuti perilaku atau sudah aman. Spesifikasi OpenAPI memang mendeskripsikan antarmuka, bukan jaminan implementasi [OpenAPI Specification 3.1.1](https://spec.openapis.org/oas/v3.1.1.html).

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

Ilustrasi umum dari aset lokal codev.id; bukan dokumentasi proyek tertentu.

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

Contract-first bukan berarti semua keputusan teknis harus selesai sebelum satu baris kode ditulis. Yang harus dibekukan lebih dahulu adalah perilaku yang perlu disepakati lintas tim: operasi dan method, input, output, error, aturan autentikasi, contoh, serta kompatibilitas versi. Detail internal seperti framework, tabel database, dan struktur modul boleh berkembang selama tidak melanggar kontrak.

Salah paham yang berbahaya adalah menganggap file OpenAPI sebagai sertifikat keamanan. Dokumen dapat menyatakan `securitySchemes`, namun server mungkin lupa menerapkannya, mengizinkan cakupan akses terlalu luas, atau menerima input yang tidak sesuai skema. Karena itu, kontrak harus menjadi objek review dan pengujian, bukan satu-satunya bukti. Untuk OAuth, pilihan alur bergantung pada jenis klien dan ancaman; RFC 9700 adalah pembaruan praktik terbaik OAuth 2.0 pada 2025, tetapi tetap perlu analisis konteks [OAuth 2.0 Security BCP—RFC 9700](https://www.rfc-editor.org/info/rfc9700/).

## Definisi dan batas objek

Kontrak adalah kesepakatan yang dapat dibaca manusia dan alat tentang HTTP API. Di dalam OpenAPI, tim biasanya memodelkan `paths` dan operasi, parameter path/query/header, request body, respons berdasarkan status, komponen skema, serta mekanisme keamanan. Setiap perubahan pada elemen itu harus terlihat dalam diff dan diberi versi atau aturan kompatibilitas yang disepakati.

Kontrak bukan implementasi, bukan spesifikasi database, dan bukan pengganti persetujuan pemilik produk. Ia juga bukan tempat menaruh rahasia, token nyata, alamat internal, atau skema yang belum boleh dipublikasikan. Jika endpoint masih eksperimental, tandai statusnya dan tentukan apakah konsumen boleh mengandalkannya.

Batas ini mengubah keputusan: “valid menurut schema” hanya menjawab bentuk data, bukan apakah aturan bisnis benar. Respons `200` dengan objek kosong mungkin sah secara sintaksis, tetapi tetap gagal bila konsumen membutuhkan `id` dan status. Sebaliknya, menambahkan field opsional biasanya lebih mudah kompatibel daripada mengubah arti field wajib; keputusan final tetap mengikuti kebijakan versi tim.

## Cara kerjanya

Mulai dari skenario konsumen, bukan dari daftar tabel. Tulis tujuan operasi, aktor, prasyarat, dan hasil yang dapat diamati. Lalu susun kontrak dalam urutan berikut.

1. **Operasi dan identitas.** Beri method, path, `operationId`, ringkasan, dan tag yang stabil. Jelaskan parameter mana yang wajib serta formatnya.
2. **Skema dan batas input.** Definisikan tipe, kewajiban, enum, format, batas panjang, dan hubungan antarobjek. Bedakan `null`, field hilang, dan nilai kosong bila konsumen memerlukannya.
3. **Respons dan error.** Cantumkan status sukses, status validasi, autentikasi/otorisasi, konflik, dan kegagalan sementara yang memang dapat terjadi. Bentuk error perlu konsisten: kode yang dapat diproses mesin, pesan aman untuk pengguna, dan korelasi untuk penelusuran.
4. **Keamanan.** Nyatakan skema keamanan pada operasi yang memerlukannya dan ruang lingkup akses. Jangan menyalin alur OAuth secara otomatis. Untuk klien yang mendukung passkey, WebAuthn mendefinisikan API dan protokol kredensial kunci publik, tetapi pilihan pendaftaran, login, pemulihan, dan perangkat tetap keputusan desain [WebAuthn Level 3](https://www.w3.org/TR/webauthn-3/).
5. **Contoh realistis.** Sediakan request, respons, dan error yang cukup untuk membuat stub serta pengujian. Contoh harus anonim dan tidak mengandung kredensial.
6. **Pemeriksaan otomatis.** Lint kontrak, validasi contoh terhadap skema, buat mock atau stub, lalu jalankan contract test terhadap implementasi. Hasil ini menunjukkan pemeriksaan yang dilakukan pada build dan data tertentu, bukan jaminan universal.

Setelah pull request kontrak disetujui, produsen dan konsumen dapat mengembangkan cabang masing-masing. Perubahan breaking memerlukan versi atau persetujuan eksplisit; perubahan non-breaking tetap perlu changelog dan notifikasi. Hubungkan requirement, risiko, test, dan cacat terbuka agar keputusan rilis dapat ditelusuri, sejalan dengan penekanan traceability dalam NIST SSDF 1.1 [NIST SP 800-218](https://csrc.nist.gov/pubs/sp/800/218/final).

## Faktor yang mengubah hasil

Jenis klien menentukan keamanan dan kompatibilitas. Browser publik, aplikasi mobile, service-to-service, dan perangkat dengan koneksi terbatas memiliki kemampuan penyimpanan kredensial, rotasi, dan jaringan yang berbeda. Threat model harus menjawab siapa penyerang, aset apa yang dilindungi, dan dampak penyalahgunaan sebelum memilih token, cakupan, atau passkey. **[NEEDS THREAT-MODEL AND CLIENT-CONTEXT REVIEW: alur keamanan tidak boleh diputuskan dari kontrak saja.]**

Perubahan data juga berpengaruh. Apakah konsumen mengabaikan field tambahan? Apakah urutan array bermakna? Apakah timestamp selalu UTC? Pertanyaan ini perlu aturan tertulis dan contoh negatif, bukan asumsi dari nama field.

Lingkungan rilis mengubah bukti. Mock server dapat memeriksa bentuk respons, sedangkan pengujian integrasi memeriksa wiring, otorisasi, dan data uji. Pemeriksaan keamanan khusus, review privasi, dan uji beban memerlukan pemilik yang kompeten. Tidak ada ambang coverage atau “piramida tes” universal yang otomatis menyatakan API siap; tetapkan kriteria berdasarkan risiko dan bukti yang benar-benar dikumpulkan.

Kawan Codev.id, jadikan setiap hasil pemeriksaan sebagai catatan yang bisa ditelusuri: versi kontrak, commit yang diuji, lingkungan, data uji, dan cacat yang masih terbuka. Dengan begitu, perdebatan “sudah dites” berubah menjadi pertanyaan yang dapat dijawab.

## Contoh keputusan praktis

Bayangkan tim perlu operasi `POST /orders`. Konsumen meminta membuat pesanan dan mengetahui kapan aman mencoba ulang.

| Keputusan kontrak | Pilihan yang dapat ditinjau | Pertanyaan sebelum merge |
|---|---|---|
| Identitas operasi | `operationId: createOrder`, request memiliki `items` wajib | Apakah nama stabil untuk generator dan log? |
| Duplikasi request | Header idempotensi didokumentasikan bila server mendukungnya | Apa respons ketika kunci yang sama dikirim ulang? |
| Error | `400` untuk bentuk invalid, `409` untuk konflik, `401/403` sesuai kebijakan | Apakah konsumen dapat membedakan perbaikan input dan retry? |
| Keamanan | Skema autentikasi dan scope ditulis pada operasi | Siapa yang menyetujui scope minimum? |
| Evolusi | Field baru opsional, perubahan makna memerlukan versi | Bagaimana konsumen diberi tahu dan kapan versi lama dihentikan? |

Jika server belum mendukung idempotensi, jangan menuliskannya seolah-olah tersedia. Tandai keputusan terbuka, buat test yang gagal secara sengaja, dan minta pemilik layanan memilih perilaku. Kontrak yang jujur lebih berguna daripada contoh yang tampak lengkap tetapi menyesatkan.

Untuk langkah awal, Anda dapat menaruh kontrak dan changelog di repositori bersama kode. Halaman [beranda Codev.id](/) dapat menjadi titik kembali untuk konteks teknis lain, tetapi jangan mengganti tautan itu dengan rute sibling yang masih direncanakan.

## Kesalahan umum dan cara memeriksanya

**Mendokumentasikan setelah coding.** Bandingkan diff implementasi dengan kontrak sebelum merge; perubahan path, status, atau required field harus terlihat.

**Hanya menguji `200`.** Tambahkan contoh invalid, tanpa kredensial, scope kurang, konflik, timeout, dan respons parsial yang memang mungkin. OWASP API Security Top 10 menekankan risiko seperti broken object-level authorization dan unrestricted resource consumption; gunakan daftar itu untuk memeriksa kontrol, bukan untuk mengklaim sistem sudah aman [OWASP API Security Top 10 2023](https://owasp.org/API-Security/editions/2023/en/0x11-t10/).

**Menaruh secret di contoh.** Gunakan placeholder yang jelas dan pemindaian secret pada CI. Kontrak boleh dibagikan; kredensial tidak.

**Menganggap generator sebagai oracle.** Kode hasil generator tetap perlu review, test perilaku, dan pemeriksaan keamanan. OpenAPI yang valid tidak membuktikan handler, policy gateway, atau logging sudah benar.

**Memilih OAuth atau passkey karena sedang populer.** Kembalikan keputusan ke tipe klien, threat model, pemulihan akun, dan kemampuan operasional. Bila konteks itu belum tersedia, berhenti pada kontrak abstrak dan sisakan keputusan untuk review spesialis.

## Jalan pintas yang perlu ditolak

Shortcut paling menggoda adalah menyalin file OpenAPI lama, mengganti path, lalu menyatakan API “contract-first”. Cara itu gagal ketika asumsi autentikasi, error, atau kompatibilitas dari layanan lama tidak berlaku. Alternatif yang lebih aman: buat change request kecil, tulis skenario konsumen dan contoh error, minta produsen serta konsumen menyetujui diff, kemudian buktikan implementasi dengan lint, contract test, dan review risiko. Bukti yang lulus hanya berlaku untuk build, lingkungan, dan data yang diperiksa.

## Kesimpulan

Contract-first API dengan OpenAPI berarti menyepakati antarmuka versioned sebelum produsen dan konsumen mengimplementasikan bagian masing-masing. Modelkan operasi, skema, error, keamanan, contoh, dan aturan perubahan; gunakan sumber resmi untuk memahami batas format dan praktik keamanan, lalu verifikasi perilaku nyata melalui test serta review.

Teman Codev.id, tindakan berikutnya adalah membuka pull request kontrak untuk satu alur bisnis, melampirkan threat model singkat, contoh sukses dan gagal, matriks kompatibilitas, serta daftar pemeriksaan yang sudah dijalankan. Minta pemilik keamanan meninjau alur autentikasi dan otorisasi sebelum kontrak dianggap siap. Aturan operasionalnya: kontrak menyatakan janji antarmuka, sedangkan implementasi dan review manusia yang membuktikan apakah janji itu benar-benar dipenuhi.
