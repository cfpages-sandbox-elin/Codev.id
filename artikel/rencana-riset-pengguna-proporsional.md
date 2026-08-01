---
article_id: CDV-02-A01
writing_contract_version: "native-id-v2"
title: "Rencana Riset Pengguna yang Proporsional"
slug: "rencana-riset-pengguna-proporsional"
description: "Match questions to interviews, observation, surveys, analytics, and usability methods with sampling limits"
status: draft
publication_date: "2025-04-15"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-02
primary_intent: "Choose research methods for a product decision"
reader_community: "Codev.id"
reader_address: "Sobat Codev.id"
final_route: "/artikel/rencana-riset-pengguna-proporsional.html"
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

# Rencana Riset Pengguna yang Proporsional

Halo, Sobat Codev.id! Rencana riset pengguna yang proporsional bukanlah riset paling besar yang dapat dilakukan tim. Rencana ini memilih metode yang cukup untuk menjawab keputusan tertentu, pada saat yang tepat, dengan batas sampel dan tingkat keyakinan yang jujur. Wawancara cocok untuk memahami alasan dan bahasa pengguna; observasi untuk melihat pekerjaan nyata; survei untuk memetakan pola pada kelompok yang lebih luas; analitik untuk membaca perilaku yang sudah terjadi; dan usability test untuk menemukan hambatan pada alur atau rancangan.

Mulailah dari keputusan, bukan dari daftar metode. Tulis: “Keputusan apa yang harus dibuat, asumsi apa yang paling berisiko, dan bukti minimum apa yang akan mengubah keputusan?” Jika pertanyaannya masih kabur, lakukan percakapan singkat dengan pemangku kepentingan dan calon pengguna. Jika risikonya adalah orang gagal menyelesaikan tugas, uji tugas itu dengan prototipe sederhana. Jika risikonya adalah prioritas masalah, gabungkan wawancara atau observasi dengan data perilaku. Pilihan akhir berubah ketika akses pengguna, tahap produk, konsekuensi kesalahan, atau kualitas data berubah.

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

*Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.*

## Jawaban singkat dan salah paham utama

Kesalahan paling mahal adalah menganggap satu survei atau satu sesi uji sudah mewakili semua orang. Setiap metode menjawab jenis pertanyaan berbeda. Data analitik dapat menunjukkan halaman yang ditinggalkan, tetapi tidak otomatis menjelaskan niat pengguna. Wawancara dapat mengungkap kebutuhan dan istilah, tetapi jawaban yang diucapkan bukan bukti bahwa perilaku itu selalu terjadi. Uji usability dapat menemukan masalah dalam tugas yang diuji, tetapi bukan sertifikat bahwa seluruh produk mudah dipakai.

