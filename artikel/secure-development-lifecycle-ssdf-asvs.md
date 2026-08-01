---
article_id: CDV-09-A02
title: "Secure Development Lifecycle dengan SSDF dan ASVS"
slug: "secure-development-lifecycle-ssdf-asvs"
description: "Panduan memetakan praktik NIST SSDF dan persyaratan OWASP ASVS ke peran, daftar kerja, kode, pengujian, rilis, serta bukti respons keamanan."
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2025-10-13"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-09
primary_intent: "Integrate security practices and verification into delivery"
reader_community: "Codev.id"
reader_address: "Kawan Codev.id"
final_route: "/artikel/secure-development-lifecycle-ssdf-asvs.html"
technical_review: required
sources:
  - "https://spec.openapis.org/oas/v3.1.1.html"
  - "https://www.rfc-editor.org/info/rfc9700/"
  - "https://www.w3.org/TR/webauthn-3/"
  - "https://owasp.org/API-Security/editions/2023/en/0x11-t10/"
  - "https://www.cisa.gov/sbom"
  - "https://csrc.nist.gov/pubs/sp/800/161/r1/final"
  - "https://securityscorecards.dev/"
---

# Secure Development Lifecycle dengan SSDF dan ASVS

Halo, Kawan Codev.id!

Jika keamanan baru diperiksa menjelang rilis, tim biasanya menemukan temuan ketika biaya perubahan sudah tinggi. Secure Development Lifecycle (SDL) dengan NIST Secure Software Development Framework (SSDF) dan OWASP Application Security Verification Standard (ASVS) membantu memindahkan keputusan keamanan ke sepanjang alur kerja: mulai dari penentuan risiko, backlog, desain, kode, pengujian, rilis, sampai respons insiden.

Jawaban singkatnya: gunakan SSDF sebagai kerangka praktik dan pembagian tanggung jawab, lalu gunakan ASVS versi yang dipilih sebagai daftar persyaratan verifikasi yang dapat ditelusuri ke bukti. Keduanya tidak otomatis menyatakan aplikasi patuh atau aman. Ruang lingkup harus dipilih dari threat model, jenis data, jalur akses, dan keputusan pemilik sistem. [NEEDS THREAT MODEL DAN PERSETUJUAN SCOPE PROYEK]

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

Ilustrasi umum dari aset lokal codev.id; bukan dokumentasi proyek tertentu.

## Tentukan objek, kondisi, dan tahap siklus hidup

Mulailah dengan memetakan objek yang benar-benar akan dilindungi, bukan langsung menyalin semua kontrol. Objeknya dapat berupa API, aplikasi web, layanan internal, pipeline CI/CD, dependensi, kredensial, atau data yang diproses. Catat pemilik produk, pemilik teknis, tim pengembang, penguji, operator, dan pihak yang menyetujui risiko. Peta ini menjadi dasar penerapan SSDF: praktik organisasi, perlindungan perangkat lunak, produksi perangkat lunak yang aman, serta respons terhadap kerentanan.

ASVS kemudian menerjemahkan risiko aplikasi menjadi persyaratan yang bisa diverifikasi. Pilih versi ASVS dan tingkat verifikasi yang sesuai dengan konteks proyek; jangan mengklaim seluruh standar sudah dipenuhi hanya karena beberapa butir tercentang. Setiap persyaratan perlu memiliki pemilik, status, bukti, dan alasan bila dinyatakan tidak berlaku.

Pada lokakarya awal, tanyakan tiga hal. Data apa yang paling berdampak bila bocor atau berubah? Jalur mana yang dapat dipanggil pengguna, integrasi, atau administrator? Tahap mana yang saat ini tidak memiliki pemilik keputusan? Jawaban tersebut menentukan apakah item keamanan masuk ke discovery, desain, implementasi, atau rilis. Untuk pekerjaan pengembangan yang lebih luas, Anda dapat melihat konteks layanan pada halaman [web development](/web-development), tetapi keputusan scope tetap harus berasal dari proyek yang sedang dinilai.

## Mekanisme perubahan atau penurunan kinerja

Risiko keamanan berubah ketika kode, konfigurasi, dependensi, identitas, atau lingkungan berubah. Endpoint baru dapat memperluas permukaan serangan; perubahan otorisasi dapat mengubah siapa yang boleh membaca atau mengubah objek; pembaruan library dapat mengubah perilaku transitive dependency. Karena itu, backlog keamanan tidak boleh dianggap pekerjaan satu kali.

