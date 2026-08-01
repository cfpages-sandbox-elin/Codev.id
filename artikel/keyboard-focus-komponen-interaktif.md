---
article_id: CDV-13-A02
writing_contract_version: "native-id-v2"
title: "Keyboard, Focus, dan Komponen Interaktif"
slug: "keyboard-focus-komponen-interaktif"
description: "Panduan praktis untuk merancang dan menguji semantik native, fokus, urutan keyboard, serta perilaku komponen interaktif"
status: draft
publication_date: "2026-01-19"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-13
primary_intent: "Implement and test non-pointer interaction"
reader_community: "Codev.id"
reader_address: "Kawan Codev.id"
final_route: "/artikel/keyboard-focus-komponen-interaktif.html"
technical_review: required
sources:
  - "https://www.w3.org/TR/WCAG22/"
  - "https://csrc.nist.gov/pubs/sp/800/218/final"
  - "https://www.w3.org/TR/WCAG-EM/"
  - "https://www.w3.org/WAI/test-evaluate/preliminary/"
---

# Keyboard, Focus, dan Komponen Interaktif

Halo, Kawan Codev.id! Komponen interaktif belum aksesibel hanya karena dapat diklik dengan mouse. Jika urutan `Tab` melompat, fokus tidak terlihat, atau dialog tertutup tanpa mengembalikan fokus, pengguna keyboard bisa kehilangan konteks dan tugas berhenti di tengah jalan. Keputusan praktisnya: mulai dari elemen HTML native yang tepat, tetapkan urutan fokus yang mengikuti alur tugas, lalu uji seluruh keadaan komponen tanpa pointer.

