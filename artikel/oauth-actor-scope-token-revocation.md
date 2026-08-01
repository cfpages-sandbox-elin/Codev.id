---
article_id: CDV-08-A02
title: "OAuth Integration: Actor, Scope, Token, dan Revocation"
slug: "oauth-actor-scope-token-revocation"
description: "Panduan memetakan pelaku, alur pengalihan, ruang izin, persetujuan, penyimpanan dan pergantian token, pembaruan, pencabutan, kesalahan, serta pemutusan akun pada integrasi OAuth."
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2025-09-17"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-08
primary_intent: "Model a delegated-authorization integration safely"
reader_community: "Codev.id"
reader_address: "Teman Codev.id"
final_route: "/artikel/oauth-actor-scope-token-revocation.html"
technical_review: required
sources:
  - "https://spec.openapis.org/oas/v3.1.1.html"
  - "https://www.rfc-editor.org/info/rfc9700/"
  - "https://www.w3.org/TR/webauthn-3/"
  - "https://owasp.org/API-Security/editions/2023/en/0x11-t10/"
  - "https://www.cisa.gov/sbom"
  - "https://csrc.nist.gov/pubs/sp/800/161/r1/final"
  - "https://securityscorecards.dev/"
---

Halo, Teman Codev.id!

OAuth bukan sekadar tombol “Izinkan”. Integrasi yang aman harus dapat menjawab siapa meminta akses, atas nama siapa, akses apa yang diberikan, di mana token disimpan, dan bagaimana akses dihentikan. Jika salah satu jawaban itu kabur, akun yang sudah tidak dipakai bisa tetap memiliki jalur ke data.

