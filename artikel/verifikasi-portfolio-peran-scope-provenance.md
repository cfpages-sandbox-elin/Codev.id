---
article_id: CDV-20-A02
writing_contract_version: "native-id-v2"
title: "Memverifikasi Portfolio: Peran, Scope, dan Provenance"
slug: "verifikasi-portfolio-peran-scope-provenance"
description: "Verify relationship/permission, date, problem, exact contribution, technology, collaborators/subcontractors, artifacts, acceptance, current status, and claim limits"
status: draft
publication_date: "2026-07-09"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-20
primary_intent: "Determine what a portfolio entry actually proves"
reader_community: "Codev.id"
reader_address: "Kawan Codev.id"
final_route: "/artikel/verifikasi-portfolio-peran-scope-provenance.html"
technical_review: required
sources:
  - "https://csrc.nist.gov/pubs/sp/800/161/r1/final"
  - "https://www.cisa.gov/securebydesign"
  - "https://www.gov.uk/guidance/the-technology-code-of-practice"
  - "https://www.gov.uk/service-manual/service-standard"
  - "https://csrc.nist.gov/pubs/sp/800/218/final"
  - "https://www.w3.org/TR/WCAG-EM/"
---

# Memverifikasi Portfolio: Peran, Scope, dan Provenance

Halo, Kawan Codev.id! Ketika sebuah portfolio menampilkan logo, domain, tangkapan layar, atau daftar teknologi, pertanyaan yang perlu dijawab bukan “Apakah tampilannya meyakinkan?” melainkan “Bagian mana yang benar-benar dikerjakan oleh penyedia ini, dengan kewenangan apa, dan bukti apa yang masih bisa diperiksa?”

