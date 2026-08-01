---
article_id: CDV-16-A05
writing_contract_version: "native-id-v2"
title: "RAG dan Knowledge Assistant dengan Sumber yang Bisa Dilacak"
slug: "rag-knowledge-assistant-sumber-terlacak"
description: "Panduan merancang knowledge assistant berbasis RAG dengan sumber, versi, akses, sitasi, ketidakpastian, penghapusan, dan pengujian yang dapat dilacak."
status: draft
publication_date: "2026-04-11"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-16
primary_intent: "Design retrieval-assisted answers with provenance and limits"
reader_community: "Codev.id"
reader_address: "Sobat Codev.id"
final_route: "/artikel/rag-knowledge-assistant-sumber-terlacak.html"
technical_review: required
sources:
  - "https://peraturan.bpk.go.id/Details/229798/uu-no-27-tahun-2022"
  - "https://peraturan.bpk.go.id/Details/122030/pp-no-71-tahun-2019"
  - "https://www.nist.gov/privacy-framework"
  - "https://www.nist.gov/itl/ai-risk-management-framework"
  - "https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf"
  - "https://csrc.nist.gov/pubs/sp/800/218/a/final"
---

# RAG dan Knowledge Assistant dengan Sumber yang Bisa Dilacak

Halo, Sobat Codev.id! Jawaban yang terdengar meyakinkan belum tentu jawaban yang dapat dipertanggungjawabkan. Dalam RAG (retrieval-augmented generation), keputusan penting bukan sekadar memilih model, melainkan memastikan setiap jawaban mengambil dokumen yang berwenang, versi yang jelas, dan bukti yang bisa dibuka kembali.

Rancangan yang aman memisahkan tiga hal: sumber pengetahuan, proses pencarian, dan kalimat yang dihasilkan. Simpan identitas dokumen serta potongan yang dipakai, tampilkan sitasi yang dapat diklik, dan nyatakan ketidakpastian ketika bukti tidak cukup. Hasil itu tetap bantuan analisis, bukan kewenangan baru. Detail hak akses, dasar pemrosesan, retensi, transfer, dan penghapusan harus ditetapkan pemilik data serta ditinjau secara profesional; paket ini belum memberi fakta proyek untuk mengisinya. **[NEEDS GATE-05 REVIEW: tetapkan dasar pemrosesan, peran pengendali/prosesor, retensi, transfer, dan kewajiban sektor sebelum produksi.]**

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.

## Definisi dan batas objek

Knowledge assistant adalah antarmuka tanya jawab yang membantu orang menemukan dan memahami dokumen terkendali. RAG menambahkan tahap pengambilan (retrieval) sebelum model menyusun jawaban. Sistem ini berbeda dari melatih model dengan seluruh arsip: dokumen tetap berada di penyimpanan yang dikelola organisasi, sedangkan indeks menyimpan representasi untuk pencarian dan hubungan kembali ke sumber.

Objek yang perlu dilacak bukan hanya teks. Untuk setiap dokumen, catat pemilik, otoritas, status berlaku, versi, tanggal efektif, klasifikasi akses, lokasi asli, dan hash atau pengenal perubahan. Potongan teks (chunk) harus membawa identitas itu saat masuk ke indeks. Dengan begitu, pembaca dapat membedakan kebijakan yang berlaku dari draf lama atau salinan yang tidak sah.

