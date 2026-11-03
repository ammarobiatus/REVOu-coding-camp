# Implementation Plan: Wisuda Sash Store

## Overview

Implementasi website toko selempang wisuda sebagai **single-file HTML** (vanilla HTML/CSS/JS) tanpa dependensi eksternal berbayar. Pendekatan bertahap: mulai dari struktur HTML semantik dan CSS design tokens, lalu modul-modul JavaScript (ColorAnimator, MotionAnimator), diikuti komponen UI (Product Gallery, Contact Section), dan diakhiri dengan integrasi penuh serta pengujian.

## Tasks

- [x] 1. Setup struktur dasar HTML dan CSS design tokens
  - Buat file `index.html` dengan struktur semantik: `<header>`, `<main>`, `<section>`, `<footer>`
  - Tambahkan `<meta>` viewport, charset, dan deskripsi untuk SEO dasar
  - Definisikan CSS Custom Properties (`:root`) untuk semua design tokens: warna, tipografi, spacing, animasi sesuai spesifikasi di design.md
  - Terapkan CSS reset minimal dan box-sizing global
  - _Requirements: 1.1, 1.4, 6.1, 6.4_

- [x] 2. Implementasi Navigation Component
  - [x] 2.1 Buat HTML dan CSS untuk komponen `<header>` navigation
    - Tambahkan logo teks toko dengan ikon selempang SVG inline
    - Buat menu navigasi: Beranda, Produk, Tentang, Kontak
    - Tambahkan tombol CTA "Pesan Sekarang" dengan smooth scroll ke `#contact`
    - Terapkan `position: fixed` agar sticky saat scroll
    - Terapkan kontras warna WCAG 2.1 AA (minimal 4.5:1)
    - _Requirements: 1.2, 6.3, 6.5_

  - [x] 2.2 Implementasi hamburger menu untuk mobile
    - Buat tombol hamburger yang toggle visibility menu pada viewport < 768px
    - Pastikan fokus keyboard pada tombol hamburger menampilkan indikator fokus yang jelas
    - _Requirements: 1.5, 6.5_

- [x] 3. Implementasi Hero Section dan Sash_Display
  - [x] 3.1 Buat HTML dan CSS untuk Hero Section
    - Tambahkan judul toko, deskripsi singkat, dan tombol CTA sekunder
    - Buat elemen `<div id="sash-display">` sebagai target animasi
    - Desain visual selempang menggunakan CSS shape atau SVG inline
    - Terapkan gradien biru-putih sebagai latar belakang hero
    - _Requirements: 1.1, 1.3, 6.1_

  - [ ]* 3.2 Tulis unit test untuk Hero Section render
    - Verifikasi elemen `#sash-display` ada di DOM
    - Verifikasi judul dan deskripsi toko ter-render dengan benar
    - _Requirements: 1.3_