Gunakan SSDF untuk menetapkan pemicu perubahan: pull request pada komponen sensitif, perubahan skema API, perubahan penyedia identitas, penambahan dependensi, atau migrasi lingkungan. Gunakan ASVS untuk menentukan pemeriksaan yang relevan pada pemicu itu. Misalnya, kontrak API dapat ditulis dalam OpenAPI agar bentuk antarmuka, parameter, dan respons terlihat oleh pengembang serta penguji. Spesifikasi OpenAPI mendeskripsikan antarmuka; spesifikasi itu tidak membuktikan implementasi benar-benar menegakkan autentikasi atau otorisasi ([OpenAPI Specification 3.1.1](https://spec.openapis.org/oas/v3.1.1.html)).

Untuk alur OAuth, dokumentasikan klien, redirect, token, dan batas kepercayaan sebelum memilih pola implementasi. RFC 9700 adalah pembaruan best current practice keamanan OAuth 2.0, tetapi penerapannya tetap bergantung pada konteks klien dan ancaman ([RFC 9700](https://www.rfc-editor.org/info/rfc9700/)). Jika passkey dipertimbangkan, catat perangkat, pemulihan akun, dan fallback; WebAuthn mendefinisikan API dan protokol untuk autentikasi berbasis kriptografi, bukan jaminan bahwa desain pemulihan Anda aman ([WebAuthn Level 3](https://www.w3.org/TR/webauthn-3/)).

## Inspeksi dan data yang perlu dicatat

Buat satu matriks yang menghubungkan risiko, persyaratan ASVS, perubahan kode, pemeriksaan, dan bukti. Kolom minimalnya:

| Area | Pertanyaan inspeksi | Bukti yang disimpan |
|---|---|---|
| Peran | Siapa pemilik risiko dan siapa yang menyetujui pengecualian? | keputusan, tiket, dan tanggal review |
| Desain | Aset, trust boundary, dan penyalahgunaan apa yang dipertimbangkan? | threat model dan keputusan desain |
| Kode | Validasi input, otorisasi objek, error, dan secret ditangani di mana? | pull request, hasil review, dan konfigurasi |
| Uji | Skenario negatif apa yang harus gagal? | hasil unit, integrasi, dan security test |
| Rilis | Apa syarat go/no-go dan siapa yang berwenang menunda? | checklist rilis, approval, dan artefak build |
| Respons | Bagaimana kerentanan dilaporkan, diprioritaskan, dan ditutup? | tiket, timeline, patch, dan verifikasi ulang |

Untuk API, cocokkan kontrak dengan pemeriksaan abuse case seperti otorisasi tingkat objek, pembatasan resource, dan validasi alur bisnis. OWASP API Security Top 10 2023 dapat menjadi daftar risiko untuk dipetakan ke skenario uji, bukan bukti bahwa API Anda telah aman ([OWASP API Security Top 10 2023](https://owasp.org/API-Security/editions/2023/en/0x11-t10/)). Simpan versi dokumen, commit atau build identifier, lingkungan pengujian, dan hasil yang dapat direproduksi. Hindari menyimpan token, secret, atau data produksi dalam tiket.

Dependensi juga perlu baseline. SBOM membantu transparansi komponen dan relasinya, tetapi tidak menetapkan bahwa setiap komponen aman ([CISA SBOM resources](https://www.cisa.gov/sbom)). Catat sumber paket, versi yang dipakai, lisensi, pemilik tindak lanjut, serta cara menanggapi advisory. Skor repositori dari OpenSSF Scorecard dapat menjadi sinyal untuk pemeriksaan awal, bukan pengganti due diligence terhadap pemasok ([OpenSSF Scorecard](https://securityscorecards.dev/)).

## Pilihan perawatan atau intervensi

Tidak semua temuan harus diselesaikan dengan cara yang sama. Pilihan yang masuk akal bergantung pada exploitability, dampak aset, paparan, dan kemampuan rollback.

- **Pantau:** dipakai bila risiko rendah, terisolasi, dan ada indikator yang jelas. Tetapkan tanggal peninjauan, bukan status “nanti”.
- **Perbaiki di kode:** ubah validasi, otorisasi, manajemen sesi, logging, atau dependensi; sertakan regresi agar masalah tidak kembali.
- **Perkuat kontrol kompensasi:** batasi akses, tambahkan rate limit, isolasi layanan, atau perketat konfigurasi sementara. Dokumentasikan bahwa ini bukan perbaikan akar masalah.
- **Tunda atau hentikan rilis:** pilih ini bila bukti minimum belum ada, jalur serangan terbuka, atau pemilik risiko tidak menyetujui pengecualian.
- **Ganti komponen atau vendor:** lakukan bila asal-usul, dukungan, atau respons pemasok tidak dapat diverifikasi. NIST SP 800-161 Rev.1 memberi konteks pengelolaan risiko rantai pasok siber; ia tidak menilai vendor tertentu untuk Anda ([NIST SP 800-161 Rev.1](https://csrc.nist.gov/pubs/sp/800/161/r1/final)).

Kawan Codev.id, bedakan “kontrol ada” dari “kontrol terbukti bekerja”. Sebuah scan yang hijau hanya menjawab aturan yang dijalankan oleh alat tersebut. Bukti yang lebih kuat menggabungkan review manusia, uji negatif, konfigurasi yang diterapkan, dan jejak rilis.

## Cara menentukan prioritas

Prioritaskan dengan matriks sederhana: dampak aset × kemungkinan penyalahgunaan × paparan × kesulitan pemulihan. Tambahkan urgensi bila ada indikasi eksploitasi aktif atau perubahan yang akan segera dirilis. Skala angka hanyalah alat bantu; keputusan akhir harus menyebut asumsi dan otoritas yang menyetujuinya.

Contoh: endpoint yang mengubah data pelanggan dan dapat dipanggil lintas tenant mendapat prioritas lebih tinggi daripada halaman informasi publik, meskipun keduanya memiliki temuan validasi input. Untuk temuan dependency, pertimbangkan apakah komponen dimuat saat runtime, jalur kodenya tercapai, dan apakah patch kompatibel. Jangan menjadikan skor otomatis sebagai vonis. [NEEDS REVIEW KEPEMILIKAN RISIKO, PAPARAN, DAN KONSEKUENSI AKTUAL]

Tetapkan Definition of Done keamanan untuk tiap jenis perubahan. Perubahan otorisasi mungkin memerlukan threat model yang diperbarui, uji akses lintas peran, dan approval pemilik data. Perubahan library mungkin memerlukan SBOM baru, pemeriksaan advisory, dan rencana rollback. Dengan begitu, ASVS masuk ke backlog sebagai pekerjaan berukuran jelas, sedangkan SSDF menjaga agar pekerjaan itu memiliki pemilik dan bukti.

## Rekaman, serah terima, dan pemicu pemeriksaan ulang

Simpan paket bukti yang dapat dibaca orang berikutnya: scope dan versi ASVS, keputusan threat model, pemetaan SSDF, matriks risiko, kontrak API, hasil pengujian, SBOM, keputusan vendor, pengecualian, serta bukti build dan deployment. Tautkan artefak ke commit atau release yang tepat. Hapus secret dan data pribadi dari lampiran; catat lokasi aman bila reviewer berwenang perlu mengaksesnya.

Serah terima bukan sekadar mengirim tautan. Tulis apa yang berubah, apa yang belum diverifikasi, siapa pemilik tindak lanjut, dan kapan pemeriksaan diulang. Pemicu review ulang dapat berupa perubahan autentikasi, penambahan endpoint, insiden, advisory kritis, pergantian vendor, atau perubahan batas tenant. Jika bukti tidak cukup untuk menyimpulkan, tampilkan status “belum diverifikasi” dan eskalasikan—jangan mengubahnya menjadi klaim kepatuhan.

## Jalan pintas yang sering dipilih

Jalan pintas yang menggoda adalah menjalankan satu scanner pada branch utama, menyalin hasilnya ke tiket, lalu menyebut SDL selesai. Cara ini gagal karena scanner tidak mengetahui seluruh konteks bisnis, bisa melewatkan penyalahgunaan alur, dan tidak menetapkan siapa yang menerima risiko. Ia juga tidak memberi jejak keputusan untuk perubahan berikutnya.

Alternatif yang lebih dapat dipertanggungjawabkan adalah membuat baseline kecil namun tertelusur: pilih risiko paling penting, petakan ke persyaratan ASVS, tetapkan praktik SSDF dan pemiliknya, jalankan uji yang sesuai, lalu simpan keputusan rilis. Perluas cakupan saat bukti dan kapasitas bertambah. Sobat Codev.id, lebih baik satu alur sensitif memiliki bukti lengkap daripada seluruh aplikasi memiliki checklist tanpa pemilik.

## Kesimpulan

Secure Development Lifecycle dengan SSDF dan ASVS berarti menghubungkan praktik organisasi dengan persyaratan verifikasi yang dipilih untuk risiko proyek. SSDF mengatur siapa melakukan apa dan bukti apa yang harus bertahan; ASVS membantu merumuskan pemeriksaan aplikasi yang dapat diuji. OpenAPI, panduan OAuth, WebAuthn, OWASP API Top 10, SBOM, dan penilaian rantai pasok memberi bahan keputusan—bukan sertifikat keamanan otomatis.

Langkah berikutnya: pilih satu alur paling sensitif, tulis threat model ringkas, tetapkan versi dan tingkat ASVS, buat matriks risiko-ke-bukti, lalu minta persetujuan pemilik risiko sebelum rilis. Teman Codev.id, operating rule-nya sederhana: tidak ada klaim “aman” atau “patuh” tanpa scope, bukti yang dapat ditelusuri, dan review teknis yang berwenang.
