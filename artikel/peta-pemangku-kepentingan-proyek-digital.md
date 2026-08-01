---
article_id: CDV-01-A02
writing_contract_version: "native-id-v2"
title: "Peta Pemangku Kepentingan untuk Proyek Digital"
slug: "peta-pemangku-kepentingan-proyek-digital"
description: "Panduan menyusun peta pemangku kepentingan dengan pengaruh, kebutuhan, keputusan, risiko, dan prioritas wawancara."
status: draft
publication_date: "2025-03-28"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-01
primary_intent: "Identify stakeholders, decisions, conflicts, and evidence needs"
reader_community: "Codev.id"
reader_address: "Kawan Codev.id"
final_route: "/artikel/peta-pemangku-kepentingan-proyek-digital.html"
technical_review: required
sources:
  - "https://www.gov.uk/service-manual/agile-delivery"
  - "https://www.w3.org/TR/WCAG22/"
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

# Peta Pemangku Kepentingan untuk Proyek Digital

Halo, Kawan Codev.id! Banyak proyek digital tersendat bukan karena tim tidak punya alat, melainkan karena keputusan penting dibuat oleh orang yang tidak tepat, sementara pengguna yang terdampak tidak pernah didengar. Peta pemangku kepentingan membantu Anda melihat siapa yang memengaruhi proyek, siapa yang menanggung akibatnya, dan bukti apa yang harus dikumpulkan sebelum memilih arah.

Jawaban singkatnya: buat satu daftar yang dapat ditelusuri dari setiap kelompok terkait ke kebutuhan, keputusan yang mereka pegang, tingkat pengaruh, risiko bila diabaikan, serta prioritas wawancara. Perbarui peta itu ketika asumsi berubah. Peta ini belum membuktikan bahwa kebutuhan pasar benar atau menetapkan pembagian tugas delivery; ia adalah alat discovery untuk mengarahkan percakapan dan menguji bukti. **[NEEDS GATE-01: validasi pemangku kepentingan, kebutuhan, dan pemilik keputusan pada proyek nyata.]**

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

*Aset lokal proyek; bukan dokumentasi proyek tertentu.*

## Jawaban singkat dan salah paham utama

Mulailah dari keputusan yang harus dibuat, bukan dari jabatan di bagan organisasi. Untuk tiap keputusan, tulis siapa yang mengusulkan, memberi persetujuan, menjalankan, memakai hasil, mengawasi risiko, dan dapat memblokirnya. Lalu tanyakan kebutuhan apa yang mereka bawa dan bukti apa yang akan membuat mereka mengubah pendapat.

