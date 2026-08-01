---
article_id: CDV-03-A05
title: "Merancang Kapasitas dan Resiliensi dari Skenario"
slug: "kapasitas-dan-resiliensi-dari-skenario"
description: "Panduan menerjemahkan asumsi lalu lintas, kegagalan dependensi, penurunan layanan, pemulihan, uji kapasitas, dan pemicu peninjauan menjadi kebutuhan arsitektur"
status: draft
publication_date: "2025-05-26"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-03
primary_intent: "Translate demand and failure assumptions into architecture requirements"
reader_community: "Codev.id"
reader_address: "Sobat Codev.id"
final_route: "/artikel/kapasitas-dan-resiliensi-dari-skenario.html"
technical_review: required
writing_contract_version: "native-id-v2"
sources:
  - "https://docs.aws.amazon.com/prescriptive-guidance/latest/architectural-decision-records/adr-process.html"
  - "https://html.spec.whatwg.org/"
  - "https://www.rfc-editor.org/rfc/rfc9110"
  - "https://www.cisa.gov/sbom"
  - "https://csrc.nist.gov/pubs/sp/800/161/r1/final"
  - "https://securityscorecards.dev/"
---

# Merancang Kapasitas dan Resiliensi dari Skenario

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
Halo, Sobat Codev.id! Kapasitas dan resiliensi sebaiknya dirancang dari skenario yang dapat diuji, bukan dari tebakan jumlah server atau janji uptime. Mulailah dengan beban normal dan puncak, alur data, kegagalan dependensi, mode degradasi, serta target pemulihan. Dari sana barulah kebutuhan arsitektur dan eksperimen kapasitas ditulis.

Kesimpulan itu masih bersyarat. Tanpa data traffic, ukuran payload, pola batch, dan bukti batas layanan yang dipakai, angka kapasitas hanyalah asumsi. Mulai dari konteks layanan di [Codev.id](/) bila Anda perlu menyamakan istilah dengan tim. [NEEDS GATE-02: validasi asumsi beban, dependensi, dan target pemulihan dengan pemilik sistem sebelum keputusan arsitektur disahkan.]

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

Ilustrasi umum dari aset lokal codev.id; bukan dokumentasi proyek tertentu.

## Jawaban singkat dan salah paham utama

Resiliensi bukan berarti sistem tidak pernah gagal. Resiliensi berarti layanan memiliki perilaku yang dipilih ketika komponen gagal: menolak dengan jelas, memakai data terakhir yang aman, mengantrekan pekerjaan, atau memulihkan alur setelah dependensi kembali. Kapasitas pun bukan satu angka. Ia adalah hubungan antara laju masuk, biaya pemrosesan, batas sumber daya, dan tingkat layanan yang masih diterima.

