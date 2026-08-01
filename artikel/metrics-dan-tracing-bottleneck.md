---
article_id: CDV-12-A03
title: "Metrics dan Tracing untuk Mencari Bottleneck"
slug: "metrics-dan-tracing-bottleneck"
description: "Panduan memilih metrik dan penelusuran untuk menemukan sumber perlambatan layanan tanpa mengubah dugaan menjadi kepastian."
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2025-12-29"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-12
primary_intent: "Choose telemetry that explains service behavior"
reader_community: "Codev.id"
reader_address: "Kawan Codev.id"
final_route: "/artikel/metrics-dan-tracing-bottleneck.html"
technical_review: required
sources:
  - "https://sre.google/workbook/implementing-slos/"
  - "https://opentelemetry.io/docs/"
  - "https://web.dev/articles/vitals"
---
# Metrics dan Tracing untuk Mencari Bottleneck

Halo, Kawan Codev.id! Ketika satu permintaan pengguna terasa lambat, menambah dashboard atau menaikkan kapasitas sering menjadi jalan pintas yang mahal. Jalan yang lebih berguna adalah memakai metrics untuk mengetahui *di mana* gejala terjadi dan tracing untuk mengikuti *mengapa* satu permintaan melewati jalur yang lambat.

Singkatnya, pilih sedikit metrik yang mewakili pengalaman pengguna dan kesehatan layanan, lalu hubungkan metrik itu dengan trace yang membawa konteks lintas layanan. Dengan begitu, tim dapat membedakan apakah keterlambatan terjadi di worker, layanan asal, kueri data, atau panggilan dependensi. Jawaban ini dapat berubah oleh cakupan trafik, versi aplikasi, pola sampling, dan data yang benar-benar terkumpul; telemetry memberi bukti untuk menyelidiki, bukan jaminan bahwa layanan sudah andal.

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.

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

Bottleneck bukan otomatis komponen dengan angka waktu terbesar pada satu layar. Ia adalah bagian dari alur yang, pada kondisi tertentu, membatasi permintaan sampai pengguna menerima respons. Karena itu satu angka rata-rata tidak cukup: rata-rata dapat menyamarkan sebagian kecil permintaan yang sangat lambat, sementara keluhan pengguna justru datang dari bagian tersebut.

