# Requirements Document

## Introduction

Website toko selempang wisuda (graduation sash) yang menampilkan produk dengan tampilan elegan
menggunakan tema warna biru dan putih. Website ini menyediakan pengalaman interaktif di mana
pengunjung dapat melihat animasi selempang bergerak ketika diklik, serta animasi pergantian warna
(color cycling) pada produk. Tujuan utama adalah menarik calon pembeli dan menampilkan produk
secara visual yang menarik.

## Glossary

- **Website**: Aplikasi web satu halaman (single-page) toko selempang wisuda
- **Sash_Display**: Komponen tampilan visual produk selempang wisuda
- **Color_Animator**: Komponen yang mengelola animasi pergantian warna pada selempang
- **Motion_Animator**: Komponen yang mengelola animasi gerakan selempang saat diklik
- **Product_Card**: Kartu produk yang menampilkan informasi dan gambar selempang
- **Navigation**: Komponen menu navigasi halaman
- **Hero_Section**: Bagian utama halaman beranda dengan tampilan produk unggulan
- **Product_Gallery**: Seksi galeri yang menampilkan koleksi produk selempang
- **Contact_Section**: Seksi informasi kontak dan pemesanan

---

## Requirements

### Requirement 1: Tampilan Halaman Utama

**User Story:** Sebagai pengunjung toko, saya ingin melihat halaman utama yang menarik dengan tema biru dan putih elegan, agar saya mendapatkan kesan profesional tentang toko ini.

#### Acceptance Criteria

1. THE Website SHALL menampilkan halaman beranda dengan latar belakang warna gradien biru dan putih yang elegan
2. THE Navigation SHALL menampilkan logo toko, menu navigasi (Beranda, Produk, Tentang, Kontak), dan tombol "Pesan Sekarang"
3. WHEN pengunjung membuka halaman, THE Hero_Section SHALL menampilkan judul toko, deskripsi singkat, dan gambar selempang unggulan
4. THE Website SHALL menggunakan tipografi yang konsisten dengan font yang bersih dan mudah dibaca
5. THE Website SHALL responsif dan dapat ditampilkan dengan baik pada layar desktop (lebar minimum 320px) dan perangkat mobile

---

### Requirement 2: Animasi Gerakan Selempang Saat Diklik

**User Story:** Sebagai pengunjung, saya ingin selempang bergerak ketika saya klik, agar pengalaman berbelanja menjadi lebih interaktif dan menarik.

#### Acceptance Criteria

1. WHEN pengunjung mengklik elemen Sash_Display, THE Motion_Animator SHALL memulai animasi gerakan selempang berupa efek berayun (swing) selama 800ms
2. WHEN animasi gerakan selesai, THE Motion_Animator SHALL mengembalikan selempang ke posisi semula secara halus
3. WHILE animasi sedang berjalan, THE Motion_Animator SHALL mengabaikan klik tambahan untuk mencegah tumpang tindih animasi
4. THE Motion_Animator SHALL menggunakan CSS transform (rotate dan translateY) untuk efek gerakan yang halus
5. IF perangkat pengguna mendukung `prefers-reduced-motion`, THEN THE Motion_Animator SHALL menonaktifkan animasi gerakan dan hanya menampilkan efek visual statis

---

### Requirement 3: Animasi Pergantian Warna (Color Cycling)

**User Story:** Sebagai pengunjung, saya ingin melihat animasi pergantian warna pada selempang, agar saya dapat membayangkan variasi warna produk yang tersedia.

#### Acceptance Criteria

1. THE Color_Animator SHALL menjalankan animasi siklus warna otomatis pada elemen Sash_Display dengan interval pergantian setiap 2000ms
2. THE Color_Animator SHALL menampilkan minimal 5 variasi warna: biru tua (#1a237e), biru sedang (#1565c0), biru muda (#42a5f5), emas (#ffd700), dan merah (#c62828)
3. WHEN transisi warna terjadi, THE Color_Animator SHALL menggunakan efek fade/transition selama 500ms agar pergantian warna terlihat halus
4. WHEN pengunjung mengarahkan kursor ke Sash_Display, THE Color_Animator SHALL menjeda animasi siklus warna dan mempertahankan warna saat ini
5. WHEN kursor pengunjung meninggalkan Sash_Display, THE Color_Animator SHALL melanjutkan animasi siklus warna dari posisi terakhir
6. IF perangkat pengguna mendukung `prefers-reduced-motion`, THEN THE Color_Animator SHALL menampilkan satu warna statis tanpa animasi pergantian

---

### Requirement 4: Galeri Produk Selempang

**User Story:** Sebagai pengunjung, saya ingin melihat berbagai pilihan produk selempang wisuda, agar saya dapat memilih produk yang sesuai dengan kebutuhan.

#### Acceptance Criteria

1. THE Product_Gallery SHALL menampilkan minimal 6 Product_Card produk selempang dengan variasi warna dan model
2. EACH Product_Card SHALL menampilkan gambar selempang, nama produk, deskripsi singkat (maksimal 100 karakter), harga, dan tombol "Pesan"
3. WHEN pengunjung mengklik tombol "Pesan" pada Product_Card, THE Website SHALL menggulir halaman ke Contact_Section secara halus (smooth scroll)
4. THE Product_Gallery SHALL menampilkan produk dalam tata letak grid yang responsif: 3 kolom pada desktop, 2 kolom pada tablet, 1 kolom pada mobile
5. WHEN pengunjung mengarahkan kursor ke Product_Card, THE Product_Card SHALL menampilkan efek hover berupa bayangan (box-shadow) dan translasi vertikal naik 8px

---

### Requirement 5: Seksi Kontak dan Pemesanan

**User Story:** Sebagai calon pembeli, saya ingin mengetahui cara memesan selempang, agar saya dapat menghubungi toko dengan mudah.

#### Acceptance Criteria

1. THE Contact_Section SHALL menampilkan informasi kontak berupa nomor WhatsApp, alamat email, dan media sosial toko
2. THE Contact_Section SHALL menyediakan tombol "Chat WhatsApp" yang membuka tautan WhatsApp dengan pesan default pemesanan dalam tab baru
3. THE Website SHALL menampilkan footer yang berisi nama toko, deskripsi singkat, dan hak cipta

---

### Requirement 6: Performa dan Aksesibilitas

**User Story:** Sebagai pengunjung, saya ingin website dapat diakses dengan cepat dan mudah, agar pengalaman berbelanja tidak terganggu.

#### Acceptance Criteria

1. THE Website SHALL menggunakan HTML semantik dengan elemen `<header>`, `<main>`, `<section>`, dan `<footer>` yang tepat
2. THE Website SHALL menyediakan atribut `alt` yang deskriptif pada setiap elemen gambar
3. THE Website SHALL menggunakan kontras warna minimal 4.5:1 antara teks dan latar belakang sesuai standar WCAG 2.1 AA
4. THE Website SHALL dapat dimuat dari satu file HTML tunggal yang mengandung CSS dan JavaScript inline tanpa dependensi eksternal berbayar
5. WHEN elemen interaktif (tombol, tautan) difokus menggunakan keyboard, THE Website SHALL menampilkan indikator fokus yang terlihat jelas