Kesalahan paling mahal adalah menguji hanya jalur sukses pada beban rata-rata. Uji harus memasukkan lonjakan, retry, antrean yang menumpuk, dan kegagalan satu dependensi. Pilihan arsitektur—monolit, modular, server-rendered, client-rendered, CMS, atau serverless—adalah opsi dengan trade-off, bukan peringkat kematangan. Catat alasan, konsekuensi, dan kondisi pembatalannya dalam decision record; panduan AWS menjelaskan pola dokumentasi keputusan semacam itu ([AWS ADR guidance](https://docs.aws.amazon.com/prescriptive-guidance/latest/architectural-decision-records/adr-process.html)).


## Definisi dan batas objek

Objek perancangan di sini adalah asumsi operasional yang diterjemahkan menjadi persyaratan: berapa permintaan per detik, berapa ukuran dan pertumbuhan data, pekerjaan mana yang sinkron, serta apa yang boleh ditunda. Batasnya jelas: artikel ini tidak menetapkan SLO produksi, menjamin uptime, atau menggantikan diagnosis beban di lingkungan nyata. Target SLO dan analisis insiden memerlukan keputusan serta bukti yang dibedakan dengan jelas; pemiliknya dapat sama atau berbeda sesuai struktur tanggung jawab organisasi.

Untuk web, definisikan perilaku protokol secara eksplisit—status respons, cache, timeout, dan retry—dengan merujuk semantik HTTP RFC 9110 ([RFC 9110](https://www.rfc-editor.org/rfc/rfc9110)). Struktur dokumen dan parsing mengikuti standar HTML Living Standard ([WHATWG HTML](https://html.spec.whatwg.org/)); standar tersebut membantu konsistensi perilaku, bukan bukti bahwa aplikasi Anda telah tahan beban.


## Cara kerjanya

Urutkan pekerjaan dalam enam langkah. Pertama, tulis skenario permintaan: pengguna aktif bersamaan, laju puncak, durasi puncak, distribusi endpoint, ukuran payload, dan pertumbuhan data. Bedakan trafik manusia, webhook, batch, dan retry.

Kedua, petakan rantai dependensi. Untuk setiap database, cache, queue, API pihak ketiga, DNS, atau penyedia identitas, catat timeout, kuota, mode gagal, dan pemilik keputusan. Ketiga, tentukan kontrak degradasi: fitur apa yang dimatikan, data apa yang boleh stale, dan operasi mana yang harus tetap aman.

Keempat, tetapkan tujuan pemulihan sebagai pertanyaan keputusan: berapa banyak data yang boleh hilang, berapa lama pemulihan boleh berlangsung, dan siapa yang mengaktifkan prosedur. Jangan mengubahnya menjadi angka “standar” tanpa persetujuan pemilik risiko.

Kelima, buat uji kapasitas berlapis: baseline, kenaikan bertahap, spike, soak, dan pemulihan setelah kegagalan. Ukur latency, error, saturation, backlog, serta biaya; simpan konfigurasi dan versi aplikasi agar hasil dapat diulang. Keenam, catat keputusan dan pemicu review—misalnya perubahan volume, payload, dependensi, atau pola retry.


## Faktor yang mengubah hasil

Hasil berubah ketika asumsi input berubah. Payload besar dapat menekan bandwidth dan memori walau jumlah request tetap. Operasi tulis yang bersifat bursty mengubah kebutuhan queue dan database. Retry tanpa batas dapat menggandakan beban saat layanan mitra lambat. Perubahan skema, library, atau penyedia juga menambah jalur kegagalan.

Inventaris dependensi dengan SBOM agar komponen dan versinya terlihat, tetapi jangan menyamakan transparansi dengan keamanan; CISA menempatkan SBOM sebagai sarana visibilitas ([CISA SBOM](https://www.cisa.gov/sbom)). Penilaian rantai pasok perlu konteks proses dan vendor menurut NIST SP 800-161 Rev.1 ([NIST](https://csrc.nist.gov/pubs/sp/800/161/r1/final)). Skor repositori OpenSSF Scorecard hanyalah sinyal untuk ditindaklanjuti, bukan due diligence lengkap ([OpenSSF Scorecard](https://securityscorecards.dev/)). Kuota API, syarat vendor, subprosesor, dan kerentanan terkini harus diverifikasi langsung sebelum keputusan final.


## Contoh keputusan praktis

Bayangkan proses checkout menerima lonjakan singkat. Jika pembayaran adalah dependency wajib, sistem dapat menahan order dalam status “menunggu konfirmasi”, membatasi retry dengan backoff, dan menyediakan jalur rekonsiliasi. Jika katalog gagal, katalog terakhir yang diberi penanda waktu mungkin masih boleh ditampilkan; keputusan itu harus disetujui pemilik bisnis.

Gunakan tabel sederhana berikut untuk memaksa asumsi menjadi keputusan:

| Skenario | Sinyal yang diamati | Respons yang dipilih | Bukti yang harus dicari |
|---|---|---|---|
| Beban puncak | latency naik, CPU jenuh | scale atau batasi trafik | hasil spike test dan batas resource |
| API mitra timeout | timeout dan retry meningkat | antrekan, fallback, atau tolak | kontrak timeout, kuota, dan rekonsiliasi |
| Database read-only | error tulis, backlog naik | hentikan fitur tulis tertentu | prosedur pemulihan dan kehilangan data yang diterima |
| Deploy mengubah payload | error parsing lintas versi | kompatibilitas bertahap | contract test dan rollback |

Kawan Codev.id, tulis siapa yang berwenang memilih tiap baris. Tanpa pemilik, mode degradasi mudah berubah menjadi keputusan darurat yang tidak konsisten.


## Kesalahan umum dan cara memeriksanya

“Tambah instance saja” gagal bila bottleneck berada pada kuota API, lock database, atau antrean. “Pakai cache untuk semuanya” gagal bila data stale tidak boleh dipakai. “Skor keamanan tinggi berarti aman” mengabaikan konfigurasi runtime dan proses respons insiden. Periksa dengan pertanyaan berikut:

- Apakah setiap asumsi beban memiliki sumber data, rentang ketidakpastian, dan tanggal review?
- Apakah setiap dependency memiliki timeout, budget retry, fallback, serta pemilik kontrak?
- Apakah uji memverifikasi pemulihan, bukan hanya angka throughput?
- Apakah hasil uji menyebut versi kode, konfigurasi, dataset, dan batas yang ditemukan?
- Apakah SBOM dan evaluasi vendor diperbarui ketika komponen berubah?

Shortcut yang paling menggoda adalah menetapkan target kapasitas dari benchmark vendor. Benchmark itu dapat membantu membentuk hipotesis, tetapi tidak mewakili payload, konkurensi, dan kegagalan sistem Anda. Ulangi pengukuran pada lingkungan yang terkontrol, lalu minta review teknis sebelum mengunci rancangan.


## Penutup: ubah skenario menjadi dokumen kerja

Jawaban untuk “merancang kapasitas dan resiliensi dari skenario” adalah membuat paket kecil yang dapat ditinjau: lembar asumsi beban, peta dependensi, matriks mode degradasi, tujuan pemulihan, rencana uji, dan decision record. Sobat Codev.id, jadwalkan review setiap kali pola trafik, payload, dependency, atau risiko bisnis berubah.

Langkah berikutnya: minta pemilik produk dan operasi menyetujui asumsi serta batas kehilangan data, jalankan uji bertahap dengan konfigurasi tercatat, lalu simpan hasil dan pemicu eskalasi. Dokumen ini membantu menerjemahkan skenario menjadi kebutuhan arsitektur dan dapat diperdalam melalui daftar artikel; ia tidak menjamin uptime dan tidak menggantikan SLO produksi, diagnosis beban, atau persetujuan teknis proyek.