Riset yang proporsional berarti membatasi klaim sesuai bukti. Untuk keputusan awal, beberapa percakapan terarah dan pengamatan terhadap pekerjaan nyata bisa cukup untuk memperbaiki hipotesis. Untuk memilih antara dua solusi, uji tugas pada prototipe yang sama-sama realistis lebih relevan daripada menambah pertanyaan survei. Untuk memantau perubahan setelah rilis, analitik dan pertanyaan dukungan memberi sinyal, lalu riset kualitatif membantu menjelaskan sinyal tersebut. Praktik agile juga menempatkan pembelajaran, pengujian, dan pengiriman bertahap sebagai siklus, bukan gerbang riset sekali jadi ([UK Government Service Manual—agile delivery](https://www.gov.uk/service-manual/agile-delivery)).

Catat keputusan yang belum dapat dibuktikan: **[NEEDS PROJECT RESEARCH AND DECISION OWNER: tetapkan siapa yang berwenang menerima atau menolak bukti sebelum pengambilan sampel dimulai.]** Tanpa pemilik keputusan, tim mudah mengumpulkan data yang menarik tetapi tidak pernah mengubah pekerjaan.

## Definisi dan batas objek

Objek rencana ini adalah hubungan antara pertanyaan produk, metode, peserta atau sumber data, batas waktu, dan keputusan. “Proporsional” bukan istilah untuk mengurangi kualitas. Artinya, usaha riset mengikuti besarnya risiko, kebaruan asumsi, dan biaya jika keputusan keliru.

Yang dibahas:

- pertanyaan eksploratif tentang kebutuhan, konteks, dan bahasa pengguna;
- pertanyaan perilaku tentang apa yang benar-benar dilakukan;
- pertanyaan evaluatif tentang kemampuan menyelesaikan tugas;
- pertanyaan pemantauan tentang perubahan setelah peluncuran;
- batas siapa yang diamati, berapa banyak bukti yang diperlukan, dan kapan berhenti.

Yang tidak dibahas adalah klaim keterwakilan statistik, persetujuan profesional untuk proyek tertentu, atau pernyataan bahwa produk sudah memenuhi persyaratan aksesibilitas. WCAG menjelaskan kriteria dan ruang lingkup evaluasi aksesibilitas, tetapi rencana riset umum ini tidak boleh dipasarkan sebagai bukti conformance. Evaluasi yang bermakna mempertimbangkan cakupan halaman, proses, dan kondisi penggunaan; metodologi WCAG-EM menekankan penentuan cakupan dan pemilihan sampel sebagai bagian dari evaluasi ([WCAG 2.2](https://www.w3.org/TR/WCAG22/), [WCAG-EM 1.0](https://www.w3.org/TR/WCAG-EM/)). Partisipasi pengguna disabilitas, bila dibutuhkan, memerlukan rencana khusus dan tidak boleh diasumsikan tercakup di sini.

## Cara kerjanya

Susun rencana dalam urutan berikut.

1. **Nyatakan keputusan.** Contohnya memilih label navigasi, menentukan urutan langkah, atau memutuskan apakah masalah tertentu layak diprioritaskan. Hindari tujuan seperti “memahami pengguna” tanpa keputusan turunan.
2. **Petakan ketidakpastian.** Bedakan asumsi tentang motivasi, perilaku, kelayakan teknis, dan akses. Tulis konsekuensi jika asumsi itu salah.
3. **Pilih metode yang menutup celah.** Wawancara menjawab mengapa dan bagaimana orang menjelaskan kebutuhannya. Observasi mengikuti konteks, alat, gangguan, dan jalan pintas. Survei mengukur jawaban terstruktur pada populasi yang dapat dijangkau. Analitik membaca jejak penggunaan. Usability test meminta peserta menjalankan tugas dengan kriteria pengamatan yang jelas.
4. **Tentukan sumber dan batas sampel.** Untuk kualitatif, cari variasi konteks yang relevan dan berhenti ketika sesi tambahan tidak lagi mengubah pemahaman kerja. Untuk survei, jelaskan siapa yang diundang, siapa yang tidak terjangkau, dan bahwa hasilnya tidak otomatis representatif. Untuk analitik, periksa definisi event, periode, dan data yang hilang.
5. **Siapkan protokol singkat.** Tulis skrip, tugas, urutan, cara mencatat, aturan privasi, dan kondisi penghentian. Pilot kecil dapat menunjukkan apakah pertanyaan dipahami sebelum tim memperluas pengumpulan data.
6. **Hubungkan temuan ke keputusan.** Setelah sintesis, pisahkan observasi, kutipan, angka, interpretasi, dan usulan. Buat jejak dari temuan ke risiko, opsi, keputusan, serta bukti yang masih kurang.

Kawan Codev.id, “sampel kecil” bukan izin untuk memilih peserta yang paling mudah ditemui. Ia hanya masuk akal bila pertanyaan, variasi konteks, dan batas klaim ditulis terang. Bila keputusan berdampak pada kelompok yang tidak terjangkau, tandai kekosongan itu dan cari masukan tambahan atau tunda klaim.

## Faktor yang mengubah hasil

Beberapa kondisi membuat rencana yang sama menghasilkan keyakinan berbeda:

- **Tahap produk.** Pada tahap penemuan, pertanyaan terbuka dan observasi biasanya lebih berguna. Pada tahap rancangan, uji tugas menguji alur tertentu. Setelah rilis, analitik memerlukan definisi event yang stabil dan konteks dari dukungan pengguna.
- **Risiko keputusan.** Kesalahan kecil pada label dapat diuji cepat; keputusan yang menyentuh transaksi, keselamatan, atau akses membutuhkan pemeriksaan lebih ketat dan pemilik keputusan yang jelas.
- **Keragaman konteks.** Peran, perangkat, koneksi, bahasa, lokasi kerja, dan frekuensi tugas dapat mengubah pengalaman. Pilih variasi yang berhubungan dengan keputusan, bukan sekadar menambah jumlah.
- **Kualitas instrumen.** Pertanyaan yang menggiring, tugas yang tidak lengkap, event analitik yang salah nama, atau survei dengan opsi jawaban timpang dapat menghasilkan kepastian palsu.
- **Aksesibilitas dan evaluasi.** Pemeriksaan awal seperti keyboard, fokus, label formulir, pesan kesalahan, pembesaran, dan reflow dapat menemukan isu yang tampak cepat, tetapi pemeriksaan seperti itu bukan pengganti evaluasi proses dan cakupan penuh ([WAI Easy Checks](https://www.w3.org/WAI/test-evaluate/preliminary/)). Jangan menyimpulkan conformance atau kepatuhan hukum Indonesia dari satu scanner atau satu sesi.
- **Kendala pelaksanaan.** Waktu, insentif, keamanan data, penerjemahan, dan kapasitas moderator memengaruhi siapa yang hadir serta apa yang dapat dicatat. Tuliskan komprominya agar pembaca hasil tidak salah menafsirkan.

## Contoh keputusan praktis

Gunakan tabel ini sebagai titik awal, lalu sesuaikan dengan risiko dan akses yang nyata.

| Keputusan yang hendak dibuat | Bukti awal yang paling tepat | Batas dan pemeriksaan |
|---|---|---|
| Apakah istilah menu dipahami? | Wawancara singkat untuk bahasa, lalu uji tugas pada beberapa variasi istilah | Jangan mengklaim seluruh populasi; catat peran dan konteks peserta. |
| Mengapa proses kerja terhenti? | Observasi alur nyata, dilengkapi analitik atau tiket dukungan | Pastikan event dan catatan tidak mencampur penyebab yang berbeda. |
| Masalah mana yang paling sering dirasakan? | Survei terstruktur setelah hipotesis masalah dirumuskan | Laporkan siapa yang menjawab dan siapa yang tidak terjangkau. |
| Apakah rancangan baru membantu tugas utama? | Usability test berbasis skenario dengan kriteria berhasil yang eksplisit | Temuan berlaku pada tugas dan kondisi yang diuji, bukan seluruh produk. |
| Apakah ada hambatan akses yang terlihat? | Pemeriksaan manual terarah dan evaluasi dengan cakupan yang ditetapkan | Jangan menyebutnya sertifikasi; pertimbangkan evaluasi aksesibilitas yang lengkap bila keputusan menuntutnya. |

Misalnya, tim hanya punya tiga hari untuk memilih antara dua alur pendaftaran. Pertanyaan utamanya bukan “alur mana yang disukai?”, melainkan “di langkah mana pengguna gagal atau ragu, dan apakah alur alternatif mengurangi risiko itu?” Rekrut variasi konteks yang paling relevan, jalankan tugas yang sama pada kedua alur, catat perilaku dan pertanyaan spontan, lalu tetapkan ambang keputusan sebelum melihat hasil. Jika peserta sulit diakses, hasilkan rekomendasi sementara dengan batasan tertulis—bukan kepastian palsu.

## Kesalahan umum dan cara memeriksanya

**Memulai dari metode favorit.** Periksa setiap metode dengan kalimat: “Jawaban apa yang dapat diberikan metode ini, dan jawaban apa yang tidak?” Bila tidak ada keputusan yang terhubung, hentikan pengumpulan.

**Menyamakan ucapan dengan perilaku.** Minta contoh kejadian terakhir, amati bila memungkinkan, dan bandingkan dengan jejak penggunaan. Perbedaan bukan kegagalan peserta; itu sinyal untuk diselidiki.

**Menambah peserta tanpa menambah variasi.** Tanyakan konteks apa yang belum tercakup. Sampel yang lebih besar dari kelompok yang sama belum tentu menjawab risiko yang berbeda.

**Mengubah temuan menjadi angka yang tampak presisi.** Cantumkan ukuran dan cara pengumpulan, tetapi hindari persentase yang tidak memiliki denominator jelas. Untuk hasil kualitatif, jelaskan pola, contoh, dan pengecualian tanpa mengklaim prevalensi.

**Mengandalkan satu alat aksesibilitas.** Gunakan pemeriksaan manual dan evaluasi yang cakupannya jelas; isu keyboard, fokus, semantik, formulir, reflow, autentikasi, media, dan teknologi bantu memerlukan lebih dari satu pemeriksaan ([WCAG 2.2](https://www.w3.org/TR/WCAG22/)).

Sebelum rapat keputusan, minta setiap temuan menjawab empat pertanyaan: sumbernya apa, konteksnya siapa, seberapa langsung bukti itu, dan tindakan apa yang berubah karenanya. Temuan tanpa jawaban tersebut masuk daftar asumsi, bukan daftar fakta.

## Mengapa survei cepat tidak selalu menjadi pilihan aman

Shortcut yang sering dipilih adalah mengirim survei umum karena cepat dan mudah dihitung. Ini dapat gagal ketika tim belum tahu istilah pengguna, pilihan jawaban belum mencerminkan konteks, atau responden yang paling terdampak tidak terjangkau. Angka yang rapi lalu memberi ilusi kepastian.

Alternatif yang lebih aman adalah melakukan percakapan atau observasi terbatas untuk membentuk hipotesis, menguji pertanyaan survei pada skala kecil, lalu memakai survei hanya untuk keputusan yang memang memerlukan pemetaan terstruktur. Simpan batas responden dan nonresponden di halaman hasil. Bila keputusan menyangkut alur tertentu, dahulukan uji tugas yang mereproduksi pekerjaan itu.

## Kesimpulan: mulai dari keputusan, berhenti pada bukti yang cukup

Rencana riset pengguna yang proporsional memasangkan pertanyaan dengan metode: wawancara untuk alasan dan bahasa, observasi untuk konteks, survei untuk pola pada kelompok terjangkau, analitik untuk perilaku yang tercatat, dan usability test untuk hambatan pada tugas. Ukuran sampel, variasi peserta, serta kekuatan klaim harus mengikuti risiko keputusan—bukan target angka yang berdiri sendiri.

Teman Codev.id, sebelum mengundang peserta, tulis satu halaman berisi keputusan, asumsi berisiko, metode, batas sampel, kriteria berhenti, pemilik keputusan, dan bukti yang akan mengubah arah. Setelah pengumpulan, pisahkan fakta dari interpretasi dan catat siapa yang belum terwakili. Untuk langkah berikutnya, Anda dapat meninjau konteks layanan dan keputusan produk di [halaman utama Codev.id](/). Jika keputusan memerlukan kepastian aksesibilitas, keamanan, hukum, atau dampak tinggi, minta evaluasi profesional yang sesuai. Aturan operasionalnya sederhana: jangan memperbesar klaim melebihi konteks dan bukti yang benar-benar Anda miliki.
