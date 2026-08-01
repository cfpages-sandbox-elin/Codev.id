---
article_id: CDV-09-A03
title: "Login, Session, dan Access Control yang Bisa Diuji"
slug: "login-session-dan-access-control"
description: "Cover enrollment, authentication, recovery, session lifecycle, authorization, reauthentication, admin/service accounts, audit, and negative tests"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2025-10-17"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-09
primary_intent: "Specify identity and permission behavior"
reader_community: "Codev.id"
reader_address: "Teman Codev.id"
final_route: "/artikel/login-session-dan-access-control.html"
technical_review: required
sources:
  - "https://spec.openapis.org/oas/v3.1.1.html"
  - "https://www.rfc-editor.org/info/rfc9700/"
  - "https://www.w3.org/TR/webauthn-3/"
  - "https://owasp.org/API-Security/editions/2023/en/0x11-t10/"
  - "https://www.cisa.gov/sbom"
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

# Login, Session, dan Access Control yang Bisa Diuji

Halo, Teman Codev.id! Login yang “berhasil” belum membuktikan bahwa akun aman atau bahwa pengguna hanya bisa melakukan tindakan yang semestinya. Keputusan yang perlu dibuat sejak awal adalah: identitas siapa yang diterima, berapa lama bukti login berlaku, tindakan apa yang diizinkan, dan bagaimana semua penolakan dapat diuji.

