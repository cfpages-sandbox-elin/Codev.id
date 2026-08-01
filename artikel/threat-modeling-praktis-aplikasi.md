---
article_id: CDV-09-A01
title: "Threat Modeling Praktis untuk Aplikasi"
slug: "threat-modeling-praktis-aplikasi"
description: "Panduan memetakan aset, aktor, titik masuk, aliran data, ancaman, dampak, kontrol, pemilik, dan verifikasi sebelum rilis"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2025-10-10"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-09
primary_intent: "Identify assets, trust boundaries, abuse, and controls before release"
reader_community: "Codev.id"
reader_address: "Kawan Codev.id"
final_route: "/artikel/threat-modeling-praktis-aplikasi.html"
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

Halo, Kawan Codev.id!

Threat modeling praktis berarti memetakan apa yang harus dilindungi, siapa yang dapat memengaruhinya, dari mana permintaan masuk, bagaimana data bergerak, lalu memutuskan kontrol dan pemilik verifikasinya sebelum rilis. Ini bukan sekadar membuat daftar serangan atau menjalankan pemindai. Hasil yang berguna adalah keputusan yang bisa ditindaklanjuti: ancaman mana yang diterima, dikurangi, dipindahkan, atau menghentikan rilis sampai ada bukti.

Mulailah dari perubahan sistem yang nyata—misalnya endpoint baru, integrasi pembayaran, atau perubahan cara login. Gambar alur sederhana, tandai batas kepercayaan (trust boundary), dan tulis aset serta dampaknya bila disalahgunakan. Kondisi proyek, data yang diproses, dan bukti kontrol dapat mengubah prioritas; tanpa informasi itu, penilaian hanya hipotesis yang perlu ditinjau tim teknis dan pemilik risiko.

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

*Gambar ini merupakan aset lokal untuk ilustrasi dan bukan dokumentasi proyek tertentu.*

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

# Threat Modeling Praktis untuk Aplikasi

## Definisi dan batas objek

Threat model adalah catatan keputusan tentang aset, aktor, entry point, aliran data, batas kepercayaan, ancaman, dampak, kontrol yang sudah ada, perlakuan (treatment), pemilik, dan cara verifikasi. “Aktor” mencakup pengguna sah, administrator, layanan pihak ketiga, proses otomatis, serta pihak yang tidak berwenang. “Aset” bukan hanya database; token, konfigurasi, reputasi, ketersediaan, dan kemampuan mengubah transaksi juga layak dicatat.

