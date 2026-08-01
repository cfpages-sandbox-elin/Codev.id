---
article_id: CDV-16-A01
writing_contract_version: "native-id-v2"
title: "Memilih Use Case AI dan Automation dari Baseline"
slug: "memilih-use-case-ai-automation-baseline"
description: "Panduan menetapkan kondisi awal, variasi tugas, biaya kesalahan, data, volume, integrasi, penilaian manusia, manfaat terukur, batas uji coba, dan aturan berhenti"
status: draft
publication_date: "2026-03-27"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-16
primary_intent: "Decide whether AI, rules, workflow automation, or no change is justified"
reader_community: "Codev.id"
reader_address: "Sobat Codev.id"
final_route: "/artikel/memilih-use-case-ai-automation-baseline.html"
technical_review: required
sources:
  - "https://peraturan.bpk.go.id/Details/229798/uu-no-27-tahun-2022"
  - "https://peraturan.bpk.go.id/Details/122030/pp-no-71-tahun-2019"
  - "https://www.nist.gov/privacy-framework"
  - "https://www.nist.gov/itl/ai-risk-management-framework"
  - "https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf"
  - "https://csrc.nist.gov/pubs/sp/800/218/a/final"
---

# Memilih Use Case AI dan Automation dari Baseline

Halo, Sobat Codev.id!

Use case yang layak diautomasi bukan yang paling terdengar canggih, melainkan pekerjaan yang baseline-nya sudah terlihat. Catat bagaimana pekerjaan berjalan sekarang: volume, variasi input, waktu, error, biaya koreksi, data yang disentuh, sistem yang harus dihubungkan, dan keputusan yang tetap membutuhkan manusia. Dari catatan itu, Anda dapat memilih aturan sederhana, workflow automation, AI, atau tidak mengubah apa pun.

