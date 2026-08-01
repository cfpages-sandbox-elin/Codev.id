---
article_id: CDV-14-A02
writing_contract_version: "native-id-v2"
title: "Lab Data, Field Data, dan Core Web Vitals"
slug: "lab-field-data-core-web-vitals"
description: "Distinguish controlled lab diagnosis from real-user distributions, document metric/version/sample context, segment data, and choose follow-up"
status: draft
publication_date: "2026-02-12"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-14
primary_intent: "Interpret performance evidence without mixing populations"
reader_community: "Codev.id"
reader_address: "Teman Codev.id"
final_route: "/artikel/lab-field-data-core-web-vitals.html"
technical_review: required
sources:
  - "https://web.dev/articles/vitals"
  - "https://developer.chrome.com/docs/crux"
  - "https://sre.google/workbook/implementing-slos/"
  - "https://opentelemetry.io/docs/"
  - "https://csrc.nist.gov/pubs/sp/800/61/r3/final"
  - "https://www.rfc-editor.org/rfc/rfc9111"
---

# Lab Data, Field Data, dan Core Web Vitals

Halo, Teman Codev.id! Ketika satu laporan menyebut halaman “lulus” sementara pengguna nyata masih mengeluh lambat, masalahnya sering bukan alat yang rusak. Lab data dan field data memang mengamati populasi serta kondisi yang berbeda. Lab membantu mengisolasi penyebab dalam kondisi terkendali; field data menunjukkan distribusi pengalaman pengguna nyata. Core Web Vitals (CWV) adalah sekumpulan metrik pengalaman yang ditetapkan penyedianya dan dapat berevolusi, bukan cap mutu yang berlaku untuk semua konteks.

Jadi, jangan memilih angka terbaik dari salah satu laporan. Simpan konteks metrik, versi alat, URL atau template yang diuji, kondisi jaringan/perangkat, periode, ukuran sampel, dan segmentasi. Gunakan lab untuk mendiagnosis perubahan; gunakan field untuk menilai apakah perubahan itu terlihat pada pengguna sasaran. Ambang dan definisi yang berubah harus ditinjau ulang sebelum keputusan rilis. **[NEEDS TECHNICAL REVIEW: verifikasi ambang CWV dan versi alat yang berlaku saat publikasi.]**

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

Gambar ini merupakan aset lokal untuk ilustrasi dan bukan dokumentasi proyek tertentu.

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

## Jawaban singkat dan salah paham utama

