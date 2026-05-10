# 📝 Changelog

Semua perubahan penting di SekolahCMS dicatat di sini.

Format mengacu ke [Keep a Changelog](https://keepachangelog.com/id-ID/1.1.0/),
dan project ini mengikuti [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

### Akan Ditambahkan
- 🔐 Two-factor authentication (2FA) untuk superadmin
- 💾 Backup database otomatis built-in (cron scheduler)
- 📡 RSS feed untuk berita
- 🗺️ Sitemap.xml generator
- 🌐 Multi-bahasa (ID/EN)
- 🔧 Auto-setup HTTPS via Certbot integration
- 📊 Analytics dashboard untuk admin

### Dipertimbangkan
- Plugin/extension system
- Komentar di berita dengan moderation
- Newsletter subscription dengan email queue
- Versioning konten (history edit)
- PWA support (offline-capable)

---

## [1.0.1] - 2026-05-10

### ✨ Ditambahkan
- **Image Auto-Optimizer** — Setiap gambar yang di-upload otomatis di-optimize:
  - Auto-resize maksimal 1280×1280px (foto besar otomatis dikecilkan)
  - Auto-compress dengan quality 75% (sweet spot kualitas vs size)
  - Smart PNG → JPEG conversion: PNG tanpa transparansi otomatis dikonversi (hemat 80-90% size)
  - Auto-rotate berdasarkan EXIF orientation (foto dari HP tidak terbalik lagi)
  - Auto-strip EXIF metadata sensitif (GPS location, camera serial)
  - Auto-generate thumbnail 400×400 untuk preview cepat
- File baru: `cms/config/image-optimizer.php`

### 🔒 Keamanan
- Hapus tulisan "Login default: admin / Admin@1234" dari halaman login (`cms/admin/login.php`). Petunjuk ini sebelumnya berbahaya karena memberi tahu attacker default credentials.

### 📈 Performa
- Setelah update ini, foto 5 MB dari HP otomatis menjadi ~250 KB di server (95% reduction)
- Page load lebih cepat, terutama halaman galeri dengan banyak gambar
- Hemat storage server signifikan untuk sekolah dengan banyak konten visual

### 🔧 Diubah
- `cms/config/helpers.php` — function `uploadImage()` sekarang otomatis call `optimizeImage()` setelah upload berhasil

### 📦 Persyaratan Tambahan
- PHP extension `php-gd` (image manipulation)
- PHP extension `php-exif` (auto-rotate dari EXIF)
- Install: `sudo apt install -y php-gd php-exif && sudo systemctl reload apache2`

### 🔗 Resources
- [Release v1.0.1](https://github.com/ryuherdianto/sekolahcms/releases/tag/v1.0.1)
- [Download ZIP](https://github.com/ryuherdianto/sekolahcms/releases/download/v1.0.1/sekolahcms-v1.0.1.zip)

---

## [1.0.0] - 2026-05-09

### 🎉 Initial Release

Rilis perdana SekolahCMS sebagai produk open source untuk sekolah-sekolah di Indonesia.

### ✨ Fitur Pengelolaan Konten (17 Modul)
- **Berita & Kegiatan** — Editor rich text dengan image picker, lampiran multi-file, kategorisasi, slug URL SEO-friendly
- **Halaman Statis** — Buat halaman bebas (Tentang Kami, PPDB, dll) dengan editor lengkap
- **Hero Slides** — Carousel banner beranda dengan 5 posisi teks fleksibel
- **Pengumuman** — Ticker berjalan dengan 4 tipe (info/penting/sukses/mendesak)
- **Galeri Foto & Video** — Album-based dengan dukungan YouTube embed, lightbox interaktif
- **Prestasi Siswa** — Filter by tingkat (Internasional/Nasional/Provinsi) & kategori
- **Ekstrakurikuler** — Direktori ekskul dengan jadwal & pembina
- **Kalender Pendidikan** — Event sekolah dengan filter bulan & kategori berwarna
- **Menu Navbar** — Tree menu drag-drop dengan dropdown
- **Tautan Footer** — Quick links dengan ikon Bootstrap
- **Profil Sekolah** — Visi, misi, sejarah, sambutan kepsek, statistik animated
- **Media Library** — Storage terpisah untuk gambar editor (curated vs uploads)
- **Manajemen User** — 3 role (Superadmin/Admin/Editor)
- **Audit Log** — Login attempts & activity log dengan IP + user agent

### 🛡️ Keamanan Berlapis (8 Layer)
- ✅ CSRF Protection di semua form & AJAX
- ✅ XSS Sanitization dengan DOMDocument
- ✅ SQL Injection prevention (100% PDO prepared statements)
- ✅ Login Rate Limiting (5x/15min per IP)
- ✅ Password BCRYPT hashing
- ✅ Upload Security 5-layer (MIME + ext whitelist + blacklist + .htaccess + force download)
- ✅ Session security (httponly, samesite=Strict, regenerate)
- ✅ Production error handler (no info leak)

### 🌐 Frontend Publik (9 Halaman)
- `index.html` — Beranda dengan hero, statistik, sambutan, prestasi, galeri, berita, kalender
- `berita-kegiatan.html` — Daftar berita dengan filter & search
- `berita-page.html` — Detail berita dengan share sosial media
- `prestasi.html` — Daftar prestasi dengan filter
- `profil-sekolah.html` — Profil sekolah lengkap
- `galeri.html` — Galeri dengan lightbox interaktif
- `kalender.html` — Kalender pendidikan interaktif
- `pengumuman.html` — Daftar pengumuman dengan search & arsip
- `halaman.php` — Halaman statis dinamis

### 🔧 Developer Tools
- ✅ Web installer 7-step (auto-detect requirements, generate config, dll)
- ✅ REST API publik (13 endpoints)
- ✅ Headless architecture (frontend & backend terpisah)
- ✅ Vanilla PHP (no framework lock-in)

### 📚 Dokumentasi
- `README.md` — Landing page dengan badges, fitur, quick start
- `INSTALL.md` — Panduan deployment lengkap
- `SECURITY.md` — Kebijakan keamanan & vulnerability disclosure
- `CONTRIBUTING.md` — Panduan kontribusi
- `LICENSE` — GPL-3.0

### 🎯 Tech Stack
- PHP 8.0+ (vanilla, no framework)
- MySQL 8.0+ / MariaDB 10.5+
- Apache 2.4+ dengan mod_rewrite
- Bootstrap 5.3
- Vanilla JavaScript

### 🌐 Live Demo
[sman14jkt.sch.id](https://sman14jkt.sch.id) — implementasi production di SMA Negeri 14 Jakarta

### 🔗 Resources
- [Release v1.0.0](https://github.com/ryuherdianto/sekolahcms/releases/tag/v1.0.0)
- [Download ZIP](https://github.com/ryuherdianto/sekolahcms/releases/download/v1.0.0/sekolahcms-v1.0.zip)

---

## Format Versi

SekolahCMS mengikuti [Semantic Versioning](https://semver.org):

```
MAJOR.MINOR.PATCH
```

- **MAJOR** (1.x.x) — Breaking changes yang tidak backward-compatible
- **MINOR** (x.1.x) — Fitur baru yang backward-compatible
- **PATCH** (x.x.1) — Bug fix & security patch

## Format Entry Changelog

Setiap rilis mengelompokkan perubahan ke kategori berikut:

- **✨ Ditambahkan** — Fitur baru
- **🔧 Diubah** — Perubahan pada fitur existing
- **⚠️ Deprecated** — Fitur yang akan dihapus di rilis mendatang
- **❌ Dihapus** — Fitur yang sudah dihapus
- **🐛 Diperbaiki** — Bug fixes
- **🔒 Keamanan** — Security patches
- **📈 Performa** — Performance improvements
- **📦 Persyaratan** — Perubahan persyaratan sistem

---

## Cara Berkontribusi ke Changelog

Saat membuat Pull Request, **tambahkan entry ke section `[Unreleased]`**:

```markdown
## [Unreleased]

### Ditambahkan
- Fitur X yang Anda buat ([#123](link-ke-PR))
```

Maintainer akan memindahkan entry tersebut ke section versi baru saat release.

---

## 🔗 Link

- [GitHub Repository](https://github.com/ryuherdianto/sekolahcms)
- [Releases](https://github.com/ryuherdianto/sekolahcms/releases)
- [Issues](https://github.com/ryuherdianto/sekolahcms/issues)
- [Discussions](https://github.com/ryuherdianto/sekolahcms/discussions)