Batasnya penting. Model ini membantu memilih kontrol sebelum rilis, tetapi tidak mensertifikasi keamanan, tidak memuat rahasia atau langkah eksploitasi yang siap disalahgunakan, dan tidak menggantikan pengujian maupun persetujuan profesional untuk risiko tinggi. OpenAPI mendeskripsikan kontrak antarmuka; keberadaan dokumen spesifikasi tidak membuktikan implementasi mematuhi otorisasi atau validasi keamanan ([OpenAPI Specification 3.1.1](https://spec.openapis.org/oas/v3.1.1.html)).

Tulis asumsi secara eksplisit: jenis data, peran pengguna, lingkungan deployment, dependensi, dan tujuan bisnis. Jika salah satu asumsi belum diketahui, tandai sebagai pertanyaan terbuka, bukan fakta. Sobat Codev.id, model yang jujur dengan celah informasi lebih aman daripada diagram rapi yang menyembunyikan ketidakpastian.

## Cara kerjanya

1. **Tetapkan perubahan dan aset.** Beri nama fitur atau arsitektur yang dianalisis, lalu inventarisasi data, kredensial, fungsi bisnis, dan sasaran ketersediaan. Catat pemilik masing-masing aset.
2. **Petakan aktor dan entry point.** Daftar UI, API, webhook, pekerjaan terjadwal, kanal dukungan, serta integrasi. Untuk tiap entry point, tanyakan identitas apa yang dibawa dan kontrol mana yang memeriksa konteksnya.
3. **Gambar aliran data dan batas kepercayaan.** Tandai saat data berpindah dari browser ke API, dari API ke queue, database, atau vendor. Setiap perpindahan lintas batas memerlukan asumsi autentikasi, otorisasi, validasi, dan pencatatan yang dapat diperiksa.
4. **Uji skenario penyalahgunaan.** Untuk setiap aset, tanyakan: apa yang terjadi jika aktor membaca, mengubah, mengulang, menunda, atau membanjiri operasi? Hindari detail payload atau urutan serangan; cukup rumuskan kondisi dan dampaknya untuk memilih kontrol.
5. **Nilai dampak dan prioritas.** Pisahkan dampak pada kerahasiaan, integritas, ketersediaan, pengguna, dan operasi. Prioritaskan skenario yang menggabungkan aset bernilai tinggi, akses luas, dan pemulihan sulit.
6. **Catat kontrol, treatment, owner, dan verifikasi.** Kontrol bisa berupa pembatasan akses, validasi skema, rate limit, logging, pemisahan tugas, atau prosedur pemulihan. Setiap tindakan harus memiliki pemilik, tenggat yang disepakati tim, serta bukti lulus—misalnya hasil pengujian, konfigurasi yang ditinjau, atau rekaman keputusan.

Untuk API, gunakan kontrak sebagai bahan pemeriksaan, bukan sebagai bukti akhir. OWASP menempatkan kegagalan otorisasi tingkat objek, autentikasi, dan konsumsi sumber daya sebagai risiko API yang perlu dinilai dalam konteks endpoint dan aktor ([OWASP API Security Top 10 2023](https://owasp.org/API-Security/editions/2023/en/0x11-t10/)). Untuk alur OAuth, perlakukan rekomendasi keamanan sebagai bahan desain dan cocokkan dengan klien, redirect, token, dan ancaman yang benar-benar ada; RFC 9700 adalah pembaruan best current practice pada 2025, bukan jaminan bahwa implementasi tertentu aman ([OAuth 2.0 Security BCP—RFC 9700](https://www.rfc-editor.org/info/rfc9700/)).

## Faktor yang mengubah hasil

Beberapa kondisi mengubah prioritas tanpa mengubah metode dasarnya:

| Faktor | Pertanyaan yang mengubah keputusan |
| --- | --- |
| Data dan dampak | Apakah kegagalan mengekspos data pribadi, mengubah saldo, atau hanya mengganggu fitur sementara? |
| Aktor dan hak akses | Apakah pengguna anonim, pengguna biasa, admin, service account, atau vendor dapat mencapai entry point? |
| Batas kepercayaan | Di mana validasi ulang dan pemeriksaan otorisasi wajib dilakukan karena komponen tidak saling percaya? |
| Perubahan deployment | Apakah ada jaringan, queue, cache, region, atau konfigurasi baru yang memperluas jalur data? |
| Dependensi | Komponen apa yang masuk melalui build, runtime, SDK, atau vendor; siapa yang memantau perubahan dan kerentanannya? |
| Bukti kontrol | Bisakah tim menunjukkan konfigurasi, log, hasil uji, dan pemilik tindak lanjut—bukan hanya menyatakan “sudah aman”? |

Inventaris komponen melalui SBOM dapat meningkatkan transparansi, tetapi tidak menetapkan bahwa komponen itu aman ([CISA SBOM resources](https://www.cisa.gov/sbom)). Untuk pemasok dan integrasi, NIST SP 800-161 Rev. 1 membantu membingkai risiko rantai pasok; gunakan sebagai bahan penilaian, lalu cocokkan dengan kontrak, layanan, dan proses aktual ([NIST SP 800-161 Rev. 1](https://csrc.nist.gov/pubs/sp/800/161/r1/final)). Skor repositori dari OpenSSF Scorecard adalah sinyal untuk ditindaklanjuti, bukan due diligence atau bukti bebas kerentanan ([OpenSSF Scorecard](https://securityscorecards.dev/)).

Autentikasi yang lebih kuat juga tidak otomatis menyelesaikan semua risiko. WebAuthn mendefinisikan API dan alur kredensial berbasis kunci publik; tim tetap harus memodelkan pemulihan akun, perangkat hilang, sesi aktif, dan otorisasi operasi setelah autentikasi ([WebAuthn Level 3](https://www.w3.org/TR/webauthn-3/)).

## Contoh keputusan praktis

Bayangkan fitur “buat invoice” yang menerima permintaan dari browser, menyimpan data di API, lalu mengirim webhook ke penyedia pembayaran. Ini contoh bersyarat, bukan dokumentasi proyek tertentu. Peta awalnya:

| Elemen | Pertanyaan dan keputusan awal |
| --- | --- |
| Aset | Invoice, identitas pelanggan, status pembayaran, token integrasi, dan kemampuan mengubah jumlah. |
| Aktor | Pelanggan, staf keuangan, admin, worker webhook, dan penyedia pembayaran. |
| Entry point | Endpoint create/update, webhook masuk, job retry, serta panel staf. |
| Batas | Browser–API, API–database, API–vendor, dan webhook–worker. |
| Penyalahgunaan | Pengguna mengubah invoice milik orang lain, webhook diputar ulang, atau retry membuat transaksi ganda. |
| Treatment | Otorisasi per objek, idempotency, validasi skema, verifikasi asal webhook, rate limit, dan audit log—dipilih setelah konteks dikonfirmasi. |
| Owner dan bukti | Pemilik API menunjukkan uji otorisasi; pemilik integrasi menunjukkan uji replay; reviewer risiko menyetujui residual risk. |

Keputusan rilis bukan “ada diagram, maka aman”. Jika verifikasi webhook belum tersedia atau pemilik tidak jelas, tindakan yang wajar adalah menahan fitur terkait dan meminta bukti. Kawan Codev.id, tulis juga keputusan menerima risiko beserta alasan dan masa tinjau; jangan mengubah asumsi menjadi lampu hijau permanen.

## Kesalahan umum dan cara memeriksanya

**Mengandalkan pemindai saja.** Pemindai dapat menemukan pola tertentu, tetapi tidak tahu apakah aktor boleh mengubah objek tertentu atau apakah alur bisnis dapat diulang. Periksa matriks aktor–aksi–objek dan uji skenario prioritas.

**Menganggap spesifikasi sebagai implementasi.** Cocokkan endpoint yang terdokumentasi dengan routing, middleware otorisasi, validasi input, dan telemetri yang benar-benar aktif. OpenAPI menjadi kontrak pembanding, bukan sertifikat.

**Memberi nilai risiko tanpa owner.** Setiap item harus memiliki orang atau tim yang berwenang menerima atau mengurangi risiko. Jika owner dan bukti verifikasi kosong, statusnya belum selesai.

**Menilai vendor dari satu skor.** Gabungkan SBOM, provenance, persyaratan layanan, akses data, jalur notifikasi kerentanan, dan rencana penggantian. Skor eksternal membantu menyusun pertanyaan, bukan menggantikan pemeriksaan.

**Menyimpan detail serangan yang sensitif.** Simpan rincian teknis di kanal dan akses yang sesuai. Artikel atau tiket lintas tim cukup memuat kondisi, dampak, kontrol, dan cara uji yang aman.

Checklist sebelum keputusan:

- Apakah setiap aset punya pemilik dan dampak yang tertulis?
- Apakah semua entry point dan lintasan lintas batas kepercayaan terpetakan?
- Apakah skenario prioritas memiliki kontrol, owner, tenggat, dan bukti verifikasi?
- Apakah asumsi, residual risk, dan kondisi yang memicu peninjauan ulang terlihat?
- Apakah risiko tinggi sudah mendapat review yang memenuhi kualifikasi proyek?

## Jalan pintas yang perlu dihindari

Shortcut yang sering muncul adalah “kita pakai passkey atau gateway API, jadi threat modeling tidak perlu.” Kontrol tersebut bisa mengurangi kelas risiko tertentu, tetapi tidak menjawab salah-otorisasi objek, penyalahgunaan alur bisnis, replay webhook, kebocoran token, atau kegagalan pemulihan. Alternatif yang lebih andal adalah memetakan kontrol itu pada entry point dan skenario yang dilindunginya, lalu menguji batas yang masih terbuka. Jika konteks klien, data, atau dampak belum jelas, tinggalkan item sebagai pertanyaan dan minta review teknis sebelum menyimpulkan.

## Kesimpulan

Threat modeling praktis untuk aplikasi adalah lembar keputusan yang menghubungkan aset, aktor, entry point, aliran data, batas kepercayaan, ancaman, dampak, kontrol, treatment, owner, dan verifikasi. Langkah berikutnya: pilih satu perubahan yang akan dirilis, buat peta aliran datanya, isi matriks skenario di atas, lalu minta pemilik kontrol menunjukkan bukti sebelum persetujuan rilis. Untuk menyelaraskan istilah lintas tim, Anda dapat merujuk [beranda Codev.id](/) sebagai titik awal konteks. Model ini membantu mengarahkan pekerjaan; ia tidak mensertifikasi keamanan dan tidak menggantikan review profesional untuk risiko tinggi.
