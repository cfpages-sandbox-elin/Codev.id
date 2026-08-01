---
article_id: CDV-17-A02
writing_contract_version: "native-id-v2"
title: "Estimasi Software dengan Range, Asumsi, dan Risiko"
slug: "estimasi-software-range-asumsi-risiko"
description: "Panduan menilai estimasi software melalui ruang lingkup, asumsi, rentang ketidakpastian, cadangan risiko, validasi, pengecualian, dan pemicu perubahan."
status: draft
publication_date: "2026-04-26"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-17
primary_intent: "Understand and review an uncertain software estimate"
reader_community: "Codev.id"
reader_address: "Kawan Codev.id"
final_route: "/artikel/estimasi-software-range-asumsi-risiko.html"
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

# Estimasi Software dengan Range, Asumsi, dan Risiko

Halo, Kawan Codev.id! Angka tunggal seperti “selesai 12 minggu dengan biaya sekian” terlihat tegas, tetapi sering menyembunyikan pekerjaan yang belum diurai, ketergantungan yang belum tersedia, dan keputusan yang masih berubah. Estimasi yang dapat ditinjau seharusnya berbentuk rentang, dilengkapi asumsi, cadangan risiko, serta cara memperbarui keyakinan setelah bukti baru muncul.

Jawaban singkatnya: mulai dari *scope* yang dipecah menjadi pekerjaan dan dependensi, tulis asumsi yang membuat hitungan berlaku, lalu tampilkan rentang (misalnya skenario rendah–tengah–tinggi) beserta pemicu perubahan. Sisihkan pekerjaan validasi dan cadangan untuk risiko yang masuk akal. Rentang itu bukan janji waktu atau harga; ia adalah alat untuk memilih keputusan yang masih aman ketika informasi belum lengkap. Kondisi yang dapat mengubahnya meliputi akses sistem, batas API, kualitas data, keputusan pemilik produk, dan hasil uji awal.

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

*Ilustrasi umum dari aset lokal codev.id; bukan dokumentasi proyek tertentu.*

## Jawaban singkat dan salah paham utama

Kesalahan paling mahal adalah menganggap angka terendah sebagai harga “sebenarnya”. Angka rendah biasanya hanya menghitung pembuatan fitur inti, bukan klarifikasi kebutuhan, integrasi, pengujian, migrasi, dokumentasi, atau dukungan setelah rilis. Sebaliknya, angka tinggi tanpa penjelasan juga tidak membantu karena pembeli tidak tahu risiko apa yang sedang dibayar.

Mintalah tiga hal pada setiap baris estimasi: pekerjaan apa yang termasuk, bukti apa yang mendasarinya, dan kejadian apa yang membuatnya berubah. Bila vendor hanya memberi total, minta *work breakdown structure* (WBS), yaitu daftar pekerjaan yang cukup rinci untuk dibandingkan. Tambahkan kolom pemilik keputusan dan status bukti: sudah diverifikasi, asumsi, atau perlu validasi.