Jawaban singkatnya: portfolio hanya membuktikan klaim yang dapat ditautkan ke identitas pihak, izin publikasi, waktu, ruang lingkup, kontribusi spesifik, artefak, dan status yang dapat diverifikasi. Logo atau domain sendiri tidak membuktikan kepemilikan build, hasil bisnis, keamanan, aksesibilitas, maupun persetujuan klien. Jika hubungan, scope, atau sumber artefaknya belum jelas, turunkan klaim menjadi “contoh tampilan yang pernah ditangani” atau tahan keputusan sampai bukti tersedia. Kerangka pengadaan NIST juga menempatkan pembagian peran, risiko, dan bukti serah-terima sebagai hal yang perlu dinormalisasi, bukan diasumsikan dari materi promosi ([NIST SP 800-161 Rev. 1](https://csrc.nist.gov/pubs/sp/800/161/r1/final)).

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

*Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.*

## Definisi dan batas objek

Dalam pemeriksaan ini, **provenance** berarti jejak asal-usul klaim: siapa yang memberikan informasi, kapan artefak dibuat, versi mana yang diperiksa, dan bagaimana artefak itu berhubungan dengan pekerjaan yang disebut. **Peran** menjawab posisi penyedia—misalnya pemilik produk, pelaksana implementasi, kontributor terbatas, konsultan, atau subkontraktor. **Scope** menjawab bagian pekerjaan yang masuk kontrak dan bagian yang berada di luar kewenangan.

Tiga hal itu berbeda dari “halaman yang bisa dibuka”. Domain mungkin masih aktif setelah tim berganti. Screenshot dapat berasal dari lingkungan demo. Badge alat hanya menunjukkan alat yang disebut, bukan siapa yang mengonfigurasi atau memeliharanya. Panduan Service Standard Inggris menekankan perlunya memahami kebutuhan, menguji layanan, dan memperbaikinya secara berulang; daftar teknologi tanpa jejak keputusan dan pengujian tidak cukup untuk menyimpulkan kualitas layanan ([UK Government Service Standard](https://www.gov.uk/service-manual/service-standard)). Prinsip pengadaan teknologi juga mendorong kejelasan kepemilikan, risiko, dan kemampuan untuk berpindah penyedia ketika hubungan kerja berubah ([UK Technology Code of Practice](https://www.gov.uk/guidance/the-technology-code-of-practice)).

Batasnya penting. Artikel ini membantu pembeli atau editor menguji apa yang dibuktikan oleh satu entri portfolio. Artikel ini tidak menetapkan hasil bisnis, menyatakan kepatuhan hukum, atau menggantikan persetujuan proyek dan pemeriksaan profesional. Studi kasus before-after, testimonial, dan bukti outcome memerlukan verifikasi tersendiri. Untuk langkah awal, gunakan [halaman portfolio](/portfolio) sebagai daftar klaim yang harus dimintakan buktinya, bukan sebagai bukti final.

## Cara kerjanya

Verifikasi paling rapi berjalan dari identitas ke klaim, lalu dari klaim ke artefak. Minta jawaban tertulis singkat untuk urutan berikut.

1. **Hubungan dan izin.** Siapa entitas yang mengontrak pekerjaan? Siapa kontak yang berwenang mengonfirmasi? Apakah nama, logo, domain, dan gambar boleh dipublikasikan, dan untuk periode apa? Tanpa izin, materi publik tidak boleh dipakai sebagai kesaksian klien.
2. **Tanggal dan keadaan.** Catat kapan pekerjaan dimulai, kapan kontribusi berakhir, dan status saat diperiksa. Bedakan versi yang dibuat penyedia dari versi yang telah diubah pihak lain.
3. **Masalah dan batas pekerjaan.** Minta rumusan masalah, deliverable, lingkungan, serta item yang secara eksplisit dikecualikan. “Membangun website” terlalu luas untuk menjadi scope yang dapat diuji.
4. **Kontribusi dan pelaku.** Petakan siapa yang memimpin discovery, desain, kode, deployment, konten, pengujian, dan operasi. Tandai kolaborator atau subkontraktor serta bagian yang mereka kerjakan. CISA mengingatkan bahwa keamanan perlu dipikirkan sejak desain dan sepanjang siklus hidup; menyebut “secure by design” tanpa menunjukkan tanggung jawab dan proses tidak membuktikan hasil keamanan ([CISA Secure by Design](https://www.cisa.gov/securebydesign)).
5. **Teknologi dan artefak.** Hubungkan versi, repository atau export yang boleh dilihat, keputusan arsitektur, tiket, catatan rilis, laporan uji, dan dokumentasi handover dengan kontribusi orang atau tim tertentu. Jangan meminta rahasia dagang; cukup minta artefak yang dapat disanitasi dan jejak kepemilikan.
6. **Penerimaan dan status kini.** Siapa acceptance owner yang menyetujui deliverable? Apakah ada daftar pengecualian? Apakah layanan masih dipelihara oleh penyedia, sudah dialihkan, atau sudah dekomisioning? Status “live” hanya menjelaskan keadaan saat ini, bukan keberhasilan masa lalu.
7. **Batas klaim.** Tulis kalimat yang boleh digunakan dan kalimat yang harus ditahan. Contoh: “tim mengerjakan integrasi formulir pada versi X” lebih terukur daripada “tim meningkatkan konversi”.

Setiap langkah menghasilkan pasangan **klaim–bukti–pemilik konfirmasi**. Jika salah satu kosong, tandai sebagai belum terverifikasi, bukan diisi dengan dugaan.

## Faktor yang mengubah hasil

Kualitas verifikasi berubah menurut empat kondisi. Pertama, **perubahan kepemilikan**: domain, repository, akun cloud, atau kontrak dukungan dapat berpindah. Kedua, **scope berlapis**: vendor utama dapat menyerahkan desain kepada studio lain dan deployment kepada subkontraktor. Ketiga, **perbedaan lingkungan**: demo, staging, dan produksi bisa memakai konfigurasi serta data yang berbeda. Keempat, **umur bukti**: dokumentasi lama tidak otomatis menggambarkan versi yang sedang dilihat.

Klaim teknis juga membutuhkan disiplin. NIST SSDF menempatkan praktik pengembangan aman dalam konteks versi, organisasi, dan proses; namanya tidak membuktikan bahwa rilis tertentu telah diuji ([NIST SSDF 1.1](https://csrc.nist.gov/pubs/sp/800/218/final)). Demikian pula, evaluasi aksesibilitas W3C membutuhkan scope, metode, sampel, dan hasil evaluasi; satu screenshot atau badge tidak cukup untuk menyatakan conformance ([W3C WCAG-EM](https://www.w3.org/TR/WCAG-EM/)).

Untuk klaim vendor, identitas, kepemilikan, consent, subkontraktor, warranty, hasil, handover, dan exit perlu ditutup oleh kontrak atau bukti penyedia yang masih berlaku. Jika paket itu belum lengkap, **[NEEDS GATE-09 REVIEW: hubungan, izin publikasi, scope, ownership, kolaborator, dan batas klaim belum terverifikasi]**. Penanda ini bukan tuduhan; ia mencegah editor mengubah materi promosi menjadi fakta proyek.

## Contoh keputusan praktis

Bayangkan editor menemukan entri yang menampilkan logo organisasi, tautan domain aktif, dan teks “platform aman serta mudah diakses”. Tidak ada nama peran, tanggal, atau laporan uji.

| Temuan | Yang boleh disimpulkan | Keputusan |
|---|---|---|
| Logo dan domain dapat dibuka | Ada materi publik yang mengaitkan proyek dengan nama tersebut | Minta konfirmasi hubungan dan izin; jangan nyatakan build ownership |
| Screenshot menampilkan fitur tertentu | Fitur terlihat pada gambar yang disediakan | Minta versi, environment, dan kontribusi; jangan nyatakan hasil pengguna |
| Badge teknologi tercantum | Teknologi disebut dalam materi | Minta artefak konfigurasi atau keputusan; jangan nyatakan kompetensi tim |
| Testimonial menyebut hasil | Ada pernyataan yang dikaitkan pada pemberi testimoni | Verifikasi identitas, tanggal, metode ukur, dan consent; tahan angka tanpa sumber |
| Laporan uji dan acceptance owner tersedia | Klaim terbatas dapat dihubungkan ke rilis dan kriteria | Catat versi, pengecualian, serta status pemeliharaan |

Kawan Codev.id, gunakan tabel seperti ini saat rapat pengadaan. Minta penyedia mengisi kolom “peran saya”, “bagian pihak lain”, “artefak yang bisa dibagikan”, dan “klaim yang tidak saya ajukan”. Bila mereka hanya dapat mengulang logo dan URL, keputusan yang wajar adalah meminta bukti tambahan atau memperkecil bobot portfolio dalam penilaian.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah menyamakan **akses** dengan **kepemilikan**. Periksa nama kontraktor, repository, akun deployment, dan catatan handover. Kesalahan kedua adalah menganggap **tanggal halaman** sebagai tanggal pekerjaan. Minta tanggal kontrak, rilis, atau persetujuan yang relevan. Kesalahan ketiga adalah mencampur **kontribusi tim** dengan kontribusi individu. Minta pembagian peran dan subkontraktor.

Kesalahan keempat adalah memakai **alat atau standar sebagai hasil**. Tanyakan versi, scope, metode pengujian, defect atau pengecualian, dan siapa yang menerima rilis. Kesalahan kelima adalah mengutip **angka outcome tanpa baseline**. Tanpa jendela waktu, definisi metrik, dan pemilik data, angka itu sebaiknya dihapus dari naskah.

Terakhir, jangan menganggap persetujuan sekali berlaku selamanya. Simpan sumber, tanggal pemeriksaan, status kini, dan batas penggunaan. Jika status atau izin berubah, perbarui entri atau cabut klaimnya.

## Jalan pintas yang tampak praktis

Shortcut yang sering dipilih adalah: “URL-nya live dan kliennya terkenal, jadi kita bisa langsung percaya.” Shortcut ini gagal karena URL hanya menunjukkan keadaan yang dapat dilihat sekarang, sedangkan pekerjaan mungkin dikerjakan pihak berbeda, sudah berubah, atau tidak pernah mencakup klaim yang ditulis. Cara yang lebih aman adalah meminta konfirmasi peran dan izin, menautkan setiap klaim ke artefak bertanggal, lalu menuliskan apa yang tidak diketahui. Proses itu mungkin membuat portfolio lebih pendek, tetapi keputusan menjadi dapat dipertanggungjawabkan.

## Kesimpulan

Portfolio yang terverifikasi bukan yang paling banyak logo, melainkan yang setiap klaimnya punya provenance: hubungan dan izin jelas, tanggal serta scope terbatas, kontribusi dan kolaborator terpetakan, artefak serta acceptance dapat ditunjukkan, dan status kini tidak dibesar-besarkan. Sebelum menyetujui penyedia atau menerbitkan entri, minta satu lembar matriks klaim–bukti–pemilik konfirmasi dan cocokkan dengan kontrak atau catatan handover. Jika GATE-09 belum terpenuhi, pertahankan penanda review dan jangan mengubah tampilan publik menjadi klaim kompetensi, kepemilikan, atau hasil.

Aturan operasionalnya sederhana, Teman Codev.id: nyatakan hanya apa yang dapat Anda tunjukkan, dan sebutkan batas yang belum dapat Anda buktikan.

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
