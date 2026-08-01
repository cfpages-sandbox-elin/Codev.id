---
article_id: CDV-16-A04
title: "Evaluation Set untuk Prompt, Model, dan Workflow AI"
slug: "evaluation-set-prompt-model-workflow-ai"
description: "Panduan menyusun kasus uji berversi, rubrik, slice, uji adversarial dan abstention, penilaian manusia, latency, biaya, ambang lulus, serta bukti regresi untuk prompt, model, dan workflow AI."
status: draft
publication_date: "2026-04-07"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-16
primary_intent: "Test AI behavior against representative tasks and failure modes"
reader_community: "Codev.id"
reader_address: "Teman Codev.id"
final_route: "/artikel/evaluation-set-prompt-model-workflow-ai.html"
technical_review: required
writing_contract_version: "native-id-v2"
sources:
  - "https://peraturan.bpk.go.id/Details/229798/uu-no-27-tahun-2022"
  - "https://peraturan.bpk.go.id/Details/122030/pp-no-71-tahun-2019"
  - "https://www.nist.gov/privacy-framework"
  - "https://www.nist.gov/itl/ai-risk-management-framework"
  - "https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf"
  - "https://csrc.nist.gov/pubs/sp/800/218/a/final"
---

# Evaluation Set untuk Prompt, Model, dan Workflow AI

Halo, Teman Codev.id!

Evaluation set bukan kumpulan beberapa contoh prompt yang terlihat bagus di demo. Ia adalah kumpulan kasus uji yang berversi untuk memeriksa apakah prompt, model, dan workflow menghasilkan perilaku yang dapat diterima pada tugas nyata—termasuk saat input tidak lengkap, berbahaya, atau di luar kewenangan. Untuk pindah dari demo ke pilot terkendali, buat kasus representatif, jawaban acuan atau rubrik, pembagian slice, uji adversarial dan abstention, penilaian manusia, serta catatan latency dan biaya. Tetapkan ambang lulus sebelum membandingkan versi.

Jawaban dapat berubah ketika risiko domain, pemilik keputusan, atau data yang dipakai berubah. Tidak ada angka akurasi universal yang aman untuk semua penggunaan. Reviewer domain harus menyetujui definisi “cukup baik”, jalur eskalasi, dan kondisi berhenti. NIST menekankan bahwa keluaran yang fasih bukan bukti kebenaran; evaluasi perlu mencakup tugas yang dimaksud dan skenario kegagalan atau penyalahgunaan ([NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)).

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

Ilustrasi umum dari aset lokal codev.id; bukan dokumentasi proyek tertentu.

## Definisi dan batas objek

Anggap evaluation set sebagai kontrak pengujian, bukan laporan promosi model. Setiap kasus minimal menyimpan ID stabil, versi, input, konteks yang diizinkan, keluaran yang diharapkan, kriteria penilaian, dan keputusan lulus atau gagal. Versi prompt, model, parameter, alat yang dipanggil, dan tanggal eksekusi juga harus tercatat agar hasil dapat diulang.

Objeknya mencakup tiga lapis. Pada lapis prompt, Anda menguji instruksi, format, contoh, dan aturan abstention. Pada lapis model, Anda menguji kemampuan, batas konteks, konsistensi, dan respons terhadap input bermusuhan. Pada lapis workflow, Anda menguji routing, retrieval, tool call, persetujuan manusia, fallback, logging, serta dampak latency dan biaya. Uji salah satu lapis saja dapat menyembunyikan kegagalan di lapis lain.

