---
article_id: CDV-20-A01
title: "Audit Bukti Kapabilitas Perusahaan Software"
slug: "audit-bukti-kapabilitas-perusahaan-software"
description: "Memeriksa identitas legal, peran tim, ruang lingkup layanan, artefak proses, bukti teknologi, keamanan dan privasi, aksesibilitas, operasi, subkontraktor, asal-usul portfolio, serta dukungan"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2026-07-05"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-20
primary_intent: "Verify a provider's claimed services and delivery capability"
reader_community: "Codev.id"
reader_address: "Sobat Codev.id"
final_route: "/artikel/audit-bukti-kapabilitas-perusahaan-software.html"
technical_review: required
sources:
  - "https://csrc.nist.gov/pubs/sp/800/161/r1/final"
  - "https://www.cisa.gov/securebydesign"
  - "https://www.gov.uk/guidance/the-technology-code-of-practice"
  - "https://www.gov.uk/service-manual/service-standard"
  - "https://csrc.nist.gov/pubs/sp/800/218/final"
  - "https://www.w3.org/TR/WCAG-EM/"
---

# Audit Bukti Kapabilitas Perusahaan Software

Halo, Sobat Codev.id! Memilih perusahaan software tidak cukup dengan melihat logo klien, demo yang mulus, atau daftar teknologi. Audit bukti kapabilitas berarti mencocokkan siapa penyedianya, siapa yang benar-benar mengerjakan, ruang lingkup yang didukung, artefak proses, dan bukti hasil yang bisa diperiksa. Tanpa pencocokan itu, harga awal yang tampak murah dapat berubah menjadi biaya perbaikan, ketergantungan, dan risiko operasional.

Jawaban singkatnya: minta bukti yang spesifik terhadap proyek Anda, bukan klaim umum. Identitas legal, nama dan peran tim, scope tertulis, contoh artefak, cara pengamanan, pengelolaan akses, dukungan setelah rilis, serta hak atas source dan akun harus dapat ditelusuri ke penawaran atau kontrak. Portfolio, screenshot, sertifikasi, maupun testimoni hanya titik awal; masing-masing tidak sendirian membuktikan kepengarangan, kecocokan scope, keamanan, aksesibilitas, atau dampak bisnis. Jika bukti untuk entitas, tim, scope, kepemilikan, persetujuan, vendor, garansi, hasil, serah terima, dan exit belum lengkap, tahan keputusan dan tandai [NEEDS GATE-09: verifikasi penawaran/kontrak serta bukti penyedia belum lengkap].

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

Ilustrasi umum dari aset lokal codev.id; bukan dokumentasi proyek tertentu.

## Definisi dan batas objek

Objek audit adalah kemampuan penyedia untuk mengirimkan layanan tertentu pada konteks tertentu. “Bisa membuat website” terlalu luas untuk menjadi kesimpulan. Ubah menjadi pernyataan yang dapat diuji: misalnya membangun aplikasi web dengan peran pengguna tertentu, integrasi yang disebutkan, lingkungan rilis yang disepakati, dan pemilik keputusan yang jelas.

Audit ini bukan sertifikasi, endorsement, atau pengganti pemeriksaan legal dan kontrak. Ia juga tidak menilai kualitas bisnis klien atau menjanjikan hasil masa depan. Standar dan panduan publik dapat membantu menyusun pertanyaan, tetapi tidak membuktikan bahwa penyedia tertentu mematuhi semuanya. [NEEDS GATE-09: minta peninjauan kontrak dan bukti terkini oleh pihak yang berwenang sebelum menyatakan kompetensi, garansi, atau hasil.]

## Cara kerjanya

Mulailah dari paket bukti yang sama untuk semua kandidat. Simpan versi dan tanggal dokumen agar perbandingan tidak bergeser.

