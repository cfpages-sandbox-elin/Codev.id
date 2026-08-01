---
article_id: CDV-14-A05
title: "Cache dan CDN: Freshness, Invalidation, dan Variasi"
slug: "cache-cdn-freshness-invalidation"
description: "Define cacheability, key/variation, freshness, revalidation, purge, personalized/private data, browser/edge layers, tests, and rollout"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2026-02-22"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-14
primary_intent: "Design cache behavior without serving wrong or private data"
reader_community: "Codev.id"
reader_address: "Sobat Codev.id"
final_route: "/artikel/cache-cdn-freshness-invalidation.html"
technical_review: required
sources:
  - "https://www.rfc-editor.org/rfc/rfc9111"
  - "https://opentelemetry.io/docs/"
  - "https://sre.google/workbook/implementing-slos/"
  - "https://csrc.nist.gov/pubs/sp/800/61/r3/final"
  - "https://web.dev/articles/vitals"
---

# Cache dan CDN: Freshness, Invalidation, dan Variasi

Halo, Sobat Codev.id! Cache dan CDN aman dipakai bila Anda menetapkan tiga hal sebelum mengaktifkannya: respons mana yang boleh disimpan, variasi apa yang membentuk cache key, dan kapan salinan dianggap kedaluwarsa atau harus dibuang. Tanpa keputusan itu, “lebih cepat” dapat berubah menjadi halaman lama, harga yang salah, atau data pengguna tampil kepada orang lain.

Jawaban praktisnya: pisahkan data publik yang dapat dibagi dari data personal yang harus privat; tetapkan `Cache-Control` dan key berdasarkan seluruh input yang memengaruhi respons; lalu siapkan revalidasi, purge terarah, dan pengujian sebelum rollout. Freshness bukan janji bahwa semua edge selalu memegang versi terbaru. Ia adalah aturan waktu dan validasi, sedangkan invalidation adalah tindakan eksplisit ketika perubahan tidak boleh menunggu.

Kondisi yang dapat mengubah keputusan adalah sifat endpoint, risiko kebocoran, pola perubahan, dan kemampuan Anda mengamati hasilnya. [NEEDS TECHNICAL REVIEW: konfirmasi kebijakan provider CDN, default cache key, dan perilaku purge/revalidasi yang benar-benar dipakai sebelum produksi.]

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

*Gambar ini merupakan aset lokal untuk ilustrasi dan bukan dokumentasi proyek tertentu.*

## Definisi dan batas objek

