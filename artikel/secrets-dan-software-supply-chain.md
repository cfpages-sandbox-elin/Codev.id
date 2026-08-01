---
article_id: CDV-09-A04
writing_contract_version: "native-id-v2"
title: "Secrets dan Software Supply Chain"
slug: "secrets-dan-software-supply-chain"
description: "Menginventarisasi rahasia dan komponen, membatasi akses, merotasi kredensial, memindai dengan aman, meninjau dependensi, memverifikasi build, melacak kerentanan, dan merespons"
status: draft
publication_date: "2025-10-21"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-09
primary_intent: "Protect credentials and reduce dependency/build risk"
reader_community: "Codev.id"
reader_address: "Kawan Codev.id"
final_route: "/artikel/secrets-dan-software-supply-chain.html"
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

# Secrets dan Software Supply Chain

Halo, Kawan Codev.id!

Rahasia yang bocor dan komponen yang disusupi biasanya tidak muncul sebagai satu bug besar. Masalahnya sering dimulai dari token yang tertinggal di repositori, dependensi yang diambil tanpa jejak, atau pipeline yang membangun artefak tanpa bisa menjawab asal-usulnya. Jadi, jawaban praktis untuk “Secrets dan Software Supply Chain” adalah membuat inventaris, membatasi akses, memindai secara aman, mengunci serta meninjau dependensi, memverifikasi build, dan menyiapkan respons yang bisa dijalankan.

Urutannya penting. Rotasi token tidak memperbaiki dependensi berbahaya; SBOM tidak membuktikan komponen aman; dan skor repositori hanya sinyal awal. Detail penyedia cloud, jenis ancaman, alur OAuth, serta kewajiban organisasi dapat mengubah keputusan. Karena konteks proyek dan threat model belum tersedia di paket ini, pilihan alur autentikasi dan kontrol API harus melewati review teknis sebelum diaktifkan.

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

*Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.*

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

“Secrets” adalah kredensial atau material autentikasi yang memungkinkan akses, misalnya token CI, kunci API, kata sandi layanan, atau kunci penandatanganan. “Software supply chain” adalah jalur komponen dan alat dari sumber, pengelola paket, repositori, proses build, sampai artefak yang dirilis. Fokus artikel ini adalah mengurangi peluang akses tidak sah dan ketidakjelasan asal komponen.

Yang dibahas bukan cara membocorkan nilai rahasia, hasil pemindaian tertentu, atau janji bahwa pohon dependensi bebas kerentanan. Artikel ini juga bukan jadwal upgrade dependensi. Bila suatu temuan membutuhkan perubahan versi, pemilik produk tetap perlu menetapkan ritme perbaikan dan pengujian secara terpisah. Batas ini membantu tim membedakan kontrol yang bisa dipasang sekarang dari keputusan yang memerlukan persetujuan pemilik sistem.

## Cara kerjanya

