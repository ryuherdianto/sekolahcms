<div align="center">

<img src="https://github.com/ryuherdianto/sekolahcms/blob/main/banner.jpg?raw=true" alt="Dashboard" width="80%">

### Sistem Manajemen Konten Modern, Aman, dan Gratis untuk Sekolah Indonesia

**Bangun website sekolah profesional dalam hitungan menit, bukan minggu.**

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![PHP](https://img.shields.io/badge/PHP-8.0+-777BB4?logo=php&logoColor=white)](https://www.php.net/)
[![MySQL](https://img.shields.io/badge/MySQL-8.0+-4479A1?logo=mysql&logoColor=white)](https://www.mysql.com/)
[![Apache](https://img.shields.io/badge/Apache-2.4+-D22128?logo=apache&logoColor=white)](https://httpd.apache.org/)
[![Made in Indonesia](https://img.shields.io/badge/Made%20in-🇮🇩%20Indonesia-red)](https://github.com/ryuherdianto/sekolahcms)

[**📥 Download v1.0.0**](https://github.com/ryuherdianto/sekolahcms/releases/latest) ·
[**🌐 Live Demo**](https://sman14jkt.sch.id) ·
[**📚 Dokumentasi**](#-dokumentasi) ·
[**💬 Diskusi**](https://github.com/ryuherdianto/sekolahcms/discussions)

</div>

---

## ✨ Tentang SekolahCMS

**SekolahCMS** adalah Sistem Manajemen Konten (CMS) yang dirancang khusus untuk kebutuhan website sekolah di Indonesia. Dibangun dari pengalaman nyata mengelola website sekolah, fokus pada:

- 🚀 **Cepat dipasang** — Web installer 7 langkah, tanpa edit konfigurasi manual
- 🛡️ **Aman dari awal** — 8 layer security hardening (CSRF, XSS, SQL injection, brute force, dll)
- 📝 **Mudah dikelola** — Dashboard admin intuitif, tidak perlu skill coding
- 🇮🇩 **Khas Indonesia** — Antarmuka bahasa Indonesia, fitur sesuai kebutuhan sekolah lokal
- 💰 **100% gratis** — Open source di bawah lisensi GPL-3.0

> *"Membangun website sekolah seharusnya tidak butuh budget puluhan juta atau tim developer 10 orang. SekolahCMS membuktikan itu."*

---

## 🎯 Fitur Lengkap

### 📝 Pengelolaan Konten

- **Berita & Kegiatan** — Editor rich text dengan image picker, lampiran multi-file, kategorisasi, slug URL SEO-friendly
- **Halaman Statis** — Buat halaman bebas (Tentang Kami, PPDB, Kurikulum, dll) dengan editor lengkap
- **Hero Slides** — Carousel banner beranda dengan 5 posisi teks fleksibel
- **Pengumuman** — Ticker berjalan dengan tipe info/penting/sukses/mendesak
- **Galeri Foto & Video** — Album-based dengan dukungan YouTube embed, lightbox interaktif
- **Prestasi Siswa** — Filter by tingkat (Internasional/Nasional/Provinsi) & kategori
- **Ekstrakurikuler** — Direktori ekskul dengan jadwal & pembina
- **Kalender Pendidikan** — Event sekolah dengan filter bulan & kategori berwarna
- **Menu Navbar** — Tree menu drag-drop dengan dropdown
- **Tautan Footer** — Quick links dengan ikon Bootstrap
- **Profil Sekolah** — Visi, misi, sejarah, sambutan kepsek, statistik animated
- **Media Library** — Storage terpisah untuk gambar editor (curated vs uploads)

### 🛡️ Keamanan Berlapis

- ✅ **CSRF Protection** — Token verification di semua form & AJAX
- ✅ **XSS Sanitization** — HTML sanitizer berbasis DOMDocument untuk rich editor
- ✅ **SQL Injection Safe** — 100% prepared statements (PDO)
- ✅ **Login Rate Limiting** — Maks 5 percobaan gagal / 15 menit per IP
- ✅ **Password Hashing** — BCRYPT dengan cost adaptive
- ✅ **Upload Security** — 5 layer protection (MIME + ext whitelist + blacklist + .htaccess deny + force download)
- ✅ **Session Security** — httponly, samesite=Strict, regenerate, secure cookies
- ✅ **Production Error Handler** — Tidak bocorkan info sensitif ke user
- ✅ **Audit Log** — Login attempts & activity log dengan IP + user agent

### 🎨 Frontend Publik

- Beranda dinamis dengan hero, statistik, sambutan, prestasi, galeri, berita, kalender
- Halaman daftar berita dengan filter & search
- Detail berita dengan share sosial media (WA/FB/X/TG/Copy Link)
- Halaman daftar prestasi dengan filter
- Profil sekolah lengkap
- Galeri dengan lightbox & filter foto/video
- Kalender pendidikan interaktif
- Pengumuman dengan search & arsip
- 100% responsive (mobile/tablet/desktop)
- Fast loading (vanilla JS, no heavy framework)

### 🔌 Developer Friendly

- **REST API** publik untuk integrasi
- **Headless architecture** — frontend dan backend terpisah
- **No framework lock-in** — vanilla PHP, mudah dimodifikasi
- **Dokumentasi lengkap** — README, INSTALL, SECURITY, CHANGELOG
- **Web installer** — deploy di server fresh dalam 5 menit

---

## 🚀 Instalasi Cepat (3 Langkah)

### 1️⃣ Download & Extract

```bash
# Download paket terbaru
wget https://github.com/ryuherdianto/sekolahcms/releases/latest/download/sekolahcms-v1.0.zip

# Extract ke web root
sudo unzip sekolahcms-v1.0.zip -d /var/www/html/
sudo mv /var/www/html/sekolahcms-v1.0/* /var/www/html/
sudo chown -R www-data:www-data /var/www/html/
sudo chmod -R 775 /var/www/html/cms/{uploads,logs,config}
```

### 2️⃣ Buat Database

```sql
CREATE DATABASE sekolahcms CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'sekolahcms_user'@'localhost' IDENTIFIED BY 'PASSWORD_KUAT_ANDA';
GRANT ALL PRIVILEGES ON sekolahcms.* TO 'sekolahcms_user'@'localhost';
FLUSH PRIVILEGES;
```

### 3️⃣ Jalankan Web Installer

Buka browser ke:
```
https://your-domain.com/install.php
```

Ikuti wizard 7 langkah:
1. Welcome
2. Cek persyaratan sistem
3. Konfigurasi database
4. Import schema
5. Buat akun admin
6. Finalize
7. Selesai → Login!

**Selesai!** ✅ CMS Anda sudah siap dipakai.

📖 Panduan lengkap: [INSTALL.md](INSTALL.md)

---

## 📋 Persyaratan Sistem

| Komponen | Versi Minimum | Rekomendasi |
|---|---|---|
| **OS** | Ubuntu 20.04 / Debian 11 | Ubuntu 22.04 LTS |
| **Web Server** | Apache 2.4 | Apache 2.4 + mod_rewrite |
| **PHP** | 8.0 | 8.1+ |
| **MySQL/MariaDB** | MySQL 8.0 / MariaDB 10.5 | MySQL 8.0+ |
| **RAM** | 1 GB | 2 GB |
| **Storage** | 5 GB | 10 GB+ |

**Ekstensi PHP yang dibutuhkan:**
`pdo_mysql`, `gd`, `fileinfo`, `mbstring`, `dom`, `json`

---

## 🛠️ Tech Stack

- **Backend:** Vanilla PHP 8.0+ (no framework)
- **Database:** MySQL/MariaDB dengan PDO
- **Web Server:** Apache 2.4 + mod_rewrite
- **Frontend:** HTML5 + CSS3 + Bootstrap 5.3 + Vanilla JavaScript
- **Editor:** Custom rich text editor dengan image picker
- **Icons:** Bootstrap Icons
- **Fonts:** Google Fonts (Montserrat)

> 💡 **Mengapa vanilla PHP?** Framework seperti Laravel/CodeIgniter punya overhead besar dan attack surface luas. Untuk CMS sederhana seperti website sekolah, vanilla PHP lebih ringan, lebih cepat, dan lebih mudah di-audit security-nya.

---

## 🌟 Mengapa SekolahCMS?

### Vs WordPress
- ✅ Tidak ada plugin vulnerability hell — security terkontrol
- ✅ Performance lebih cepat — tidak ada overhead plugin/theme
- ✅ Khusus untuk sekolah — fitur sudah disesuaikan
- ❌ Ekosistem lebih kecil — tidak ada marketplace plugin

### Vs Build Custom
- ✅ Hemat waktu — tidak perlu coding dari nol (3 hari vs 3 bulan)
- ✅ Sudah teruji — 8 layer security hardening
- ✅ Dokumentasi lengkap — onboarding tim IT cepat
- ❌ Kurang fleksibel kalau butuh kustomisasi ekstrem

### Vs CMS Sekolah Komersial
- ✅ **100% gratis** vs subscription Rp 2-10 juta/tahun
- ✅ **Self-hosted** — data sekolah di server sendiri
- ✅ **Open source** — bisa dimodifikasi sesuai kebutuhan
- ❌ Perlu admin teknis untuk setup awal

---

## 🌐 Live Demo

Lihat SekolahCMS dalam aksi di website resmi **SMA Negeri 14 Jakarta**:

🔗 **[sman14jkt.sch.id](https://sman14jkt.sch.id)**

Implementasi production yang sudah running stabil dengan ribuan pengunjung per bulan.

---

## 📚 Dokumentasi

- 📖 [**README.md**](README.md) — Overview (Anda di sini)
- 🚀 [**INSTALL.md**](INSTALL.md) — Panduan instalasi lengkap
- 🛡️ [**SECURITY.md**](SECURITY.md) — Detail keamanan & best practices
- 📝 [**CHANGELOG.md**](CHANGELOG.md) — History perubahan versi
- 🤝 [**CONTRIBUTING.md**](CONTRIBUTING.md) — Panduan kontribusi
- 📄 [**LICENSE**](LICENSE) — Lisensi GPL-3.0

---

## 🗺️ Roadmap

### v1.0 (Current Release)
- ✅ Core CMS dengan 17 modul
- ✅ Web installer
- ✅ 8 layer security
- ✅ Frontend publik responsive
- ✅ REST API

### v1.1 (Planned)
- ⬜ Two-factor authentication (2FA) untuk admin
- ⬜ Backup database otomatis (built-in scheduler)
- ⬜ RSS feed untuk berita
- ⬜ Sitemap.xml generator
- ⬜ Multi-bahasa (ID/EN)

### v2.0 (Future)
- ⬜ Sistem komentar dengan moderation
- ⬜ Newsletter subscription dengan email queue
- ⬜ Versioning konten (history edit)
- ⬜ PWA (offline-capable)
- ⬜ Plugin/extension system

Punya ide fitur? [Buka Discussion](https://github.com/ryuherdianto/sekolahcms/discussions/new) atau [submit Issue](https://github.com/ryuherdianto/sekolahcms/issues/new)!

---

## 🤝 Berkontribusi

Kontribusi sangat dihargai! Apapun bentuknya:

- 🐛 [Lapor bug](https://github.com/ryuherdianto/sekolahcms/issues/new?template=bug_report.md)
- 💡 [Usulkan fitur baru](https://github.com/ryuherdianto/sekolahcms/issues/new?template=feature_request.md)
- 📝 Perbaikan dokumentasi
- 🌐 Translation
- 🎨 Tema/styling
- 🔧 Pull request kode

Lihat [CONTRIBUTING.md](CONTRIBUTING.md) untuk panduan lengkap.

### Hall of Fame Kontributor

<a href="https://github.com/ryuherdianto/sekolahcms/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=ryuherdianto/sekolahcms" />
</a>

---

## 💬 Komunitas & Bantuan

- 💭 **Diskusi umum:** [GitHub Discussions](https://github.com/ryuherdianto/sekolahcms/discussions)
- 🐛 **Bug & feature request:** [GitHub Issues](https://github.com/ryuherdianto/sekolahcms/issues)
- 📧 **Kontak langsung:** [Email pengembang](mailto:rizha.yuherdianto@gmail.com)

---

## 📊 Statistik

![GitHub stars](https://img.shields.io/github/stars/ryuherdianto/sekolahcms?style=social)
![GitHub forks](https://img.shields.io/github/forks/ryuherdianto/sekolahcms?style=social)
![GitHub downloads](https://img.shields.io/github/downloads/ryuherdianto/sekolahcms/total?label=Downloads)
![GitHub last commit](https://img.shields.io/github/last-commit/ryuherdianto/sekolahcms)
![GitHub issues](https://img.shields.io/github/issues/ryuherdianto/sekolahcms)

---

## 🙏 Penghargaan

SekolahCMS dibangun dari inspirasi dan kontribusi banyak pihak:

- **Bootstrap Team** — UI framework
- **PHP Community** — Bahasa & ekosistem
- **MySQL Team** — Database engine
- **Komunitas open-source Indonesia** — Inspirasi & dukungan

<!--
====================================================================
SECTION LAYANAN SETUP - Untuk ditambahkan ke README.md
====================================================================
Letakkan section ini di README.md, persis SEBELUM section "☕ Dukung Pengembangan"
(atau setelah section "🌐 Live Demo")
====================================================================
-->

---

## 🛠️ Tidak Mau Ribet Setup Sendiri?

Sudah download SekolahCMS tapi pusing dengan command line? Bingung setup VPS, install Apache, atau konfigurasi domain? **Tenang, kami siap bantu.**

Cocok untuk:
- 🏫 **Kepala Sekolah** yang ingin website jadi cepat tanpa harus mempelajari teknis IT
- 👨‍🏫 **Tim IT Sekolah** yang sibuk dengan tugas lain dan butuh setup yang siap pakai
- 🚀 **Sekolah baru** yang ingin website online dalam hitungan hari, bukan minggu

### 💼 Layanan yang Tersedia

<table>
<tr>
<td>🖥️</td>
<td><strong>Setup Server VPS</strong> dari nol — pemilihan provider, instalasi OS, security hardening</td>
</tr>
<tr>
<td>📦</td>
<td><strong>Instalasi SekolahCMS</strong> + konfigurasi sesuai identitas sekolah</td>
</tr>
<tr>
<td>🌐</td>
<td><strong>Setup Domain & DNS</strong> — pemilihan, registrasi, dan pointing ke server</td>
</tr>
<tr>
<td>🔒</td>
<td><strong>Install SSL/HTTPS</strong> via Let's Encrypt (gratis, auto-renewal)</td>
</tr>
<tr>
<td>🔄</td>
<td><strong>Migrasi dari hosting lama</strong> — transfer data tanpa kehilangan konten</td>
</tr>
<tr>
<td>🎨</td>
<td><strong>Custom Modifikasi</strong> — sesuaikan warna, layout, atau tambah fitur khusus</td>
</tr>
<tr>
<td>🎓</td>
<td><strong>Training Admin Sekolah</strong> — cara pakai CMS untuk staff (1-2 sesi online)</td>
</tr>
<tr>
<td>💾</td>
<td><strong>Setup Backup Otomatis</strong> — daily backup dengan rotasi 7 hari</td>
</tr>
<tr>
<td>🛡️</td>
<td><strong>Maintenance & Support Bulanan</strong> — update keamanan, monitoring, troubleshooting</td>
</tr>
</table>

### 🌍 Layanan 100% Online

Tidak peduli sekolah Anda di Aceh, Papua, atau di antara keduanya — kami melayani **seluruh Indonesia secara remote** via WhatsApp dan AnyDesk. Tidak perlu meeting tatap muka, tidak perlu antri.

### 💬 Konsultasi Gratis

Punya pertanyaan? Belum yakin paket mana yang cocok? **Konsultasi awal sepenuhnya gratis** — kami akan analisa kebutuhan sekolah Anda dan kasih rekomendasi yang sesuai dengan budget.

<div align="center">

<a href="https://wa.me/6287779049421?text=Halo,%20saya%20tertarik%20dengan%20layanan%20setup%20SekolahCMS.%20Bisa%20konsultasi?" target="_blank">
  <img src="https://img.shields.io/badge/💬_Konsultasi_via_WhatsApp-25D366?style=for-the-badge&logo=whatsapp&logoColor=white" alt="WhatsApp" height="50">
</a>

<br>

**📱 +62 877-7904-9421**

*Respon cepat di jam kerja (08.00 - 21.00 WIB)*

</div>

### 🎁 Yang Anda Dapatkan

✅ Website sekolah online & siap pakai
✅ HTTPS aktif (gembok hijau di browser)
✅ Panel admin yang sudah dikonfigurasi
✅ Backup otomatis tersetting
✅ Training penggunaan untuk tim sekolah
✅ Garansi support pasca-instalasi

> 💡 **Catatan:** SekolahCMS sendiri **100% gratis dan open source**. Yang berbayar adalah jasa setup, instalasi, dan dukungan teknis — opsional kalau Anda merasa lebih efisien diserahkan ke yang sudah berpengalaman. Tidak ada vendor lock-in: kapanpun Anda mau migrasi atau handle sendiri, semua kode dan data tetap milik sekolah Anda.

---

---

<!--
====================================================================
SECTION DUKUNGAN - Untuk ditambahkan ke README.md
====================================================================
Letakkan section ini di README.md, persis SEBELUM section "📜 Lisensi"
(atau sebelum closing div terakhir)
====================================================================
-->

---

## ☕ Dukung Pengembangan

SekolahCMS dibangun dan di-maintain di waktu luang sebagai kontribusi untuk dunia pendidikan Indonesia. Jika project ini membantu sekolah Anda, mohon pertimbangkan untuk mendukung pengembangan agar tetap aktif dan **selamanya gratis** untuk semua sekolah:

<div align="center">

<a href="https://trakteer.id/rizha_yuherdianto" target="_blank">
  <img src="https://cdn.trakteer.id/images/embed/trbtn-red-6.png?v=04-12-2024" alt="Trakteer Saya" height="50">
</a>

<br><br>

**[☕ Dukung via Trakteer](https://trakteer.id/rizha_yuherdianto)**

</div>

### Cara Lain untuk Mendukung

Tidak harus dalam bentuk uang — kontribusi non-finansial juga sangat berarti:

- ⭐ **Kasih bintang** repository ini
- 🐛 **Lapor bug** atau usulkan fitur via [Issues](https://github.com/ryuherdianto/sekolahcms/issues)
- 📢 **Share ke sekolah lain** yang membutuhkan
- 📝 **Tulis review** atau blog post tentang SekolahCMS
- 🤝 **Berkontribusi kode/dokumentasi** lihat [CONTRIBUTING.md](CONTRIBUTING.md)
- 💬 **Aktif di [Discussions](https://github.com/ryuherdianto/sekolahcms/discussions)** bantu jawab pertanyaan user lain

### 🎁 Penggunaan Donasi

100% donasi yang masuk akan digunakan untuk:

- 🖥️ Server demo & testing environment
- 📚 Pembuatan dokumentasi & tutorial video
- 🔒 Audit keamanan berkala
- 🎨 Asset (icon, illustration, screenshot)
- ☕ Kopi untuk maintainer agar tetap produktif 😄

Setiap donasi, sekecil apapun, sangat berarti. Terima kasih! 🙏


---

## 📜 Lisensi

SekolahCMS dirilis di bawah lisensi [**GPL-3.0**](LICENSE).

Anda bebas:
- ✅ Menggunakan untuk sekolah Anda
- ✅ Memodifikasi sesuai kebutuhan
- ✅ Mendistribusikan ulang
- ✅ Komersial

Dengan syarat:
- 📄 Mencantumkan lisensi & copyright
- 📝 Membuka source code modifikasi (GPL viral)
- 🔄 Modifikasi harus pakai lisensi sama (GPL-3.0)

---

<div align="center">

### ⭐ Jika SekolahCMS membantu sekolah Anda, mohon kasih bintang!

**Made with ❤️ in Indonesia**

[![Star on GitHub](https://img.shields.io/github/stars/ryuherdianto/sekolahcms?style=for-the-badge&logo=github)](https://github.com/ryuherdianto/sekolahcms)

</div>