Kesalahan umum adalah menganggap pemangku kepentingan hanya sponsor dan pengguna akhir. Operator layanan, admin data, keamanan, aksesibilitas, legal, dukungan pelanggan, vendor, bahkan kelompok yang tidak dapat menggunakan kanal tertentu dapat mengubah desain. Panduan agile pemerintah Inggris menempatkan riset kebutuhan, pengujian, dan penyampaian bertahap sebagai aktivitas berulang, sehingga daftar orang yang perlu diajak bicara juga harus mengikuti pembelajaran proyek ([UK Government Service Manual—agile delivery](https://www.gov.uk/service-manual/agile-delivery)).

## Definisi dan batas objek

Peta pemangku kepentingan adalah artefak kerja: tabel atau visualisasi yang menghubungkan aktor dengan kepentingan, pengaruh, keputusan, ketergantungan, risiko, dan rencana keterlibatan. “Pengaruh” bukan ukuran senioritas saja. Seorang petugas loket mungkin punya pengaruh tinggi terhadap kelancaran proses karena ia menguasai pengecualian yang tidak tercatat. “Kebutuhan” juga bukan daftar fitur; itu dapat berupa batas waktu, privasi, kemampuan akses, atau bukti audit.

Batasnya perlu tegas. Peta ini tidak menggantikan user journey, spesifikasi kebutuhan, atau matriks RACI untuk akuntabilitas delivery. Ia hanya menjawab: siapa yang harus dipahami atau dilibatkan agar keputusan discovery tidak berdiri di atas asumsi. Setiap entri sebaiknya memiliki sumber: wawancara, observasi, dokumen kebijakan, data layanan, atau catatan keputusan. Tanpa sumber, tandai sebagai asumsi dan jadwalkan verifikasi.

## Cara kerjanya

Gunakan urutan berikut agar peta menghasilkan tindakan, bukan sekadar daftar nama.

1. **Tetapkan keputusan dan batas proses.** Tulis keputusan terdekat, misalnya memilih alur pengajuan atau menentukan data yang wajib dikumpulkan. Nyatakan kanal, periode, dan proses yang termasuk; hal di luar batas menjadi catatan, bukan alasan memperluas proyek tanpa persetujuan.
2. **Temukan kelompok, lalu nama peran.** Mulai dari pengguna langsung, pemilik layanan, pelaksana operasi, pemilik data, teknis, keamanan, aksesibilitas, kepatuhan, dan pihak eksternal. Hindari nama individu sebagai satu-satunya representasi; satu orang dapat pindah, sedangkan tanggung jawab peran tetap perlu diwakili.
3. **Catat kepentingan dan keputusan.** Tanyakan: hasil apa yang dianggap berhasil, keputusan apa yang dapat mereka setujui atau tolak, dan kendala apa yang tidak boleh dilanggar? Pisahkan pernyataan faktual dari dugaan tim.
4. **Nilai pengaruh dan dampak.** Pakai skala sederhana (rendah, sedang, tinggi) dengan alasan tertulis. Pengaruh tinggi berarti mampu mengubah prioritas, akses, anggaran, atau penerimaan; dampak tinggi berarti hasil buruk langsung membebani pekerjaan atau hak kelompok tersebut.
5. **Hubungkan risiko dan bukti.** Untuk risiko “proses manual tidak terwakili”, bukti yang dibutuhkan bisa observasi dan contoh pengecualian. Untuk risiko aksesibilitas, uji dengan pengguna dan kriteria yang dapat diperiksa; WCAG 2.2 menyediakan rujukan teknis untuk menilai aksesibilitas, tetapi tidak membuktikan bahwa produk Anda sudah sesuai ([W3C WCAG 2.2](https://www.w3.org/TR/WCAG22/)).
6. **Tetapkan prioritas wawancara.** Dahulukan kombinasi pengaruh tinggi, dampak tinggi, dan ketidakpastian tinggi. Sertakan suara dengan pengaruh rendah namun dampak tinggi agar keputusan tidak hanya mengikuti pemegang otoritas.
7. **Jadikan peta hidup.** Setelah setiap wawancara atau uji, perbarui status bukti, konflik, dan pertanyaan terbuka. Simpan tanggal dan pemilik tindak lanjut sehingga perubahan dapat dilacak.

Format minimum yang cukup praktis:

| Peran/kelompok | Kebutuhan atau kekhawatiran | Keputusan/pengaruh | Dampak bila terlewat | Bukti yang ada | Prioritas & langkah |
|---|---|---|---|---|---|
| Operator layanan | Pengecualian proses tercatat | Tinggi pada kelancaran operasi | Gangguan dan pekerjaan ulang | Observasi belum dilakukan | Tinggi; jadwalkan observasi |
| Pengguna dengan kebutuhan akses | Tugas dapat diselesaikan mandiri | Sedang pada penerimaan, tinggi pada dampak | Akses layanan terhambat | Satu laporan dukungan | Tinggi; uji tugas dengan beberapa pengguna |

Contoh di atas adalah format ilustratif, bukan fakta proyek tertentu.

## Faktor yang mengubah hasil

Peta berubah ketika konteks berubah. Regulasi baru dapat menaikkan pengaruh pemilik kepatuhan. Integrasi dengan sistem lama dapat membuat admin data menjadi penjaga keputusan teknis. Layanan publik dengan pengguna yang beragam memerlukan sampel wawancara yang tidak hanya mencerminkan pengguna paling mudah dijangkau. Pergantian vendor atau sponsor juga mengubah jalur persetujuan.

Perhatikan empat sinyal: **konflik tujuan** (kecepatan peluncuran berbenturan dengan verifikasi keamanan), **ketergantungan tersembunyi** (tim yang mengendalikan akses data), **ketidakpastian bukti** (klaim “semua orang membutuhkan ini”), dan **kualitas representasi** (satu narasumber tidak mewakili seluruh kelompok). Jika salah satu muncul, naikkan prioritas verifikasi, bukan tingkat keyakinan.

## Contoh keputusan praktis

Bayangkan tim hendak memilih antara mempertahankan formulir lama atau membuat alur digital baru. Peta menunjukkan sponsor berpengaruh tinggi, operator berpengaruh tinggi terhadap operasi, pengguna akhir berdampak tinggi, dan tim keamanan berhak menolak risiko tertentu. Keputusan belum boleh diambil hanya dari preferensi sponsor.

| Pertanyaan | Jika jawabannya “ya” | Tindakan berikutnya |
|---|---|---|
| Apakah operator menemukan pengecualian yang tidak ada di formulir? | Risiko pekerjaan ulang tinggi | Observasi beberapa kasus dan dokumentasikan pengecualian |
| Apakah data sensitif melewati kanal baru? | Risiko keamanan/kepatuhan meningkat | Minta penilaian pemilik keamanan dan kebijakan yang berlaku |
| Apakah pengguna dengan kebutuhan akses dapat menyelesaikan tugas? | Bukti penerimaan belum cukup | Uji tugas dan catat hambatan |
| Apakah keputusan memiliki pemilik yang jelas? | Tidak ada otorisasi yang dapat ditelusuri | Tunda keputusan sampai pemilik ditunjuk |

Sobat Codev.id, bedakan “belum diwawancarai” dari “tidak berkepentingan”. Kategori pertama adalah kekosongan bukti; kategori kedua adalah kesimpulan yang memerlukan alasan.

## Kesalahan umum dan cara memeriksanya

**Hanya memetakan struktur organisasi.** Periksa siapa yang memegang pengetahuan kerja, akses, atau hak veto; tambahkan peran yang tidak tampak di bagan.

**Mengurutkan orang berdasarkan jabatan.** Minta alasan di balik setiap skor pengaruh dan dampak. Bila alasan tidak menyebut keputusan atau konsekuensi, skor itu belum berguna.

**Mengubah peta menjadi daftar undangan rapat.** Setiap pertemuan harus punya pertanyaan, bukti yang dicari, dan keputusan yang mungkin berubah.

**Menganggap satu wawancara sebagai validasi.** Catat variasi sumber dan tandai pola yang belum konsisten. Pembelajaran berulang diperlukan; satu sesi tidak menutup kebutuhan riset ([UK Government Service Manual—agile delivery](https://www.gov.uk/service-manual/agile-delivery)).

**Memakai label “sesuai WCAG” sebagai klaim selesai.** Gunakan WCAG 2.2 untuk kriteria yang dapat diuji, lalu dokumentasikan hasil pengujian yang benar-benar dilakukan. Standar tidak menggantikan pengujian dengan pengguna ([W3C WCAG 2.2](https://www.w3.org/TR/WCAG22/)).

Sebelum rapat keputusan, periksa: apakah setiap keputusan punya pemilik, setiap risiko punya bukti atau pertanyaan, kelompok berdampak tinggi masuk daftar wawancara, dan konflik dicatat tanpa dipoles menjadi konsensus palsu?

## Kesimpulan

Peta pemangku kepentingan proyek digital yang berguna menghubungkan kelompok terkait dengan kebutuhan, pengaruh, keputusan, risiko, bukti, dan urutan wawancara. Buat versi pertama dari keputusan terdekat, uji melalui percakapan dan observasi, lalu perbarui ketika bukti berubah. Teman Codev.id, minta pemilik proyek mengesahkan batas proses dan daftar keputusan, kemudian jadwalkan wawancara untuk entri berdampak serta berketidakpastian tinggi. Jika perlu menyelaraskan langkah discovery berikutnya, gunakan [beranda Codev.id](/) sebagai titik masuk resmi.

Jangan menyatakan kebutuhan sudah tervalidasi atau akuntabilitas delivery sudah jelas hanya karena petanya rapi. Tandai kekosongan bukti, libatkan peninjau berwenang, dan tunda keputusan yang belum memiliki pemilik serta dasar yang dapat diperiksa.