Model praktisnya adalah memisahkan aktor, alur pengalihan (redirect), scope, consent, token, refresh, dan revocation sebagai keputusan yang bisa diaudit. OAuth 2.0 Security Best Current Practice (BCP) edisi yang dirujuk RFC 9700 menjadi rujukan keamanan terkini, tetapi tidak menggantikan dokumentasi provider atau penilaian ancaman untuk proyek Anda ([RFC 9700](https://www.rfc-editor.org/info/rfc9700/)). Jadi, gunakan artikel ini untuk membentuk model dan daftar pemeriksaan; detail endpoint, parameter, serta perilaku error tetap harus dikonfirmasi pada provider yang dipilih.

# OAuth Integration: Actor, Scope, Token, dan Revocation

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

Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.

*Gambar ini merupakan aset lokal untuk ilustrasi dan bukan dokumentasi proyek tertentu.*

## Jawaban singkat dan salah paham utama

Dalam integrasi OAuth, **resource owner** adalah pemilik akun atau data, **client** adalah aplikasi Anda, **authorization server** menerbitkan izin dan token, sedangkan **resource server** melayani API yang dilindungi. Provider dapat menggabungkan dua peran server itu, tetapi model tanggung jawabnya tetap perlu ditulis. Pengguna memberi consent kepada client untuk scope tertentu; client tidak otomatis memiliki seluruh hak akun.

Salah paham yang paling mahal adalah menganggap access token sama dengan sesi login permanen. Access token adalah kredensial berumur terbatas untuk API. Refresh token, bila provider menerbitkannya, adalah kredensial berbeda dengan dampak lebih besar karena dapat dipakai memperoleh access token baru. Keduanya harus memiliki jalur penyimpanan, rotasi, pencabutan, dan penghapusan yang jelas.

Keputusan akhir berubah menurut provider, tipe client (server-side, mobile, atau browser), data yang diakses, dan ancaman yang diterima. [NEEDS GATE-04: konfirmasi flow resmi, endpoint, dukungan PKCE, masa berlaku, rotasi, dan mekanisme revocation pada provider target sebelum implementasi.] Tanpa konfirmasi itu, contoh di sini hanya model kontrol, bukan resep endpoint.

## Definisi dan batas objek

Mulailah dengan lembar kontrak satu halaman. Tuliskan aktor dan data yang mengalir, lalu bedakan tiga objek berikut.

| Objek | Fungsi | Pertanyaan kontrol |
| --- | --- | --- |
| Scope | Hak yang diminta, misalnya membaca kalender atau membuat transaksi | Apakah tiap scope benar-benar dibutuhkan? |
| Access token | Bukti akses sementara ke resource server | Di mana disimpan, kapan kedaluwarsa, dan bagaimana dicabut? |
| Refresh token | Bukti untuk meminta access token baru | Siapa yang boleh menggunakannya, apakah berotasi, dan bagaimana dihapus? |

Consent bukan formalitas UI. Teks consent harus menyebut tindakan yang mungkin dilakukan client dan membedakan akses baca dari tulis. Jika scope berubah, perlakukan sebagai perubahan persetujuan, bukan sekadar migrasi konfigurasi.

Batas artikel ini adalah pemodelan integrasi delegated authorization. Ia tidak mengimplementasikan flow provider tertentu, tidak menetapkan kepatuhan hukum, dan tidak membuktikan bahwa library atau vendor aman. OpenAPI dapat membantu mendeskripsikan interface—endpoint, parameter, dan respons—namun spesifikasi interface tidak membuktikan perilaku implementasi atau keamanannya ([OpenAPI Specification 3.1.1](https://spec.openapis.org/oas/v3.1.1.html)).

## Cara kerjanya

Urutan yang dapat diaudit biasanya seperti ini:

1. **Discovery dan pendaftaran client.** Catat client ID, redirect URI yang diizinkan, tipe client, dan environment. Jangan menaruh client secret di aplikasi publik.
2. **Permintaan authorization.** Client mengirim pengguna ke authorization server dengan state anti-CSRF, scope minimum, dan parameter keamanan yang diwajibkan provider. Untuk client yang sesuai, konfirmasi penggunaan PKCE pada dokumentasi provider.
3. **Consent dan redirect.** Setelah pengguna menyetujui atau menolak, server mengembalikan hasil ke redirect URI yang telah didaftarkan. Aplikasi memeriksa state dan error sebelum menukar authorization code.
4. **Penukaran code.** Backend menukar code melalui kanal yang ditentukan provider. Simpan hanya token yang diperlukan dan kaitkan dengan identitas internal yang tepat.
5. **Pemanggilan resource server.** Access token dikirim sesuai metode provider. Respons 401/403 tidak boleh otomatis dianggap sebagai izin baru; bedakan token kedaluwarsa, scope kurang, dan akun yang dicabut.
6. **Refresh dan rotasi.** Saat access token kedaluwarsa, gunakan refresh token sesuai aturan provider. Jika rotasi diterapkan, token lama harus dianggap tidak dapat dipakai lagi dan kegagalan reuse perlu memutus sesi integrasi.
7. **Revocation dan disconnect.** Tombol “Putuskan akun” harus menonaktifkan kredensial lokal, memanggil endpoint revocation bila tersedia, menghentikan job terjadwal, dan menghapus salinan data yang tidak lagi memiliki dasar penggunaan.

Passkey atau WebAuthn dapat menjadi faktor autentikasi pada sisi akun dan consent, tetapi bukan pengganti access token untuk memanggil API. WebAuthn mendefinisikan autentikasi berbasis kredensial publik; penerapannya tetap bergantung pada autentikator dan kebijakan provider ([WebAuthn Level 3](https://www.w3.org/TR/webauthn-3/)).

## Faktor yang mengubah hasil

**Jenis client.** Backend rahasia dapat menjaga kredensial server-side; aplikasi mobile dan browser harus diasumsikan lebih mudah diinspeksi. Perlakukan secret yang tertanam sebagai tidak rahasia dan gunakan mekanisme yang memang didukung provider.

**Nilai data dan scope.** Scope tulis, data finansial, atau data lintas pengguna memerlukan persetujuan dan pemantauan lebih ketat daripada scope baca yang sempit. Pecah scope per kemampuan agar pencabutan tidak selalu mematikan seluruh integrasi.

**Penyimpanan.** Enkripsi token saat tersimpan, batasi akses service account, hindari token di log dan URL, dan siapkan penghapusan ketika pengguna disconnect. Atur retensi berdasarkan kebutuhan bisnis yang terdokumentasi, bukan karena penyimpanan dianggap murah.

**Error dan observabilitas.** Simpan correlation ID, tipe error, waktu, dan provider tanpa menulis token mentah. Alarm untuk lonjakan 401, 403, refresh gagal, atau reuse refresh token membantu membedakan masalah konfigurasi dari indikasi penyalahgunaan. OWASP menempatkan pengendalian penyalahgunaan API sebagai bagian penting dari pertahanan API; kontrol tersebut harus diuji pada endpoint nyata, bukan hanya dicantumkan dalam desain ([OWASP API Security Top 10 2023](https://owasp.org/API-Security/editions/2023/en/0x11-t10/)).

**Provider dan rantai pasok.** Dokumentasikan versi SDK, dependensi, subprosesor, kuota, serta kontak insiden. SBOM meningkatkan transparansi komponen tetapi tidak membuktikan keamanan komponen ([CISA SBOM resources](https://www.cisa.gov/sbom)). Penilaian rantai pasok perlu mempertimbangkan proses dan bukti vendor, seperti kerangka NIST SP 800-161 Rev. 1, bukan hanya brosur ([NIST SP 800-161 Rev. 1](https://csrc.nist.gov/pubs/sp/800/161/r1/final)). Skor repositori dari OpenSSF Scorecard dapat menjadi sinyal awal, bukan pengganti due diligence ([OpenSSF Scorecard](https://securityscorecards.dev/)). [NEEDS GATE-09: verifikasi syarat layanan, kuota, subprosesor, riwayat insiden, dan mekanisme dukungan provider yang benar-benar akan dipakai.]

## Contoh keputusan praktis

Bayangkan tim ingin membaca kalender pengguna dan membuat agenda. Mereka dapat memulai dengan scope baca kalender, lalu meminta scope tulis hanya saat fitur pembuatan agenda dipakai. Keputusan itu mengurangi dampak jika token bocor dan membuat consent lebih mudah dipahami. Jika provider hanya menawarkan scope gabungan, catat keterbatasan tersebut sebagai risiko yang harus disetujui pemilik sistem.

Gunakan tabel keputusan berikut saat review desain:

| Situasi | Keputusan awal | Bukti yang harus diminta |
| --- | --- | --- |
| Aplikasi publik memerlukan akses atas nama pengguna | Jangan tanam secret; validasi redirect dan state | Dokumentasi tipe client, PKCE, dan redirect URI |
| Access token kedaluwarsa | Coba refresh sekali sesuai kebijakan; jangan mengulang tanpa batas | Masa berlaku, error refresh, dan aturan rotasi |
| Pengguna menekan disconnect | Hentikan job, hapus token lokal, panggil revocation | Log tindakan tanpa token dan konfirmasi status provider |
| Banyak 401 setelah perubahan provider | Bekukan retry agresif dan buka insiden konfigurasi | Correlation ID, perubahan versi, dan status provider |

Teman Codev.id, keputusan “boleh lanjut” seharusnya keluar dari bukti tersebut, bukan dari asumsi bahwa semua OAuth bekerja seragam.

## Kesalahan umum dan cara memeriksanya

**Meminta semua scope sejak awal.** Periksa manifest scope terhadap fitur yang benar-benar dipakai. Hapus scope yatim dan minta persetujuan ulang untuk perluasan.

**Menyimpan token di local storage tanpa model ancaman.** Tanyakan siapa yang dapat membaca penyimpanan, bagaimana XSS atau perangkat hilang ditangani, dan apakah backend dapat menjadi tempat penyimpanan yang lebih terkendali.

**Menganggap logout lokal mencabut izin provider.** Uji disconnect end-to-end: apakah token lokal dihapus, refresh berhenti, revocation dipanggil, dan akses provider benar-benar gagal setelah pencabutan?

**Mencetak token saat debugging.** Cari token, authorization code, dan header Authorization di log, tracing, error report, serta backup. Ganti dengan redaksi dan identifier yang tidak dapat dipakai ulang.

**Retry tanpa batas pada 401/403.** Pastikan setiap kelas error memiliki batas percobaan, jalur re-authorization yang eksplisit, dan alarm. Jangan mengubah 403 menjadi permintaan scope baru tanpa consent.

**Mengandalkan skor vendor.** Kawan Codev.id, minta bukti kontrak, subprosesor, proses patch, dan respons insiden. Gunakan skor atau SBOM sebagai input review, bukan keputusan tunggal.

## Jalan pintas yang perlu diluruskan

Shortcut yang sering dipilih adalah “pakai satu token jangka panjang supaya sinkronisasi tidak pernah putus”. Ini menghilangkan pekerjaan refresh, tetapi memperbesar jendela penyalahgunaan dan membuat disconnect sulit dipercaya. Alternatif yang lebih dapat dikendalikan adalah access token berumur sesuai kebijakan provider, refresh yang diproteksi dan dipantau, scope minimum, serta jalur revocation yang diuji. Jika provider tidak menyediakan kemampuan penting, tuliskan gap itu dan minta keputusan pemilik risiko; jangan menyamarkannya dengan retry atau token yang tidak pernah kedaluwarsa.

## Penutup

OAuth Integration yang aman memetakan empat aktor, membatasi scope, memvalidasi redirect dan consent, memisahkan access token dari refresh token, lalu menyediakan refresh, rotasi, revocation, error handling, dan disconnect yang dapat dibuktikan. Detail provider tetap menjadi penentu implementasi.

Langkah berikutnya: mulai dari [halaman utama Codev.id](/) bila Anda perlu menempatkan integrasi ini dalam konteks pekerjaan lain, lalu minta dokumentasi resmi provider, isi lembar aktor–scope–token, uji skenario consent ditolak, token kedaluwarsa, refresh reuse, dan disconnect, lalu minta review teknis atas [NEEDS GATE-04] dan [NEEDS GATE-09]. Teman Codev.id, aturan operasionalnya sederhana: jangan menganggap akses berhenti sebelum token lokal, job, dan izin provider sama-sama diperiksa.
