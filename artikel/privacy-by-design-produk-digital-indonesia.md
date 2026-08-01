---
article_id: CDV-09-A05
title: "Privacy by Design untuk Produk Digital di Indonesia"
slug: "privacy-by-design-produk-digital-indonesia"
description: "Panduan memetakan tujuan, kategori dan subjek data, peran, pemberitahuan, minimisasi, akses, retensi, vendor, hak, keamanan, serta penghapusan sejak tahap desain."
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2025-10-24"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-09
primary_intent: "Translate personal-data processing into design decisions"
reader_community: "Codev.id"
reader_address: "Sobat Codev.id"
final_route: "/artikel/privacy-by-design-produk-digital-indonesia.html"
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

# Privacy by Design untuk Produk Digital di Indonesia

Halo, Sobat Codev.id! Privacy by Design berarti keputusan tentang data pribadi dibuat sejak discovery dan desain, bukan ditempelkan ketika produk hampir rilis. Jawaban singkatnya: petakan alasan pemrosesan, jenis data, subjek, peran, dasar yang akan ditinjau secara hukum, alur akses, masa simpan, pihak ketiga, keamanan, hak, dan penghapusan; lalu ubah peta itu menjadi keputusan produk yang dapat diuji.

Dengan cara ini, layar pendaftaran, API, log, dashboard admin, kontrak vendor, dan proses penghapusan menyampaikan keputusan yang sama. Ini adalah kerangka desain dan pendidikan umum, bukan penetapan dasar pemrosesan atau bukti kepatuhan. [NEEDS LEGAL REVIEW: lawful basis, notice, retention, rights, and Indonesian regulatory obligations under UU PDP and implementing rules.] Fakta proyek, kategori data, dan peran organisasi dapat mengubah keputusan akhir.

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

*Aset lokal proyek; bukan dokumentasi proyek tertentu.*

## Hasil akhir dan prasyarat

Hasil yang dituju adalah satu *data decision record*: tabel yang menghubungkan tujuan bisnis dengan kategori data, subjek, peran pengendali/prosesor, titik pengumpulan, penerima, dasar yang perlu divalidasi, masa simpan, kontrol akses, dan jalur hak subjek. Setiap keputusan memiliki pemilik, tanggal peninjauan, dan bukti pengujian. Tim produk, engineering, keamanan, operasi, vendor management, dan penasihat hukum perlu menyepakati siapa yang berwenang mengubahnya.

Mulailah dengan data awal yang nyata: diagram alur, daftar endpoint dan event, contoh formulir, skema basis data, daftar peran pengguna, konfigurasi log, kontrak pemroses, serta inventaris dependensi. Jika ada data yang belum diketahui, tandai sebagai pertanyaan terbuka; jangan mengisinya dengan asumsi. Pertanyaan pertama dalam rapat adalah, “Tujuan apa yang benar-benar membutuhkan atribut ini, dan kapan tujuan itu berakhir?”

## Langkah 1 — tetapkan ruang lingkup

Tulis batas produk dan batas analisis: fitur yang dicakup, lingkungan (aplikasi, API, analitik, dukungan), antarmuka internal-eksternal, serta alur keluar ke vendor atau negara lain. Pisahkan data yang dikumpulkan langsung, dihasilkan dari aktivitas, diterima dari pihak lain, dan muncul hanya di log. Catat subjeknya—misalnya pengguna, staf, kontak darurat, atau administrator—tanpa menebak kategori sensitif sebelum konteks dikonfirmasi.

Kemudian bedakan peran. Satu organisasi bisa menjadi pengendali untuk tujuan tertentu dan prosesor untuk tujuan pihak lain. Penilaiannya harus melihat fakta pemrosesan yang benar-benar terjadi, kewenangan menentukan tujuan dan cara, kontrak serta instruksi aktual, dan hukum yang berlaku—bukan sekadar label di slide. Tandai pula hal yang sengaja tidak dikerjakan: penentuan dasar hukum, pendapat tentang transfer lintas batas, penilaian dampak formal, atau persetujuan rilis. Batas ini mencegah desain terlihat “selesai” padahal keputusan legal dan proyek belum disetujui.