Mulailah dari inventaris yang dapat ditelusuri. Catat nama secret, pemilik, sistem pemakai, lingkungan, masa berlaku, dan cara pencabutannya—bukan nilainya. Untuk komponen, catat nama paket, versi, sumber, lisensi bila relevan, serta artefak yang memasukkannya. SBOM (software bill of materials) membantu transparansi komponen, tetapi [CISA menjelaskan bahwa SBOM bukan bukti otomatis bahwa perangkat lunak aman](https://www.cisa.gov/sbom).

Berikut alur minimum yang dapat diulang:

1. **Temukan tanpa mengekspos.** Pindai perubahan dan riwayat repositori dengan aturan yang menyamarkan hasil. Batasi siapa yang dapat melihat temuan, simpan sidik jari atau lokasi baris seperlunya, lalu hapus salinan rahasia yang tidak diperlukan.
2. **Klasifikasikan dan batasi.** Bedakan akses manusia, layanan, dan deploy. Berikan izin minimum, pisahkan lingkungan, dan hindari token bersama. Secret diambil saat runtime dari penyimpanan yang sesuai, bukan ditanam di kode atau log.
3. **Cabut dan rotasi.** Jika token pernah terlihat oleh pihak yang tidak semestinya, anggap ia berisiko: cabut, terbitkan pengganti, perbarui konsumen, dan catat waktu serta pemilik perubahan. Rotasi tanpa pencabutan hanya memindahkan masalah.
4. **Kunci rantai dependensi.** Gunakan lockfile, rentang versi yang sadar risiko, sumber paket yang disetujui, dan review perubahan. Verifikasi bahwa artefak build berasal dari commit dan konfigurasi yang diharapkan.
5. **Pantau dan respons.** Cocokkan SBOM dengan pemberitahuan kerentanan, tentukan prioritas berdasarkan pemakaian nyata, lalu siapkan jalur karantina, rollback, dan komunikasi internal.

Kontrak API harus diperlakukan sebagai deskripsi antarmuka, bukan sertifikat keamanan. [OpenAPI Specification 3.1.1](https://spec.openapis.org/oas/v3.1.1.html) membantu mendokumentasikan endpoint dan skema; implementasi tetap harus menguji autentikasi, otorisasi, validasi input, serta batas laju. Untuk alur OAuth, [RFC 9700](https://www.rfc-editor.org/info/rfc9700/) adalah pembaruan praktik terbaik keamanan OAuth 2.0, namun pemilihan grant, client, dan mekanisme token tetap bergantung pada threat model. **[NEEDS THREAT-MODEL REVIEW: validasi alur OAuth/passkey, batas API, dan dampak akses untuk konteks proyek sebelum implementasi.]**

Passkey berbasis WebAuthn dapat menjadi opsi autentikasi yang kuat bila perangkat, pemulihan akun, dan kebijakan pendaftaran dipahami. [WebAuthn Level 3](https://www.w3.org/TR/webauthn-3/) mendefinisikan API dan konsep kredensial publik; dokumen itu tidak menggantikan keputusan UX, pemulihan, dan kontrol server. Untuk permukaan API, gunakan kategori risiko sebagai daftar uji: [OWASP API Security Top 10 2023](https://owasp.org/API-Security/editions/2023/en/0x11-t10/) dapat membantu mengarahkan pemeriksaan otorisasi dan penyalahgunaan.

## Faktor yang mengubah hasil

Beberapa kondisi membuat kontrol yang sama menghasilkan risiko berbeda:

- **Lingkungan dan umur secret.** Token development yang tertulis di contoh belum tentu memiliki dampak produksi, tetapi kebiasaan berbagi format dapat membuat perpindahan lingkungan mudah keliru. Tetapkan pemisahan nama, izin, dan penyimpanan sejak awal.
- **Jalur build.** Runner CI, cache, plugin, dan akun penerbit adalah bagian dari rantai. Jika satu tahap dapat mengubah artefak tanpa jejak, verifikasi di tahap akhir tidak cukup.
- **Sumber komponen.** Paket publik, mirror internal, vendor, dan subprosesor memiliki bukti berbeda. [NIST SP 800-161 Rev. 1](https://csrc.nist.gov/pubs/sp/800/161/r1/final) membahas pengelolaan risiko rantai pasok siber; gunakan sebagai kerangka pertanyaan, bukan bukti bahwa vendor tertentu telah lulus.
- **Kedalaman review.** Tinjauan hash, tanda tangan, provenance, dan perubahan maintainer membutuhkan kemampuan berbeda. [OpenSSF Scorecard](https://securityscorecards.dev/) memberi sinyal tentang praktik repositori, bukan pengganti due diligence, pengujian, atau persetujuan kontrak.
- **Kemampuan merespons.** Inventaris yang tidak memiliki pemilik, tanggal kedaluwarsa, dan jalur pencabutan hanya menjadi daftar. Uji siapa yang berwenang mematikan kredensial serta bagaimana layanan dipulihkan.

Sobat Codev.id, tanyakan juga apa yang terjadi ketika alat pemindai menghasilkan temuan palsu. Jika tim tidak punya prosedur redaksi, eskalasi, dan pengecualian yang tercatat, pemindaian agresif bisa memperbanyak salinan rahasia atau membuat orang mengabaikan semua peringatan.

## Contoh keputusan praktis

Gunakan tabel ini untuk menentukan langkah pertama, bukan untuk menyimpulkan tingkat keamanan akhir.

| Situasi yang teramati | Keputusan segera | Bukti yang perlu disimpan |
|---|---|---|
| Token muncul di commit yang dapat diakses luas | Cabut token, terbitkan pengganti, dan telusuri pemakaian | Pemilik, waktu pencabutan, layanan terdampak |
| Paket transitif berubah tanpa lockfile konsisten | Hentikan rilis terkait dan buat ulang resolusi dependensi | Lockfile, commit pemicu, hasil review |
| SBOM tersedia tetapi sumber build tidak jelas | Jangan menyebut artefak terverifikasi; perbaiki provenance | Input build, identitas runner, hash artefak |
| Skor repositori vendor tinggi | Lanjutkan pertanyaan kontrak, akses, dan respons insiden | Jawaban vendor dan keputusan risiko |
| API baru memakai OAuth atau passkey | Tahan peluncuran sampai threat model dan pengujian otorisasi disetujui | Diagram alur, skenario penyalahgunaan, hasil uji |

Contoh bersyarat ini sengaja tidak menyatakan ada insiden atau vendor tertentu. Jika konteks Anda berbeda, ubah pemilik, tingkat akses, dan bukti yang diwajibkan sebelum mengambil tindakan.

## Kesalahan umum dan cara memeriksanya

**“Kami hanya perlu menambah pemindai secret.”** Periksa apakah pemindaian mencakup riwayat, apakah output disamarkan, dan siapa yang dapat mengunduh laporan. Temuan yang tidak berujung pada pencabutan dan rotasi belum menutup akses.

**“Lockfile berarti dependensi aman.”** Periksa sumber paket, integritas artefak, perubahan transitive, dan apakah lockfile benar-benar dipakai oleh CI. Pinning memberi keterulangan; ia tidak menghapus kerentanan yang sudah ada.

**“SBOM sudah cukup untuk audit.”** Periksa proses pembuatan, cakupan komponen, versi, dan kecocokan dengan artefak yang dirilis. SBOM tanpa provenance dapat salah merepresentasikan isi build.

**“Skor publik menggantikan review vendor.”** Periksa izin akses, lokasi data, jalur pemberitahuan kerentanan, komitmen pemulihan, dan subprosesor. Sinyal eksternal membantu menyusun pertanyaan, bukan menjawab semuanya.

**“Menerapkan alur autentikasi dari contoh umum pasti aman.”** Periksa client, redirect, penyimpanan token, pemulihan akun, otorisasi per objek, dan batas laju terhadap ancaman yang nyata. Hentikan implementasi bila keputusan itu belum ditinjau secara teknis.

## Jalan pintas yang perlu ditolak

Jalan pintas yang sering menggoda adalah menyimpan satu token bersama di CI agar semua layanan “segera jalan”. Mekanismenya justru memperlebar blast radius: satu log, runner, atau akun yang salah konfigurasi dapat membuka banyak sistem, sementara rotasi menjadi operasi besar. Alternatif yang lebih dapat ditelusuri adalah kredensial per layanan dengan izin minimum, masa berlaku yang jelas, dan prosedur pencabutan yang pernah diuji.

## Kesimpulan

Secrets dan software supply chain dikelola sebagai rangkaian kontrol: inventaris tanpa nilai rahasia, akses minimum, pencabutan dan rotasi, dependensi yang terkunci dan ditinjau, build yang dapat diverifikasi, lalu pemantauan serta respons. Mulailah dengan daftar secret dan komponen yang memiliki pemilik; pilih satu jalur build untuk mencocokkan input, provenance, dan artefaknya; kemudian minta review teknis untuk alur autentikasi dan risiko API yang spesifik.

Kawan Codev.id, simpan bukti keputusan dan tanggal pemeriksaan agar orang berikutnya dapat mengulanginya. Untuk menempatkan langkah ini dalam konteks layanan Codev.id, Anda dapat mulai dari [beranda Codev.id](/). Aturan operasionalnya sederhana: jangan menyebut secret terlindungi, dependensi aman, atau build tepercaya sebelum akses, asal-usul, dan batas ancamannya benar-benar diperiksa.