Cache adalah salinan respons yang disimpan agar permintaan berikutnya tidak selalu mencapai origin. CDN (content delivery network) menempatkan cache pada jaringan edge; browser dan service worker dapat menjadi lapisan lain. HTTP caching mendefinisikan freshness, validasi ulang, dan aturan penyimpanan melalui metadata respons, bukan melalui asumsi aplikasi semata ([RFC 9111](https://www.rfc-editor.org/rfc/rfc9111)).

Cacheability menjawab “bolehkah respons disimpan dan dibagikan?”. Freshness menjawab “sampai kapan salinan boleh dipakai tanpa bertanya lagi?”. Revalidation mengirim pemeriksaan ke origin untuk memastikan salinan masih berlaku, biasanya memakai validator seperti `ETag` atau `Last-Modified`. Purge menghapus objek lebih awal dari umur normalnya. Variasi adalah perbedaan respons berdasarkan bahasa, perangkat, cookie, otorisasi, atau parameter lain.

Batas pentingnya: halaman atau API yang memuat identitas, token, keranjang, saldo, atau keputusan per pengguna tidak otomatis aman di cache bersama. `private` pada respons browser tidak sama dengan “boleh disimpan di edge”. Untuk objek publik, Anda tetap perlu menentukan data apa yang tidak boleh ikut masuk ke key. Artikel ini membahas perilaku cache dan pembuktiannya, bukan kontrak uptime, konfigurasi vendor tertentu, atau pengganti persetujuan keamanan proyek.

## Cara kerjanya

Alurnya dapat ditulis sebagai urutan keputusan:

1. Klien mengirim URL, metode, header, dan kredensial.
2. Lapisan cache menghitung key. Key yang terlalu sempit mencampur variasi; terlalu lebar menurunkan hit rate.
3. Jika objek ada dan masih fresh, cache mengembalikannya. Jika stale, lapisan itu melakukan revalidasi atau meminta objek baru, sesuai kebijakan.
4. Origin menghasilkan respons beserta `Cache-Control`, validator, dan penanda variasi yang diperlukan.
5. Saat konten berubah, aplikasi menerbitkan versi URL baru atau memicu purge pada objek yang terdampak.

Gunakan versi URL (misalnya hash aset) untuk file statis yang berubah sebagai unit; ini mengurangi kebutuhan purge menyeluruh. Untuk data yang sering berubah, umur singkat dengan revalidasi lebih dapat diprediksi daripada TTL panjang yang berharap operator mengingat purge. Jangan memasukkan seluruh cookie secara membabi buta ke key: itu bisa memecah cache menjadi terlalu banyak varian, tetapi mengabaikannya ketika cookie memengaruhi isi juga berbahaya.

Observabilitas harus membedakan hit, miss, stale, revalidated, bypass, dan purge. OpenTelemetry menyediakan kerangka untuk mengumpulkan traces, metrics, dan logs, tetapi sinyal itu perlu definisi atribut dan sampling yang konsisten agar berguna ([OpenTelemetry documentation](https://opentelemetry.io/docs/)). SLO adalah tujuan layanan dan alat pengambilan keputusan, bukan bukti bahwa cache Anda selalu benar atau janji uptime kontraktual ([Google SRE Workbook](https://sre.google/workbook/implementing-slos/)).

## Faktor yang mengubah hasil

Pertama, klasifikasi data. Respons katalog publik, CSS, dan JavaScript biasanya kandidat cache bersama setelah header sensitif dipastikan tidak ikut. Respons yang bergantung pada `Authorization`, cookie sesi, atau izin harus default bypass/private sampai desain variasinya dibuktikan.

Kedua, sumber variasi. Bahasa, negara, device hint, eksperimen, dan query parameter dapat mengubah representasi. Dokumentasikan mana yang benar-benar memengaruhi body dan mana yang hanya untuk logging. Cache key harus mencerminkan yang pertama.

Ketiga, pola perubahan dan toleransi stale. Banner berita mungkin menerima keterlambatan singkat; saldo atau status pembayaran tidak. Pilih TTL, revalidasi, dan purge berdasarkan dampak kesalahan, bukan angka default provider.

Keempat, lapisan. Browser bisa menyajikan salinan walau edge sudah dipurge; edge bisa menyimpan salinan walau origin sudah berubah. Uji dari klien bersih, edge berbeda, dan origin agar Anda tidak salah menyimpulkan satu lapisan sebagai sumber kebenaran.

Kelima, bukti operasi. Pantau rasio hit, usia objek, status revalidasi, error origin, dan indikasi respons salah-variasi. Metrik pengalaman seperti Core Web Vitals adalah metrik yang didefinisikan dan berkembang; gunakan sebagai sinyal terukur dengan ruang lingkup dan kondisi yang jelas, bukan jaminan ranking, waktu muat, atau konversi ([web.dev Core Web Vitals](https://web.dev/articles/vitals)).

## Contoh keputusan praktis

| Situasi | Kebijakan awal | Bukti sebelum diperluas |
|---|---|---|
| Aset statis berversi | Cache bersama dengan TTL panjang; URL berubah saat isi berubah | Tes bahwa HTML menunjuk versi baru dan klien lama tetap mendapat aset valid |
| Halaman publik dengan bahasa | Variasikan key pada bahasa yang benar-benar didukung; tetapkan fallback | Permintaan berulang lintas bahasa tidak bertukar body |
| API katalog tanpa akun | Cache bersama dengan TTL yang sesuai toleransi stale; revalidasi saat perlu | Perbandingan body origin dan edge pada perubahan data |
| API memakai sesi/otorisasi | Bypass cache bersama atau tandai private; jangan mengandalkan purge sebagai pagar keamanan | Tes dua akun, token kadaluarsa, dan respons tanpa kredensial |
| Perubahan darurat | Purge objek spesifik atau matikan cache untuk rute terdampak | Log purge, waktu propagasi yang terukur, dan verifikasi dari beberapa lokasi |

Kawan Codev.id, tabel ini bukan konfigurasi vendor. Ia adalah hipotesis yang harus diterjemahkan ke kontrak header, key, dan runbook tim Anda. Jika dampak salah-saji tinggi, hubungi tim untuk peninjauan keamanan dan pemilik data sebelum cache bersama diaktifkan.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah memakai TTL panjang untuk semua rute. Periksa daftar endpoint yang berubah dan tulis toleransi stale masing-masing. Kedua, menganggap `no-cache` berarti “jangan simpan”; dalam HTTP, itu umumnya berarti harus memvalidasi sebelum dipakai kembali. Baca header aktual dari browser dan edge, lalu cocokkan dengan kebijakan RFC, bukan dengan nama opsi dashboard.

Kesalahan ketiga adalah purge wildcard setiap deploy. Ukur objek yang benar-benar berubah dan gunakan versi URL atau purge terarah; wildcard dapat menciptakan lonjakan ke origin. Keempat, menguji hanya dari satu browser. Buat matriks pengujian: anonim versus login, bahasa berbeda, parameter berbeda, edge berbeda, dan setelah purge. Simpan body, header, usia, dan cache status sebagai artefak uji.

Kesalahan kelima adalah menganggap dashboard hit-rate membuktikan kebenaran. Hit tinggi dapat menyembunyikan key yang salah. Tambahkan assertion bahwa identitas, harga, izin, dan bahasa pada respons cocok dengan permintaan. Untuk insiden, siapkan jalur deteksi, containment, pemulihan, dan pembelajaran; kerangka respons insiden NIST menekankan siklus tersebut, tanpa memberi Anda bukti bahwa implementasi tertentu sudah memadai ([NIST SP 800-61 Rev. 3](https://csrc.nist.gov/pubs/sp/800/61/r3/final)).

## Mengapa purge saja bukan strategi

Shortcut yang sering dipilih adalah “cache saja semuanya, lalu purge saat ada perubahan”. Ini gagal ketika data personal masuk ke cache bersama, ketika purge terlambat di lapisan browser, atau ketika daftar objek yang harus dibuang tidak lengkap. Purge juga tidak memperbaiki key yang sejak awal salah.

Alternatif yang lebih dapat diaudit: mulai dari allowlist rute publik, definisikan key dan header untuk setiap variasi, gunakan versi URL untuk aset, dan perlakukan bypass/private sebagai default untuk data berotorisasi. Lakukan rollout bertahap dengan observabilitas; hentikan perluasan bila ada respons silang antar-konteks atau metrik origin memburuk. [NEEDS TECHNICAL REVIEW: tetapkan kriteria rollback dan batas toleransi stale berdasarkan risiko layanan aktual.]

## Langkah berikutnya

Cache dan CDN bukan sakelar “cepat”, melainkan kontrak tentang siapa boleh menerima salinan apa, dari key mana, dan sampai kapan. Freshness mengatur penggunaan normal; revalidasi menguji ulang; invalidation atau versi URL menangani perubahan yang tidak boleh menunggu. Variasi dan data privat menentukan batas keamanan.

Teman Codev.id, langkah berikutnya adalah membuat tabel rute berisi klasifikasi data, sumber variasi, TTL, validator, mekanisme purge, dan pemilik keputusan. Jalankan matriks uji anonim/login serta lintas-lapisan, simpan header dan body sebagai bukti, lalu [minta technical review melalui kanal tim](/cdn-cgi/l/email-protection/) untuk kebijakan provider dan kriteria rollback. Aturan operasinya: jangan menaikkan TTL atau memperluas cache bersama sebelum Anda dapat membuktikan bahwa setiap variasi menerima respons yang benar dan tidak ada data privat yang terbagi.
