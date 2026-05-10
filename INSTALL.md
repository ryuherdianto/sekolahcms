# 📦 Panduan Instalasi SekolahCMS

Panduan lengkap deploy SekolahCMS dari nol di server Ubuntu/Debian. Ikuti step-by-step, total waktu ~10-15 menit.

---

## 📋 Daftar Isi

1. [Persyaratan Sistem](#persyaratan-sistem)
2. [Persiapan Server](#1-persiapan-server)
3. [Setup Database](#2-setup-database)
4. [Download & Extract SekolahCMS](#3-download--extract-sekolahcms)
5. [Set Permission](#4-set-permission)
6. [Konfigurasi Apache](#5-konfigurasi-apache)
7. [Jalankan Web Installer](#6-jalankan-web-installer)
8. [Konfigurasi Tambahan](#7-konfigurasi-tambahan)
9. [HTTPS dengan Let's Encrypt](#8-https-dengan-lets-encrypt)
10. [Backup Database Otomatis](#9-backup-database-otomatis)
11. [Troubleshooting](#-troubleshooting)

---

## 🖥️ Persyaratan Sistem

### Server
| Komponen | Minimum | Rekomendasi |
|---|---|---|
| **OS** | Ubuntu 20.04 / Debian 11 | Ubuntu 22.04 LTS |
| **RAM** | 1 GB | 2 GB |
| **Storage** | 5 GB | 10 GB+ |
| **CPU** | 1 vCPU | 2 vCPU |

### Software
- **Apache** 2.4+ dengan `mod_rewrite` dan `mod_headers`
- **PHP** 8.0+ dengan ekstensi: `pdo_mysql`, `gd`, `exif`, `fileinfo`, `mbstring`, `dom`, `json`, `zip`
- **MySQL** 8.0+ atau **MariaDB** 10.5+

### Akses
- Akses SSH ke server (`root` atau user dengan `sudo`)
- Akses ke panel manajemen domain (untuk pointing DNS)

---

## 1. Persiapan Server

### Update sistem dan install LAMP stack

```bash
# Update package list
sudo apt update && sudo apt upgrade -y

# Install LAMP stack lengkap
sudo apt install -y apache2 mysql-server \
  php php-cli php-mysql php-gd php-exif \
  php-mbstring php-xml php-curl php-zip \
  libapache2-mod-php unzip

# Aktifkan modul Apache yang dibutuhkan
sudo a2enmod rewrite headers
sudo systemctl restart apache2
```

### Verifikasi Versi

```bash
php -v          # PHP 8.0+
apache2 -v      # Apache 2.4+
mysql --version # MySQL 8.0+
```

### Verifikasi Ekstensi PHP

```bash
php -m | grep -E "^(pdo_mysql|gd|exif|fileinfo|mbstring|dom|json)$"
```

Harus muncul **7 baris**. Kalau ada yang kurang, install paket-nya:

```bash
sudo apt install -y php-{ekstensi-yang-kurang}
sudo systemctl reload apache2
```

---

## 2. Setup Database

### Login ke MySQL sebagai root

```bash
sudo mysql
```

### Buat database dan user khusus untuk SekolahCMS

Di prompt MySQL, jalankan:

```sql
-- Buat database dengan UTF-8 lengkap (support emoji & karakter internasional)
CREATE DATABASE sekolahcms 
  CHARACTER SET utf8mb4 
  COLLATE utf8mb4_unicode_ci;

-- Buat user khusus (jangan pakai root!)
CREATE USER 'sekolahcms_user'@'localhost' 
  IDENTIFIED BY 'GANTI_DENGAN_PASSWORD_KUAT';

-- Grant privilege ke database SekolahCMS saja
GRANT ALL PRIVILEGES ON sekolahcms.* TO 'sekolahcms_user'@'localhost';

-- Apply changes
FLUSH PRIVILEGES;

-- Keluar
EXIT;
```

> ⚠️ **PENTING**: Ganti `GANTI_DENGAN_PASSWORD_KUAT` dengan password kuat (min 16 karakter, kombinasi huruf besar/kecil, angka, simbol). **Catat password ini** — akan dibutuhkan di step 6.

### Test koneksi database

```bash
mysql -u sekolahcms_user -p sekolahcms -e "SELECT 'Koneksi OK' AS status;"
```

Masukkan password Anda. Output harus: `status: Koneksi OK`.

---

## 3. Download & Extract SekolahCMS

### Download paket terbaru

```bash
# Download versi terbaru
cd /tmp
wget https://github.com/ryuherdianto/sekolahcms/releases/latest/download/sekolahcms-v1.0.1.zip

# Atau ganti dengan versi spesifik:
# wget https://github.com/ryuherdianto/sekolahcms/releases/download/v1.0.1/sekolahcms-v1.0.1.zip
```

### Extract ke web root

```bash
# Backup web root current (kalau ada)
sudo mv /var/www/html /var/www/html.bak.$(date +%s) 2>/dev/null

# Buat folder fresh
sudo mkdir -p /var/www/html

# Extract dan pindahkan
unzip -q sekolahcms-v1.0.1.zip -d /tmp/
sudo mv /tmp/sekolahcms-v1.0.1/* /var/www/html/
sudo mv /tmp/sekolahcms-v1.0.1/.* /var/www/html/ 2>/dev/null

# Cleanup
rm -rf /tmp/sekolahcms-v1.0.1 /tmp/sekolahcms-v1.0.1.zip
```

### Verifikasi struktur

```bash
ls -la /var/www/html/
```

Harus ada:
- `install.php`
- `index.html`
- `cms/`
- `assets/`

---

## 4. Set Permission

```bash
# Set ownership ke www-data (user Apache)
sudo chown -R www-data:www-data /var/www/html

# Permission default (dapat dibaca semua, ditulis owner)
sudo find /var/www/html -type d -exec chmod 755 {} \;
sudo find /var/www/html -type f -exec chmod 644 {} \;

# Folder-folder yang harus writable oleh Apache
sudo chmod -R 775 /var/www/html/cms/uploads
sudo chmod -R 775 /var/www/html/cms/logs
sudo chmod -R 775 /var/www/html/cms/config

# Buat folder cache jika belum ada
sudo mkdir -p /var/www/html/cms/cache
sudo chown www-data:www-data /var/www/html/cms/cache
sudo chmod 775 /var/www/html/cms/cache
```

---

## 5. Konfigurasi Apache

### Aktifkan AllowOverride untuk `.htaccess`

`.htaccess` digunakan untuk security (deny PHP execution di folder uploads, dll).

Edit konfigurasi Apache:

```bash
sudo nano /etc/apache2/apache2.conf
```

Cari blok `<Directory /var/www/>` (sekitar baris 170), ubah `AllowOverride None` menjadi `AllowOverride All`:

```apache
<Directory /var/www/>
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
```

Simpan (`Ctrl+O`, Enter, `Ctrl+X`).

### Test config dan restart

```bash
sudo apache2ctl configtest    # Harus muncul "Syntax OK"
sudo systemctl restart apache2
```

### Test akses dari browser

Buka browser, akses:
```
http://your-server-ip/
```

Harus muncul halaman beranda SekolahCMS (placeholder dengan "Nama Sekolah").

---

## 6. Jalankan Web Installer

Buka browser, akses:
```
http://your-server-ip/install.php
```

### Wizard 7 Langkah:

#### Step 1: Welcome
Klik **"Mulai Instalasi →"**

#### Step 2: Cek Persyaratan Sistem
Installer akan cek otomatis:
- ✅ PHP ≥ 8.0
- ✅ Ekstensi pdo_mysql, gd, fileinfo, mbstring, dom, json
- ✅ Folder cms/, cms/config/, cms/uploads/ writable
- ✅ File database.sql ada

Kalau semua ✅, klik **"Lanjut →"**.

#### Step 3: Konfigurasi Database
Isi dengan data dari Step 2 (Setup Database):

| Field | Value |
|---|---|
| Database Host | `localhost` |
| Nama Database | `sekolahcms` |
| Username Database | `sekolahcms_user` |
| Password Database | (password yang Anda buat) |

Klik **"Test Koneksi & Lanjut →"**.

#### Step 4: Import Schema
Installer akan import schema database otomatis (~10 detik). Klik **"Import Sekarang →"**.

#### Step 5: Buat Akun Administrator
Buat akun superadmin pertama:

| Field | Contoh |
|---|---|
| Nama Lengkap | Budi Santoso |
| Username | admin |
| Email | admin@sekolah.id |
| Password | (min 8 karakter, kuat) |
| Konfirmasi Password | (sama dengan di atas) |

Klik **"Buat Akun & Lanjut →"**.

#### Step 6: Finalize
Installer akan:
- Generate file `cms/config/database.php`
- Update URL frontend HTML otomatis
- Buat lock file `.installed`

Klik **"Selesaikan Instalasi 🚀"**.

#### Step 7: Selesai!
Installer menampilkan ringkasan instalasi. Klik **"🗑️ Hapus Installer & Login"** untuk:
- Menghapus `install.php` (security)
- Redirect ke halaman login admin

🎉 **Instalasi selesai!** Login dengan kredensial admin yang baru dibuat.

---

## 7. Konfigurasi Tambahan

### A. Set ServerName Apache (hilangkan warning)

```bash
echo "ServerName localhost" | sudo tee /etc/apache2/conf-available/servername.conf
sudo a2enconf servername
sudo systemctl reload apache2
```

### B. Tingkatkan Upload Size PHP

Edit `php.ini`:
```bash
sudo nano /etc/php/8.1/apache2/php.ini
```

Cari dan ubah:
```ini
upload_max_filesize = 20M
post_max_size = 25M
max_execution_time = 60
memory_limit = 256M
```

Restart Apache:
```bash
sudo systemctl restart apache2
```

### C. Aktifkan Firewall (UFW)

```bash
sudo ufw allow OpenSSH
sudo ufw allow 'Apache Full'   # Port 80 & 443
sudo ufw enable
sudo ufw status
```

### D. Sesuaikan Identitas Sekolah

Login ke admin (`http://your-server-ip/cms`), lalu:

1. **Profil Sekolah**: Edit nama, alamat, kontak, visi, misi, dll
2. **Hero Slides**: Tambah/edit slide banner beranda
3. **Menu Navbar**: Sesuaikan menu navigasi
4. **Tautan Footer**: Tambah link sosial media, dll
5. **Upload logo**: di Profil Sekolah → Logo

---

## 8. HTTPS dengan Let's Encrypt

> ⚠️ **Prasyarat**: Domain Anda sudah pointing ke IP server.

### Install Certbot

```bash
sudo apt install -y certbot python3-certbot-apache
```

### Setup VirtualHost dengan domain

Edit:
```bash
sudo nano /etc/apache2/sites-enabled/000-default.conf
```

Tambahkan `ServerName` dan `ServerAlias` di dalam `<VirtualHost *:80>`:

```apache
<VirtualHost *:80>
    ServerName sekolah-anda.sch.id
    ServerAlias www.sekolah-anda.sch.id
    ServerAdmin webmaster@sekolah-anda.sch.id
    DocumentRoot /var/www/html
    
    # ... (config existing) ...
</VirtualHost>
```

Reload:
```bash
sudo apache2ctl configtest
sudo systemctl reload apache2
```

### Generate sertifikat SSL

```bash
sudo certbot --apache -d sekolah-anda.sch.id -d www.sekolah-anda.sch.id
```

Ikuti prompt:
- Email Anda (untuk notifikasi expired)
- Setuju Terms of Service: `Y`
- Share email ke EFF: `N` (opsional)
- HTTPS redirect: pilih opsi `2` (Redirect)

### Test

```bash
curl -I http://sekolah-anda.sch.id   # Harus 301 → HTTPS
curl -I https://sekolah-anda.sch.id  # Harus 200 OK
```

Auto-renewal sudah aktif (cron job otomatis).

---

## 9. Backup Database Otomatis

### Buat script backup

```bash
sudo nano /usr/local/bin/backup-sekolahcms.sh
```

Isi:

```bash
#!/bin/bash
BACKUP_DIR="/var/backups/sekolahcms"
DATE=$(date +%Y%m%d-%H%M%S)
DB_USER="sekolahcms_user"
DB_PASS="GANTI_DENGAN_PASSWORD_DB"
DB_NAME="sekolahcms"

mkdir -p "$BACKUP_DIR"

# Backup database
mysqldump -u $DB_USER -p"$DB_PASS" $DB_NAME | gzip > "$BACKUP_DIR/db_$DATE.sql.gz"

# Hapus backup > 7 hari
find "$BACKUP_DIR" -name "db_*.sql.gz" -mtime +7 -delete

echo "$(date) - Backup OK" >> "$BACKUP_DIR/backup.log"
```

Set executable:
```bash
sudo chmod +x /usr/local/bin/backup-sekolahcms.sh
```

### Setup cron job (jam 2 pagi setiap hari)

```bash
sudo crontab -e
```

Tambahkan:
```
0 2 * * * /usr/local/bin/backup-sekolahcms.sh
```

### Test manual

```bash
sudo /usr/local/bin/backup-sekolahcms.sh
ls -lh /var/backups/sekolahcms/
```

Harus muncul file `db_*.sql.gz`.

---

## 🐛 Troubleshooting

### Error 500 di halaman admin

```bash
# Cek error log Apache
sudo tail -50 /var/log/apache2/error.log

# Pastikan permission benar
sudo chown -R www-data:www-data /var/www/html/cms

# Pastikan database.php benar
cat /var/www/html/cms/config/database.php | head -20
```

### `.htaccess` tidak berfungsi

```bash
# Cek AllowOverride
sudo grep "AllowOverride" /etc/apache2/apache2.conf

# Pastikan mod_rewrite aktif
sudo a2enmod rewrite
sudo systemctl restart apache2
```

### "Access denied for user 'sekolahcms_user'"

```bash
# Test koneksi manual
mysql -u sekolahcms_user -p sekolahcms -e "SELECT 1;"

# Kalau gagal, reset password
sudo mysql -e "ALTER USER 'sekolahcms_user'@'localhost' IDENTIFIED BY 'PASSWORD_BARU';"
sudo mysql -e "FLUSH PRIVILEGES;"

# Update di config
sudo nano /var/www/html/cms/config/database.php
# Ubah DB_PASS dengan password baru
```

### Halaman blank putih / error tidak terlihat

Untuk debugging sementara, set environment ke development:

```bash
sudo nano /var/www/html/cms/config/error-handler.php
```

Ubah:
```php
define('ENVIRONMENT', 'development');
```

Refresh halaman, error akan tampil. **Jangan lupa balikin ke `production` setelah selesai debug**.

### Tidak bisa upload gambar

```bash
# Cek permission folder uploads
ls -la /var/www/html/cms/uploads

# Pastikan writable
sudo chmod -R 775 /var/www/html/cms/uploads
sudo chown -R www-data:www-data /var/www/html/cms/uploads

# Cek upload_max_filesize di php.ini
php -i | grep upload_max_filesize
```

### Installer error "Lock file ada"

```bash
# Hapus lock file kalau perlu install ulang
sudo rm /var/www/html/.installed

# Pastikan installer ada
sudo cp ~/sekolahcms-v1.0.1.zip /tmp/
cd /tmp && unzip sekolahcms-v1.0.1.zip
sudo cp /tmp/sekolahcms-v1.0.1/install.php /var/www/html/
```

---

## 📞 Bantuan Lebih Lanjut

- 🐛 [Lapor bug](https://github.com/ryuherdianto/sekolahcms/issues/new?template=bug_report.md)
- 💡 [Usulkan fitur](https://github.com/ryuherdianto/sekolahcms/issues/new?template=feature_request.md)
- 💬 [Diskusi umum](https://github.com/ryuherdianto/sekolahcms/discussions)
- 📧 [Email pengembang](mailto:rizha.yuherdianto@gmail.com)

---

**Made with ❤️ in Indonesia**

— Tim SekolahCMS
