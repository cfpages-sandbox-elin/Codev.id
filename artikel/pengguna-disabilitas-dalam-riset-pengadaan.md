---
article_id: CDV-13-A06
title: "Melibatkan Pengguna Disabilitas dalam Riset dan Pengadaan"
slug: "pengguna-disabilitas-dalam-riset-pengadaan"
description: "Plan accessible recruitment/materials/sessions, consent, accommodations, compensation, task evidence, privacy, interpretation, and procurement questions"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2026-02-03"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-13
primary_intent: "Include disabled users and evidence in product decisions"
reader_community: "Codev.id"
reader_address: "Teman Codev.id"
final_route: "/artikel/pengguna-disabilitas-dalam-riset-pengadaan.html"
technical_review: required
sources:
  - "https://www.gov.uk/service-manual/agile-delivery"
  - "https://www.w3.org/TR/WCAG22/"
  - "https://csrc.nist.gov/pubs/sp/800/218/final"
  - "https://www.w3.org/TR/WCAG-EM/"
  - "https://spec.openapis.org/oas/v3.1.1.html"
  - "https://www.w3.org/WAI/test-evaluate/preliminary/"
---

# Melibatkan Pengguna Disabilitas dalam Riset dan Pengadaan

Halo, Teman Codev.id!

Melibatkan pengguna disabilitas dalam riset dan pengadaan bukan sekadar menambah satu sesi uji atau mencentang pernyataan “inklusif”. Keputusan yang dapat dipertanggungjawabkan harus menunjukkan siapa yang direkrut, tugas apa yang dicoba, dukungan apa yang tersedia, persetujuan apa yang diberikan, dan bagaimana temuan itu mengubah prioritas produk. Jawaban singkatnya: libatkan orang dengan kebutuhan akses yang relevan sejak discovery, rancang sesi yang bisa mereka ikuti, dokumentasikan bukti tugas dan hambatan, lalu jadikan bukti tersebut masukan eksplisit dalam spesifikasi dan penerimaan pengadaan.

Jumlah peserta yang sedikit tetap berguna untuk menemukan hambatan nyata, tetapi tidak menjadikan mereka wakil universal semua penyandang disabilitas. Riset pengguna juga tidak menggantikan evaluasi teknis WCAG atau pengujian dengan teknologi bantu. Jika keputusan rilis bergantung pada kepatuhan, keamanan, atau perilaku perangkat tertentu, hasilnya perlu pemeriksaan spesialis dan persetujuan pemilik risiko. [NEEDS TECHNICAL REVIEW: GATE-05 dan GATE-06 harus dikonfirmasi pada produk dan lingkungan target.]

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

## Definisi dan batas objek

“Pengguna disabilitas” di sini berarti orang yang akan memakai layanan atau berinteraksi dengan hasil pengadaan dan memiliki kebutuhan akses yang relevan dengan konteks itu. Kebutuhan dapat berkaitan dengan penglihatan, pendengaran, gerak, kognisi, bicara, atau kombinasi beberapa hal. Tanyakan dukungan yang dibutuhkan; jangan menebak diagnosis dari penampilan atau meminta peserta membuktikan kondisi medis yang tidak diperlukan untuk riset.

