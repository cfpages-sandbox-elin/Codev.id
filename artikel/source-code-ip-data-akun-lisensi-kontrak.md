---
article_id: CDV-17-A04
title: "Source Code, IP, Data, Akun, dan Lisensi dalam Kontrak"
slug: "source-code-ip-data-akun-lisensi-kontrak"
description: "Inventory background/new IP, source/repository, open-source/third-party licenses, data roles/exports, domains/cloud/app accounts, credentials, documentation, and exit rights"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2026-05-05"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-17
primary_intent: "Clarify ownership and use rights before work starts"
reader_community: "Codev.id"
reader_address: "Sobat Codev.id"
final_route: "/artikel/source-code-ip-data-akun-lisensi-kontrak.html"
technical_review: required
sources:
  - "https://www.cisa.gov/sbom"
  - "https://csrc.nist.gov/pubs/sp/800/161/r1/final"
  - "https://securityscorecards.dev/"
  - "https://www.cisa.gov/securebydesign"
  - "https://www.gov.uk/guidance/the-technology-code-of-practice"
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

# Source Code, IP, Data, Akun, dan Lisensi dalam Kontrak

Halo, Sobat Codev.id! Jika kontrak hanya menyebut “aplikasi selesai” tanpa menjelaskan source code, hak kekayaan intelektual (IP), data, akun, dan lisensi, Anda belum tentu bisa mengoperasikan atau memindahkan sistem itu secara mandiri. Harga dan tanggal serah terima tidak otomatis memberi Anda repository, kredensial, hak memakai komponen pihak ketiga, atau jalan keluar dari vendor.

Jawaban praktisnya: buat inventaris aset dan hak sebelum pekerjaan dimulai, lalu jadikan inventaris itu bagian dari kontrak dan berita acara serah terima. Pisahkan apa yang sudah dimiliki masing-masing pihak (background IP) dari hasil baru yang dibuat khusus (new IP), jelaskan siapa menguasai data dan akun, dan catat kewajiban lisensi. Detail kepemilikan tetap bergantung pada hukum dan redaksi kontrak yang sebenarnya; [NEEDS CONTRACT REVIEW: minta peninjauan ahli hukum Indonesia yang berkualifikasi untuk klausul ownership, lisensi, peran data, transfer akun, dan exit rights].

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

*Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.*

## Jawaban singkat dan salah paham utama

Kontrak yang aman menjawab lima pertanyaan berbeda, bukan satu kalimat “milik klien”: (1) artefak apa yang diserahkan, (2) hak apa yang diberikan atau dialihkan, (3) akun dan data berada di bawah kendali siapa, (4) lisensi apa yang membatasi penggunaan, dan (5) apa yang terjadi ketika hubungan berakhir. Source code hanyalah salah satu artefak; build script, konfigurasi, dokumentasi, skema basis data, kunci domain, dan pipeline deployment bisa sama pentingnya untuk keberlangsungan operasi.