Jawaban bisa berubah setelah data diperiksa. Jika manfaat belum dapat diukur, otorisasi data belum jelas, atau orang yang meninjau hasil tidak punya kewenangan menghentikan proses, tunda pilot. Untuk data pribadi, konteks sistem elektronik dan pengamanan perlu dipetakan sesuai [UU No. 27 Tahun 2022](https://peraturan.bpk.go.id/Details/229798/uu-no-27-tahun-2022), [PP No. 71 Tahun 2019](https://peraturan.bpk.go.id/Details/122030/pp-no-71-tahun-2019), serta kerangka pemetaan risiko seperti [NIST Privacy Framework](https://www.nist.gov/privacy-framework). Ketiganya tidak otomatis menjawab dasar pemrosesan atau masa simpan kasus Anda; [NEEDS GATE-05 REVIEW: verifikasi dasar pemrosesan, peran, transfer, retensi, dan aturan sektor sebelum pilot data pribadi].

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

Baseline adalah gambaran terukur tentang pekerjaan sebelum perubahan. Ia bukan target optimistis dan bukan angka dari vendor. Satu baseline yang berguna menyebut siapa yang memulai tugas, input apa yang masuk, langkah yang dilakukan, keputusan yang dibuat, keluaran yang diharapkan, serta kondisi pengecualian. Ukur beberapa siklus yang wajar agar variasi terlihat.

Automation di sini mencakup aturan deterministik dan perpindahan data antarsistem. AI menambah komponen yang menafsirkan, mengklasifikasikan, atau menghasilkan keluaran dari pola; hasilnya dapat berubah dan perlu dievaluasi. “Tidak berubah” juga keputusan yang sah bila pekerjaan jarang terjadi, risikonya tinggi, atau biaya integrasi melebihi manfaat.

Batas artikel ini adalah pemilihan dan perancangan pilot, bukan persetujuan hukum, pengadaan vendor, atau jaminan hasil produksi. Produk workflow tertentu memiliki katalog dan penilaian sendiri; jangan menganggap kebutuhan di sini otomatis berarti memakai satu produk.

## Cara kerjanya

Mulai dengan lembar satu halaman. Tulis baseline dalam urutan berikut.

1. **Pemicu dan volume.** Berapa item masuk per hari atau per minggu, pada jam berapa, dan apakah ada lonjakan musiman? Volume kecil sering tidak membayar biaya integrasi.
2. **Variasi dan pengecualian.** Kelompokkan input yang seragam, semi-terstruktur, dan benar-benar berbeda. Hitung berapa sering aturan biasa tidak berlaku.
3. **Error dan konsekuensi.** Bedakan salah ketik yang mudah dibetulkan dari salah klasifikasi yang memicu pembayaran, keputusan pelanggan, atau kebocoran data. Catat waktu koreksi dan siapa yang menanggungnya.
4. **Data.** Inventarisasikan sumber, pemilik, format, akses, kebutuhan penghapusan, serta apakah data pribadi atau rahasia ikut diproses. Pemetaan tersebut sejalan dengan pendekatan identifikasi dan pengelolaan risiko dalam [NIST Privacy Framework](https://www.nist.gov/privacy-framework).
5. **Antarmuka.** Daftar sistem yang membaca dan menulis data, kredensial yang diperlukan, serta titik kegagalan bila salah satu sistem tidak tersedia.
6. **Penilaian manusia.** Tandai keputusan yang memerlukan konteks, negosiasi, atau pengecualian. Reviewer harus menerima informasi yang cukup, dapat menolak keluaran, dan punya jalur pemulihan.

Setelah itu, tulis metrik manfaat sebelum memilih teknologi: waktu siklus, backlog, tingkat rework, error yang lolos, atau jam review. Gunakan definisi dan periode yang sama pada baseline dan pilot. Jangan menyebut “lebih efisien” tanpa menyatakan apa yang turun, untuk siapa, dan pada mutu seperti apa.

Untuk kandidat AI, siapkan set evaluasi yang mewakili input normal sekaligus kegagalan dan penyalahgunaan. [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework) dan [NIST AI 600-1 Generative AI Profile](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf) menekankan bahwa keluaran yang lancar belum tentu benar; evaluasi, pemantauan, kontrol manusia, fallback, dan penghentian perlu dirancang sejak awal. Jangan mengisi angka akurasi sebelum set uji dan kriteria lulus tersedia.

## Faktor yang mengubah hasil

Empat kelompok faktor biasanya mengubah keputusan.

**Keteraturan tugas.** Jika input dan aturan stabil, validasi berbasis aturan atau workflow biasanya lebih mudah diaudit. Variasi bahasa, dokumen, atau maksud dapat menjadi alasan mempertimbangkan AI, tetapi juga memperbesar kebutuhan review.

**Biaya salah.** Pilih otomatisasi penuh hanya ketika kesalahan dapat dideteksi dan dipulihkan dengan cepat. Untuk keputusan berdampak besar, pertahankan persetujuan manusia dan jalur banding. Sobat Codev.id, orang yang namanya ada di diagram bukan berarti kontrolnya nyata: pastikan ia melihat input, alasan untuk menolak, dan cara mengembalikan keadaan.

**Data dan akses.** Data minimum yang diperlukan mengurangi permukaan risiko. Konfirmasikan hak akses, lokasi pemrosesan, penghapusan, backup, serta uji pemulihan. Backup yang belum pernah direstorasi hanyalah klaim, bukan bukti operasional. Detail kewajiban dapat berbeda menurut konteks; [NEEDS GATE-05 REVIEW: minta peninjauan profesional untuk lawful basis, transfer, retensi, notifikasi, dan aturan sektoral].

**Integrasi dan operasi.** API yang tidak stabil, identitas ganda, atau proses manual di tengah alur dapat menghapus manfaat. Tetapkan pemilik operasional, log yang cukup untuk menelusuri keputusan, jadwal pemantauan, dan siapa yang mematikan alur ketika indikator memburuk. Praktik pengembangan aman dan pemeliharaan berkelanjutan dapat dirujuk pada [NIST SP 800-218A](https://csrc.nist.gov/pubs/sp/800/218/a/final), tanpa menganggap rujukan itu sebagai bukti kepatuhan otomatis.

## Contoh keputusan praktis

Gunakan tabel ini sebagai penyaring awal, bukan kalkulator ROI.

| Kondisi baseline | Pilihan awal | Syarat pilot |
|---|---|---|
| Input seragam, aturan jelas, error mudah dipulihkan | Aturan atau workflow | Log, validasi, dan rollback teruji |
| Input semi-terstruktur, volume cukup, manusia memeriksa | AI untuk saran atau ekstraksi | Set uji representatif, review wajib, metrik mutu |
| Pengecualian jarang tetapi dampaknya besar | Otomatisasi terbatas | Persetujuan eksplisit dan jalur banding |
| Volume rendah, integrasi mahal, manfaat kabur | Tidak berubah dulu | Ukur baseline lebih lama atau sederhanakan proses |

Misalnya, tim menerima formulir dengan tiga format tetap dan aturan routing yang terdokumentasi. Aturan deterministik dapat menjadi pilot kecil. Jika dokumen datang dalam banyak bentuk dan petugas hanya memakai keluaran sebagai draf, AI mungkin membantu ekstraksi; ia tidak boleh mengirim keputusan tanpa pemeriksaan. Bila data berisi identitas pelanggan dan vendor belum menjelaskan perilaku penyimpanan, hentikan pemilihan sampai pertanyaan GATE-05 terjawab.

Tetapkan cap pilot: periode terbatas, kelompok data yang dibatasi, jumlah pengguna, dan volume maksimum. Sebelum mulai, tulis ambang lanjut, perbaikan, dan berhenti. Contoh bentuknya: lanjut hanya bila waktu siklus membaik tanpa kenaikan error kritis; berhenti bila log tidak lengkap, fallback gagal, atau reviewer kehilangan kendali. Angka ambang harus berasal dari baseline dan toleransi bisnis Anda, bukan rekaan artikel ini.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah memilih tool lalu mencari pekerjaan untuknya. Balik urutannya: minta pemilik proses menunjukkan lima sampai sepuluh contoh nyata, termasuk pengecualian, sebelum demo. Kesalahan kedua adalah memakai satu angka akurasi dari vendor. Tanyakan definisi label, komposisi set uji, kasus gagal, dan bagaimana hasil dipantau di operasi.

Kesalahan ketiga adalah menganggap human-in-the-loop sebagai stempel. Periksa apakah reviewer punya waktu, konteks, kewenangan, dan cara memulihkan keluaran. Kesalahan keempat adalah mengirim seluruh arsip “agar model belajar”. Mulai dari data minimum, pastikan hak akses, dan dokumentasikan penghapusan serta backup.

Kawan Codev.id, jangan lupakan stop rule. Tinjau metrik pada interval yang disepakati; bandingkan dengan baseline yang sama; catat insiden dan near miss. Jika hasil memburuk atau asumsi awal tidak berlaku, matikan pilot dan kembali ke proses aman. [NEEDS GATE-10 REVIEW: verifikasi kontrol provider, hak data, evaluasi penyalahgunaan, monitoring, fallback, dan prosedur retirement sebelum penggunaan generatif atau berisiko tinggi].

## Kesimpulan

Memilih use case dimulai dari baseline, bukan dari label AI. Ukur volume, variasi, biaya error, data, integrasi, dan kebutuhan penilaian manusia; lalu cocokkan dengan aturan, workflow, AI, atau keputusan untuk tidak berubah. Buat lembar keputusan yang memuat metrik, cap pilot, pemilik kontrol, ambang lanjut, dan stop rule.

Langkah berikutnya adalah meminta pemilik proses mengisi lembar itu dengan contoh nyata dan meminta peninjauan privasi/keamanan untuk data yang sensitif. Teman Codev.id, bila bukti manfaat, hak data, atau kendali manusia belum jelas, keputusan yang benar adalah menunda—bukan menebak. Setelah bukti cukup, lakukan pilot terbatas dan pertahankan kemampuan untuk menghentikannya.

Untuk menyiapkan pertanyaan awal bersama tim, Anda dapat mulai dari [beranda Codev.id](/) lalu kembali ke lembar baseline ini.
