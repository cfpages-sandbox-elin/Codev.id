---
article_id: CDV-01-A03
writing_contract_version: "native-id-v2"
title: "Memetakan Proses Saat Ini dan User Journey"
slug: "proses-saat-ini-dan-user-journey"
description: "Panduan memetakan perjalanan kerja saat ini dengan pelaku, langkah, data, penundaan, kegagalan, jalan pemulihan, dan ukuran awal yang dapat diuji"
status: draft
publication_date: "2025-03-31"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-01
primary_intent: "Map present tasks, handoffs, pain points, and evidence"
reader_community: "Codev.id"
reader_address: "Kawan Codev.id"
final_route: "/artikel/proses-saat-ini-dan-user-journey.html"
technical_review: required
sources:
  - "https://www.gov.uk/service-manual/agile-delivery"
  - "https://www.w3.org/TR/WCAG22/"
---

# Memetakan Proses Saat Ini dan User Journey

Halo, Kawan Codev.id! Saat tim hendak mengganti alur manual dengan software, godaan terbesarnya adalah langsung menggambar layar baru. Padahal keputusan yang lebih aman dimulai dari peta proses saat ini (*as-is*) dan *user journey*: siapa melakukan apa, memakai data apa, menunggu di mana, gagal dengan cara apa, lalu mencari jalan keluar bagaimana.

Jawaban singkatnya: petakan perjalanan nyata sebelum merancang solusi. Ikuti satu pekerjaan dari pemicu sampai selesai, wawancarai pelaku di setiap perpindahan, dan catat bukti yang dapat dihitung. Hasilnya bukan sekadar diagram rapi, melainkan baseline yang menunjukkan bagian mana yang layak diperbaiki dan bagian mana yang masih perlu penelitian. Jika belum ada observasi, log, atau keputusan pemilik proses, tandai sebagai `[NEEDS PROJECT EVIDENCE]`; jangan mengisi celah dengan asumsi.

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

*Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.*

## Definisi dan batas objek

Proses saat ini adalah urutan kerja yang benar-benar berlangsung hari ini, termasuk langkah tidak resmi seperti menyalin data ke spreadsheet, mengirim pesan pengingat, atau meminta persetujuan lewat telepon. *User journey* memperluas pandangan itu dari perspektif pelaku: tujuan, konteks, pertanyaan, hambatan, dan hasil di setiap tahap. Keduanya saling melengkapi—proses memperlihatkan handoff dan aturan, sedangkan journey memperlihatkan pengalaman dan beban kognitif.