Mulailah dari pertanyaan keputusan: “Pengalaman pengguna mana yang sedang kita lindungi, dan jalur mana yang harus dijelaskan ketika pengalaman itu memburuk?” Google SRE menjelaskan SLO sebagai sasaran layanan yang membantu mengambil keputusan, bukan janji uptime kontraktual. [Panduan SLO Google SRE](https://sre.google/workbook/implementing-slos/) berguna sebagai kerangka untuk menyepakati sinyal dan batas yang akan dipantau sebelum memilih grafik.

## Definisi dan batas objek

Metrics adalah pengukuran agregat yang dicatat berulang, misalnya jumlah permintaan, tingkat kegagalan, atau distribusi durasi. Ia menjawab pertanyaan luas: apakah gejala meningkat, pada layanan mana, dan sejak kapan? Tracing adalah catatan satu perjalanan permintaan yang dipecah menjadi *span*—unit kerja seperti handler worker, pemanggilan API, atau akses data. Trace dan span yang memiliki konteks bersama membuat urutan lintas dependensi dapat ditelusuri.

OpenTelemetry menyediakan konsep dan dokumentasi untuk menghasilkan serta mengirim sinyal observability, termasuk traces dan metrics. [Dokumentasi OpenTelemetry](https://opentelemetry.io/docs/) tidak menjadikan semua instrumentasi otomatis tepat; tim tetap perlu menentukan nama operasi, atribut, dan tujuan pemeriksaannya.

Artikel ini membahas cara memilih telemetry agar perilaku layanan dapat dijelaskan. Ia bukan diagnosis performa sebuah aplikasi tertentu, bukan rekomendasi kuota layanan, dan bukan klaim bahwa perubahan tertentu akan mempercepat sistem. Untuk langkah awal yang lebih umum, Anda dapat melihat [halaman utama Codev.id](/) dan membawa daftar pertanyaan dari artikel ini ke konteks sistem Anda sendiri.

## Cara kerjanya

Urutannya paling mudah dipakai dari luar ke dalam. Pertama, tetapkan metrik pengguna atau layanan: misalnya keberhasilan menyelesaikan aksi, latensi respons pada jalur penting, dan kegagalan yang terlihat pengguna. Untuk antarmuka web, metrik pengalaman lapangan dan lab perlu dibedakan; Core Web Vitals sendiri merupakan sekumpulan metrik yang didefinisikan dan dapat berkembang oleh penyedianya. [Penjelasan Core Web Vitals](https://web.dev/articles/vitals) membantu menempatkan metrik tersebut sebagai sinyal pengalaman, bukan bukti tunggal penyebab masalah.

Kedua, buat metrics layanan yang membantu mempersempit gejala: volume permintaan, error, durasi, dan pemakaian sumber daya yang benar-benar relevan. Beri label dengan hemat. Label seperti nama endpoint atau jenis hasil dapat membantu pemecahan masalah; label yang nilainya berubah hampir setiap permintaan—ID pengguna, ID pesanan, atau URL mentah—membuat cardinality (banyaknya kombinasi nilai deret waktu) melonjak. Akibatnya pencarian, penyimpanan, dan biaya observability bisa membesar tanpa memperjelas keputusan.

Ketiga, saat metrik menunjukkan perubahan, buka trace yang mewakili waktu dan hasil tersebut. Context propagation memastikan pemanggilan berikutnya tetap terkait dengan trace yang sama. Di sana tim membandingkan span: apakah waktu habis sebelum panggilan keluar, pada dependensi, atau setelah respons kembali. Kawan Codev.id, jangan menyimpulkan penyebab hanya karena span paling panjang; periksa juga apakah span itu memang berada di jalur kritis, terjadi berulang, dan selaras dengan gejala pada metrics.

Hubungan ini juga membantu membedakan alarm dari diagnosis. Alarm dapat berangkat dari metric yang sudah disepakati, misalnya perubahan keberhasilan atau durasi pada operasi penting. Trace bukan alasan untuk membunyikan alarm bagi setiap permintaan, melainkan bahan yang dicari ketika alarm atau laporan pengguna membutuhkan penjelasan. Dengan pemisahan ini, dashboard tetap ringkas dan penyelidikan tetap memiliki jejak yang dapat diperiksa.

Keempat, gunakan sampling dengan tujuan jelas. Menyimpan semua trace mungkin berguna dalam kondisi terbatas, tetapi volume, biaya, dan data sensitif perlu dipertimbangkan. Sampling probabilistik dapat memberi gambaran umum; sampling berbasis keputusan dapat mempertahankan trace gagal atau lambat jika aturan dan konteksnya dirancang. Apa pun pendekatannya, dokumentasikan trace mana yang mungkin hilang. Tanpa catatan itu, ketiadaan trace mudah keliru dianggap sebagai ketiadaan masalah.

## Faktor yang mengubah hasil

Nilai telemetry bergantung pada bentuk lalu lintas dan desain sistem. Jalur yang jarang dipakai tetapi penting bagi pengguna dapat luput jika hanya memantau volume. Sebaliknya, dashboard yang dipecah terlalu rinci sulit dibaca ketika insiden berlangsung. Pilih dashboard sebagai alat menjawab pertanyaan operasional, bukan sebagai tempat memajang seluruh metrik yang tersedia.

Versi aplikasi, konfigurasi cache, lokasi pengguna, jenis perangkat, dan perubahan dependensi juga dapat mengubah hasil pengukuran. Perbandingan sebelum–sesudah baru layak dipakai jika ruang lingkup, sampel, kondisi, versi, dan keterbatasannya dicatat. Sobat Codev.id, tanpa pembanding yang stabil, grafik yang membaik belum membuktikan perubahan kode sebagai penyebabnya.

Biaya dan privasi turut membatasi rancangan. Jangan menaruh identitas pribadi, token, isi formulir, atau payload lengkap ke atribut trace demi kenyamanan pencarian. Buat daftar atribut yang diizinkan, lalu tinjau kembali saat kontrak API atau alur data berubah. Dalam layanan berbasis worker dan dependensi terdistribusi, konteks yang tidak diteruskan pada satu batas layanan saja sudah cukup untuk membuat trace tampak terputus.

## Contoh keputusan praktis

Bayangkan sebuah aksi pengguna melewati worker, API asal, lalu layanan data. Ini bukan laporan hasil sistem nyata, melainkan pola keputusan yang dapat diuji.

| Gejala yang terlihat | Telemetry awal | Pertanyaan lanjutan | Keputusan sementara |
| --- | --- | --- | --- |
| Kegagalan aksi naik | rasio keberhasilan per operasi | apakah gagal terkonsentrasi pada satu rute atau hasil tertentu? | buka trace gagal dan cek batas dependensi |
| Sebagian respons melambat | distribusi durasi jalur penting | apakah perlambatan muncul pada rentang waktu dan operasi yang sama? | bandingkan trace lambat dengan trace normal |
| Trace mahal atau sulit dicari | volume span dan kombinasi atribut | atribut mana yang bernilai unik hampir di setiap permintaan? | hapus atau normalkan label bernilai tinggi; tinjau sampling |
| Dashboard ramai tetapi tak memberi tindakan | metrik panel yang ada | keputusan apa yang dapat dibuat dari masing-masing panel? | pertahankan panel yang menjawab pertanyaan, arsipkan sisanya |

Contohnya, jika metrik menunjukkan lonjakan durasi pada satu operasi, ambil beberapa trace dari rentang waktu yang sama dan bandingkan urutan span dengan permintaan normal. Jika span dependensi sering mendominasi jalur kritis, temuan yang tepat adalah “dependensi ini perlu pemeriksaan lebih lanjut pada kondisi yang tercatat”, bukan “dependensi pasti penyebab tunggal”. Teman Codev.id, bahasa yang hati-hati menjaga tim agar tidak mengubah korelasi menjadi kepastian.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah mengukur segala hal sebelum mengetahui pertanyaannya. Periksa setiap metrik dengan kalimat sederhana: tindakan apa yang akan berubah bila angka ini naik atau turun? Bila tidak ada jawaban, metrik itu belum tentu pantas masuk dashboard utama.

Kesalahan kedua adalah memakai rata-rata sebagai satu-satunya ringkasan latensi. Tambahkan pemisahan berdasarkan operasi dan hasil, lalu lihat distribusi serta contoh trace. Kesalahan ketiga adalah memakai trace sebagai log payload. Trace perlu cukup konteks untuk mengikuti alur, tetapi bukan gudang data mentah atau data pribadi.

Shortcut yang sering menggoda adalah “aktifkan semua instrumentasi dan simpan semua trace; nanti pasti ketemu.” Ini dapat gagal karena volume serta cardinality menaikkan biaya dan kebisingan, sedangkan konteks yang tidak konsisten tetap meninggalkan celah. Alternatif yang lebih aman ialah memulai dari satu atau dua perjalanan pengguna, mendefinisikan atribut minimum, menguji propagasi konteks di setiap batas, kemudian mengevaluasi sampling berdasarkan pertanyaan yang belum terjawab.

Sebelum mengubah sistem, gunakan pemeriksaan ini:

- Apakah metric pengguna, metric layanan, dan trace merujuk pada operasi yang sama?
- Apakah trace membawa konteks melewati worker, API, dan dependensi yang relevan?
- Apakah atributnya cukup untuk menyaring masalah tanpa memasukkan data sensitif atau nilai unik per permintaan?
- Apakah sampel dan kondisi pembanding dicatat sebelum menyatakan ada regresi atau perbaikan?
- Apakah setiap panel dashboard berakhir pada pemilik dan tindakan yang jelas?

## Langkah berikutnya

Metrics dan tracing efektif untuk mencari bottleneck ketika keduanya dipakai sebagai rangkaian bukti: metric menunjukkan perubahan yang berdampak, trace menjelaskan jalur permintaan yang berkaitan, lalu tim menguji dugaan pada kondisi yang terdokumentasi. Jangan memulai dari jumlah dashboard atau klaim peningkatan; mulailah dari satu perjalanan pengguna dan satu keputusan yang harus dapat diambil ketika sinyalnya berubah.

Buatlah daftar operasi penting, definisi keberhasilannya, atribut minimum yang aman, serta aturan sampling yang dapat diaudit. Kawan Codev.id, bila data yang tersedia belum mencakup sampel, kondisi, atau konteks lintas layanan yang diperlukan, catat temuan sebagai dugaan dan lakukan peninjauan teknis sebelum menyebutnya bottleneck.
