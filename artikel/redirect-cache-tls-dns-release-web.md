---
article_id: CDV-11-A06
title: "Redirect, Cache, TLS, dan DNS dalam Release Web"
slug: "redirect-cache-tls-dns-release-web"
description: "Panduan memetakan redirect rute, cache, header, TLS, DNS, pengujian, propagasi, pemulihan, dan penanggung jawab saat rilis web"
status: draft
writing_contract_version: "native-id-v2"
publication_date: "2025-12-18"
publication_date_basis: editorial_backfill
date_modified: null
parent_topic: CDV-11
primary_intent: "Control edge behavior during a web release"
reader_community: "Codev.id"
reader_address: "Teman Codev.id"
final_route: "/artikel/redirect-cache-tls-dns-release-web.html"
technical_review: required
sources:
  - "https://developers.cloudflare.com/pages/"
  - "https://developers.cloudflare.com/workers/"
  - "https://developers.cloudflare.com/workers/configuration/versions-and-deployments/"
  - "https://sre.google/workbook/implementing-slos/"
  - "https://opentelemetry.io/docs/"
  - "https://csrc.nist.gov/pubs/sp/800/61/r3/final"
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

# Redirect, Cache, TLS, dan DNS dalam Release Web

Halo, Teman Codev.id! Saat release web mengubah URL, origin, atau domain, empat lapisan ini harus dipetakan sebagai satu rantai: DNS mengarahkan nama ke edge, TLS memeriksa identitas koneksi, aturan redirect memilih URL tujuan, lalu cache menentukan respons mana yang disajikan. Jadi, upload build yang sukses belum berarti release aman.

