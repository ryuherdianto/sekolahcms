# 🔒 Kebijakan Keamanan SekolahCMS

Keamanan adalah prioritas utama SekolahCMS. Dokumen ini menjelaskan fitur keamanan yang sudah terimplementasi, cara melaporkan vulnerability, dan best practices untuk user.

---

## 📋 Daftar Isi

1. [Versi yang Didukung](#versi-yang-didukung)
2. [Lapor Vulnerability](#-lapor-vulnerability)
3. [Fitur Keamanan Built-in](#-fitur-keamanan-built-in)
4. [Threat Model](#-threat-model)
5. [Best Practices untuk User](#-best-practices-untuk-user)
6. [Security Checklist](#-security-checklist)
7. [Hall of Fame](#-hall-of-fame)

---

## Versi yang Didukung

Hanya versi terbaru yang menerima security updates. Versi lama tidak akan di-patch.

| Versi | Status |
|---|---|
| 1.0.1 | ✅ **Aktif** — menerima security updates |
| 1.0.0 | ⚠️ Recommended upgrade ke 1.0.1 |
| < 1.0.0 | ❌ Tidak didukung |

---

## 🚨 Lapor Vulnerability

### ⛔ JANGAN

- ❌ Buka public issue di GitHub untuk vulnerability
- ❌ Diskusikan di forum public, social media, atau Discord
- ❌ Posting proof-of-concept exploit secara terbuka
- ❌ Akses data user lain tanpa izin

### ✅ LAKUKAN

**Email langsung ke maintainer:**

📧 **rizha.yuherdianto@gmail.com**

**Sertakan informasi berikut:**

1. **Deskripsi vulnerability** — apa masalahnya
2. **Severity assessment** — seberapa parah dampaknya (critical/high/medium/low)
3. **Langkah reproduksi** — bagaimana cara mereproduksi bug
4. **Dampak potensial** — apa yang bisa dilakukan attacker
5. **Saran perbaikan** — kalau ada ide solusi (opsional)
6. **Versi yang affected** — versi SekolahCMS mana
7. **Environment** — PHP version, MySQL version, OS

### Format Email yang Disarankan

```
Subject: [SECURITY] Vulnerability di SekolahCMS - [Brief description]

Severity: High/Medium/Low
Affected version: 1.0.1
Affected component: [misal: Admin login, Image upload, dll]

Deskripsi:
[Detail vulnerability]

Steps to reproduce:
1. ...
2. ...
3. ...

Impact:
[Apa yang bisa dilakukan attacker]

Suggested fix:
[Kalau ada]
```

### Response Timeline

| Tahap | Estimasi Waktu |
|---|---|
| Acknowledgment email | < 48 jam |
| Initial assessment | < 7 hari |
| Patch development | < 14 hari (tergantung severity) |
| Public disclosure | Setelah patch dirilis |

### Disclosure Policy

- **Coordinated disclosure**: Vulnerability tidak akan dipublikasikan sampai patch tersedia
- **Credit**: Reporter akan disebut di [Hall of Fame](#-hall-of-fame) dan release notes (kecuali minta anonymous)
- **No bounty**: SekolahCMS adalah project open source, tidak ada bug bounty program
- **CVE**: Untuk vulnerability serius, akan request CVE ID

---

## 🛡️ Fitur Keamanan Built-in

SekolahCMS dirancang dengan **defense in depth** — beberapa lapisan keamanan agar attacker harus melewati banyak barrier.

### 1. CSRF Protection

Semua form admin dan AJAX endpoint dilindungi CSRF token:

- Token unik per session, di-generate dengan `random_bytes(32)`
- Verifikasi otomatis di setiap POST request
- AJAX request menggunakan header `X-CSRF-Token`
- Token regenerasi setiap 30 menit

**Implementasi:** `cms/config/auth.php` (functions: `generateCsrfToken`, `verifyCsrfToken`, `requireCsrf`)

### 2. XSS Sanitization

Konten dari rich text editor disanitasi menggunakan DOMDocument-based sanitizer:

- Whitelist tag aman (p, h1-h6, strong, em, ul, ol, li, a, img, table, dll)
- Strip semua event handlers (`onclick`, `onerror`, `onload`)
- Block URL berbahaya (`javascript:`, `data:text/html`, `vbscript:`)
- Strip CSS berbahaya (`expression()`, `behavior:`, `@import`)
- Auto-add `rel="noopener noreferrer"` untuk external links (anti-tabnabbing)

**Implementasi:** `cms/config/sanitizer.php` (class `HtmlSanitizer`)

### 3. SQL Injection Prevention

100% query database menggunakan **prepared statements** (PDO):

```php
// ✅ Aman (yang dipakai SekolahCMS)
$stmt = $db->prepare("SELECT * FROM users WHERE username = ?");
$stmt->execute([$username]);

// ❌ TIDAK PERNAH dipakai
$db->query("SELECT * FROM users WHERE username = '$username'");
```

Tidak ada string concatenation di query.

### 4. Login Rate Limiting

Mencegah brute force attack:

- Max **5 percobaan gagal** per IP dalam **15 menit**
- IP yang melebihi limit diblokir 15 menit
- Pesan "sisa Xx percobaan" untuk informasi user
- Auto-cleanup data > 24 jam

**Implementasi:** `cms/config/auth.php` (function: `checkLoginRateLimit`)

### 5. Password Hashing

Password di-hash dengan **BCRYPT**:

- Cost factor adaptive (default 10)
- Salt unik per password
- Tidak bisa di-reverse engineer

```php
$hash = password_hash($password, PASSWORD_BCRYPT);
$valid = password_verify($input, $hash);
```

### 6. Upload Security (5 Layer Protection)

File upload memiliki **5 layer defense**:

#### Layer 1: MIME Validation
```php
$mime = finfo_file(finfo_open(FILEINFO_MIME_TYPE), $file['tmp_name']);
```
Cek **isi file** (bukan ekstensi), tidak bisa di-spoof.

#### Layer 2: Extension Whitelist
Hanya ekstensi aman yang diterima: `pdf, doc, docx, jpg, png, webp, dll`.

#### Layer 3: Extension Blacklist
Filename dengan ekstensi berbahaya **di posisi manapun** di-reject:
- `shell.php` ❌
- `shell.php.pdf` ❌ (trick double extension)
- `shell.phtml` ❌

#### Layer 4: Apache `.htaccess` Deny
File `.htaccess` di folder `uploads/`:
```apache
<FilesMatch "\.(php|phtml|phar|cgi|pl|py|sh)$">
    Require all denied
</FilesMatch>
```
Bahkan kalau file masuk, Apache menolak menjalankannya.

#### Layer 5: Force Download untuk HTML/SVG
```apache
<FilesMatch "\.(html|svg|xml)$">
    Header set Content-Disposition "attachment"
</FilesMatch>
```
Mencegah XSS via HTML/SVG upload.

### 7. Session Security

```php
session_set_cookie_params([
    'httponly' => true,                       // JavaScript tidak bisa baca cookie
    'samesite' => 'Strict',                   // Anti-CSRF
    'secure'   => isset($_SERVER['HTTPS']),   // HTTPS only saat aktif
]);
ini_set('session.use_strict_mode', 1);
session_regenerate_id(true);                  // Regenerate setiap 30 menit
```

### 8. Production Error Handler

`display_errors = Off` di production:

- Error PHP **tidak bocor** ke user (path server, struktur DB, dll)
- Error tetap di-log ke Apache untuk debugging
- User dapat halaman error generik dengan ID unik
- AJAX request dapat JSON response yang aman

**Implementasi:** `cms/config/error-handler.php`

### 9. Audit Log

Semua aktivitas penting tercatat:

- **Activity log**: Setiap CRUD admin (siapa, kapan, apa)
- **Login log**: Semua percobaan login (sukses & gagal) dengan IP & user agent
- Halaman audit khusus superadmin di `/cms/admin/login-log.php`

### 10. Auto-Strip EXIF Metadata

Saat user upload gambar (v1.0.1+):

- Metadata sensitif dihapus: GPS location, camera serial, software version
- Auto-rotate berdasarkan EXIF orientation
- Mencegah privacy leak (foto guru/siswa tidak expose lokasi rumah, dll)

---

## 🎯 Threat Model

### ✅ Yang Diproteksi

| Threat | Mitigasi |
|---|---|
| **SQL Injection** | Prepared statements (PDO) |
| **XSS (Stored & Reflected)** | DOMDocument sanitizer, htmlspecialchars |
| **CSRF** | Token verification, samesite cookies |
| **Brute force login** | Rate limiting 5x/15min per IP |
| **Shell upload** | 5-layer protection |
| **Session hijacking (basic)** | httponly + samesite + regenerate |
| **Information disclosure** | Production error handler |
| **Default credentials** | Mandatory ganti password setelah install |
| **Privacy leak via EXIF** | Auto-strip metadata |

### ❌ Yang TIDAK Diproteksi

SekolahCMS tidak melindungi terhadap:

| Threat | Mitigasi yang Disarankan |
|---|---|
| **Compromised admin account** | Strong password, 2FA (planned v1.1) |
| **Insider threat** | Role-based access, audit log, multiple admin |
| **Server compromise (SSH)** | SSH key only, fail2ban, regular OS updates |
| **DDoS** | Cloudflare/CDN, rate limiting di network level |
| **Physical access** | Server hosting yang aman |
| **Social engineering** | Training admin sekolah |
| **Zero-day in PHP/Apache/MySQL** | Regular updates |

### 🔐 Dependencies & Trust

SekolahCMS bergantung pada:

- **PHP** — Mengikuti security advisory PHP, recommended 8.1+
- **Apache** — Mengikuti security advisory Apache, recommended 2.4.52+
- **MySQL/MariaDB** — Mengikuti security advisory database
- **Bootstrap 5.3** — UI framework (low risk, hanya CSS+JS)
- **Bootstrap Icons** — Icon font (no executable code)

**Tidak ada dependency 3rd party PHP** (no Composer, no Laravel, no symfony) — mengurangi attack surface.

---

## 📋 Best Practices untuk User

### Untuk Admin Sekolah

#### ⚙️ Setelah Install Pertama

- [ ] Ganti password default admin segera
- [ ] Pakai password kuat: min 12 karakter, kombinasi huruf besar/kecil/angka/simbol
- [ ] Aktifkan HTTPS via Let's Encrypt
- [ ] Setup backup database otomatis (cron job)
- [ ] Aktifkan firewall UFW (allow SSH + HTTP/HTTPS only)

#### 🔄 Maintenance Berkala

- [ ] **Mingguan**: Cek halaman Login Log untuk anomali
- [ ] **Bulanan**: Update OS dan packages (`sudo apt update && upgrade`)
- [ ] **Bulanan**: Cek backup database masih jalan dengan baik
- [ ] **6 bulan**: Audit user list — hapus akun yang tidak aktif
- [ ] **Tahunan**: Rotate password admin & database

#### 🚨 Saat Mencurigai Kompromise

```bash
# 1. Cek log percobaan login mencurigakan
sudo less /var/log/apache2/access.log | grep "POST /cms/admin/login.php"

# 2. Cek file aneh di uploads
sudo find /var/www/html/cms/uploads -name "*.php" -o -name "*.phtml"

# 3. Cek activity log di admin (Login Log page)

# 4. Restore dari backup terakhir kalau perlu
gunzip < /var/backups/sekolahcms/db_20260510-020000.sql.gz | mysql -u sekolahcms_user -p sekolahcms

# 5. Ganti SEMUA password (admin, database, SSH)
```

### Untuk Tim IT/Developer

#### 🛠️ Saat Modifikasi Kode

- ✅ **Selalu** gunakan prepared statements untuk query
- ✅ **Selalu** sanitasi input user dengan `sanitizeHtml()` atau `htmlspecialchars()`
- ✅ **Selalu** validasi CSRF token di handler POST: `requireCsrf();`
- ✅ **Selalu** validasi authentication: `requireLogin();`
- ✅ **Selalu** validasi role kalau perlu: `requireRole(['superadmin']);`
- ❌ **JANGAN** trust input user, sekecil apapun
- ❌ **JANGAN** menggunakan `eval()`, `exec()`, `system()` dengan input user
- ❌ **JANGAN** disable error handler di production

#### 📦 Saat Update SekolahCMS

```bash
# Backup dulu (database + files)
mysqldump -u sekolahcms_user -p sekolahcms | gzip > backup-pre-update.sql.gz
tar -czf backup-pre-update-files.tar.gz /var/www/html

# Update file (pertahankan database.php, uploads, logs)
sudo cp -r new-version/cms/admin /var/www/html/cms/admin
sudo cp -r new-version/cms/api /var/www/html/cms/api
sudo cp new-version/cms/config/*.php /var/www/html/cms/config/
# JANGAN replace database.php (sudah ada credentials)
sudo cp new-version/cms/database.sql /var/www/html/cms/

# Run migration kalau ada (lihat CHANGELOG)

# Test
curl -I https://your-domain.com/cms/admin/login.php
```

---

## ✅ Security Checklist

Sebelum production, pastikan:

### Server-Side

- [ ] OS Ubuntu/Debian sudah updated (`sudo apt update && upgrade`)
- [ ] PHP 8.0+ dengan security patch terbaru
- [ ] Apache `display_errors` Off (`/etc/php/8.x/apache2/php.ini`)
- [ ] Apache `expose_php` Off (untuk hide PHP version)
- [ ] Apache `ServerTokens Prod` (di apache2.conf)
- [ ] `AllowOverride All` di blok `/var/www/`
- [ ] Folder uploads punya `.htaccess` deny PHP
- [ ] HTTPS aktif via Let's Encrypt
- [ ] Firewall (UFW) hanya allow SSH + HTTP/HTTPS
- [ ] SSH disable password login (key only)
- [ ] Fail2ban aktif untuk SSH brute force protection

### Database

- [ ] Database user khusus untuk SekolahCMS (BUKAN root)
- [ ] Password database min 16 karakter
- [ ] User database hanya privilege ke schema SekolahCMS (bukan all DB)
- [ ] Backup harian otomatis
- [ ] MySQL `bind-address = 127.0.0.1` (no remote access)

### Application

- [ ] Default credentials sudah diganti
- [ ] Akun admin pakai password kuat
- [ ] File `.installed` ada (installer disabled)
- [ ] File `install.php` sudah dihapus
- [ ] File `cms/config/database.php` permission 640
- [ ] Tidak ada file `*.bak`, `*.sql`, `database.example.php` di production
- [ ] `error-handler.php` set ENVIRONMENT = 'production'

### Operational

- [ ] Backup database otomatis aktif (cek log)
- [ ] Multiple admin (jangan single point of failure)
- [ ] Dokumentasi credentials di tempat aman (1Password, Bitwarden, dll)
- [ ] Plan untuk disaster recovery

---

## 🏆 Hall of Fame

Terima kasih kepada peneliti keamanan yang telah membantu meningkatkan keamanan SekolahCMS:

<!--
Format:
- **[Name]** — [Vulnerability description] (vX.X.X) — [Date]
-->

*Belum ada laporan vulnerability sejauh ini. Jadilah yang pertama!* 🎖️

---

## 📚 Resource Tambahan

### Standard & Best Practices

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [PHP Security Best Practices](https://www.php.net/manual/en/security.php)
- [Apache Security Tips](https://httpd.apache.org/docs/2.4/misc/security_tips.html)
- [MySQL Security Best Practices](https://dev.mysql.com/doc/refman/8.0/en/security-guidelines.html)

### Tools untuk Audit

```bash
# Cek file PHP suspicious
find /var/www/html -name "*.php" -newer /var/log/dpkg.log

# Cek modifikasi file
sudo aide --check

# Scan vulnerability dengan Lynis
sudo apt install -y lynis
sudo lynis audit system
```

---

## 📞 Kontak

- 🔒 **Security email:** rizha.yuherdianto@gmail.com
- 🐛 **Bug umum (non-security):** [GitHub Issues](https://github.com/ryuherdianto/sekolahcms/issues)
- 💬 **Diskusi:** [GitHub Discussions](https://github.com/ryuherdianto/sekolahcms/discussions)

---

**Made with ❤️ in Indonesia**

*Document version: 1.0 — Last updated: 10 Mei 2026*