Batasnya penting. Artikel ini tidak merancang arsitektur informasi, layar, atau *to-be flow*; keputusan tersebut dilakukan setelah fakta proses terkumpul. Peta ini juga bukan bukti bahwa semua pengguna mengalami hal yang sama. Tim perlu menyebutkan segmen yang diamati, periode pengamatan, serta siapa yang belum terwakili. Panduan agile delivery pemerintah Inggris menempatkan pemahaman pengguna, masalah, dan pengujian sebagai pekerjaan berulang, bukan satu lokakarya yang dianggap final ([Service Manual—agile delivery](https://www.gov.uk/service-manual/agile-delivery)).

Pisahkan pula jenis pernyataannya: fakta observasi (misalnya “petugas memasukkan nomor dua kali”), laporan pengguna (“saya sering menunggu balasan”), hipotesis (“duplikasi mungkin menyebabkan salah kirim”), dan keputusan (“uji pengisian otomatis lebih dulu”). Label ini mencegah opini berubah menjadi kebutuhan wajib.

## Cara kerjanya

Mulai dengan satu skenario bernama jelas, bukan “seluruh operasional”. Tentukan pemicu, aktor utama, keluaran, dan kondisi selesai. Kumpulkan artefak yang sudah ada—formulir, email, tiket, log, serta aturan persetujuan—lalu telusuri satu atau beberapa kasus aktual. Minta pelaku memperagakan langkahnya sambil menjelaskan apa yang dilihat dan diputuskan. Jangan hanya bertanya “biasanya bagaimana?” karena jawaban kebiasaan sering melewatkan pengecualian.

Gunakan baris tahap berikut sebagai templat kerja:

| Tahap | Yang dicatat | Bukti minimum |
| --- | --- | --- |
| Pemicu | peristiwa yang memulai pekerjaan dan aktor | tiket, formulir, atau catatan waktu |
| Langkah | tindakan, alat, dan keputusan | rekaman layar, dokumen, atau observasi |
| Handoff | penerima, format data, dan syarat lanjut | pesan, status, atau tanda terima |
| Tunggu | awal-akhir penundaan dan penyebabnya | cap waktu atau estimasi yang diberi label |
| Gagal | validasi, kesalahan, penolakan, atau kehilangan konteks | contoh kasus dan log bila ada |
| Workaround | cara pelaku memulihkan pekerjaan | artefak pengganti dan siapa yang menyetujui |
| Selesai | keluaran dan kriteria penerimaan | hasil akhir atau konfirmasi pemilik proses |

Setelah tabel terisi, gambar dua lapisan: *swimlane* untuk aktor dan sistem, lalu garis perjalanan pengguna untuk tujuan serta titik frustrasi. Hubungkan setiap klaim ke sumbernya dengan ID kasus atau tautan artefak. Hitung baseline yang realistis: waktu aktif, waktu tunggu, jumlah perpindahan, pengulangan input, dan frekuensi kegagalan. Bila datanya belum tersedia, tulis “belum diukur”, bukan angka perkiraan.

Validasi cepat dilakukan bersama pelaku, bukan hanya desainer. Minta mereka menunjuk langkah yang salah, pengecualian yang hilang, dan istilah yang ambigu. Untuk aksesibilitas, masukkan kebutuhan seperti navigasi keyboard, label formulir, urutan fokus, dan pesan kesalahan yang dapat dipahami sebagai pertanyaan uji; WCAG 2.2 menyediakan rujukan kriteria aksesibilitas yang dapat ditelusuri, bukan jaminan bahwa implementasi tertentu sudah sesuai ([W3C WCAG 2.2](https://www.w3.org/TR/WCAG22/)).

## Faktor yang mengubah hasil

Kualitas peta dipengaruhi oleh beberapa kondisi yang perlu ditulis di samping diagram:

- **Variasi pelaku dan konteks.** Pengguna baru, pengguna berpengalaman, pelanggan dengan kebutuhan akses berbeda, serta pekerjaan di perangkat atau jaringan berbeda dapat mengambil jalur berlainan.
- **Volume dan waktu.** Proses yang tampak lancar pada satu kasus bisa menumpuk saat volume meningkat atau ketika persetujuan hanya tersedia pada jam tertentu.
- **Kualitas data.** Format tanggal, identitas, lampiran, dan status yang tidak konsisten menambah kerja tersembunyi. Catat siapa yang memperbaiki data dan kapan.
- **Kebijakan dan kewenangan.** Handoff sering terjadi karena batas otorisasi, bukan karena aplikasi. Tandai keputusan yang memerlukan pemilik proses atau tinjauan profesional.
- **Bukti yang hilang.** Tanpa cap waktu, sampel kasus, atau log, ukuran delay dan tingkat gagal belum dapat disimpulkan. Letakkan `[NEEDS PROJECT EVIDENCE]` pada klaim yang memengaruhi prioritas.

Sobat Codev.id, bedakan juga korelasi dari sebab. Banyak antrean bukan otomatis berarti satu layar buruk; mungkin ada kapasitas pemeriksa, aturan risiko, atau data masuk yang tidak lengkap. Peta yang baik membuat penyebab alternatif terlihat sehingga tim dapat mengujinya satu per satu.

## Contoh keputusan praktis

Bayangkan alur permintaan internal: seorang staf mengisi formulir, koordinator memeriksa, data disalin ke lembar kerja, lalu manajer menyetujui lewat pesan. Ini skenario ilustratif, bukan laporan proyek tertentu. Peta as-is mencatat bahwa koordinator menunggu lampiran (waktu tunggu), menyalin nomor referensi (langkah manual), mengirim pertanyaan ulang (workaround), dan tidak memiliki tanda terima yang konsisten (risiko kehilangan konteks).

Dari sana, keputusan dapat dibuat secara bertahap:

| Temuan terverifikasi | Pertanyaan keputusan | Tindakan aman berikutnya |
| --- | --- | --- |
| Nomor referensi diketik ulang pada beberapa kasus | Apakah sumber data memiliki ID stabil? | Ambil sampel kasus dan cocokkan sumber sebelum mengusulkan otomasi |
| Persetujuan menunggu di luar sistem | Siapa pemilik keputusan dan batas waktunya? | Ukur cap waktu persetujuan; jangan menjanjikan SLA tanpa mandat |
| Lampiran sering tidak lengkap | Informasi apa yang benar-benar diperlukan? | Tinjau contoh penolakan bersama pemilik proses |
| Pengguna memakai pembaca layar atau keyboard | Apakah jalur dan pesan kesalahan dapat dioperasikan? | Jadikan kriteria uji aksesibilitas, bukan asumsi desain |

Jika satu temuan belum memiliki sampel atau log, keputusan yang tepat adalah mengumpulkan bukti tambahan. Prioritas bukan perlombaan memilih fitur paling populer; prioritas mengikuti dampak yang dapat dibuktikan dan biaya verifikasi berikutnya.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah menggambar happy path saja. Tanyakan, “Apa yang terjadi jika data kosong, persetujuan ditolak, atau orang yang biasanya memeriksa tidak tersedia?” Tambahkan jalur pengecualian dengan pemilik dan bukti.

Kesalahan kedua adalah mencampur proses saat ini dengan solusi. Kata seperti “harus ada tombol” atau “gunakan chatbot” sebaiknya dipindahkan ke daftar hipotesis. Periksa kembali setiap kotak: apakah ini observasi, aturan, atau usulan?

Kesalahan ketiga adalah memakai rata-rata tanpa rentang. Tulis jumlah kasus dan periode pengukuran bila tersedia; jika tidak, gunakan deskripsi kualitatif dan tandai `[NEEDS PROJECT EVIDENCE]`. Jangan mengklaim penghematan, kepatuhan, atau peningkatan performa sebelum ada uji yang disepakati.

Kesalahan keempat adalah menganggap satu wawancara mewakili semua orang. Bandingkan peran, tingkat pengalaman, dan kebutuhan akses. Minta pemilik proses menyetujui batas representasi peta.

## Jalan pintas yang tampak cepat tetapi berisiko

Shortcut yang sering dipilih adalah melewati pemetaan dan langsung membuat formulir digital dari formulir kertas. Cara ini dapat memindahkan kolom tanpa memindahkan antrean, handoff, atau aturan tersembunyi. Data mungkin terlihat lebih modern, sementara penyebab penundaan tetap sama dan jalur gagal menjadi lebih sulit dilacak.

Alternatif yang lebih dapat dipertanggungjawabkan hanya membutuhkan disiplin sederhana: pilih satu kasus nyata, telusuri sampai selesai, minta pelaku menunjukkan artefak, ukur waktu tunggu yang bisa dibuktikan, lalu uji peta itu pada kasus kedua. Kawan Codev.id, bila dua kasus bertentangan, jangan memaksa satu diagram; dokumentasikan variasinya dan minta keputusan pemilik proses tentang cakupan yang hendak diperbaiki.

## Langkah berikutnya

Memetakan proses saat ini dan user journey berarti membuat rekaman yang dapat ditelusuri tentang aktor, langkah, data, handoff, delay, kegagalan, workaround, dan baseline—bukan menggambar antarmuka masa depan. Mulailah dengan satu skenario, simpan sumber setiap klaim, dan tandai fakta yang belum terukur.

Sebelum menyusun solusi, serahkan peta beserta daftar bukti yang hilang kepada pemilik proses untuk ditinjau. Anda dapat memakai [beranda Codev.id](/) sebagai titik kembali untuk konteks proyek, tetapi jangan menganggap persetujuan di halaman itu menggantikan keputusan dan pengujian proyek. Aturan operasionalnya: tidak ada klaim prioritas, target waktu, atau kualitas yang final sampai aktor terkait mengonfirmasi peta dan bukti pengukurnya.

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