Inventaris komponen juga penting ketika aplikasi memakai layanan pihak ketiga. CISA menjelaskan bahwa *software bill of materials* (SBOM) meningkatkan transparansi komponen, tetapi SBOM sendiri tidak membuktikan bahwa komponen aman. [CISA SBOM resources](https://www.cisa.gov/sbom) Karena itu, estimasi untuk integrasi harus memuat pemeriksaan lisensi, asal komponen, serta rencana jika dependensi berubah.

## Definisi dan batas objek

Estimasi di sini adalah model keputusan untuk pekerjaan software tertentu, bukan daftar harga pasar atau jaminan durasi. Objeknya mencakup ruang lingkup produk, aktivitas rekayasa, dependensi teknis dan organisasi, ketidakpastian, cadangan risiko, serta pekerjaan pembuktian seperti prototipe dan uji penerimaan.

Yang tidak dicakup: penetapan harga aktual untuk proyek Anda, interpretasi kewajiban hukum, atau persetujuan arsitektur dan kontrak. Angka final memerlukan data proyek, negosiasi, dan tinjauan profesional yang berwenang. [NEEDS GATE-09 REVIEW: verifikasi syarat vendor, kuota API, subprosesor, kerentanan terkini, dan pembagian tanggung jawab kontraktual sebelum angka dipakai sebagai komitmen.]

## Cara kerjanya

Urutan praktisnya dapat dibuat seperti berikut.

1. **Tetapkan hasil dan batas.** Tulis siapa pengguna, alur yang harus berhasil, sistem yang disentuh, serta fitur yang sengaja dikeluarkan. “Integrasi pembayaran” perlu dijelaskan apakah hanya pengiriman transaksi, rekonsiliasi, notifikasi, atau semuanya.
2. **Pecah pekerjaan dan dependensi.** Untuk tiap hasil, catat desain, implementasi, migrasi, pengujian, rilis, dan handover. Tandai akses yang harus disediakan pihak lain; pekerjaan yang menunggu akses bukan sekadar tambahan jam coding.
3. **Nyatakan asumsi.** Contoh: API tersedia di lingkungan uji, format data disepakati, satu pengambil keputusan hadir mingguan, dan tidak ada perubahan regulasi selama iterasi. Setiap asumsi harus punya pemilik dan tanggal pemeriksaan.
4. **Buat rentang.** Skenario rendah mengandaikan asumsi utama benar; skenario tengah memasukkan gangguan biasa; skenario tinggi memasukkan risiko yang sudah teridentifikasi. Jelaskan penggerak rentang, bukan sekadar menambahkan persentase acak.
5. **Pisahkan validasi dan cadangan.** Prototipe, *spike* teknis, uji beban, atau pembersihan data adalah pekerjaan nyata. Cadangan risiko dipakai hanya saat pemicu yang disepakati terjadi, dengan catatan keputusan dan sisa saldo.
6. **Perbarui keyakinan.** Setelah akses diuji atau prototipe berjalan, ubah asumsi menjadi fakta, pertahankan, atau gugurkan. Rentang boleh menyempit maupun melebar; perubahan itu tanda model diperbarui, bukan kegagalan moral.

Untuk dependensi rantai pasok, NIST SP 800-161 Rev.1 menekankan perlunya mengelola risiko pemasok dan komponen sepanjang siklus hidup. [NIST SP 800-161 Rev.1](https://csrc.nist.gov/pubs/sp/800/161/r1/final) Dalam praktik estimasi, itu berarti menyebutkan siapa yang menyediakan komponen, bagaimana asalnya diverifikasi, dan apa rencana penggantiannya.

## Faktor yang mengubah hasil

Kelompokkan penggerak perubahan agar diskusi tidak kabur.

| Kelompok | Pertanyaan pemeriksaan | Dampak pada estimasi |
|---|---|---|
| Scope | Apa definisi “selesai” dan apa yang dikecualikan? | Menambah atau mengurangi WBS dan pengujian |
| Data | Apakah contoh data nyata tersedia dan bersih? | Mengubah pekerjaan migrasi, pemetaan, dan validasi |
| Integrasi | Siapa pemilik API, kredensial, kuota, dan lingkungan uji? | Menambah waktu tunggu serta *fallback* |
| Keputusan | Berapa lama persetujuan desain dan penerimaan? | Mengubah urutan kerja dan kapasitas tim |
| Operasi | Siapa memantau, merespons insiden, dan memegang akun? | Menambah handover, pelatihan, dan biaya siklus hidup |
| Bukti | Uji apa yang harus lulus sebelum rilis? | Menentukan pekerjaan validasi dan kriteria berhenti |

Logo sertifikasi atau skor repositori dapat menjadi sinyal awal, bukan pengganti pemeriksaan. OpenSSF Scorecard sendiri menyajikan sinyal praktik keamanan repositori; [OpenSSF Scorecard](https://securityscorecards.dev/) gunakan hasilnya untuk menentukan pertanyaan lanjutan, bukan untuk menyimpulkan vendor pasti aman atau cocok.

## Contoh keputusan praktis

Bayangkan estimasi integrasi dengan sistem akuntansi yang dokumentasi API-nya belum diuji. Buat tiga skenario berikut, lalu tulis pemicu perpindahannya.

| Skenario | Asumsi kunci | Pekerjaan wajib | Keputusan |
|---|---|---|---|
| Rendah | Endpoint uji lengkap dan format data stabil | Implementasi, uji kontrak, penerimaan | Boleh mulai setelah akses diverifikasi |
| Tengah | Ada dua format data dan respons vendor lambat | Mapping tambahan, *retry*, dan satu putaran perbaikan | Jadwalkan cadangan serta titik keputusan |
| Tinggi | Endpoint penting belum tersedia | Prototipe, *mock*, desain fallback, negosiasi ulang | Jangan mengunci tanggal rilis; hentikan perluasan scope |

Kawan Codev.id, minta setiap skenario menjawab “bukti apa yang menurunkan ketidakpastian?” Jika akses sandbox berhasil, asumsi akses berubah menjadi fakta. Jika tidak, cadangan tidak boleh diam-diam dipakai untuk menutup masalah; pemilik produk harus memilih fallback atau mengubah scope.

## Kesalahan umum dan cara memeriksanya

**Mengunci angka sebelum WBS.** Periksa apakah setiap fitur memiliki aktivitas pengujian, rilis, dan handover. Jika tidak, angka belum sebanding.

**Menyamakan cadangan dengan keuntungan tersembunyi.** Minta daftar risiko, pemicu penggunaan, pemilik persetujuan, dan saldo cadangan pada setiap pembaruan.

**Menganggap dependensi eksternal pasti tersedia.** Minta bukti akses, batas kuota, lingkungan uji, dan jalur eskalasi. Tanpa itu, tulis sebagai asumsi dan jadwalkan validasi.

**Memakai skor atau logo sebagai bukti lengkap.** Cocokkan sinyal dengan ruang lingkup yang benar-benar akan dikerjakan, referensi yang relevan, dan proses serah-terima. Prinsip *secure by design* CISA menempatkan pertimbangan keamanan sejak awal, bukan sebagai pemeriksaan kosmetik di akhir. [CISA Secure by Design](https://www.cisa.gov/securebydesign)

**Menyamakan harga build dengan biaya seumur hidup.** Rencana operasi, akun, lisensi, dukungan, dan penggantian komponen dapat menentukan biaya setelah rilis. UK Technology Code of Practice juga mendorong keputusan teknologi yang mempertimbangkan pengguna, nilai, dan pengelolaan sepanjang layanan, bukan hanya pengadaan awal. [UK Technology Code of Practice](https://www.gov.uk/guidance/the-technology-code-of-practice)

## Saat angka tunggal terasa lebih mudah

Shortcut yang sering dipilih adalah meminta satu vendor “langsung beri harga terbaik” lalu membandingkan totalnya. Cara ini gagal ketika vendor menghitung scope dan asumsi yang berbeda; total tampak murah karena pekerjaan validasi atau risiko dipindahkan tanpa nama. Alternatif yang lebih aman: kirimkan batas hasil yang sama, minta WBS dan rentang dengan format seragam, kemudian bandingkan asumsi, dependensi, bukti, pengecualian, serta pemicu perubahan. Perbandingan harga baru bermakna setelah objek yang dihitung setara.

## Langkah berikutnya

Estimasi software yang dapat dipercaya bukan satu angka, melainkan rentang yang dapat ditelusuri ke scope, WBS, asumsi, dependensi, pekerjaan validasi, dan cadangan risiko. Sebelum menyetujui, minta satu lembar yang mencantumkan skenario, bukti pemicu, pengecualian, pemilik keputusan, dan tanggal pembaruan keyakinan. Anda dapat memulai dari [beranda Codev.id](/) untuk menemukan konteks layanan sebelum membawa lembar tersebut ke review teknis dan kontrak; jangan menganggapnya sebagai penawaran harga atau jaminan waktu. Teman Codev.id, aturan operasionalnya sederhana: setiap angka harus punya asumsi yang bisa diuji dan tindakan yang jelas ketika asumsi itu berubah.
