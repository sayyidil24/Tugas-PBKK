# Tugas 4: Portal Akademik Multi-View

Laporan Tugas Mandiri Pertemuan 4, mata kuliah Pemrograman Berbasis Kerangka Kerja (PBKK), Institut Teknologi Sepuluh Nopember.

## Identitas

| | |
|---|---|
| Nama | [ISI NAMA LENGKAP] |
| NRP | [ISI NRP] |
| Kelas | [ISI KELAS] |
| Dosen Pengampu | [ISI NAMA DOSEN] |

## Deskripsi

Mini-website akademik pribadi yang terdiri dari tiga halaman: Beranda, Profil Mahasiswa, dan Ide Riset (visualisasi rancangan platform Agentic AI beserta formulir pengumpulan ide). Seluruh halaman dikelola oleh satu master layout Blade, sehingga tidak ada struktur HTML yang ditulis berulang di file view.

## Teknologi

- Laravel 13 (Blade templating)
- Tailwind CSS, dikelola lewat NPM dan dikompilasi oleh Vite secara lokal (tanpa CDN)
- PHPUnit untuk pengujian fitur

## Struktur Utama

```
app/Http/Controllers/PageController.php     Satu controller untuk semua halaman
routes/web.php                              Definisi rute
resources/views/layouts/app.blade.php       Master layout (title dinamis, navbar, konten, footer)
resources/views/beranda.blade.php           Halaman Beranda
resources/views/profil.blade.php            Halaman Profil
resources/views/ide-agent.blade.php         Halaman Ide Riset + formulir
resources/views/components/info-card.blade.php       Komponen kartu data profil
resources/views/components/status-banner.blade.php   Komponen notifikasi
resources/css/app.css, vite.config.js       Konfigurasi aset
tests/Feature/PageTest.php                  Pengujian otomatis
```

## Rute

| Metode | URL | Fungsi |
|---|---|---|
| GET | `/` dan `/beranda` | Beranda |
| GET | `/profil-mahasiswa` | Profil mahasiswa |
| GET | `/ide-agent` | Visualisasi alur dan formulir ide |
| POST | `/ide-agent` | Menerima formulir, validasi, lalu redirect dengan pesan status |

## Pemenuhan Spesifikasi dan Rubrik

| Kriteria | Implementasi |
|---|---|
| Pewarisan layout (`@extends`) | Ketiga halaman memakai `@extends('layouts.app')` dengan `@section('title')` dan `@section('konten')`. |
| Blade components dan slots | `<x-info-card>` (props `judul`, slot isi) dipakai di Profil dan Beranda. `<x-status-banner>` (props `tipe`, slot pesan) dipakai untuk alert form dan sambutan. Keduanya memakai `$attributes->merge()`. |
| Vite asset bundling | Tailwind dipasang lewat NPM, dikompilasi Vite, dan dimuat dengan `@vite([...])` di layout. |
| Desain UI dan kerapian | Layout responsif berbasis grid dan flexbox, navbar dengan menu aktif otomatis, tampilan terang dan gelap. |
| Challenge | Kedua tantangan diselesaikan (lihat bagian di bawah). |

## Challenge

**1. Toggle tema dinamis.** Controller membaca `?mode=` dan meneruskan `light` atau `dark` ke view. Layout memasang class `dark` pada elemen `<html>` lewat variabel Blade, dan seluruh elemen memakai varian `dark:` Tailwind. Contoh: `/ide-agent?mode=dark`. Mode gelap tetap terjaga saat berpindah halaman dan setelah formulir dikirim.

**2. Alert status interaktif.** Parameter `?user=` ditampilkan lewat komponen `<x-status-banner>`. Contoh: `/beranda?user=Andi` menampilkan "Selamat datang, Andi!". Nilai parameter di-escape oleh sintaks `{{ }}`, sehingga input berisi tag HTML tidak dieksekusi.

## Tangkapan Layar

Simpan gambar di folder `docs/screenshots/` dengan nama berikut.

### Beranda
![Beranda](docs/screenshots/beranda.png)

### Beranda dengan alert (`/beranda?user=Andi`)
![Beranda dengan alert](docs/screenshots/beranda-user.png)

### Profil Mahasiswa
![Profil](docs/screenshots/profil.png)

### Ide Riset
![Ide Riset](docs/screenshots/ide-agent.png)

### Mode Gelap (`/ide-agent?mode=dark`)
![Mode gelap](docs/screenshots/ide-agent-dark.png)

### Validasi Formulir
![Validasi formulir](docs/screenshots/form-error.png)

## Cara Menjalankan

Prasyarat: PHP, Composer, Node.js dengan npm, dan Git.

```bash
git clone https://github.com/[USERNAME]/[NAMA-REPO].git
cd [NAMA-REPO]

composer install
npm install

cp .env.example .env        # di Windows PowerShell: copy .env.example .env
php artisan key:generate
```

Ubah `SESSION_DRIVER=file` di `.env` agar formulir tidak membutuhkan tabel database.

Jalankan di dua terminal terpisah:

```bash
php artisan serve
npm run dev
```

Lalu buka http://127.0.0.1:8000.

## Pengujian

```bash
php artisan test --filter=PageTest
```

Pengujian mencakup akses ketiga halaman, alert dari parameter `user`, perlindungan terhadap injeksi HTML, mode gelap, serta validasi dan pengiriman formulir.

## Catatan

- Ide yang dikirim lewat formulir belum disimpan ke database. Penyimpanan data dibahas pada pertemuan berikutnya.
- Folder `vendor/` dan file `.env` tidak diunggah ke repositori (dikecualikan oleh `.gitignore`).