- [x] 4. Implementasi ColorAnimator Module
  - [x] 4.1 Tulis modul `ColorAnimator` dalam `<script>` tag
    - Implementasikan `init(element)` yang memulai `setInterval` dengan durasi 2000ms
    - Implementasikan `setColor(index)` dengan CSS `transition` 500ms untuk efek fade
    - Implementasikan `pause()` yang membersihkan `intervalId` dan menyimpan `currentIndex`
    - Implementasikan `resume()` yang melanjutkan dari `currentIndex` terakhir
    - Implementasikan `destroy()` untuk membersihkan interval pada `visibilitychange`
    - Daftarkan 5 warna: `#1a237e`, `#1565c0`, `#42a5f5`, `#ffd700`, `#c62828`
    - Tambahkan deteksi `prefers-reduced-motion`: jika aktif, hanya panggil `setColor(0)` tanpa interval
    - _Requirements: 3.1, 3.2, 3.3, 3.6_

  - [ ]* 4.2 Tulis property test untuk Property 1: Color Cycling Menggunakan Warna Valid
    - **Property 1: Color Cycling Menggunakan Warna yang Valid**
    - **Validates: Requirements 3.2**
    - Generate random sequence operasi cycling, verifikasi setiap warna yang diterapkan selalu dari array `colors[]`
    - Gunakan `fast-check` dengan `numRuns: 100`
    - Tag: `// Feature: wisuda-sash-store, Property 1: Color Cycling Menggunakan Warna yang Valid`

  - [ ]* 4.3 Tulis property test untuk Property 2: Color Index Selalu Dalam Batas
    - **Property 2: Color Index Selalu Dalam Batas Array**
    - **Validates: Requirements 3.1, 3.5**
    - Generate random sequence `advance()` operations, verifikasi `0 <= currentIndex < colors.length`
    - Gunakan `fast-check` dengan `numRuns: 100`
    - Tag: `// Feature: wisuda-sash-store, Property 2: Color Index Selalu Dalam Batas Array`

  - [ ]* 4.4 Tulis property test untuk Property 5: Hover Pause–Resume Mempertahankan Posisi
    - **Property 5: Hover Pause–Resume Mempertahankan Posisi Warna**
    - **Validates: Requirements 3.4, 3.5**
    - Generate random `currentIndex`, panggil `pause()` lalu `resume()`, verifikasi `currentIndex` tidak berubah
    - Gunakan `fast-check` dengan `numRuns: 100`
    - Tag: `// Feature: wisuda-sash-store, Property 5: Hover Pause–Resume Mempertahankan Posisi Warna`

  - [ ]* 4.5 Tulis unit test untuk ColorAnimator
    - Test `init()` dengan `prefers-reduced-motion` aktif tidak memulai interval
    - Test `pause()` menghentikan interval (`intervalId === null`)
    - Test `resume()` melanjutkan dari `currentIndex` yang sama setelah pause
    - _Requirements: 3.1, 3.3, 3.4, 3.5, 3.6_

- [x] 5. Implementasi MotionAnimator Module
  - [x] 5.1 Tulis modul `MotionAnimator` dalam `<script>` tag
    - Implementasikan `init(element)` yang mendaftarkan event listener `click` pada `#sash-display`
    - Implementasikan `triggerSwing()` yang menambahkan class `.swinging` dan set `isAnimating = true`
    - Implementasikan `onAnimationEnd()` sebagai callback yang menghapus class `.swinging` dan reset `isAnimating = false`
    - Tambahkan guard: jika `isAnimating === true`, abaikan klik tambahan
    - Tambahkan deteksi `prefers-reduced-motion`: jika aktif, `triggerSwing()` hanya memicu brief opacity change
    - Definisikan CSS `@keyframes` untuk efek swing (rotate + translateY) dengan durasi 800ms
    - _Requirements: 2.1, 2.2, 2.3, 2.4, 2.5_

  - [ ]* 5.2 Tulis property test untuk Property 3: Anti-Overlap Animation
    - **Property 3: MotionAnimator Mencegah Tumpang Tindih Animasi**
    - **Validates: Requirements 2.3**
    - Generate random sequence klik saat `isAnimating = true`, verifikasi tidak ada animasi baru dimulai
    - Gunakan `fast-check` dengan `numRuns: 100`
    - Tag: `// Feature: wisuda-sash-store, Property 3: MotionAnimator Mencegah Tumpang Tindih Animasi`

  - [ ]* 5.3 Tulis unit test untuk MotionAnimator
    - Test `triggerSwing()` mengatur `isAnimating = true` saat animasi berlangsung
    - Test klik kedua diabaikan saat `isAnimating = true`
    - _Requirements: 2.1, 2.3_

