---
article_id: CDV-10-A06
title: "Bug Triage: Severity, Priority, Root Cause, dan Retest"
slug: "bug-triage-severity-priority-retest"
description: "Capture reproducibility, environment, impact, evidence, severity, priority, workaround, owner, fix, regression risk, retest, and root-cause follow-up"
status: draft
publication_date: "2025-11-25"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-10
primary_intent: "Turn defects into consistent decisions and learning"
reader_community: "Codev.id"
reader_address: "Sobat Codev.id"
final_route: "/artikel/bug-triage-severity-priority-retest.html"
technical_review: required
writing_contract_version: "native-id-v2"
sources:
  - "https://csrc.nist.gov/pubs/sp/800/218/final"
  - "https://spec.openapis.org/oas/v3.1.1.html"
  - "https://www.w3.org/TR/WCAG-EM/"
  - "https://www.w3.org/WAI/test-evaluate/preliminary/"
  - "https://web.dev/articles/vitals"
  - "https://developer.chrome.com/docs/crux"
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

# Bug Triage: Severity, Priority, Root Cause, dan Retest

Halo, Sobat Codev.id! Bug triage bukan lomba memberi label “critical”. Tujuannya adalah membuat keputusan yang dapat ditelusuri: apa yang terjadi, seberapa besar dampaknya, kapan harus ditangani, siapa pemiliknya, lalu bukti apa yang menutup pekerjaan. **Severity** menggambarkan dampak teknis atau pengguna; **priority** menggambarkan urutan kerja berdasarkan risiko, nilai, dan kapasitas saat ini. Keduanya boleh berbeda.