Salah paham yang sering terjadi adalah menganggap repository kosong setelah demo sebagai bukti kepemilikan, atau menganggap library open-source bebas dipakai tanpa syarat. Inventaris komponen (termasuk dependensi dan asalnya) membantu transparansi, tetapi tidak membuktikan sistem aman dengan sendirinya. CISA menjelaskan fungsi SBOM sebagai daftar komponen perangkat lunak, sementara NIST menempatkan inventaris dan penilaian pemasok sebagai bagian dari manajemen risiko rantai pasok—bukan pengganti pemeriksaan kontrak atau keamanan. [SBOM CISA](https://www.cisa.gov/sbom) dan [NIST SP 800-161 Rev.1](https://csrc.nist.gov/pubs/sp/800/161/r1/final)

## Definisi dan batas objek

Mulailah dengan kamus aset yang bisa diperiksa:

| Kelompok | Yang perlu dicatat | Pertanyaan hak dan kendali |
| --- | --- | --- |
| Background IP | framework, template, modul, metode, dan materi yang sudah ada | Bolehkah vendor memakainya kembali? Apa lisensi yang Anda dapat? |
| New IP | kode, desain, konfigurasi, dokumentasi, dan artefak yang dibuat untuk proyek | Apakah hak dialihkan, dilisensikan, atau dibatasi setelah pembayaran? |
| Repository dan build | lokasi Git, branch, pipeline, image, secret store, dan instruksi build | Siapa admin, siapa memiliki salinan, dan kapan akses diberikan? |
| Data | data produksi, backup, log, ekspor, dan metadata | Siapa menentukan tujuan penggunaan, format ekspor, dan penghapusan? |
| Akun | domain, cloud, email, app store, SaaS, DNS, dan monitoring | Atas nama siapa akun dibuat dan bagaimana pemulihan akses dilakukan? |
| Lisensi | open-source, font, API, SDK, gambar, dan layanan berbayar | Apa kewajiban atribusi, redistribusi, seat, masa berlaku, atau biaya? |

Artikel ini tidak menetapkan siapa pemilik menurut hukum untuk kontrak tertentu. Ia juga tidak menentukan peran pengendali atau pemroses data, kecuali kontrak dan penasihat yang menelaah fakta proyek menetapkannya. Batas ini penting agar checklist operasional tidak disalahartikan sebagai pendapat hukum.

## Cara kerjanya

Sebelum tanda tangan, minta vendor mengisi register aset dengan kolom nama, versi, sumber, pemilik akun, hak pakai, dan bukti serah terima. Tandai setiap item sebagai background, new IP, atau pihak ketiga. Untuk source code, tetapkan repository utama, riwayat commit, branch yang dianggap rilis, serta artefak yang diperlukan untuk membangun ulang. Jangan hanya menerima hasil kompilasi jika tujuan Anda adalah operasional mandiri.

Pada tahap pengerjaan, gunakan akun organisasi pembeli untuk domain, cloud, app store, dan layanan penting bila memungkinkan. Jika akun vendor harus dipakai sementara, kontrak perlu mengatur akses admin, pencatatan perubahan, ekspor berkala, dan pemindahan saat terminasi. Rahasia (secret) tidak ditaruh di dokumen publik; serah terima dilakukan melalui kanal aman dan diikuti rotasi kredensial.

Untuk dependensi, minta daftar nama, versi, sumber, dan lisensi. Score dari OpenSSF dapat menjadi sinyal awal tentang praktik repositori, tetapi bukan due diligence atau jaminan bebas kerentanan. [OpenSSF Scorecard](https://securityscorecards.dev/) Karena itu, register harus dilengkapi pemeriksaan lisensi dan penilaian pemasok yang relevan dengan sistem Anda.

Saat serah terima, cocokkan register dengan bukti: tautan repository, arsip ekspor data, daftar akun, dokumentasi pemulihan, dan catatan lisensi. Tulis kriteria “diterima” yang dapat diperiksa, bukan sekadar “sudah diserahkan”. Pendekatan pengadaan yang mempertimbangkan risiko sejak awal sejalan dengan panduan [CISA Secure by Design](https://www.cisa.gov/securebydesign) dan [UK Technology Code of Practice](https://www.gov.uk/guidance/the-technology-code-of-practice), namun keduanya bukan hukum Indonesia dan tidak menggantikan telaah kontrak.

## Faktor yang mengubah hasil

Beberapa kondisi membuat klausul standar tidak memadai:

1. **Komponen vendor yang dipakai lintas klien.** Anda mungkin memerlukan lisensi penggunaan dan pemeliharaan, bukan pengalihan seluruh kode. Minta batas yang jelas agar background IP tidak tertukar dengan new IP.
2. **Layanan pihak ketiga dan API.** Ketentuan penyedia dapat berubah, memiliki kuota, atau melarang cara penggunaan tertentu. Catat nama akun, pemilik kontrak, tanggal pembaruan, dan rencana pengganti; statusnya harus diverifikasi saat review karena paket ini tidak memuat syarat layanan terkini.
3. **Data yang tidak mudah diekspor.** Format proprietary, lampiran besar, atau ketergantungan pada identitas vendor dapat membuat exit mahal. Tentukan format, frekuensi, media, dan uji ekspor yang disepakati.
4. **Akses berlapis.** Admin cloud, DNS, repository, dan email pemulihan bisa berada pada orang berbeda. Satu “akun utama” tanpa daftar pemulihan dan pemilik aktual bukan kontrol yang cukup.
5. **Perubahan ruang lingkup.** Modul tambahan dapat membawa lisensi dan dependensi baru. Jadikan setiap change request memperbarui register, dokumentasi, dan hak akses.

Kawan Codev.id, perlakukan dependensi, akun, dan data sebagai bagian dari produk yang harus dioperasikan, bukan lampiran administratif. Risiko rantai pasok tidak selesai ketika kode berpindah tangan; bukti asal komponen, pihak yang bertanggung jawab, dan rencana saat pemasok gagal tetap diperlukan. [NEEDS CURRENT-TERMS REVIEW: verifikasi syarat API, kuota, subprosesor, kerentanan, serta kebijakan ekspor terhadap layanan yang benar-benar dipilih proyek].

## Contoh keputusan praktis

Bayangkan pembeli menerima aplikasi yang berjalan di cloud vendor. Ada dua pilihan:

| Keputusan | Konsekuensi yang harus diterima |
| --- | --- |
| Akun cloud tetap atas nama vendor | Operasi mungkin lebih cepat, tetapi pemulihan akses, tagihan, dan perpindahan bergantung pada kerja sama vendor. Kontrak harus memuat akses admin, ekspor, dan batas waktu transfer. |
| Akun cloud dibuat atas nama pembeli | Pembeli memegang kendali tagihan dan identitas. Tanggung jawab konfigurasi, keamanan, dan prosedur pemulihan harus ditulis agar tidak diasumsikan sepihak. |

Untuk source code, tanyakan: “Jika vendor berhenti besok, apakah tim lain dapat checkout commit rilis, memasang dependensi, mengisi konfigurasi, dan melakukan deployment tanpa meminta rahasia pribadi?” Jika jawabannya belum, tambahkan item yang hilang ke daftar serah terima. Untuk data, lakukan uji ekspor dengan salinan nonproduksi dan catat siapa yang memeriksa hasilnya; jangan menguji dengan memindahkan data sensitif tanpa dasar yang sah.

## Kesalahan umum dan cara memeriksanya

**“Semua kode menjadi milik pembeli.”** Kalimat ini tidak menjelaskan template, library pihak ketiga, atau hak atas modifikasi. Minta daftar pengecualian dan bentuk hak yang diterima untuk setiap kelompok.

**“Source code sudah dikirim lewat ZIP.”** ZIP tanpa riwayat, instruksi build, konfigurasi, dan daftar versi mungkin tidak dapat dipelihara. Minta repository, tag rilis, checksum atau bukti integritas yang disepakati, serta dokumentasi pemulihan.

**“Kredensial dikirim lewat chat.”** Chat dapat tersimpan di banyak perangkat. Gunakan mekanisme aman yang disepakati, batasi penerima, lalu rotasi secret setelah serah terima.

**“Logo sertifikasi atau skor repositori sudah cukup.”** Sinyal eksternal tidak membuktikan ruang lingkup pekerjaan atau hasil sistem tertentu. [NEEDS PROJECT-EVIDENCE REVIEW: minta bukti yang relevan dengan tim, lingkungan, dan artefak proyek ini].

**“Exit dibahas nanti.”** Tanpa definisi data, format ekspor, akun, dokumentasi, dan batas waktu, “nanti” menjadi sengketa. Masukkan daftar exit ke kontrak sejak awal dan mintakan review hukum Indonesia.

Teman Codev.id, pemeriksaan sederhana yang bisa dilakukan sekarang adalah mencetak register aset satu halaman, memberi pemilik pada setiap baris, lalu menandai bukti yang belum ada. Baris tanpa pemilik, hak, atau cara ekspor adalah pertanyaan terbuka—bukan asumsi yang boleh diisi sendiri.

## Mengapa jalan pintas kontrak dapat gagal

Shortcut yang menggoda adalah memakai kontrak lama dengan satu klausul “seluruh hasil menjadi milik klien” agar proses cepat. Itu dapat gagal karena tidak menyebut akun, data, dependensi, hak pakai background IP, atau kewajiban lisensi. Alternatif yang lebih andal adalah lampiran register aset dan matriks hak: setiap item memiliki pemilik akun, bentuk hak, sumber/lisensi, bukti serah terima, serta prosedur saat kontrak berakhir. Lampiran itu tetap perlu diperiksa terhadap fakta proyek dan hukum yang berlaku. Untuk menyiapkan pertanyaan awal, Anda dapat melihat [beranda Codev.id](/).

## Kesimpulan

Source code, IP, data, akun, dan lisensi harus diperlakukan sebagai inventaris hak serta kemampuan operasi, bukan sekadar daftar file. Sebelum mulai, minta register aset; sebelum membayar tahap akhir, cocokkan bukti repository, build, ekspor data, akses akun, dokumentasi, dan lisensi; sebelum menandatangani, minta [NEEDS CONTRACT REVIEW: qualified Indonesian legal review atas ownership, lisensi, data, akses, dan exit].

Mulailah dari satu dokumen yang memiliki versi dan pemilik, lalu perbarui setiap ada perubahan ruang lingkup. Jika satu baris belum punya hak yang jelas atau jalur pemulihan yang dapat diuji, anggap pekerjaan belum siap diserahterimakan—dan jangan mengubah ketidakpastian kontrak menjadi kepastian dengan asumsi.