- [x] 6. Checkpoint — Pastikan semua animasi berfungsi
  - Pastikan ColorAnimator dan MotionAnimator terintegrasi dengan `#sash-display`
  - Pastikan event hover (`mouseenter`/`mouseleave`) pada `#sash-display` memanggil `pause()` dan `resume()`
  - Pastikan `visibilitychange` listener memanggil `destroy()` dan `init()` dengan benar
  - Pastikan semua tests pass, tanyakan ke user jika ada pertanyaan.

- [x] 7. Implementasi Product Gallery Component
  - [x] 7.1 Buat data array `PRODUCTS` dan fungsi render Product_Card
    - Definisikan array `PRODUCTS` dengan 6 produk sesuai spesifikasi data model di design.md
    - Tulis fungsi `renderProductCard(product)` yang menghasilkan HTML string untuk satu kartu
    - Setiap kartu berisi: gambar SVG inline berwarna sesuai `product.color`, nama, deskripsi, harga, tombol "Pesan"
    - Tombol "Pesan" menggunakan smooth scroll ke `#contact`
    - Tambahkan atribut `alt` deskriptif pada setiap gambar
    - _Requirements: 4.1, 4.2, 4.3, 6.2_

  - [ ]* 7.2 Tulis property test untuk Property 4: Deskripsi Produk ≤ 100 Karakter
    - **Property 4: Teks Deskripsi Produk Tidak Melebihi Batas Karakter**
    - **Validates: Requirements 4.2**
    - Untuk setiap `ProductData` dalam array `PRODUCTS`, verifikasi `description.length <= 100`
    - Gunakan `fast-check` dengan `numRuns: 100`
    - Tag: `// Feature: wisuda-sash-store, Property 4: Teks Deskripsi Produk Tidak Melebihi Batas Karakter`

  - [x] 7.3 Buat CSS grid responsif untuk Product_Gallery
    - 3 kolom pada desktop (≥ 1024px)
    - 2 kolom pada tablet (768px–1023px)
    - 1 kolom pada mobile (< 768px)
    - Tambahkan CSS transition untuk hover effect: `box-shadow` dan `transform: translateY(-8px)`
    - _Requirements: 4.4, 4.5_

  - [ ]* 7.4 Tulis unit test untuk Product Gallery
    - Verifikasi 6 Product_Card ter-render di DOM
    - Verifikasi setiap kartu memiliki atribut `alt` yang tidak kosong
    - _Requirements: 4.1, 4.2, 6.2_

- [x] 8. Implementasi Contact Section dan Footer
  - [x] 8.1 Buat HTML dan CSS untuk Contact Section
    - Tampilkan nomor WhatsApp, email, dan ikon media sosial (Instagram, TikTok)
    - Buat tombol "Chat WhatsApp" yang membuka `https://wa.me/{nomor}?text={pesan_default}` di tab baru (`target="_blank"`)
    - Tambahkan `aria-disabled="true"` dan tooltip jika nomor WhatsApp tidak dikonfigurasi (fallback)
    - _Requirements: 5.1, 5.2_

  - [x] 8.2 Buat HTML dan CSS untuk Footer
    - Tampilkan nama toko, tagline/deskripsi singkat
    - Tambahkan teks hak cipta dengan tahun dinamis: `new Date().getFullYear()`
    - _Requirements: 5.3_

  - [ ]* 8.3 Tulis unit test untuk Footer
    - Verifikasi tahun pada teks hak cipta sesuai `new Date().getFullYear()`
    - _Requirements: 5.3_

- [x] 9. Implementasi UI Interactions Module
  - [x] 9.1 Implementasi smooth scroll untuk semua tombol CTA dan link navigasi
    - Tombol "Pesan Sekarang" di nav → smooth scroll ke `#contact`
    - Tombol "Pesan" di setiap Product_Card → smooth scroll ke `#contact`
    - Tombol CTA sekunder di hero → smooth scroll ke `#products`
    - Gunakan `scrollIntoView({ behavior: 'smooth' })` atau `window.scrollTo` dengan `behavior: 'smooth'`
    - _Requirements: 4.3, 1.2_

  - [x] 9.2 Pastikan semua elemen interaktif memiliki indikator fokus keyboard yang visible
    - Tambahkan CSS `:focus-visible` outline yang kontras pada tombol, link, dan elemen interaktif lainnya
    - Pastikan tombol hamburger menu juga memiliki fokus yang terlihat
    - _Requirements: 6.5_

