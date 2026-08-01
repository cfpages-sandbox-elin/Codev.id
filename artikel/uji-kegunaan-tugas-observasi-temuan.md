---
article_id: CDV-02-A04
title: "Uji Kegunaan: Tugas, Observasi, dan Temuan"
slug: "uji-kegunaan-tugas-observasi-temuan"
description: "Define participants, realistic tasks, facilitation, observation, severity, evidence, and retest"
writing_contract_version: "native-id-v2"
status: draft
publication_date: "2025-04-26"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-02
primary_intent: "Plan and interpret a usability test"
reader_community: "Codev.id"
reader_address: "Kawan Codev.id"
final_route: "/artikel/uji-kegunaan-tugas-observasi-temuan.html"
technical_review: required
sources:
  - "https://www.gov.uk/service-manual/agile-delivery"
  - "https://www.w3.org/TR/WCAG22/"
  - "https://www.w3.org/TR/WCAG-EM/"
  - "https://www.w3.org/WAI/test-evaluate/preliminary/"
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

# Uji Kegunaan: Tugas, Observasi, dan Temuan

Halo, Kawan Codev.id!

Jika tim hanya bertanya, “Apakah desain ini sudah bagus?”, uji kegunaan akan berjalan tanpa arah. Uji yang berguna dimulai dari tugas nyata yang hendak diselesaikan peserta, diamati saat mereka mencobanya, lalu diterjemahkan menjadi temuan yang bisa diprioritaskan dan diuji ulang. Jadi, hasilnya bukan sekadar daftar komentar, melainkan bukti tentang bagian alur yang membantu atau menghambat tujuan pengguna.

Jawaban singkatnya: tetapkan profil peserta yang relevan, susun skenario tugas dengan hasil yang dapat diamati, fasilitasi tanpa membocorkan jawaban, catat perilaku dan ucapan secara terpisah, lalu nilai dampak serta buktinya sebelum mengubah rancangan. [NEEDS PROJECT RESEARCH AND DECISION OWNER] Uji ini tetap membutuhkan riset proyek dan pemilik keputusan agar temuan tidak dianggap mewakili seluruh pasar atau semua pengguna.

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

Gambar ini merupakan aset lokal untuk ilustrasi dan bukan dokumentasi proyek tertentu.

## Jawaban singkat dan salah paham utama

Uji kegunaan bukan demo yang dipresentasikan kepada calon pengguna, bukan pula polling apakah mereka menyukai warna. Peserta menerima konteks dan tugas yang masuk akal, kemudian tim melihat apa yang mereka lakukan, titik ragu, kesalahan, pertanyaan, dan hasil akhirnya. Fasilitator boleh memberi konteks atau pertanyaan netral, tetapi tidak mengarahkan langkah.