Untuk API, buat kontrak yang menyebut input, output, autentikasi, dan error yang memang diperlukan. OpenAPI mendeskripsikan antarmuka; dokumen itu tidak membuktikan implementasi telah aman atau berperilaku sesuai kontrak ([OpenAPI Specification 3.1.1](https://spec.openapis.org/oas/v3.1.1.html)). Karena itu, scope juga harus memuat pengujian terhadap implementasi, bukan hanya berkas spesifikasi.

## Langkah 2 — kumpulkan dan cocokkan bukti

Bangun matriks sederhana dengan kolom: tujuan, kategori data, subjek, sumber, sistem, penerima, peran, alasan yang akan ditinjau, pemberitahuan, akses, retensi, penghapusan, dan bukti. Cocokkan setiap baris dengan artefak: screenshot notifikasi, versi teks persetujuan atau pemberitahuan, migrasi skema, aturan otorisasi, konfigurasi *retention job*, tiket penghapusan, serta catatan keputusan vendor. Bukti harus menunjukkan kondisi produk tertentu dan tanggalnya; label “sesuai standar” saja tidak cukup.

Untuk autentikasi, dokumentasikan pilihan dan ancamannya. OAuth 2.0 Security BCP RFC 9700 adalah pembaruan praktik terbaik pada 2025, tetapi rekomendasi alur tetap perlu disesuaikan dengan klien, ancaman, dan kemampuan operasional ([RFC 9700](https://www.rfc-editor.org/info/rfc9700/)). Jika memilih passkey, WebAuthn Level 3 dapat menjadi referensi antarmuka dan proses kredensial; ia tidak menghapus kebutuhan untuk merancang pemulihan akun, pemisahan peran, dan notifikasi perubahan ([WebAuthn Level 3](https://www.w3.org/TR/webauthn-3/)). Jangan menaruh rahasia, token, atau skema privat di dokumen publik.

Untuk integrasi, minta daftar pemroses dan subprosesor, lokasi pemrosesan, tujuan penggunaan, masa simpan, jalur penghapusan, serta mekanisme pemberitahuan insiden. SBOM membantu transparansi komponen tetapi tidak menetapkan bahwa komponen aman ([CISA SBOM resources](https://www.cisa.gov/sbom)). Panduan NIST tentang risiko rantai pasok perangkat lunak membantu menyusun proses evaluasi; ia bukan pengganti penilaian vendor dan kontrak ([NIST SP 800-161 Rev. 1](https://csrc.nist.gov/pubs/sp/800/161/r1/final)). Skor repositori dari OpenSSF Scorecard dapat menjadi sinyal awal, bukan uji tuntas atau jaminan keamanan ([OpenSSF Scorecard](https://securityscorecards.dev/)).

## Langkah 3 — jalankan urutan kerja

Urutan berikut membuat peta menjadi keputusan yang bisa dikerjakan.

1. **Tulis tujuan per fitur.** Hindari tujuan payung seperti “meningkatkan layanan”. Nyatakan tindakan dan hasil yang dibutuhkan, lalu hapus atribut yang tidak berkontribusi. Simpan nilai yang lebih kasar jika cukup; jangan mengumpulkan tanggal lahir lengkap ketika rentang usia sudah memadai.
2. **Tentukan titik pemberitahuan.** Tampilkan siapa yang menerima data, tujuan, pilihan pengguna, dan kontak pertanyaan pada saat yang relevan. Sinkronkan teks dengan event yang benar-benar dikirim ke backend dan vendor.
3. **Rancang akses berlapis.** Pisahkan peran pengguna, dukungan, admin, dan pekerjaan otomatis. Terapkan otorisasi pada setiap objek dan fungsi, bukan hanya saat login. OWASP API Security Top 10 menempatkan kegagalan otorisasi objek dan fungsi serta konsumsi sumber daya yang tidak dibatasi sebagai risiko yang perlu diuji ([OWASP API Security Top 10 2023](https://owasp.org/API-Security/editions/2023/en/0x11-t10/)).
4. **Tentukan retensi dan penghapusan.** Kaitkan masa simpan dengan tujuan dan pemicu, lalu definisikan penghapusan dari basis utama, cache, indeks pencarian, ekspor, dan log yang memang berada dalam kendali tim. Koordinasikan pengecualian untuk kewajiban hukum serta pembekuan bukti ketika ada dugaan atau insiden keamanan aktif agar proses privasi tidak menghapus bukti yang masih diperlukan. Jangan menjanjikan penghapusan seketika jika sistem, kewajiban, atau kebutuhan investigasi belum dipetakan dan disetujui.
5. **Uji jalur hak dan koreksi.** Simulasikan permintaan akses, koreksi, keberatan, atau penghapusan dengan identitas yang tepat dan catat siapa yang menyetujui. Pastikan keluaran tidak membocorkan data subjek lain dan ada cara menangani data yang disalin ke vendor.
6. **Amankan perubahan dan dependensi.** Gunakan tinjauan kode, pengelolaan rahasia, pembatasan laju, validasi input, pencatatan yang minim, dan pemantauan anomali. Hubungkan komponen dalam SBOM dengan pemilik dan rencana pembaruan; jangan menganggap skor eksternal sebagai bukti lulus.

Kawan Codev.id, jadikan setiap langkah sebagai kriteria penerimaan. Contohnya: “endpoint ekspor hanya mengembalikan atribut yang terpetakan untuk subjek yang sudah terverifikasi” lebih dapat diuji daripada “data dijaga aman”. Untuk keputusan produk yang lebih luas, Anda dapat menautkan [deskripsi produk yang menjelaskan ruang lingkup fitur](/konten/deskripsi-produk) agar tujuan dan batasnya tetap konsisten.

## Titik berhenti dan kondisi berhenti

Hentikan implementasi atau rilis ketika tujuan belum disepakati, kategori data belum terpetakan, peran pengendali/prosesor masih diperdebatkan, atau pemberitahuan berbeda dari perilaku sistem. Berhenti juga bila vendor menolak menjelaskan subprosesor, lokasi, retensi, akses dukungan, atau proses penghapusan; minta klarifikasi dan tinjauan kontrak terlebih dahulu.

Untuk akses dan keamanan, jangan melanjutkan saat pengujian otorisasi objek/fungsi belum ada, pemulihan akun dapat melewati kontrol, rahasia muncul di log, atau rate limit belum diuji pada skenario penyalahgunaan. Jika alur OAuth, passkey, atau transfer data lintas pihak berubah, lakukan tinjauan ancaman dan legal yang baru. [NEEDS SECURITY AND LEGAL REVIEW: unresolved authorization, vendor-transfer, or rights-handling risks.] Dokumentasikan temuan ini secara terbuka dalam catatan keputusan agar risiko tidak tertutup oleh bahasa pemasaran.

## Verifikasi hasil dan serah terima

Sebelum serah terima, pemilik produk menandatangani matriks tujuan-data; engineering menyimpan versi kontrak API, migrasi, dan hasil uji; keamanan merekam skenario penyalahgunaan dan perbaikannya; operasi mencatat jadwal retensi, penghapusan, dan pemantauan; vendor management menyimpan inventaris pemroses dan bukti tinjauan; penasihat hukum mengonfirmasi bagian yang menjadi nasihat hukum. Simpan tautan artefak, versi, tanggal, dan orang yang menyetujui.

Lakukan pemeriksaan sampel: kirim satu permintaan hak melalui jalur dukungan, telusuri data dari UI ke log dan vendor, lalu pastikan penghapusan atau pembatasan berjalan sesuai keputusan yang disetujui. Setelah rilis, tetapkan pemicu koreksi: perubahan tujuan, skema, vendor, negara pemrosesan, metode autentikasi, insiden keamanan, atau perubahan aturan. Review berkala harus menghasilkan keputusan—lanjut, ubah, atau hentikan—bukan sekadar rapat status.

## Jalan pintas yang perlu dihindari

Jalan pintas yang sering dipilih adalah menyalin kebijakan privasi lama, menambahkan kotak persetujuan, lalu menganggap produk selesai. Cara ini gagal karena teks tidak mengubah kolom yang dikumpulkan, izin endpoint, masa simpan, salinan vendor, atau perilaku log. Kontrak OpenAPI pun hanya menggambarkan antarmuka, sementara risiko otorisasi dan penyalahgunaan API muncul pada implementasi ([OpenAPI Specification 3.1.1](https://spec.openapis.org/oas/v3.1.1.html); [OWASP API Security Top 10 2023](https://owasp.org/API-Security/editions/2023/en/0x11-t10/)).

Alternatif yang lebih dapat diandalkan adalah membuat satu matriks keputusan yang ditautkan ke desain, tes, kontrak vendor, dan jadwal retensi. Jika satu baris tidak memiliki pemilik atau bukti, tandai sebagai belum siap dan bawa ke review yang berwenang. Sobat Codev.id, kecepatan rilis lebih aman ketika pertanyaan sulit terlihat sebelum data telanjur tersebar.

## Kesimpulan

Privacy by Design untuk produk digital di Indonesia adalah praktik menerjemahkan tujuan pemrosesan menjadi pilihan data, antarmuka, akses, retensi, vendor, keamanan, hak, dan penghapusan yang dapat diuji. Mulailah dengan matriks keputusan dan diagram alur untuk satu fitur, kemudian minta review hukum Indonesia atas dasar pemrosesan, pemberitahuan, hak, dan transfer yang relevan. Teman Codev.id, jangan beri status “siap” sebelum setiap keputusan memiliki pemilik, bukti, dan pemicu koreksi. Aturan operasionalnya sederhana: kumpulkan hanya yang dapat Anda jelaskan, lindungi sesuai risikonya, dan hapus ketika tujuan yang disetujui berakhir—dengan batas akhir tetap mengikuti review profesional dan fakta proyek.