- [x] 10. Checkpoint — Integrasi penuh dan validasi aksesibilitas
  - Pastikan semua modul (ColorAnimator, MotionAnimator, UI Interactions) saling terintegrasi dengan benar
  - Verifikasi HTML semantik: `<header>`, `<main>`, `<section>`, `<footer>` digunakan dengan benar
  - Verifikasi kontras warna WCAG 2.1 AA (4.5:1) untuk semua pasangan teks-latar
  - Pastikan website dapat dimuat sebagai single-file HTML tanpa dependensi eksternal berbayar
  - Pastikan semua tests pass, tanyakan ke user jika ada pertanyaan.

- [x] 11. Pengujian responsivitas dan cross-browser
  - [x] 11.1 Verifikasi layout responsif di semua breakpoint
    - Tulis CSS media queries untuk breakpoint: < 768px (mobile), 768–1023px (tablet), ≥ 1024px (desktop)
    - Pastikan minimum lebar 320px tetap ter-render dengan baik
    - _Requirements: 1.5, 4.4_

  - [ ]* 11.2 Tulis integration test (Playwright) untuk color cycling dan swing animation
    - Verifikasi warna `#sash-display` berubah setelah > 2000ms
    - Verifikasi class `.swinging` ditambahkan lalu dihapus setelah klik
    - Verifikasi hover menjeda color cycling
    - _Requirements: 2.1, 2.2, 3.1, 3.4_

  - [ ]* 11.3 Tulis integration test (Playwright) untuk smooth scroll dan WhatsApp button
    - Verifikasi klik tombol "Pesan" men-scroll ke `#contact`
    - Verifikasi tombol "Chat WhatsApp" membuka tab baru
    - Verifikasi grid responsif: 1 kolom pada viewport mobile, 3 kolom pada desktop
    - _Requirements: 4.3, 5.2, 4.4_

- [x] 12. Final checkpoint — Semua tests pass dan deliverable siap
  - Pastikan file `index.html` adalah single-file yang lengkap dengan CSS dan JS inline
  - Pastikan semua unit tests dan property tests pass
  - Pastikan tidak ada dependensi eksternal berbayar (CDN gratis seperti fast-check via unpkg diperbolehkan untuk dev/test)
  - Tanyakan ke user jika ada pertanyaan sebelum dianggap selesai.

## Notes

- Tasks yang ditandai `*` bersifat opsional dan dapat dilewati untuk MVP yang lebih cepat
- Setiap task mereferensikan requirement spesifik untuk keterlacakan
- Property tests menggunakan `fast-check` (tersedia via npm atau unpkg CDN)
- Unit tests menggunakan `Jest` untuk menguji logika JavaScript secara terisolasi
- Integration tests menggunakan `Playwright` untuk pengujian end-to-end di browser nyata
- Checkpoint memastikan validasi inkremental sebelum lanjut ke fase berikutnya

## Task Dependency Graph

```json
{
  "waves": [
    { "id": 0, "tasks": ["2.1", "3.1"] },
    { "id": 1, "tasks": ["2.2", "3.2", "4.1", "7.1"] },
    { "id": 2, "tasks": ["4.2", "4.3", "4.4", "4.5", "5.1", "7.3"] },
    { "id": 3, "tasks": ["5.2", "5.3", "7.2", "7.4", "8.1", "8.2"] },
    { "id": 4, "tasks": ["8.3", "9.1", "9.2"] },
    { "id": 5, "tasks": ["11.1"] },
    { "id": 6, "tasks": ["11.2", "11.3"] }
  ]
}
```