Catatan yang baik memisahkan gejala dari dugaan akar masalah. Ia menyimpan langkah reproduksi, lingkungan, bukti, workaround, owner, perbaikan, risiko regresi, dan hasil retest. Keputusan rilis tetap membutuhkan kriteria produk dan persetujuan yang berlaku. Lulus satu skrip otomatis hanya membuktikan asersi, data, build, dan lingkungan yang dicakup skrip itu—bukan seluruh sistem. Prinsip pengembangan aman NIST juga menekankan penelusuran risiko, kebutuhan, dan hasil verifikasi ([NIST SSDF](https://csrc.nist.gov/pubs/sp/800/218/final)).

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

Gambar ini merupakan aset lokal untuk ilustrasi dan bukan dokumentasi proyek tertentu.

## Definisi dan batas objek

Satu tiket bug sebaiknya menjawab empat pertanyaan berbeda. **Severity**: apa yang rusak—misalnya transaksi gagal, data salah, atau hanya tata letak bergeser? **Priority**: kapan tim perlu mengerjakannya dibanding pekerjaan lain? **Root cause**: perubahan atau kondisi apa yang memungkinkan kegagalan? **Retest**: bukti bahwa perilaku yang diperbaiki sudah diuji ulang, termasuk jalur yang berisiko ikut rusak.

Jangan memakai priority untuk menghapus kewajiban insiden keamanan atau privasi. Bila ada indikasi akses tidak sah, kebocoran, atau kewajiban pelaporan, jalur insiden yang sesuai harus berjalan; triage produk tidak menggantikannya. Demikian pula, halaman ini bukan penetapan kepatuhan hukum. Evaluasi aksesibilitas perlu melihat cakupan halaman dan proses, bukan mengandalkan satu pemindai ([WCAG-EM](https://www.w3.org/TR/WCAG-EM/)).

## Cara kerjanya

Mulai dari laporan mentah, lalu ubah menjadi rekaman yang bisa diuji. Urutan berikut membantu tim berhenti berdebat berdasarkan intuisi:

1. **Tangkap fakta.** Tulis waktu, akun/peran, URL atau endpoint, build/versi, perangkat dan browser, konfigurasi, data uji, serta langkah minimal untuk mengulanginya. Sertakan hasil aktual dan hasil yang diharapkan.
2. **Simpan bukti.** Lampirkan screenshot atau video seperlunya, log dengan rahasia disamarkan, request/response, dan tautan ke observabilitas. Format API yang terdokumentasi membantu menyamakan nama parameter dan respons; spesifikasi OpenAPI menjelaskan kontrak tersebut ([OpenAPI 3.1.1](https://spec.openapis.org/oas/v3.1.1.html)).
3. **Nilai dampak (severity).** Tanyakan siapa yang terhalang, apakah data hilang atau salah, seberapa luas permukaan terdampak, dan apakah ada workaround yang aman. Gunakan skala internal yang dijelaskan; jangan menyamakan “sering terlihat” dengan “dampaknya paling berat”.
4. **Tentukan urutan (priority).** Gabungkan severity dengan tenggat, jumlah pengguna, risiko bisnis, dependensi rilis, dan biaya menunggu. Tuliskan alasan serta tanggal peninjauan ulang. Priority dapat berubah tanpa mengubah severity.
5. **Tetapkan owner dan rencana.** Satu orang bertanggung jawab mengoordinasikan diagnosis, sementara reviewer atau spesialis (misalnya keamanan dan aksesibilitas) dilibatkan saat sinyalnya muncul. Jika belum bisa diperbaiki, catat workaround dan komunikasi yang harus dilakukan.
6. **Klasifikasikan akar masalah.** Setelah perbaikan ditemukan, bedakan pemicu langsung (contoh: validasi hilang) dari kondisi sistemik (kontrak tidak jelas, test data tidak representatif, atau review terlewat). Gunakan bahasa “terkonfirmasi oleh bukti” atau “masih hipotesis”.
7. **Retest dan regresi.** Ulangi langkah awal pada build perbaikan, lalu uji variasi input, peran, browser, dan integrasi yang terdampak. Catat build, data, hasil, dan bukti. Status “fixed” tanpa hasil retest berarti pekerjaan belum tertutup.

Untuk menyusun bukti pengujian yang konsisten, gunakan [materi e-learning pengujian perangkat lunak](/website/e-learning) sebagai langkah belajar berikutnya. Bila perlu menyelaraskan istilah dengan konteks layanan, kembali ke beranda Codev.id.

## Faktor yang mengubah hasil

Severity dipengaruhi konteks penggunaan. Kegagalan checkout pada jam sibuk, formulir yang tidak dapat dipakai keyboard, dan teks salah pada halaman informasi dapat memiliki dampak berbeda meski sama-sama tampak sebagai error. Untuk aksesibilitas, keyboard/fokus, semantik, formulir dan pesan kesalahan, reflow/zoom, autentikasi, media, serta teknologi bantu membutuhkan pemeriksaan yang sesuai—bukan satu alat otomatis ([WAI Easy Checks](https://www.w3.org/WAI/test-evaluate/preliminary/)).

Lingkungan juga menentukan reproduksi. “Tidak terjadi di laptop saya” belum menjawab apakah build, flag, cache, data, jaringan, atau akun berbeda. Ukur dan catat kondisi itu. Untuk metrik performa, bedakan pengujian lab dari data lapangan; metrik Core Web Vitals dan sumber data Chrome UX Report memiliki konteks serta sampel sendiri ([web.dev Core Web Vitals](https://web.dev/articles/vitals), [Chrome UX Report](https://developer.chrome.com/docs/crux)). Jangan mengubah satu angka sebelum/sesudah menjadi janji ranking, waktu muat, atau konversi.

## Contoh keputusan praktis

Bayangkan pengguna tidak dapat mengirim formulir hanya ketika sesi kedaluwarsa. Severity dapat tinggi karena alur utama buntu, sedangkan priority bergantung pada proporsi pengguna dan jadwal rilis. Tiket harus menyertakan akun uji, durasi sesi, respons jaringan, dan build. Workaround—misalnya masuk ulang—ditulis sebagai mitigasi sementara, bukan bukti bug selesai.

Contoh lain: tombol terlihat benar tetapi tidak mendapat fokus keyboard. Severity ditetapkan berdasarkan tugas yang terhalang dan cakupan komponen; priority naik bila komponen dipakai di banyak alur atau rilis akan segera dipublikasikan. Retest mencakup keyboard saja, pembaca layar yang relevan, zoom, dan halaman lain yang memakai komponen. Kawan Codev.id, keputusan seperti ini lebih kuat bila setiap alasan tercatat daripada sekadar angka P1/P2.

| Pertanyaan | Bukti minimum | Keputusan |
|---|---|---|
| Bisa diulang? | Langkah, build, lingkungan, data | Lanjut diagnosis atau minta informasi |
| Seberapa parah? | Pengguna, fungsi, data, cakupan | Severity dan batas dampak |
| Kapan dikerjakan? | Risiko menunggu, tenggat, dependensi | Priority dan tanggal review |
| Sudah benar? | Build perbaikan, hasil retest, regresi | Tutup, kembalikan, atau eskalasi |

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah memberi label sebelum mengumpulkan fakta. Periksa apakah tiket memuat langkah minimal dan hasil aktual. Kedua, menyamakan priority dengan severity. Minta alasan urutan kerja yang menyebut risiko menunggu dan kapasitas. Ketiga, menulis root cause sebagai tebakan. Tandai hipotesis dan ubah hanya setelah log, diff, atau eksperimen mendukungnya.

Keempat, menganggap “sudah diperbaiki” sebagai “sudah diverifikasi”. Pastikan retest memakai build yang benar dan menyentuh jalur regresi. Kelima, menghapus bukti sensitif ke kolom publik. Samarkan token, data pribadi, dan rahasia; simpan akses terbatas sesuai kebijakan. Terakhir, memakai satu scanner untuk menyatakan aksesibilitas atau satu angka performa untuk menyatakan dampak bisnis. Cocokkan metode, cakupan, dan batas kesimpulannya.

## Jalan pintas yang tampak cepat

Shortcut yang sering dipilih adalah membuat semua laporan “P1” agar cepat masuk antrean. Cara ini gagal karena antrean kehilangan sinyal: insiden nyata bersaing dengan gangguan kecil, sementara alasan dan pemilik tidak jelas. Alternatif yang lebih aman adalah skala severity dan priority yang didefinisikan tim, bukti minimum wajib, owner tunggal, serta tanggal peninjauan ulang. Jika bukti belum cukup, minta data yang spesifik—jangan menebak.

## Penutup

Bug triage yang sehat memisahkan dampak (severity), urutan (priority), penjelasan yang terbukti (root cause), dan verifikasi (retest). Sebelum menutup tiket, minta rekaman reproduksi, lingkungan, bukti, owner, build perbaikan, hasil retest, dan risiko regresi; tandai isu keamanan/privasi untuk jalur insiden yang tepat. Teman Codev.id, jadikan tiket sebagai jejak keputusan dan pembelajaran, bukan tempat menyimpan label. Bila cakupan atau bukti belum memadai, hentikan klaim, minta tinjauan teknis yang relevan, lalu jadwalkan pemeriksaan ulang.