Spesifikasi yang dapat diuji menjawabnya sebagai perilaku, bukan pilihan merek penyedia. Tulis alur enrollment, authentication, pemulihan, siklus hidup session, authorization (otorisasi), reauthentication, akun admin dan service, audit, serta negative test. OpenAPI dapat mendeskripsikan kontrak antarmuka—misalnya skema keamanan dan respons—tetapi tidak membuktikan bahwa implementasi memeriksa izin dengan benar ([OpenAPI Specification 3.1.1](https://spec.openapis.org/oas/v3.1.1.html)). Threat model klien, data, dan operasi masih dapat mengubah keputusan; bagian yang belum diputuskan harus ditandai `[NEEDS THREAT-MODEL REVIEW: GATE-03/GATE-05]`.

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

Gambar ini merupakan aset lokal untuk ilustrasi dan bukan dokumentasi proyek tertentu.

## Definisi dan batas objek

Authentication (autentikasi) menjawab “siapa Anda”; session adalah bukti sementara bahwa jawaban itu masih berlaku; authorization menjawab “boleh melakukan apa”. Access control adalah aturan dan pemeriksaan yang menghubungkan identitas, peran, kepemilikan objek, dan tindakan. Keempatnya harus dibedakan dalam spesifikasi dan pengujian. Pesan “login sukses” tidak boleh menjadi satu-satunya bukti.

Artikel ini membahas produk yang memiliki akun, peran, atau tindakan sensitif. Ia tidak memilih provider, tidak memberi contoh penyimpanan kredensial, dan tidak menggantikan rancangan integrasi OAuth khusus untuk produk Anda. Fokusnya adalah kontrak perilaku yang bisa direproduksi di aplikasi, API, log, dan pengujian. Untuk flow OAuth tertentu, RFC 9700 adalah pembaruan best current practice tahun 2025; flow tetap harus disesuaikan dengan konteks klien dan ancaman ([RFC 9700](https://www.rfc-editor.org/info/rfc9700/)).

Jika Anda perlu menyelaraskan keputusan ini dengan konteks produk yang lebih luas, mulai dari [beranda Codev.id](/) lalu kembali ke matriks izin yang spesifik untuk aplikasi Anda.

## Cara kerjanya

Mulai dari state dan transisi yang terlihat oleh sistem:

1. **Enrollment.** Tentukan data minimum, verifikasi kontak atau faktor, status akun (pending, aktif, ditangguhkan), dan bagaimana permintaan duplikat ditangani. Jangan menganggap akun aktif sebelum syarat verifikasi terpenuhi.
2. **Authentication.** Nyatakan faktor yang diterima, batas percobaan, respons untuk kredensial salah, dan kapan sistem meminta faktor tambahan. Untuk passkey, WebAuthn mendefinisikan interaksi antara Relying Party atau server aplikasi yang memverifikasi, authenticator pada perangkat, dan browser atau klien yang menghubungkan keduanya; implementasi tetap perlu diuji pada perangkat dan browser yang didukung ([WebAuthn Level 3](https://www.w3.org/TR/webauthn-3/)).
3. **Session lifecycle.** Dokumentasikan pembuatan, rotasi, perpanjangan, penghentian saat logout, penghentian semua perangkat, serta perilaku setelah perubahan faktor. Uji session kedaluwarsa, token yang dicabut, dan permintaan ulang setelah idle maupun setelah aktivitas sensitif. Hindari menyamakan “tab ditutup” dengan logout server.
4. **Authorization.** Setiap endpoint dan operasi harus memeriksa aksi serta objek yang diminta. Peran `admin` bukan alasan untuk melewati pemeriksaan kepemilikan. Respons penolakan harus konsisten tanpa membocorkan keberadaan objek yang tidak boleh diketahui.
5. **Reauthentication.** Minta bukti baru untuk tindakan berisiko—misalnya mengubah faktor, mengekspor data, atau mengelola pengguna—dengan jendela waktu yang ditulis jelas. Sukses login lama tidak otomatis memenuhi syarat ini.
6. **Akun khusus dan audit.** Admin memerlukan pemisahan tugas, jejak tindakan, dan prosedur pemulihan. Service account harus memiliki cakupan izin sempit, kredensial atau kunci yang dapat dirotasi, identitas yang dapat dinonaktifkan atau dicabut, dan pemantauan pemanggilan. Audit log sebaiknya merekam aktor, target, hasil, waktu, dan korelasi permintaan tanpa menulis rahasia.

Kontrak API sebaiknya memuat precondition, status sukses, dan status gagal untuk setiap transisi. OWASP menempatkan kegagalan authorization sebagai risiko API utama; karena itu pemeriksaan harus dilakukan di server pada setiap permintaan, bukan hanya menyembunyikan tombol di antarmuka ([OWASP API Security Top 10 2023](https://owasp.org/API-Security/editions/2023/en/0x11-t10/)).

## Faktor yang mengubah hasil

Pertama, **model ancaman**: apakah penyerang dapat mengambil alih perangkat, menebak identifier, mengulang permintaan, atau menyalahgunakan akun internal? Tanpa jawaban ini, durasi session dan kebutuhan faktor tambahan hanya tebakan. Kedua, **sensitivitas tindakan**: membaca profil berbeda risikonya dengan mengganti rekening atau mengekspor data.

Ketiga, **batas objek dan multi-tenant**. Uji pengguna yang sah terhadap objek tenant lain, ID yang diubah, dan kombinasi peran yang tidak lazim. Keempat, **pemulihan**: tautan atau kode pemulihan harus memiliki masa berlaku, sekali pakai, serta membatalkan bukti lama sesuai keputusan produk. Kelima, **operasi dan dependensi**: provider, library, atau vendor dapat mengubah API, kuota, dan kebijakan. SBOM membantu transparansi komponen, tetapi tidak membuktikan komponen aman ([CISA SBOM resources](https://www.cisa.gov/sbom)); evaluasi vendor tetap membutuhkan bukti kontrak dan operasi aktual.

Terakhir, bedakan bukti rancangan dari bukti runtime. Dokumen OpenAPI, diagram, atau skor repositori hanya memberi sinyal. Rekaman pengujian, konfigurasi deployment, dan log terkontrol diperlukan untuk menyimpulkan perilaku. Jika threat model, cakupan tenant, atau bukti runtime belum tersedia, tahan klaim keamanan dan bawa marker `[NEEDS IMPLEMENTATION EVIDENCE: GATE-05]` ke review teknis.

## Contoh keputusan praktis

Gunakan tabel keputusan sederhana berikut saat menulis acceptance test:

| Situasi | Harus diizinkan | Harus ditolak atau ditunda | Bukti yang dicari |
|---|---|---|---|
| Pengguna aktif membaca objek miliknya | Respons data sesuai scope | ID tenant lain | Test dengan ID milik tenant lain dan log keputusan |
| Session idle melewati batas | Tidak ada akses tanpa transisi baru | Respons meminta login ulang | Timestamp, status respons, dan pencabutan session |
| Mengubah faktor autentikasi | Setelah reauthentication | Session lama saja | Event audit dan uji faktor lama |
| Admin mengubah peran | Hanya scope admin yang ditetapkan | Perubahan lintas tenant tanpa izin | Aktor, target, hasil, dan korelasi request |
| Service account memanggil endpoint | Endpoint serta scope yang terdaftar | Endpoint interaktif atau scope berlebih | Identitas mesin, scope, dan respons penolakan |

Contoh ini sengaja bersyarat; nilai durasi, daftar peran, dan faktor harus diisi dari threat model dan keputusan produk. Sobat Codev.id, tulis satu test positif berpasangan dengan satu test negatif untuk setiap aturan. Pasangan itu memaksa tim menunjukkan bukan hanya “jalan”, tetapi juga “berhenti pada batas”.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah menguji layar login saja. Tambahkan test langsung ke endpoint dengan session valid, session kedaluwarsa, token dicabut, dan identifier objek yang diganti. Kedua, menganggap role di token selalu benar. Verifikasi sumber otoritas, rotasi, dan perilaku ketika role berubah di tengah session.

Ketiga, memakai pesan error yang terlalu informatif sehingga enumerasi akun mudah dilakukan. Uji perbedaan waktu dan isi respons untuk akun ada, tidak ada, ditangguhkan, dan recovery yang kedaluwarsa. Keempat, menjadikan akun admin atau service sebagai pengecualian permanen. Buat jalur pencabutan, rotasi, pemantauan, dan review akses berkala.

Kelima, menganggap audit log sebagai dekorasi. Tanyakan apakah setiap perubahan izin memiliki aktor, target, hasil, waktu, dan korelasi yang dapat ditelusuri; uji juga kegagalan pencatatan. Jangan memasukkan kredensial, token, atau data sensitif mentah ke log.

Shortcut yang sering dipilih adalah “cukup pakai middleware role sekali di router”. Itu dapat gagal ketika endpoint baru lupa dipasang, objek berada di tenant lain, atau service account memakai jalur berbeda. Alternatif yang lebih dapat diuji adalah daftar endpoint dan operasi sebagai matriks izin, dengan test otomatis untuk kombinasi aktor–objek–aksi serta review manual pada tindakan berisiko. Kawan Codev.id, bila matriks belum lengkap, statusnya belum siap disimpulkan aman—bukan sekadar belum rapi.

## Penutup: aturan operasi yang bisa diuji

Login, session, dan access control dapat diuji bila setiap aturan ditulis sebagai transisi dan keputusan: siapa yang terautentikasi, bukti mana yang masih berlaku, aksi dan objek apa yang diizinkan, kapan reauthentication wajib, serta bukti penolakan dan audit apa yang dihasilkan. Dokumentasikan matriks izin, state session, recovery, dan akun khusus; lalu jalankan pasangan test positif–negatif pada endpoint nyata.

Langkah berikutnya adalah minta review teknis atas threat model dan bukti implementasi, khususnya `[NEEDS THREAT-MODEL REVIEW: GATE-03/GATE-05]` dan `[NEEDS IMPLEMENTATION EVIDENCE: GATE-05]`. Sampai bukti itu tersedia, perlakukan klaim sebagai rancangan yang belum tervalidasi. Aturan operasinya sederhana: setiap izin harus punya pemeriksaan server dan test penolakan yang dapat diulang.