1. **Cocokkan entitas dan orang.** Catat nama legal, alamat, penandatangan, kontak operasional, nama anggota tim, peran, dan porsi kerja. Tanyakan siapa pengganti bila seorang personel tidak tersedia. Logo organisasi atau profil daring tidak menggantikan verifikasi identitas.
2. **Kunci scope dan batas.** Tulis fitur, integrasi, lingkungan, asumsi data, deliverable, acceptance owner, serta hal yang dikecualikan. Minta estimasi ketidakpastian dan mekanisme perubahan. Panduan Technology Code of Practice menekankan pengelolaan risiko, kepemilikan keputusan, dan kemampuan keluar; prinsip ini relevan untuk menormalisasi penawaran, bukan untuk mengesahkan penyedia ([UK Technology Code of Practice](https://www.gov.uk/guidance/the-technology-code-of-practice)).
3. **Periksa jejak kerja.** Contoh discovery, backlog, desain, decision record, test report, changelog, dan runbook menunjukkan cara kerja. Hapus data rahasia dari contoh, lalu minta penjelasan bagian yang dibuat oleh penyedia dan bagian yang berasal dari pihak lain.
4. **Telusuri teknologi dan operasi.** Minta diagram sederhana, daftar dependency dan provider, alur deployment, pengelolaan secret, backup/restore, monitoring, incident path, serta rencana rollback. Jangan menyimpulkan performa atau uptime sebelum ada bukti lingkungan dan periode pengukuran.
5. **Uji pengamanan dan aksesibilitas.** Tanyakan threat model, secure-development practice, hasil pengujian untuk release yang relevan, pemilik remediasi, dan cara pengelolaan data pribadi. NIST SSDF memberi kerangka praktik pengembangan aman, sementara CISA Secure by Design menempatkan keamanan sebagai tanggung jawab pembuat sejak awal ([NIST SSDF](https://csrc.nist.gov/pubs/sp/800/218/final), [CISA Secure by Design](https://www.cisa.gov/securebydesign)). Untuk aksesibilitas, minta scope, metode evaluasi otomatis dan manual, pengguna bantu yang dilibatkan, temuan, serta keputusan perbaikan; WCAG-EM adalah metode evaluasi, bukan bukti bahwa situs tertentu telah sesuai ([W3C WCAG-EM](https://www.w3.org/TR/WCAG-EM/)).
6. **Pastikan serah terima dan exit.** Source code, repository, domain, akun cloud, konfigurasi, dokumentasi, data, dan kredensial harus memiliki pemilik yang disebut. Tetapkan format handover, bantuan transisi, penghapusan akses, serta siapa yang menanggung biaya dan risiko saat hubungan berakhir.

## Faktor yang mengubah hasil

Kredibilitas bukti bersifat kontekstual. Tim yang kuat untuk situs konten belum otomatis tepat untuk sistem pembayaran atau data sensitif. Perbedaan regulasi, volume, integrasi, kebutuhan offline, dan toleransi downtime mengubah bukti yang wajib diminta.

Periksa juga rantai pasok. NIST SP 800-161 Rev.1 mengarahkan organisasi untuk memahami risiko pemasok, peran, dan ketergantungan sepanjang siklus hidup ([NIST SP 800-161 Rev.1](https://csrc.nist.gov/pubs/sp/800/161/r1/final)). Minta daftar subcontractor, fungsi yang mereka pegang, lokasi pemrosesan data, akses yang diberikan, serta klausul penggantian. Kehadiran vendor pihak ketiga bukan otomatis masalah; yang berbahaya adalah akses dan tanggung jawab yang tidak terlihat.

Bukti portfolio juga perlu provenance (asal-usul yang dapat ditelusuri). Tanyakan kapan pekerjaan berlangsung, apa peran penyedia, bagian mana yang dikerjakan pihak lain, siapa yang memberi izin untuk menampilkan nama atau tangkapan layar, dan dokumen apa yang dapat diperiksa. Domain aktif hanya membuktikan sesuatu dapat diakses hari ini, bukan siapa penulisnya atau bagaimana proses pembuatannya.

## Contoh keputusan praktis

Bayangkan dua penawaran untuk portal internal. Penyedia A menunjukkan demo, logo perusahaan besar, dan daftar framework. Penyedia B menunjukkan scope bertanda versi, matriks peran, contoh keputusan arsitektur, rencana uji, daftar dependency, dan prosedur handover, tetapi belum memberikan referensi klien yang dapat dihubungi. Jangan langsung memilih A karena portfolionya atau B karena dokumennya. Beri status “perlu verifikasi” pada keduanya dan minta bukti yang sama.

Gunakan tabel kerja berikut sebelum negosiasi final:

| Area | Bukti minimum yang diminta | Keputusan bila belum ada |
|---|---|---|
| Entitas dan tim | Dokumen identitas, nama peran, penanggung jawab | Jangan menyimpulkan kapasitas personal |
| Scope dan acceptance | Scope versi, deliverable, kriteria terima, pengecualian | Revisi penawaran sebelum banding harga |
| Proses dan artefak | Contoh artefak teranonim dan riwayat perubahan | Minta sesi walkthrough |
| Keamanan, privasi, aksesibilitas | Threat/control evidence, data map, evaluasi aksesibilitas | Libatkan reviewer yang kompeten |
| Operasi dan exit | Runbook, akun, backup, rollback, handover | Tahan rilis atau pembayaran terkait |
| Portfolio dan referensi | Provenance, izin, peran, kontak referensi | Jangan klaim hasil atau kepengarangan |

Sobat Codev.id, skor “lulus” di tabel bukan vonis kualitas. Ia hanya menunjukkan bahwa pertanyaan penting telah memiliki jawaban yang bisa ditinjau. Simpan pertanyaan terbuka sebagai syarat kontrak atau milestone, bukan sebagai asumsi.

## Kesalahan umum dan cara memeriksanya

**Menganggap sertifikasi atau badge sebagai bukti proyek.** Periksa tanggal, cakupan, pemegangnya, dan hubungan dengan tim yang ditawarkan. Badge tidak membuktikan hasil pada scope Anda.

**Menganggap screenshot sebagai hasil terukur.** Minta baseline, lingkungan, periode, metode, dan pemilik data. Tanpa itu, screenshot paling jauh menunjukkan tampilan pada satu waktu.

**Memilih berdasarkan harga terendah.** Bandingkan total lifecycle cost: lisensi, operasi, perubahan, migrasi, dukungan, dan exit. NIST mencatat bahwa risiko pemasok dan ketergantungan perlu dipertimbangkan sepanjang siklus hidup, sehingga harga build saja bukan perbandingan yang lengkap ([NIST SP 800-161 Rev.1](https://csrc.nist.gov/pubs/sp/800/161/r1/final)).

**Menganggap live page membuktikan keamanan dan aksesibilitas.** Mintalah catatan pengujian release, temuan, pengecualian, dan keputusan remediasi. Service Standard Inggris menekankan layanan perlu dipahami dari kebutuhan pengguna dan pengukuran berulang; itu tidak sama dengan klaim bahwa layanan kandidat telah memenuhi standar tersebut ([UK Government Service Standard](https://www.gov.uk/service-manual/service-standard)).

**Melupakan dukungan dan keluar.** Tanyakan jam dukungan, jalur eskalasi, batas tanggung jawab, format data, dan waktu transisi. Jika jawabannya hanya “nanti dibahas”, tandai sebagai risiko komersial dan operasional, bukan detail kecil.

## Jalan pintas yang tampak praktis

Shortcut yang sering dipilih adalah “pakai saja vendor yang paling banyak memajang logo; audit dokumen akan memperlambat.” Kecepatannya semu. Logo dapat berasal dari pekerjaan lama, kontribusi parsial, atau izin yang tidak lagi berlaku. Audit ringan justru mengurangi putaran klarifikasi: satu paket scope, peran, artefak, dan exit membuat setiap kandidat menjawab pertanyaan yang sama.

Kawan Codev.id, bila waktu sangat terbatas, prioritaskan empat hal: identitas penandatangan, scope dan acceptance, orang yang memegang keputusan teknis, serta kepemilikan source dan akun. Tetapkan tanggal untuk melengkapi bukti keamanan, privasi, aksesibilitas, subcontractor, dan dukungan; jangan mengubah kekosongan itu menjadi klaim lulus.

## Kesimpulan

Audit bukti kapabilitas perusahaan software adalah pencocokan terstruktur antara klaim penyedia dan bukti yang spesifik terhadap scope Anda. Periksa entitas, tim, proses, teknologi, keamanan, privasi, aksesibilitas, operasi, rantai subcontractor, provenance portfolio, dukungan, dan exit secara bersama-sama. Bukti yang belum ada harus menjadi syarat keputusan atau [NEEDS GATE-09: review teknis dan kontrak sebelum persetujuan].

Langkah berikutnya: kirimkan paket permintaan bukti di atas kepada setiap kandidat, minta versi bertanggal, lalu lakukan walkthrough dengan pemilik keputusan proyek. Untuk memahami konteks layanan dan jalur kontak sebelum meminta paket itu, Anda dapat mulai dari [beranda Codev.id](/). Simpan keputusan, pengecualian, dan artefak handover di kontrak atau catatan proyek. Aturan operasionalnya sederhana: jangan menyebut penyedia kompeten, aman, sesuai, atau berhasil sebelum klaim itu ditopang bukti terkini dan ditinjau oleh pihak yang berwenang.

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