Salah paham yang paling mahal adalah menganggap satu sesi yang lancar sebagai bukti bahwa produk telah selesai. Satu sesi hanya memberi potongan bukti pada tugas, peserta, perangkat, dan versi yang diuji. Dalam praktik pengiriman bertahap, asumsi, perjalanan pengguna, perilaku fungsional, kualitas, batasan, dan bukti penerimaan menjawab pertanyaan yang berbeda. Karena itu, catat temuan bersama konteksnya dan cocokkan dengan keputusan produk; panduan pengiriman tangkas pemerintah Inggris juga menekankan pembelajaran dan pengujian dalam siklus bertahap ([UK Government Service Manual](https://www.gov.uk/service-manual/agile-delivery)).

Pertanyaan pembuka yang lebih tajam adalah: “Tugas apa yang harus berhasil dilakukan orang ini, dan bukti apa yang membuat kita percaya ia berhasil tanpa bantuan?”

## Definisi dan batas objek

Objek uji adalah interaksi pada alur tertentu: misalnya mencari informasi, mengisi formulir, memilih opsi, atau menyelesaikan langkah konfirmasi. “Berhasil” harus didefinisikan dengan perilaku atau keluaran yang dapat dilihat, bukan dengan kesan fasilitator. Peserta tidak harus pengguna lama; yang penting kriteria pemilihannya sesuai asumsi yang hendak diuji.

Batasnya penting. Uji kegunaan dapat menunjukkan bahwa label membingungkan, fokus keyboard tidak terlihat, pesan kesalahan tidak membantu, atau urutan langkah membuat orang tersesat. Namun, temuan itu tidak otomatis menjadi sertifikasi kesesuaian WCAG, kepatuhan hukum Indonesia, atau bukti permintaan pasar. WCAG 2.2 mencakup kebutuhan akses yang luas, sedangkan evaluasi seperti WCAG-EM meminta penentuan cakupan halaman dan proses; satu pengamatan singkat tidak menggantikan evaluasi tersebut ([WCAG 2.2](https://www.w3.org/TR/WCAG22/), [WCAG-EM 1.0](https://www.w3.org/TR/WCAG-EM/)).

Dengan batas ini, tim dapat menyebut hasil secara jujur: “Pada tugas pendaftaran dengan prototipe versi X, tiga peserta berhenti di langkah verifikasi,” bukan “produk ini mudah digunakan semua orang.”

## Cara kerjanya

Mulai dari tujuan keputusan. Apakah tim hendak memilih antara dua struktur halaman, memeriksa formulir sebelum pengembangan, atau mencari regresi setelah perubahan? Tujuan menentukan peserta, tugas, dan bukti yang perlu dicatat.

Berikut urutan yang dapat dipakai:

1. **Tetapkan peserta dan konteks.** Tulis ciri yang relevan dengan tugas, pengalaman yang diasumsikan, perangkat, serta kebutuhan akses yang ingin dipertimbangkan. Jangan menyamakan “teman satu kantor” dengan representasi pengguna.
2. **Tulis skenario tugas.** Beri tujuan dan konteks secukupnya, tanpa menyebut tombol atau menu yang ingin diuji. Definisikan kondisi berhasil, kondisi berhenti, dan data uji yang aman.
3. **Siapkan fasilitasi.** Buka dengan penjelasan bahwa yang diuji adalah rancangan, bukan kemampuan peserta. Minta peserta berpikir keras bila sesuai protokol, dan gunakan pertanyaan netral seperti “Apa yang Anda harapkan terjadi?”
4. **Observasi terstruktur.** Tandai waktu, langkah yang diambil, kesalahan, permintaan bantuan, kutipan penting, dan hasil. Bedakan fakta yang terlihat dari tafsir seperti “peserta tidak paham.”
5. **Debrief dan sintesis.** Gabungkan catatan per tugas, bukan per opini. Cari pola, tetapi simpan kasus tunggal jika dampaknya besar atau menyentuh akses.
6. **Nilai dan prioritaskan.** Untuk setiap temuan, tulis tugas terdampak, bukti, dampak terhadap tujuan, frekuensi dalam sesi, serta keyakinan pengamat. Keparahan adalah keputusan produk berdasarkan dampak dan risiko, bukan angka yang tampak ilmiah.
7. **Perbaiki lalu uji ulang.** Nyatakan perubahan yang dilakukan, versi yang diuji, dan kriteria lulus. Retest boleh terfokus pada temuan yang diperbaiki, tetapi jangan menghapus regresi pada tugas lain.

Kawan Codev.id, format catatan sederhana sering lebih berguna daripada rekaman panjang tanpa indeks: `tugas | perilaku terlihat | ucapan | dampak | bukti | hipotesis | keputusan berikutnya`.

## Faktor yang mengubah hasil

**Peserta dan konteks.** Pengalaman, bahasa, perangkat, koneksi, dan kebutuhan akses memengaruhi strategi. Jelaskan siapa yang tidak tercakup sehingga temuan tidak diperluas secara sembarangan.

**Tugas dan prototipe.** Tugas yang terlalu rinci menguji kemampuan mengikuti instruksi, bukan kemampuan menemukan jalan. Prototipe yang belum memiliki respons penting dapat menghasilkan kegagalan palsu; tandai keterbatasan itu di catatan.

**Fasilitator dan lingkungan.** Nada menghakimi, terlalu cepat memberi petunjuk, atau kehadiran pemangku kepentingan dapat mengubah perilaku. Uji jarak jauh menambah faktor audio, berbagi layar, dan gangguan rumah; uji langsung menambah efek ruangan dan perangkat.

**Cara mengamati.** Waktu penyelesaian saja tidak menjelaskan sebab. Perhatikan jalur alternatif, fokus, urutan, pemahaman pesan, serta apakah peserta dapat pulih dari kesalahan. Easy Checks WAI berguna sebagai pemeriksaan awal, tetapi bukan pengganti evaluasi menyeluruh ([WAI Easy Checks](https://www.w3.org/WAI/test-evaluate/preliminary/)).

**Bukti dan interpretasi.** Rekaman, catatan, dan artefak versi harus dapat ditelusuri ke tugas. Pisahkan “peserta memilih menu A” dari “menu A membingungkan”; yang kedua adalah hipotesis yang perlu diuji atau didukung catatan lain.

## Contoh keputusan praktis

Bayangkan tim menguji alur pengajuan layanan. Tugasnya: “Anda perlu mengirim permohonan untuk kebutuhan minggu depan. Tunjukkan bagaimana Anda akan menyelesaikannya.” Kriteria berhasil: peserta menemukan formulir, mengisi bidang wajib dengan data uji, memahami pesan kesalahan, dan mencapai konfirmasi tanpa petunjuk.

| Temuan | Bukti yang dicatat | Keputusan awal |
|---|---|---|
| Peserta tidak menemukan formulir | Dua peserta mencari di menu berbeda dan bertanya “mulai dari mana?” | Uji dua label/navigasi pada sesi berikutnya; jangan langsung menyimpulkan seluruh struktur gagal. |
| Pesan kesalahan tidak menjelaskan perbaikan | Peserta melihat pesan, mengulang nilai yang sama, lalu meminta bantuan | Prioritaskan penulisan ulang dan uji keyboard/fokus pada bidang terkait. |
| Peserta memakai jalur berbeda tetapi tetap selesai | Jalur, hasil, dan waktu dicatat; tidak ada kesalahan | Pertahankan bila memenuhi tujuan dan tidak menambah risiko; jangan memaksa satu jalur hanya demi keseragaman. |

Jika perubahan dilakukan, buat retest dengan versi dan kriteria yang disebutkan. Bila keputusan menyentuh aksesibilitas, tandai kebutuhan evaluasi tambahan: [NEEDS ACCESSIBILITY EVALUATION BEYOND USABILITY SESSION].

## Kesalahan umum dan cara memeriksanya

- **Tugas berbentuk instruksi tombol.** Periksa apakah skenario masih dapat dipahami tanpa nama menu. Jika tidak, tulis ulang berdasarkan tujuan pengguna.
- **Fasilitator menyelamatkan peserta.** Hitung setiap petunjuk dan catat kapan diberikan. Ulangi tugas atau sesi dengan skrip netral bila bantuan mengubah hasil.
- **Menggabungkan observasi dan opini.** Sorot kalimat faktual dalam catatan; ubah opini menjadi hipotesis yang dapat diperiksa.
- **Keparahan tanpa dampak.** Tanyakan tujuan apa yang terhambat, siapa yang terdampak, dan apa konsekuensinya. Gunakan prioritas yang dapat dipertanggungjawabkan, bukan sekadar “terasa kritis.”
- **Menganggap lolos satu peserta sebagai selesai.** Periksa cakupan peserta, tugas, versi, dan kondisi. Temuan yang belum diuji ulang tetap terbuka.

Checklist sebelum menutup putaran: setiap temuan punya tugas dan bukti; asumsi peserta tertulis; bantuan fasilitator tercatat; versi artefak jelas; pemilik keputusan ditunjuk; dan klaim aksesibilitas tidak melampaui metode yang dipakai.

## Pilihan cepat yang tampak praktis

Shortcut yang menggoda adalah meminta lima orang mencoba semua fitur sekaligus, lalu memilih komentar yang paling keras. Cara ini gagal karena tugas menjadi kabur, beban sesi terlalu besar, dan prioritas tercampur dengan preferensi. Alternatif yang lebih aman adalah membatasi satu putaran pada beberapa keputusan berisiko, menulis kriteria berhasil, serta menghubungkan setiap temuan ke bukti dan tindak lanjut.

Sobat Codev.id, bila waktu hanya cukup untuk satu sesi, pilih tugas yang paling menentukan keputusan rilis dan dokumentasikan apa yang belum teruji. Kejujuran tentang celah bukti lebih berguna daripada label “sudah tervalidasi.”

## Kesimpulan

Uji kegunaan yang dapat ditindaklanjuti menyatukan peserta yang relevan, tugas realistis, fasilitasi netral, observasi terstruktur, penilaian dampak, dan retest pada versi yang berubah. Hasilnya menjawab apakah alur tertentu membantu peserta mencapai tujuan dalam kondisi uji tertentu—bukan apakah seluruh pasar akan membeli atau produk otomatis memenuhi WCAG.

Langkah berikutnya: pilih satu alur berisiko, tulis dua atau tiga tugas dengan kriteria berhasil, tetapkan siapa yang mengamati dan memutuskan, lalu jadwalkan retest untuk temuan prioritas. Untuk konteks produk dan keputusan desain berikutnya, Anda dapat mulai dari [beranda Codev.id](/).

Teman Codev.id, operating rule-nya sederhana: jangan ubah opini menjadi persyaratan sebelum ada perilaku yang diamati, bukti yang tersimpan, dan batas cakupan yang dinyatakan.