Batasnya tegas: sistem tidak membuat dokumen menjadi resmi, tidak memberi izin memakai materi berlisensi atau privat, dan tidak menggantikan persetujuan pemilik data. Di Indonesia, UU No. 27 Tahun 2022 merupakan undang-undang utama tentang pelindungan data pribadi, sedangkan PP No. 71 Tahun 2019 mengatur penyelenggaraan sistem dan transaksi elektronik pada tingkat yang lebih luas ([UU No. 27 Tahun 2022](https://peraturan.bpk.go.id/Details/229798/uu-no-27-tahun-2022), [PP No. 71 Tahun 2019](https://peraturan.bpk.go.id/Details/122030/pp-no-71-tahun-2019)). Keduanya tidak boleh diterjemahkan menjadi kesimpulan hukum spesifik tanpa analisis konteks.

## Cara kerjanya

Mulai dari daftar sumber, bukan dari kotak chat. Pemilik sumber menyetujui koleksi yang boleh dicari. Pipeline ingestion mengambil file dari lokasi resmi, memeriksa format dan malware sesuai kontrol internal, lalu menempelkan metadata versi serta aturan akses. Ketika dokumen dicabut, salinan kerja dan entri indeks diberi status tidak berlaku; jangan biarkan hasil cache menyajikannya.

Berikutnya, pecah dokumen pada batas makna—judul, butir, tabel, atau paragraf—dengan ukuran yang masih menyertakan konteks. Terlalu kecil memutus pengecualian; terlalu besar mengaburkan bagian yang relevan. Simpan nomor halaman, heading, rentang karakter, dan pengenal chunk agar kutipan dapat diverifikasi. Indeks vektor membantu menemukan kemiripan makna; pencarian kata kunci membantu istilah, nomor dokumen, dan frasa wajib. Gabungan keduanya perlu diuji, bukan diasumsikan unggul.

Saat pertanyaan datang, filter hak akses lebih dulu, baru lakukan retrieval. Reranker boleh mengurutkan kandidat, tetapi tidak boleh mengangkat dokumen di luar izin pengguna. Generator menerima kandidat beserta metadata dan instruksi untuk menjawab hanya dari bukti. Lapisan keluaran lalu memeriksa bahwa setiap klaim penting memiliki rujukan, bahwa sitasi menunjuk ke sumber yang sama, dan bahwa jawaban berhenti bila bukti bertentangan atau kosong.

Catat jejak minimal: identitas pengguna atau peran, waktu, versi indeks, ID dokumen dan chunk yang dikirim, keputusan filter, jawaban, serta alasan penolakan. Lindungi log dari akses yang tidak perlu karena pertanyaan juga dapat mengandung data pribadi. Kerangka NIST Privacy Framework membantu menata inventarisasi, komunikasi, pengendalian, dan perlindungan risiko privasi ([NIST Privacy Framework](https://www.nist.gov/privacy-framework)).

## Faktor yang mengubah hasil

Empat lapisan biasanya mengubah kualitas dan risikonya.

1. **Otoritas dan versi.** Sumber resmi yang sudah kedaluwarsa tetap terlihat relevan bagi pencarian semantik. Tetapkan pemilik, tanggal tinjau, status berlaku, dan prosedur pencabutan. Jangan menyimpulkan masa retensi atau hak penghapusan hanya dari nama dokumennya.
2. **Chunk dan indeks.** Struktur dokumen, bahasa, tabel, OCR, dan istilah domain memengaruhi potongan yang ditemukan. Uji pertanyaan yang meminta definisi, pengecualian, perbandingan, serta angka yang harus persis; simpan contoh kegagalan untuk memperbaiki aturan pemecahan.
3. **Akses dan keluaran.** Filter di antarmuka saja tidak cukup. Terapkan izin di retrieval dan storage, lalu uji pengguna dengan peran berbeda. Sitasi harus menyembunyikan bagian yang tidak boleh dilihat, bukan membocorkan judul atau cuplikan sensitif.
4. **Kesegaran dan pemulihan.** Tetapkan pemicu ingestion ulang ketika sumber berubah, indikator umur indeks, dan jalur fallback ketika pipeline gagal. Backup baru menjadi bukti pemulihan setelah restore benar-benar diuji; catat hasil dan batas uji itu.

NIST menekankan bahwa kelancaran keluaran bukan bukti kebenaran. Profil risiko generatifnya menganjurkan pengujian terhadap penggunaan yang dimaksud dan skenario gagal atau disalahgunakan ([NIST AI 600-1 Generative AI Profile](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf)). Karena itu, ukuran keberhasilan harus mencakup ketepatan sumber, penolakan saat bukti kurang, kebocoran akses, dan perilaku ketika dokumen berganti.

## Contoh keputusan praktis

Bayangkan tim mengunggah prosedur operasional, FAQ pelanggan, dan draf kebijakan. Sebelum membuka akses luas, gunakan matriks berikut.

| Situasi yang ditemukan | Keputusan aman | Bukti yang harus disimpan |
|---|---|---|
| Dua dokumen menjawab hal sama dengan versi berbeda | Prioritaskan status berlaku; tampilkan konflik atau minta pemilik mengesahkan | ID, versi, tanggal efektif, keputusan pemilik |
| Pertanyaan meminta data di luar peran pengguna | Tolak retrieval dan jelaskan kanal permintaan akses | Peran, aturan yang memblokir, waktu kejadian |
| Kandidat relevan tetapi sitasi tidak menunjuk lokasi presisi | Jangan menjawab final; minta pencarian ulang atau review manusia | ID chunk, skor/urutan, alasan gagal |
| Sumber dihapus atau ditarik | Hentikan penyajian, hapus salinan sesuai kebijakan yang disahkan, uji cache dan backup | Tiket penghapusan, daftar indeks, hasil verifikasi |

Kawan Codev.id, matriks itu bukan klaim bahwa sistem sudah patuh. Ia adalah daftar keputusan yang harus disetujui pemilik sumber, keamanan, dan fungsi hukum. Kerangka manajemen risiko NIST menempatkan tata kelola, pemetaan, pengukuran, dan pengelolaan sebagai siklus; gunakan hasil pengukuran untuk memutuskan peluncuran bertahap, pembatasan, atau penghentian ([NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)).

Untuk evaluasi, buat set pertanyaan berlabel: jawaban yang diharapkan, sumber otoritatif, potongan pendukung, dan kondisi ketika sistem wajib berkata “tidak cukup bukti”. Tambahkan kasus prompt injection di dalam dokumen, dokumen yang sudah dicabut, sinonim, typo, tabel, serta pertanyaan lintas hak akses. Ulangi set yang sama pada setiap perubahan parser, chunk, embedding, reranker, atau model. Jangan menerbitkan angka akurasi sebelum metode, sampel, dan hasilnya ditinjau.

## Kesalahan umum dan cara memeriksanya

Shortcut paling menggoda adalah mengimpor seluruh folder lalu menambahkan kalimat “jawab berdasarkan dokumen”. Cara itu gagal ketika folder berisi draf, salinan pribadi, atau materi dengan izin berbeda. Perbaikannya adalah allowlist sumber, pemilik yang jelas, metadata wajib, dan pemeriksaan sebelum ingestion.

Kesalahan kedua adalah menyamakan skor kemiripan dengan kebenaran. Periksa apakah chunk memuat pengecualian, apakah versi terbaru menang, dan apakah jawaban mengutip bagian yang benar-benar mendukung klaim. Kesalahan ketiga adalah menampilkan sitasi tanpa jejak versi; tautan yang sama dapat berubah isinya. Simpan snapshot atau pengenal versi sesuai kebijakan yang disahkan.

Kesalahan keempat adalah mengandalkan backup tanpa latihan pemulihan. Jadwalkan restore terkontrol, ukur apakah indeks dan metadata kembali konsisten, lalu dokumentasikan apa yang tidak pulih. Kesalahan kelima adalah membuat reviewer manusia sebagai tombol “setuju” tanpa informasi. Reviewer perlu melihat sumber, konflik, izin, dan pilihan fallback; tanpa itu, kontrol manusia hanya formalitas.

Checklist pemeriksaan sebelum pilot:

- Apakah setiap sumber memiliki pemilik, status berlaku, versi, dan klasifikasi akses?
- Apakah filter izin diterapkan sebelum retrieval dan diuji dengan peran berbeda?
- Apakah setiap klaim penting memiliki sitasi yang membuka lokasi sumber dan versinya?
- Apakah sistem menolak pertanyaan tanpa bukti, sumber konflik, dan dokumen yang dicabut?
- Apakah perubahan sumber, penghapusan, log, backup, dan restore memiliki catatan uji?
- Apakah set evaluasi mencakup kegagalan, penyalahgunaan, dan perubahan pipeline?

NIST SP 800-218A menghubungkan praktik pengembangan aman dengan risiko sistem generatif; gunakan panduan itu untuk menata kontrol sepanjang siklus pengembangan, bukan hanya pemeriksaan saat rilis ([NIST SP 800-218A](https://csrc.nist.gov/pubs/sp/800/218/a/final)).

## Kesimpulan: sumber terlacak adalah mekanisme, bukan label

Knowledge assistant berbasis RAG layak dipertimbangkan ketika organisasi dapat mengendalikan sumber, versi, akses, retrieval, sitasi, dan jalur berhenti. Jawaban yang lancar tetap harus diperlakukan sebagai keluaran yang perlu bukti. Langkah berikutnya: pilih satu koleksi berisiko rendah, buat register sumber dan peran, susun set evaluasi dengan kasus “tidak cukup bukti”, lalu minta pemilik data dan reviewer teknis menandatangani kriteria pilot.

Jika dasar pemrosesan, retensi, transfer, penghapusan, atau perilaku penyedia belum jelas, tunda perluasan koleksi dan tandai keputusan itu untuk review. Untuk menyiapkan langkah lanjutan, gunakan [beranda Codev.id](/) sebagai titik masuk ke konteks layanan yang tersedia. Teman Codev.id, aturan operasionalnya sederhana: tidak ada sumber berwenang dan jejak yang dapat diperiksa, tidak ada jawaban final.

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