Lab adalah eksperimen yang diulang dengan input yang dikontrol: perangkat emulasi, lokasi, jaringan, cache, dan langkah navigasi dapat dibuat sama. Karena variabelnya sempit, perbandingan sebelum-sesudah lebih mudah ditafsirkan. Field adalah observasi dari kunjungan nyata; hasilnya berupa sebaran, bukan satu skor universal. Dokumentasi Core Web Vitals di web.dev juga menempatkan metrik ini dalam konteks pengukuran pengguna dan evolusi definisinya ([web.dev Core Web Vitals](https://web.dev/articles/vitals)).

Salah kaprah yang mahal adalah menganggap skor lab sebagai janji pengalaman semua orang, atau menganggap satu persentil field sebagai diagnosis akar masalah. Keduanya mencampur fungsi. Jika field memburuk, lab dapat menguji hipotesis tentang JavaScript, gambar, server, atau cache. Jika lab membaik tetapi field belum berubah, periksa cakupan rilis, distribusi perangkat, dan jeda pengumpulan data sebelum menyimpulkan optimasi gagal.

## Definisi dan batas objek

“Lab data” berarti hasil pengujian sintetis pada skenario yang Anda tetapkan. Catat profil CPU, viewport, jaringan, lokasi pengujian, status cache, URL, tanggal, dan versi runner. “Field data” berarti telemetri dari pengguna nyata dalam rentang waktu tertentu. Chrome UX Report (CrUX), misalnya, mendokumentasikan agregasi pengalaman pengguna Chrome dan cara data tersebut dikumpulkan; ia bukan rekaman lengkap setiap sesi dan bukan pengganti observabilitas internal ([dokumentasi CrUX](https://developer.chrome.com/docs/crux)).

CWV harus diperlakukan sebagai nama metrik dan aturan pengelompokan yang ditentukan penyedia, bukan sinonim “waktu muat”. Jangan menambahkan klaim tentang peringkat pencarian, konversi, energi, atau kepuasan bisnis dari angka ini tanpa bukti proyek yang terpisah. Batas artikel ini adalah interpretasi bukti performa: kita tidak membandingkan dataset yang tidak sepadan dan tidak menjanjikan dampak SEO.

## Cara kerjanya

Mulai dari pertanyaan keputusan. Untuk regresi setelah deploy, jalankan skenario lab yang sama pada commit sebelum dan sesudah perubahan. Untuk dampak pada pengguna, tetapkan periode field yang dapat dibandingkan dan tunggu data terkumpul sesuai karakteristik sumbernya. Simpan artefak mentah, konfigurasi, dan timestamp; jangan hanya menyalin skor ke spreadsheet.

Kemudian hubungkan metrik dengan sinyal. Instrumentasi memberi sinyal tentang layanan dan aplikasi, tetapi sinyal itu sendiri tidak membuat sistem andal. OpenTelemetry menjelaskan praktik pengumpulan dan ekspor telemetri lintas traces, metrics, dan logs ([dokumentasi OpenTelemetry](https://opentelemetry.io/docs/)). Di sisi operasi, SLO adalah tujuan layanan sekaligus mekanisme keputusan, bukan janji uptime kontraktual; tetapkan indikator, jendela waktu, dan tindakan ketika anggaran kesalahan terlampaui ([Google SRE Workbook](https://sre.google/workbook/implementing-slos/)).

Urutan praktisnya: (1) definisikan populasi dan pertanyaan, (2) ukur dengan konfigurasi tercatat, (3) segmentasikan hasil, (4) bentuk hipotesis penyebab, (5) uji hipotesis di lab atau observabilitas, lalu (6) pantau field setelah perubahan. Saat ada insiden atau regresi luas, alur deteksi, respons, dan pembelajaran perlu terdokumentasi; kerangka NIST untuk respons insiden menekankan siklus tersebut ([NIST SP 800-61 Rev. 3](https://csrc.nist.gov/pubs/sp/800/61/r3/final)).

## Faktor yang mengubah hasil

Perbedaan hasil dapat berasal dari beberapa kelompok berikut.

- **Populasi:** negara, jenis koneksi, kelas perangkat, browser, dan pengguna baru versus berulang. Distribusi berubah bila segmen berubah.
- **Kondisi pengujian:** profil CPU, throttling jaringan, lokasi runner, cache, cookie, dan urutan navigasi. Perubahan satu parameter dapat mengubah diagnosis lab.
- **Versi dan implementasi:** runner, browser, definisi metrik, dan pipeline agregasi harus dicatat. CWV bersifat berkembang, sehingga angka lintas periode tidak otomatis sebanding.
- **Cakupan rilis:** field mungkin masih mencampur versi aplikasi sebelum deploy, sedangkan lab menguji build terbaru. Tandai tanggal rilis dan jeda pengumpulan.
- **Jalur cache:** header cache menentukan kapan respons dapat dipakai ulang dan kapan harus divalidasi; aturan HTTP caching dijelaskan dalam RFC 9111 ([RFC 9111](https://www.rfc-editor.org/rfc/rfc9111)). Jangan menyimpulkan “server lambat” sebelum memeriksa cache hit/miss dan invalidasi.

Teman Codev.id, tuliskan semua faktor ini di samping angka. Tanpa konteks, persentil terlihat presisi tetapi tidak dapat ditindaklanjuti.

## Contoh keputusan praktis

Gunakan tabel ringkas ini saat meninjau laporan:

| Temuan | Pertanyaan berikutnya | Tindakan aman |
|---|---|---|
| Lab memburuk, field stabil | Apakah perubahan baru mengenai jalur pengguna yang cukup besar? | Bisection atau profiling lab; jangan klaim dampak pengguna. |
| Lab stabil, field memburuk | Segmen mana yang berubah dan kapan rilisnya? | Pecah menurut perangkat, negara, browser, dan versi aplikasi; cek rollback bila sinyal operasi mendukung. |
| Keduanya memburuk | Apakah ada perubahan kode, origin, jaringan, atau cache? | Bekukan perubahan berisiko, kumpulkan trace/metric/log, lalu uji hipotesis terurut. |
| Satu segmen membaik | Apakah ukuran sampel dan periodenya sebanding? | Pertahankan segmen sebagai bukti lokal; hindari generalisasi ke seluruh pengguna. |

Misalnya, laporan lab pada koneksi lambat menunjukkan perbaikan setelah aset di-cache. Itu mendukung hipotesis mekanisme, bukan bukti semua pengguna menerima manfaat yang sama. Cocokkan dengan hit rate, distribusi perangkat, dan periode field setelah cache benar-benar aktif. Jika data belum cukup, tandai keputusan sebagai “menunggu observasi”, bukan “berhasil”.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah menyatukan median lab dengan persentil field dalam satu grafik tanpa label. Periksa kolom `population`, `conditions`, `period`, dan `metric_definition` sebelum membandingkan. Kedua, mengganti alat atau versi di tengah eksperimen. Simpan versi runner dan browser pada nama artefak.

Ketiga, mengejar satu skor agregat. Minta distribusi per segmen dan ukuran sampel; agregat dapat menyembunyikan ekor pengalaman yang buruk. Keempat, menganggap instrumentasi sebagai bukti sebab. Trace dan log membantu menelusuri jalur, tetapi korelasi tetap perlu diuji dengan perubahan terisolasi.

Kelima, menghapus cache untuk “tes bersih” lalu mengekstrapolasi ke pengguna berulang. Nyatakan apakah tes cold-cache atau warm-cache dan gunakan keduanya bila pertanyaan mencakup dua kondisi. Keenam, menyebut penurunan angka sebagai keberhasilan bisnis. Minta bukti bisnis terpisah dan periode pembanding yang disepakati.

## Mengapa satu dashboard tidak cukup

Shortcut yang sering dipilih adalah memakai satu dashboard vendor sebagai sumber kebenaran tunggal. Dashboard itu mungkin praktis, tetapi dapat menyembunyikan konfigurasi, segmentasi, atau jeda data. Alternatif yang lebih dapat diaudit: ekspor hasil mentah, simpan definisi metrik dan versi, lalu tautkan perubahan ke commit, rilis, dan insiden. Untuk langkah implementasi situs, Anda dapat melanjutkan ke [layanan web development](/web-development) bila memerlukan peninjauan teknis; tautan itu bukan bukti bahwa perubahan tertentu pasti memperbaiki CWV.

## Aturan operasi berikutnya

Lab Data, Field Data, dan Core Web Vitals menjawab pertanyaan berbeda: lab mengisolasi mekanisme dalam kondisi yang dikendalikan, field memperlihatkan distribusi pengalaman nyata, dan CWV memberi bahasa metrik yang definisinya perlu dipantau. Pisahkan populasi, kondisi, versi, periode, dan sampel sebelum menilai tren.

Langkah berikutnya, buat satu lembar pengukuran berisi URL/template, commit dan tanggal rilis, profil lab, segmen field, definisi metrik, ukuran sampel, serta keputusan yang diizinkan. Tinjau lembar itu bersama penanggung jawab teknis sebelum mengubah ambang atau menyatakan regresi selesai. Kawan Codev.id, jadikan aturan operasi Anda sederhana: tidak ada angka tanpa konteks, tidak ada sebab tanpa uji, dan tidak ada jaminan di luar bukti yang benar-benar terkumpul.