HTML native memberi perilaku dasar yang lebih dapat diprediksi daripada membuat `div` menjadi tombol dengan JavaScript. Namun native semantics bukan jaminan otomatis. Menu, dialog, tab, dan kontrol khusus tetap membutuhkan aturan perilaku, manajemen fokus, dan pengujian manual. WCAG 2.2 menempatkan operabilitas keyboard dan fokus sebagai bagian dari evaluasi konformitas, tetapi hasil akhir selalu bergantung pada cakupan halaman dan proses yang benar-benar diuji ([WCAG 2.2](https://www.w3.org/TR/WCAG22/)).

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

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

Ilustrasi umum dari aset lokal Codev.id; bukan dokumentasi proyek tertentu.

## Jawaban singkat dan salah paham utama

Keyboard interaction berarti setiap fungsi yang tersedia dengan pointer juga dapat diselesaikan dengan keyboard, dengan indikator fokus yang terlihat dan urutan yang masuk akal. Fokus adalah posisi aktif yang menerima input keyboard; ia berbeda dari gaya visual `:hover` dan tidak boleh hilang hanya karena desain menghapus outline. Komponen dinilai dari perilaku saat berpindah, membuka, memilih, menutup, dan kembali ke konteks sebelumnya.

Salah paham yang sering terjadi adalah menganggap “bisa diberi fokus” sudah cukup. Tombol palsu berbasis `div` mungkin menerima `tabindex`, tetapi belum tentu merespons Enter dan Space, mengumumkan namanya dengan benar, atau menunjukkan statusnya. Sebaliknya, tombol native sudah membawa semantics dan perilaku dasar; JavaScript sebaiknya mengatur perubahan state dan fokus yang memang diperlukan.

Kawan Codev.id, pisahkan tiga pertanyaan saat menerima komponen: apakah pengguna dapat mencapai kontrolnya, apakah ia tahu kontrol itu sedang aktif, dan apakah ia dapat menyelesaikan tugas serta memahami hasilnya. Jika salah satu jawabannya “belum diuji”, jangan menyebut komponen siap rilis. [NEEDS PROJECT-SPECIFIC REVIEW: cakupan halaman, alur tugas, dan kriteria rilis belum ditetapkan dalam paket ini.]

## Definisi dan batas objek

Artikel ini membahas non-pointer interaction pada menu, dialog, tab, tombol, tautan, input, dan kontrol yang memiliki keadaan terbuka, tertutup, terpilih, atau nonaktif. Fokusnya adalah semantics native, urutan dan visibilitas fokus, pemindahan fokus yang terkontrol, perilaku tombol keyboard, Escape/Return, status disabled, serta pengujian manual.

Ini bukan katalog pola ARIA. WAI-ARIA Authoring Practices dan panduan komponen spesifik tetap menjadi rujukan ketika Anda membuat pola yang tidak disediakan HTML native. Artikel ini juga tidak menyatakan bahwa lolos satu scanner berarti konformitas WCAG atau kepatuhan hukum Indonesia; evaluasi harus menjelaskan halaman, proses, perangkat, dan batas yang diperiksa ([WCAG-EM 1.0](https://www.w3.org/TR/WCAG-EM/)).

Bedakan istilah berikut dalam catatan implementasi:

| Istilah | Pertanyaan yang dijawab |
| --- | --- |
| Semantics | Apa nama, peran, dan status kontrol yang dapat dipahami? |
| Focus order | Dalam urutan apa pengguna mencapai objek saat menekan `Tab` atau panah yang memang ditetapkan pola? |
| Focus visibility | Dapatkah posisi aktif ditemukan pada latar dan layar saat ini? |
| Focus management | Ke mana fokus dipindah ketika state berubah, dan ke mana ia kembali setelah selesai? |

## Cara kerjanya

Mulai dengan elemen native: gunakan `button` untuk aksi, `a` untuk perpindahan lokasi, `input` atau `select` untuk masukan, dan heading untuk struktur. Jangan menambahkan `tabindex="0"` ke semua elemen agar “semuanya bisa dicapai”. Biarkan urutan DOM mengikuti urutan tugas; `tabindex` positif hampir selalu membuat pemeliharaan dan prediksi urutan lebih sulit.

Ketika pengguna menekan `Tab`, browser memindahkan fokus ke kontrol berikutnya yang dapat difokuskan. Urutan ini harus mengikuti pembacaan dan tujuan kerja, bukan posisi dekoratif di CSS. Pastikan fokus tidak tertutup sticky header, panel yang berubah ukuran, atau animasi. Gaya fokus harus memiliki kontras dan bentuk yang cukup jelas; jangan menggantinya dengan warna samar yang hanya tampak saat pointer diarahkan.

Untuk dialog modal, pembukaan harus menempatkan fokus pada judul, instruksi awal, atau kontrol yang paling aman sesuai konteks. Selama modal benar-benar aktif, fokus tidak boleh bocor ke konten di belakangnya. `Escape` biasanya menutup dialog bila pola produk menetapkannya, lalu fokus kembali ke pemicu pembuka. Bila penutupan dibatalkan karena data belum disimpan, beri pesan dan jangan memindahkan fokus secara diam-diam.

Menu dan tab membutuhkan aturan yang konsisten, bukan sekadar daftar `div`. Tentukan apakah `Tab` masuk ke widget lalu tombol panah berpindah antar-item, atau setiap item menjadi stop fokus tersendiri; ikuti pola komponen yang dipilih secara utuh. Pada tab, perubahan tab harus mengubah panel yang terkait dan status terpilih secara serempak. Pada menu, `Escape` mengembalikan fokus ke pemicu ketika menu ditutup. Jangan menangkap semua tombol panah pada level global karena dapat merusak input teks dan pembaca layar.

Aktivasi juga perlu jelas. Enter lazim mengaktifkan tautan atau tombol saat fokus berada di sana; Space lazim mengaktifkan tombol dan dapat mengubah toggle. Jangan menunggu `keydown` saja lalu lupa menghindari aktivasi ganda ketika event `click` native juga berjalan. Uji keydown, keyup, dan click pada browser target, terutama bila framework menambahkan event delegation.

Status disabled harus jujur terhadap kemampuan. Atribut `disabled` pada kontrol native mencegah interaksi dan biasanya mengeluarkannya dari urutan fokus; gunakan hanya ketika memang tidak boleh dioperasikan. Jika pengguna perlu membaca alasan kontrol tidak tersedia, pertimbangkan status yang tetap dapat dicapai dengan penjelasan yang terhubung, bukan sekadar membuatnya tampak abu-abu. Atribut `aria-disabled="true"` tidak otomatis menghentikan event atau mengubah gaya; kode harus mencegah aksi dan menyampaikan alasan secara konsisten.

## Faktor yang mengubah hasil

Hasil berubah menurut struktur DOM, framework, zoom, ukuran viewport, metode input, dan teknologi bantu. Komponen yang tampak baik pada layar lebar dapat kehilangan fokus ketika zoom memperbesar konten atau ketika panel menjadi satu kolom. Perubahan route juga dapat membuat fokus jatuh ke `body`, sehingga pengguna tidak tahu halaman baru telah dimuat.

State asynchronous menambah risiko: setelah pencarian selesai atau error muncul, fokus perlu tetap pada kontrol yang relevan atau diarahkan ke pesan yang dapat ditemukan. Jangan memindahkannya setiap kali state kecil berubah. Catat alasan setiap pemindahan fokus dalam kode dan uji jalur pembatalan, error, timeout, serta data kosong.

Jangan menyimpulkan cakupan dari satu komponen. WCAG-EM menekankan penentuan scope dan sample; pemeriksaan proses juga harus mencakup alur penting, bukan hanya halaman contoh. Scanner otomatis berguna untuk menemukan sebagian masalah, tetapi pemeriksaan keyboard, fokus, semantics, dan perilaku assistive technology memerlukan pengamatan manusia ([WAI Easy Checks](https://www.w3.org/WAI/test-evaluate/preliminary/)).

## Contoh keputusan praktis

Bayangkan tombol “Filter” membuka panel samping. Keputusan yang dapat diaudit adalah:

| Keadaan | Perilaku yang ditetapkan | Bukti yang dicatat |
| --- | --- | --- |
| Tertutup | Pemicu dapat dicapai dan status terbuka diumumkan; Enter/Space membuka | Rekaman urutan `Tab` dan nama kontrol |
| Baru terbuka | Fokus pindah ke judul atau kontrol pertama yang aman | Hasil uji keyboard pada viewport target |
| Di dalam panel | Fokus tetap di panel selama modal; Escape menutup bila diizinkan | Jalur masuk, navigasi, dan pembatalan |
| Ditutup | Fokus kembali ke tombol “Filter” | Catatan fokus sebelum/sesudah penutupan |
| Filter tidak tersedia | Aksi dicegah dan alasan dapat ditemukan | Skenario disabled serta pesan terkait |

Jika panel tidak modal, jangan memakai focus trap hanya karena terlihat seperti drawer. Biarkan pengguna berpindah ke konten berikutnya bila itu memang alur yang dipilih. Keputusan tersebut adalah asumsi desain yang harus disepakati pemilik produk, bukan fakta universal.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah menghapus outline untuk mengejar tampilan. Periksa dengan keyboard pada tema terang dan gelap, termasuk saat fokus berada di tepi viewport. Kesalahan kedua adalah memakai `tabindex` positif untuk “memperbaiki” urutan. Periksa urutan DOM dan ubah struktur sumber bila urutan tugas salah.

Kesalahan ketiga adalah menutup dialog lalu membiarkan fokus berada pada elemen yang sudah hilang. Buka, tekan Escape, batalkan, dan selesaikan dialog; catat target fokus pada tiap transisi. Kesalahan keempat adalah menganggap `aria-disabled` sama dengan `disabled`. Coba Space, Enter, klik, dan aktivasi melalui script; pastikan aksi benar-benar dicegah.

Lakukan smoke test manual berikut pada setiap komponen:

1. Mulai dari address bar, tekan `Tab` tanpa mouse, dan catat urutan sampai kontrol target.
2. Pastikan setiap fokus terlihat, tidak tertutup, dan memiliki nama yang sesuai.
3. Buka, pilih, pindah, batalkan, dan tutup komponen hanya dengan keyboard.
4. Uji Escape, Enter, Space, panah, serta tombol yang tidak relevan di dalam input teks.
5. Periksa fokus setelah route berubah, dialog ditutup, error tampil, dan data kosong.
6. Ulangi pada zoom dan viewport target, lalu lakukan pemeriksaan dengan teknologi bantu yang digunakan proyek.

Hasil uji sebaiknya menyimpan URL/route, browser, ukuran viewport, langkah, hasil aktual, dan defect yang belum selesai. Praktik pengembangan aman NIST SSDF mendorong bukti dan penanganan temuan sebagai bagian dari proses, bukan klaim bahwa satu pengujian membuktikan seluruh kualitas ([NIST SP 800-218 SSDF](https://csrc.nist.gov/pubs/sp/800/218/final)). Tidak ada ambang cakupan universal yang dapat saya tetapkan untuk proyek Anda.

## Jangan berhenti pada scanner

Shortcut yang menarik adalah memasang satu pustaka “accessible modal”, menjalankan scanner, lalu menutup tiket. Pustaka dapat membantu struktur, tetapi tidak mengetahui apakah judul dialog sesuai tugas, apakah fokus kembali ke pemicu yang benar, atau apakah scope pengujian mencakup alur paling penting. Scanner pun hanya melaporkan aturan yang dapat diperiksa pada saat itu.

Alternatif yang lebih dapat dipertanggungjawabkan: tetapkan kontrak perilaku per komponen, tulis skenario keyboard yang dapat diulang, uji manual pada lingkungan yang disepakati, dan hubungkan setiap temuan dengan keputusan rilis. Jika scope, pengguna, atau kriteria penerimaan belum disetujui, tahan klaim konformitas sampai peninjauan teknis selesai. [NEEDS TECHNICAL REVIEW: verifikasi pola komponen spesifik, cakupan proses, dan keputusan rilis proyek diperlukan.]

## Langkah berikutnya

Keyboard, focus, dan komponen interaktif bekerja baik ketika semantics native, urutan fokus, indikator visual, perilaku tombol, state disabled, dan pemulihan fokus dirancang sebagai satu alur tugas. Mulailah dengan satu komponen kritis: dokumentasikan state dan target fokusnya, jalankan smoke test tanpa pointer, lalu simpan bukti bersama defect dan keputusan penerimaan.

Teman Codev.id, minta review teknis sebelum menyebut seluruh produk konform atau siap rilis. Anda dapat mulai dari [beranda Codev.id](/) untuk menetapkan halaman dan alur yang akan diuji. Aturan operasionalnya sederhana: setiap aksi pointer harus punya jalur keyboard yang terlihat, dapat diprediksi, dan dapat dibuktikan pada scope yang memang diuji—di luar itu, jangan mengisi kekosongan bukti dengan asumsi.