Objek risetnya adalah pengalaman dan tugas: menemukan informasi, mengisi formulir, masuk, mengunggah dokumen, memahami pesan kesalahan, atau menyelesaikan alur lain yang memang akan dibeli. Satu sesi tidak membuktikan seluruh pengalaman. WCAG 2.2 mencakup aspek seperti keyboard, fokus, formulir, reflow, autentikasi, media, dan perilaku dengan teknologi bantu; evaluasi harus menentukan cakupan halaman serta proses yang benar-benar digunakan ([W3C WCAG 2.2](https://www.w3.org/TR/WCAG22/), [WCAG-EM 1.0](https://www.w3.org/TR/WCAG-EM/)).

Karena itu, halaman ini membahas keputusan riset dan pengadaan—bukan audit teknis, sertifikasi, atau penetapan kepatuhan hukum Indonesia. Riset memberi bukti tentang kemampuan menyelesaikan tugas dan konteks penggunaan. Audit memberi pemeriksaan terhadap implementasi. Keduanya menjawab pertanyaan berbeda dan harus dilacak terpisah.

## Cara kerjanya

Mulailah dengan keputusan yang hendak dibuat. Tulis pertanyaan seperti “Apakah kandidat dapat mengirim permohonan tanpa mouse?” atau “Dukungan apa yang wajib tersedia ketika autentikasi gagal?” Jangan mulai dari daftar fitur aksesibilitas yang tidak terkait alur. Praktik delivery yang iteratif memisahkan asumsi, kebutuhan pengguna, perilaku fungsional, kualitas, kendala, dan bukti penerimaan ([UK Government Service Manual—agile delivery](https://www.gov.uk/service-manual/agile-delivery)).

Susun rekrutmen bersama organisasi atau jaringan yang dipercaya, lalu berikan undangan dalam format yang bisa diakses. Jelaskan durasi, aktivitas, rekaman, kompensasi, risiko ketidaknyamanan, dan hak untuk berhenti. Sediakan formulir pendaftaran alternatif—misalnya surel atau telepon—serta jangan menjadikan pengungkapan diagnosis sebagai syarat bila kebutuhan akses saja sudah cukup.

Sebelum sesi, tanyakan preferensi komunikasi dan akomodasi: pembaca layar, teks langsung, penerjemah bahasa isyarat, jeda tambahan, ruang tenang, perangkat pribadi, atau pendamping. Konfirmasi siapa yang menanggung biaya akses dan kapan kompensasi dibayar. Persetujuan harus dipisahkan dari persetujuan perekaman; peserta boleh ikut tanpa direkam jika desain riset memungkinkan.

Saat sesi berlangsung, berikan tugas yang sama dengan tujuan produk, tetapi izinkan jalur dan alat yang biasa dipakai peserta. Catat tindakan, titik berhenti, bantuan yang diminta, pesan yang membingungkan, dan kondisi lingkungan. Bedakan observasi (“fokus berpindah ke awal halaman”) dari interpretasi (“peserta tidak paham”). Tanyakan alasan setelah tugas, tanpa mengarahkan jawaban.

Setelahnya, ubah temuan menjadi keputusan yang dapat dilacak: risiko atau kebutuhan, bukti tugas, dampak, pemilik, kriteria penerimaan, dan status. Pengujian otomatis atau satu pemeriksaan manual hanya membuktikan pernyataan yang diuji pada build, data, dan lingkungan sampel. SSDF NIST menekankan praktik aman yang dapat ditelusuri dari risiko hingga hasil dan cacat yang belum terselesaikan ([NIST SP 800-218 SSDF 1.1](https://csrc.nist.gov/pubs/sp/800/218/final)). Untuk menyiapkan pertanyaan lanjutan tentang praktik digital, Anda dapat mulai dari [beranda Codev.id](/).

## Faktor yang mengubah hasil

Profil peserta mengubah pertanyaan yang dapat dijawab. Pengguna pembaca layar dapat menemukan masalah struktur dan nama kontrol; pengguna keyboard dapat mengungkap jebakan fokus; pengguna dengan gangguan pendengaran dapat memeriksa apakah informasi penting hanya disampaikan lewat suara. Ini bukan pembagian kotak-kotak permanen—tanyakan pengalaman dan alat yang nyata.

Material dan fasilitator sama pentingnya dengan produk. Dokumen pra-sesi perlu format yang dapat dibaca teknologi bantu, kontras yang memadai, bahasa langsung, dan alternatif bila lampiran tidak terbuka. Fasilitator perlu menyebutkan perubahan agenda, membaca teks yang relevan, dan memberi waktu hening untuk mengetik atau menggunakan alat komunikasi. Jika penerjemah atau pendamping hadir, sepakati peran serta kerahasiaannya terlebih dahulu.

Kompensasi, privasi, dan keamanan memengaruhi kualitas bukti. Simpan catatan dengan kode peserta, batasi akses, tetapkan masa simpan, dan hapus rekaman ketika tidak lagi diperlukan sesuai persetujuan. Jangan memasukkan rekaman mentah atau diagnosis ke tiket vendor. Untuk API atau integrasi, minta kontrak antarmuka yang jelas tentang skema, error, autentikasi, dan perubahan versi; spesifikasi OpenAPI dapat menjadi format pertukaran, bukan bukti bahwa alurnya mudah diakses ([OpenAPI Specification 3.1.1](https://spec.openapis.org/oas/v3.1.1.html)).

Lingkungan juga menentukan hasil: browser, sistem operasi, teknologi bantu, koneksi, akun uji, dan data yang dipakai. Catat semuanya agar temuan dapat diulang. Jangan mengubah kegagalan pada satu kombinasi menjadi klaim semua pengguna akan gagal, dan jangan menganggap keberhasilan pada satu kombinasi sebagai jaminan universal.

## Contoh keputusan praktis

Bayangkan tim akan membeli portal pengajuan. Pertanyaan risetnya: “Dapatkah pengguna menyelesaikan pengajuan dan memperbaiki kesalahan tanpa bantuan fasilitator?” Rekrut beberapa pengguna dengan kebutuhan akses berbeda yang memang relevan, tawarkan pilihan komunikasi, dan biarkan mereka memakai perangkat sendiri. Bukti yang disimpan bukan skor kepuasan saja, melainkan langkah yang dilakukan, hambatan, bantuan, dan pesan kesalahan yang ditemui.

Gunakan tabel keputusan berikut sebagai contoh format, bukan ambang universal:

| Temuan sesi | Keputusan pengadaan | Bukti penerimaan |
|---|---|---|
| Fokus hilang setelah dialog ditutup | Minta perilaku fokus yang dapat diprediksi dan skenario uji keyboard | Rekaman langkah pada build kandidat, lalu verifikasi manual |
| Instruksi penting hanya berupa audio | Wajibkan teks atau transkrip yang setara | Tugas diselesaikan dengan audio dimatikan |
| Peserta memerlukan bantuan fasilitator untuk error | Minta pesan error yang menjelaskan masalah dan tindakan berikutnya | Uji data tidak valid dengan akun uji |
| Vendor menyatakan “lulus scanner” | Jangan terima sebagai satu-satunya bukti | Laporan cakupan, pemeriksaan manual, dan temuan teknologi bantu |

Kawan Codev.id, minta vendor mengaitkan setiap kriteria dengan artefak dan lingkungan uji. Jika bukti belum ada, statusnya “belum terbukti”, bukan “diasumsikan lulus”. Pemilik produk kemudian memutuskan apakah risiko diterima, diperbaiki, atau menghalangi rilis.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah merekrut setelah solusi dipilih. Periksa dokumen keputusan: adakah kebutuhan pengguna disabilitas yang memengaruhi masalah, alur, dan prioritas sejak awal? Kedua, memakai satu peserta sebagai simbol representasi. Tulis karakteristik dan konteks yang benar-benar diteliti, serta nyatakan yang belum terjawab.

Ketiga, memberikan materi yang tidak dapat diakses lalu menyalahkan peserta. Kirim materi uji coba sebelum sesi dan sediakan jalur alternatif. Keempat, menganggap persetujuan umum mencakup perekaman, penggunaan kutipan, dan berbagi dengan vendor. Gunakan pilihan persetujuan terpisah dan hormati penolakan.

Kelima, mengubah komentar menjadi persyaratan tanpa bukti tugas. Sertakan langkah, kondisi, dampak, dan kriteria penerimaan. Keenam, menyamakan hasil scanner atau checklist dengan konformansi penuh. WAI Easy Checks berguna sebagai pemeriksaan awal, tetapi tidak menggantikan evaluasi proses dan cakupan yang direncanakan ([WAI Easy Checks](https://www.w3.org/WAI/test-evaluate/preliminary/)).

Sebelum menerima penawaran, ajukan pertanyaan berikut: siapa yang melakukan evaluasi dan dengan alat apa; alur, halaman, dan integrasi mana yang tercakup; bagaimana temuan diberi tingkat risiko dan ditutup; apakah vendor menyediakan build uji, data uji, serta log perubahan; dan siapa yang menandatangani penerimaan. Mintalah bukti yang dapat diperiksa, bukan janji “accessible by design”.

## Kesimpulan

Melibatkan pengguna disabilitas berarti merancang partisipasi yang dapat diakses, mengamati tugas dalam konteks nyata, menjaga persetujuan dan privasi, lalu mengubah temuan menjadi kriteria pengadaan yang dapat diuji. Riset tersebut memperjelas kebutuhan dan risiko; ia tidak mengklaim mewakili semua orang dan tidak menggantikan audit WCAG atau peninjauan profesional.

Langkah berikutnya adalah membuat satu lembar keputusan untuk alur yang akan dibeli: pertanyaan, peserta dan akomodasi, tugas, bukti, kriteria penerimaan, pemilik risiko, serta pemeriksaan teknis yang masih harus dilakukan. Teman Codev.id, tunda penerimaan bila bukti penting belum dapat ditelusuri atau kondisi uji berbeda dari lingkungan target. Aturan kerjanya sederhana: tidak ada klaim aksesibel tanpa pengguna yang dapat ikut, tugas yang dapat diamati, dan bukti yang dapat diperiksa.