Jawaban praktisnya: buat peta request dari nama domain sampai origin, tetapkan pemilik tiap aturan, uji status code dan header dari lokasi yang relevan, lalu siapkan rollback yang mengembalikan konfigurasi edge sekaligus artefak aplikasi. Pages dan Workers dapat menyediakan lingkungan serta deployment yang berbeda, tetapi detail akun, runtime, dan konfigurasi aktual tetap harus diverifikasi pada proyek Anda ([Cloudflare Pages](https://developers.cloudflare.com/pages/), [Cloudflare Workers](https://developers.cloudflare.com/workers/), [versi dan deployment Workers](https://developers.cloudflare.com/workers/configuration/versions-and-deployments/)).

![Ilustrasi CODEV](/wp-content/uploads/2022/12/CODEV.png)

<p>Gambar ini merupakan aset lokal untuk ilustrasi dan bukan dokumentasi proyek tertentu.</p>

## Definisi dan batas objek

Redirect adalah keputusan HTTP untuk memindahkan klien dari URL lama ke URL baru. Cache adalah penyimpanan respons berdasarkan kunci tertentu—biasanya gabungan host, path, query, dan header yang dipilih. TLS mengamankan koneksi dan memvalidasi sertifikat untuk nama host. DNS memetakan nama host ke layanan yang melayani trafik.

Artikel ini membahas perilaku edge selama satu release: perubahan route, origin, cache key atau invalidation, header, sertifikat, DNS, pengujian, propagasi, rollback, dan ownership. Ia tidak mengelola portofolio domain atau WHOIS; kebutuhan itu berada pada layanan domain terkait. Migrasi SEO juga bukan ruang lingkupnya, sehingga keputusan canonical, nilai pencarian, dan rencana migrasi harus ditangani pada proses khusus.

## Cara kerjanya

Mulai dari request nyata. Resolver DNS mencari alamat layanan; koneksi TLS dinegosiasikan untuk host tersebut; edge membaca aturan redirect dan header; cache memutuskan hit atau meneruskan request ke origin. Origin mengembalikan status, header, dan body; edge dapat menyimpan respons sebelum mengirimkannya kembali. Satu kesalahan di awal dapat terlihat seperti kesalahan di lapisan berikutnya: DNS yang masih menunjuk layanan lama tidak akan menguji build baru, sedangkan cache hit dapat menyembunyikan perubahan origin.

Dokumentasikan urutan ini sebagai tabel: host dan path, tujuan redirect, metode yang diizinkan, cache key, TTL atau aturan bypass, header penting, sertifikat yang mencakup nama host, record DNS, origin, serta pemilik dan cara rollback. Pada platform seperti Pages atau Workers, pisahkan preview, staging, dan production; deployment yang dipilih harus dapat diidentifikasi dan dikembalikan ([Cloudflare deployments](https://developers.cloudflare.com/workers/configuration/versions-and-deployments/)).

## Faktor yang mengubah hasil

Perubahan query string, cookie, authorization, atau header dapat mengubah cache key. Respons personal dan respons yang memiliki efek samping sebaiknya tidak diperlakukan seperti aset publik tanpa aturan eksplisit. Redirect berantai menambah titik gagal dan menyulitkan observasi; pilih satu tujuan final dan uji loop. Pastikan juga format kompresi, metode HTTP, dan variasi bahasa tidak membuat dua pengguna menerima representasi yang keliru.

Sertifikat harus mencakup setiap hostname yang benar-benar digunakan, termasuk nama alternatif bila ada. DNS memiliki TTL dan resolver berbeda, sehingga perpindahan record tidak serentak. Selama propagasi, sebagian pengguna dapat mencapai layanan lama dan sebagian lainnya layanan baru. Tandai kondisi ini di rencana release, lengkap dengan batas waktu observasi.

Telemetry membantu melihat status code, latency, cache hit/miss, error origin, dan jejak lintas komponen. OpenTelemetry mendokumentasikan cara mengumpulkan sinyal tersebut, tetapi instrumentasi sendiri bukan bukti reliabilitas ([OpenTelemetry documentation](https://opentelemetry.io/docs/)). Gunakan SLO sebagai ambang keputusan selama observasi, bukan sebagai janji uptime tanpa bukti operasi ([Google SRE Workbook](https://sre.google/workbook/implementing-slos/)).

Konfigurasi akun, batas runtime, biaya, wilayah data, dan kompatibilitas aktual belum tersedia dalam packet ini. **[NEEDS GATE-07: verifikasi konfigurasi, limit, dan jalur rollback pada akun/platform proyek sebelum menyetujui release.]**

## Contoh keputusan praktis

Misalkan `/produk-lama` dipindahkan ke `/produk-baru` dan origin juga berganti. Keputusan minimum dapat diringkas sebagai berikut:

| Temuan uji | Keputusan |
|---|---|
| DNS dan sertifikat benar, redirect satu langkah, origin baru sehat | Lanjutkan canary/rollout sesuai rencana observasi. |
| Redirect benar tetapi body lama muncul pada cache hit | Hentikan rollout; periksa cache key, TTL, dan invalidation sebelum purge terukur. |
| Sebagian resolver menuju origin lama | Pertahankan kompatibilitas sementara atau tunda cutover; jangan menyimpulkan propagasi selesai dari satu lokasi. |
| Error meningkat melewati ambang SLO | Jalankan rollback konfigurasi edge dan deployment yang sudah disiapkan, lalu catat waktu serta pemilik keputusan. |

Uji sekurangnya URL lama dan baru dengan metode yang dipakai pengguna, dari jaringan yang berbeda. Periksa status 3xx, `Location`, `Cache-Control`, `Age` atau indikator hit/miss yang tersedia, rantai sertifikat, dan respons origin. Simpan hasil beserta timestamp agar perubahan selama propagasi dapat dibandingkan.

## Kesalahan umum dan cara memeriksanya

Kesalahan pertama adalah menguji hanya dari browser yang sudah memiliki cache. Gunakan request baru dan endpoint diagnostik yang tidak memuat data sensitif. Kedua, melakukan purge global tanpa memahami kunci cache; ini bisa meningkatkan beban origin dan tidak menyelesaikan respons yang dikunci oleh variasi header.

Ketiga, mengganti DNS lalu langsung menonaktifkan origin lama. Periksa TTL, resolver utama, health check, dan log kedua origin sebelum mematikan layanan lama. Keempat, menganggap sertifikat valid karena satu hostname berhasil; uji semua nama host dan jalur HTTP-to-HTTPS.

Kelima, hanya memantau availability. Sobat Codev.id, pantau juga redirect loop, cache miss yang melonjak, latency origin, dan error per route. Sinyal yang dikumpulkan perlu ambang, owner, dan tindakan; panduan respons insiden NIST menekankan persiapan, deteksi, respons, dan pembelajaran yang terdokumentasi ([NIST SP 800-61 Rev. 3](https://csrc.nist.gov/pubs/sp/800/61/r3/final)).

## Jalan pintas yang perlu dihindari

Shortcut yang sering dipilih adalah “deploy dulu, nanti purge dan cek DNS.” Urutan ini berisiko karena pengguna bisa menerima kombinasi redirect baru, cache lama, dan origin berbeda yang tidak pernah diuji bersama. Alternatif yang lebih aman: bekukan peta route dan ownership, uji pada preview/staging, verifikasi TLS dan DNS sebelum cutover, lakukan rollout bertahap, lalu tetapkan kondisi rollback yang dapat dijalankan oleh orang yang sedang on-call.

## Penutupan sebelum perubahan

Redirect, cache, TLS, dan DNS harus diperlakukan sebagai satu kontrak perilaku request, bukan empat pekerjaan terpisah. Sebelum release, minta dokumen peta host-route-cache, bukti uji dari beberapa lokasi, daftar pemilik, serta prosedur rollback dan batas observasi. Teman Codev.id, jika salah satu bukti itu belum ada—terutama konfigurasi platform dan cakupan sertifikat—tahan cutover dan minta review teknis. Untuk langkah implementasi aplikasi, Anda dapat melanjutkan ke panduan [pengembangan web](/web-development); bila kebutuhan berikutnya adalah pengukuran monetisasi, lihat [pengelolaan Google AdSense](/web-google-adsense). Catat keputusan, waktu, dan orang yang menyetujui agar perubahan dapat diaudit setelah kondisi stabil. Operating rule-nya sederhana: tidak ada perubahan edge tanpa jalur kembali yang bisa diuji.