Batasnya penting: set ini tidak menetapkan dasar hukum pemrosesan data, izin transfer, masa retensi, atau kewajiban notifikasi untuk proyek tertentu. UU No. 27 Tahun 2022 merupakan rujukan utama nasional tentang pelindungan data pribadi, sedangkan PP No. 71 Tahun 2019 mengatur konteks sistem dan transaksi elektronik yang lebih luas ([UU No. 27 Tahun 2022](https://peraturan.bpk.go.id/Details/229798/uu-no-27-tahun-2022), [PP No. 71 Tahun 2019](https://peraturan.bpk.go.id/Details/122030/pp-no-71-tahun-2019)). Untuk keputusan tersebut, minta peninjauan yang memahami konteks organisasi: [NEEDS GATE-05 REVIEW: dasar pemrosesan, peran pengendali/prosesor, transfer, retensi, penghapusan, backup, dan notifikasi belum ditetapkan dalam paket ini].

## Cara kerjanya

Mulai dari unit kerja, bukan dari model yang ingin dipakai. Tulis pertanyaan: keputusan apa yang dibantu, siapa yang boleh mempercayai keluaran, dan kerugian apa yang terjadi bila sistem salah atau diam-diam mengarang. Dari sini, buat katalog kasus dengan skema yang tetap. Contoh kolomnya: `case_id`, `task_type`, `risk_level`, `input`, `allowed_context`, `reference`, `rubric`, `expected_action`, `slice`, dan `owner`.

Reference dapat berupa jawaban benar, kutipan sumber, label kelas, atau daftar kondisi yang wajib disebut. Jika jawaban tidak tunggal, gunakan rubric (rubrik)—aturan penilaian yang memisahkan ketepatan fakta, kelengkapan, kepatuhan format, dan tindakan aman. Simpan alasan skor, bukan hanya angka. Untuk kasus berisiko, domain reviewer harus dapat menandai “tidak dapat dinilai” dan menjelaskan bukti yang kurang.

Pisahkan set menjadi data pengembangan, validasi, dan holdout. Holdout tidak boleh terus-menerus dipakai untuk mengubah prompt; bila bocor, angka regresi terlihat baik tetapi tidak lagi mewakili penggunaan baru. Beri label slice, misalnya bahasa, jenis dokumen, panjang input, kelompok pengguna, tingkat ambiguitas, dan kondisi alat gagal. Slice membuat tim melihat apakah rata-rata menyembunyikan kegagalan pada kelompok kecil.

Jalankan baseline terlebih dahulu. Bandingkan prompt atau model baru dengan versi yang sedang dipakai menggunakan input dan konfigurasi yang sama. Catat latency per kasus, error rate, pemakaian token atau unit biaya yang tersedia, serta jumlah intervensi manusia. NIST AI 600-1 mendorong evaluasi terhadap risiko khas AI generatif, termasuk keluaran yang menyesatkan dan penyalahgunaan ([NIST AI 600-1 Generative AI Profile](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf)).

Terakhir, bekukan artefak: manifest kasus, versi rubric, konfigurasi runtime, hasil mentah, ringkasan per slice, dan keputusan reviewer. Inilah bukti regresi. Bila sebuah kasus berubah, buat versi baru dan catat alasan perubahan; jangan menimpa hasil lama.

## Faktor yang mengubah hasil

Representasi tugas paling menentukan. Set yang hanya berisi pertanyaan mudah akan mengukur kefasihan, bukan keandalan. Masukkan kasus normal, kasus batas, input kosong atau ambigu, konflik instruksi, permintaan data sensitif, dan permintaan di luar kewenangan. Untuk sistem dengan retrieval atau tool, masukkan sumber hilang, hasil usang, timeout, respons tool yang salah format, dan kredensial yang tidak tersedia.

Slice juga memengaruhi keputusan. Nilai rata-rata boleh lulus sementara satu slice kritis gagal. Tetapkan sejak awal slice mana yang memiliki ambang terpisah dan siapa yang dapat menerima pengecualian. Jangan menggabungkan label “aman” dengan “benar”: jawaban boleh faktual tetapi tetap tidak boleh diberikan karena otorisasi atau privasi belum jelas.

Desain adversarial test secara terarah. Uji prompt injection, instruksi yang saling bertentangan, permintaan untuk mengungkap konteks internal, dan pola yang mendorong model menebak. Uji abstention dengan pertanyaan yang memang tidak dapat dijawab dari konteks tersedia. Skor abstention bukan sekadar “menolak”; reviewer memeriksa apakah penolakan menjelaskan batas, meminta informasi yang tepat, dan menawarkan jalur aman.

Faktor operasional sering mengubah hasil yang tampak bagus di notebook. Antrian panjang menaikkan latency, retry dapat menggandakan biaya, dan perubahan provider dapat menggeser perilaku. NIST SP 800-218A menghubungkan praktik pengembangan perangkat lunak yang aman dengan pengujian, pelacakan risiko, dan pemeliharaan sepanjang siklus hidup ([NIST SP 800-218A](https://csrc.nist.gov/pubs/sp/800/218/a/final)). Gunakan prinsip itu untuk menyimpan jejak perubahan dan menguji fallback, tanpa menganggapnya sebagai bukti kepatuhan proyek tertentu. Sobat Codev.id, perlakukan setiap perubahan konfigurasi sebagai perubahan yang perlu diuji ulang, bukan sekadar pengaturan kecil.

Data pribadi memerlukan pemetaan terpisah: data apa yang masuk, siapa yang dapat melihat hasil, berapa lama artefak disimpan, dan bagaimana pemulihan diuji. NIST Privacy Framework dapat membantu menyusun pertanyaan tata kelola dan pengamanan ([NIST Privacy Framework](https://www.nist.gov/privacy-framework)). Namun, konfigurasi backup atau log tidak otomatis membuktikan pemulihan berhasil; lakukan restore test dan simpan buktinya. [NEEDS GATE-05 REVIEW: keputusan pengumpulan, minimisasi, retensi, penghapusan, dan pemulihan harus ditinjau berdasarkan sistem serta sektor yang sebenarnya].

## Contoh keputusan praktis

Bayangkan tim menguji asisten yang merangkum tiket dukungan. Mereka membuat tiga slice: tiket rutin, tiket dengan data pribadi, dan tiket yang meminta tindakan di luar kewenangan. Setiap kasus memiliki ringkasan acuan, rubric untuk fakta dan kelengkapan, serta aturan bahwa sistem harus abstain ketika tiket tidak menyertakan bukti.

Gunakan tabel keputusan sederhana berikut.

| Temuan pada set | Keputusan sementara | Bukti lanjutan |
|---|---|---|
| Rata-rata baik, slice data pribadi gagal | Tahan pilot pada slice itu | Review domain, aturan akses, dan pengujian redaksi |
| Fakta benar, tetapi model mengeksekusi tool tanpa otorisasi | Nonaktifkan tool atau wajibkan persetujuan | Log otorisasi, kasus penolakan, dan jalur fallback |
| Abstention terlalu sering pada tiket rutin | Jangan langsung menaikkan kreativitas prompt | Periksa kualitas konteks, rubric, dan contoh kasus |
| Skor sama, latency dan biaya naik | Pilih versi hanya bila dampak bisnis diterima | Distribusi latency, biaya per tugas, dan volume nyata |

Angka ambang di tabel itu sengaja tidak ditetapkan. Pemilik proses harus menentukan tingkat kesalahan yang dapat diterima dengan mempertimbangkan dampak dan kapasitas review. Teman Codev.id, tulis keputusan “lulus bersyarat” secara eksplisit: slice mana yang ditutup, siapa approver-nya, tanggal kedaluwarsa, dan bukti yang harus ada sebelum dibuka.

Untuk pilot, jalankan holdout dan sebagian traffic paralel bila aman. Bandingkan versi berdasarkan case ID yang sama, lalu minta reviewer memeriksa sampel yang berubah paling besar. Jangan memakai skor model sebagai satu-satunya juri; penilaian manusia diperlukan untuk kasus ambigu, bahaya, atau konsekuensi tinggi. Reviewer harus memiliki informasi, waktu, dan wewenang untuk menghentikan alur, bukan hanya memberi komentar setelah keputusan dibuat.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah memakai sepuluh contoh yang dipilih karena hasilnya bagus. Periksa asal kasus: apakah mewakili pekerjaan harian, ekor distribusi, dan penyalahgunaan yang masuk akal? Kedua, mencampur data yang dipakai menyetel prompt dengan holdout. Periksa manifest dan riwayat perubahan; kasus holdout seharusnya memiliki pemilik serta akses terbatas.

Kesalahan ketiga adalah rubric satu angka tanpa alasan. Minta reviewer menandai dimensi yang gagal dan bukti rujukannya. Keempat, hanya menguji jawaban final. Periksa juga tool call, sumber yang dipakai, alasan abstention, dan tindakan fallback. Kelima, menganggap model self-grading sebagai validasi. Bandingkan penilaian model dengan reviewer domain dan selidiki perbedaan besar.

Kesalahan keenam adalah menetapkan ambang sebelum memahami dampak. Tanyakan: apa yang terjadi pada satu kegagalan? Apakah ada approval manusia, cara membatalkan, dan rekam audit? Jika belum, angka lulus tidak cukup untuk membuka pilot. Kesalahan terakhir adalah menghapus kasus yang “mengganggu” rata-rata. Arsipkan dengan status dan alasan; penghapusan menghilangkan sinyal regresi.

## Jalan pintas yang tampak menarik

Jalan pintas yang sering dipilih adalah mengganti model, menjalankan benchmark umum, lalu menyimpulkan workflow siap. Benchmark umum dapat memberi sinyal awal, tetapi tidak mengetahui istilah internal, otorisasi, sumber yang diperbolehkan, atau konsekuensi proses Anda. Alternatif yang lebih aman adalah mempertahankan baseline lokal, menambah kasus kegagalan yang ditemukan saat pilot, dan meminta reviewer domain menyetujui rubric sebelum angka dibandingkan.

Jalan pintas lain adalah mengandalkan log provider sebagai bukti retensi dan penghapusan. Perilaku penyedia, kontrak, dan konfigurasi organisasi harus diverifikasi untuk penggunaan spesifik; jangan mengisinya dengan asumsi. Kawan Codev.id, minta pemilik data menunjukkan bukti konfigurasi dan prosedur penghapusan sebelum hasil uji dipakai sebagai dasar perluasan. [NEEDS GATE-10 REVIEW: perilaku provider, hak penggunaan data, lokasi pemrosesan, retensi, dan kemampuan penghapusan belum memiliki bukti proyek dalam paket ini].

## Kesimpulan dan langkah berikutnya

Evaluation set untuk prompt, model, dan workflow AI adalah sistem bukti yang berversi: kasus representatif, reference atau rubric, slice, adversarial dan abstention test, review manusia, metrik latency/biaya, ambang yang disetujui, serta hasil regresi. Ia menjawab “apakah perilaku ini dapat diterima untuk tugas dan risiko kita?”, bukan “model mana yang paling pintar?”.

Langkah berikutnya: pilih satu alur pilot, tulis 20–30 kasus awal berdasarkan pekerjaan nyata dan kegagalan yang dikhawatirkan, minta pemilik domain menyetujui rubric, lalu jalankan baseline dengan konfigurasi yang dibekukan. Simpan manifest dan hasil mentah; tandai setiap asumsi yang belum diverifikasi. Untuk konteks umum dan langkah lanjutan organisasi, Anda dapat mulai dari [beranda Codev.id](/), lalu minta review teknis sebelum pilot memproses data atau mengambil tindakan nyata.

Aturan operasionalnya sederhana: tidak ada ambang lulus tanpa pemilik keputusan, tidak ada klaim keamanan tanpa bukti konfigurasi dan pengujian, dan tidak ada perluasan pilot ketika slice kritis atau jalur fallback belum disetujui.

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
