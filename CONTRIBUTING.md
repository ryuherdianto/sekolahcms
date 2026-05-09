# 🤝 Panduan Berkontribusi untuk SekolahCMS

Terima kasih telah tertarik untuk berkontribusi! SekolahCMS dibangun untuk komunitas sekolah Indonesia, dan setiap kontribusi membantu meningkatkan kualitas pendidikan kita.

---

## 🌟 Cara Berkontribusi

Ada banyak cara untuk membantu, tidak harus selalu menulis kode:

### 1. 🐛 Lapor Bug

Menemukan bug? Buka [issue baru](https://github.com/ryuherdianto/sekolahcms/issues/new?template=bug_report.md) dengan informasi:

- **Deskripsi bug** — apa yang terjadi
- **Cara reproduksi** — langkah-langkah untuk reproduce
- **Yang diharapkan** — behavior yang benar seharusnya
- **Screenshot** — kalau ada
- **Environment** — versi PHP, MySQL, browser, OS

### 2. 💡 Usulkan Fitur Baru

Punya ide untuk fitur baru? Diskusikan dulu di [Discussions](https://github.com/ryuherdianto/sekolahcms/discussions) atau langsung buka [feature request](https://github.com/ryuherdianto/sekolahcms/issues/new?template=feature_request.md).

### 3. 📝 Perbaikan Dokumentasi

Dokumentasi yang baik = onboarding yang cepat. Anda bisa:

- Perbaiki typo atau grammar
- Tambah contoh penggunaan
- Translate ke bahasa lain
- Tulis tutorial baru

### 4. 🔧 Pull Request Kode

Lihat panduan teknis di bawah.

---

## 🛠️ Panduan Pull Request

### Persiapan Environment

```bash
# Fork repository di GitHub, lalu clone fork Anda
git clone https://github.com/USERNAME-ANDA/sekolahcms.git
cd sekolahcms

# Tambah upstream remote
git remote add upstream https://github.com/ryuherdianto/sekolahcms.git

# Buat branch baru untuk fitur Anda
git checkout -b feature/nama-fitur-anda
```

### Coding Guidelines

#### PHP
- **PSR-12** coding style
- Indentasi 4 spasi (bukan tab)
- Gunakan **prepared statements** untuk semua query database
- Jangan pernah `eval()` atau `exec()` data dari user
- Validasi & sanitasi semua input
- Tambah comment untuk logic kompleks (Bahasa Indonesia OK)

#### JavaScript
- Vanilla JS (no jQuery, no framework baru)
- Gunakan `const`/`let`, hindari `var`
- Async/await untuk Promise
- 2 atau 4 spasi indent (konsisten dengan file existing)

#### CSS
- Mobile-first approach
- BEM atau utility classes (tidak campur)
- Gunakan CSS variables untuk warna utama

#### HTML
- Semantic HTML5 (`<header>`, `<main>`, `<article>`, dll)
- Accessibility (alt text, ARIA labels)
- Validasi di [validator.w3.org](https://validator.w3.org)

### Commit Message Convention

Gunakan format **Conventional Commits**:

```
<type>: <description>

[optional body]
```

**Type yang disarankan:**

| Type | Deskripsi |
|---|---|
| `feat` | Fitur baru |
| `fix` | Bug fix |
| `docs` | Perubahan dokumentasi |
| `style` | Perubahan styling/CSS |
| `refactor` | Refactor kode tanpa ubah behavior |
| `perf` | Performance improvement |
| `test` | Tambah/perbaiki testing |
| `security` | Patch keamanan |
| `chore` | Maintenance, dependency updates |

**Contoh:**
```
feat: add 2FA authentication for admin
fix: prevent XSS in pengumuman editor
docs: update INSTALL.md with HTTPS guide
security: add rate limit to API endpoints
```

### Submit Pull Request

```bash
# Pastikan branch up-to-date
git fetch upstream
git rebase upstream/main

# Push ke fork Anda
git push origin feature/nama-fitur-anda
```

Lalu buka Pull Request di GitHub dengan:

1. **Judul jelas** sesuai format conventional commits
2. **Deskripsi detail** — apa yang diubah dan kenapa
3. **Screenshots** untuk perubahan UI
4. **Checklist** — testing yang sudah dilakukan
5. **Link ke issue** terkait (kalau ada)

### Review Process

- Tim akan review dalam 3-7 hari
- Mungkin akan ada request perubahan (changes requested)
- Setelah approve → akan di-merge ke `main`
- Pastikan CI/CD tests pass (kalau ada)

---

## 🔒 Lapor Vulnerability Keamanan

**JANGAN** publish security vulnerability di public issue tracker. Sebagai gantinya:

1. Email langsung ke: **rizha.yuherdianto@gmail.com**
2. Sertakan: deskripsi, langkah reproduksi, dampak potensial
3. Tunggu konfirmasi sebelum disclose
4. Setelah patch dirilis, Anda akan di-credit di CHANGELOG

---

## 📜 Code of Conduct

### Komitmen Kami

Kami berkomitmen menjadikan SekolahCMS sebagai komunitas yang **inklusif, ramah, dan profesional** untuk semua kontributor — tanpa memandang:
- Latar belakang & pengalaman
- Identitas gender & ekspresi
- Suku, ras, agama
- Tingkat pendidikan & kondisi fisik

### Perilaku yang Diharapkan

- Gunakan bahasa yang sopan & inklusif
- Hormati pendapat & sudut pandang berbeda
- Terima kritik konstruktif dengan baik
- Fokus pada kebaikan komunitas
- Tunjukkan empati ke sesama

### Perilaku yang Tidak Diterima

- Komentar/bahasa kasar, diskriminatif, atau melecehkan
- Personal attack atau political attack
- Trolling, insulting, atau derogatory comments
- Public/private harassment
- Publishing private information tanpa izin

### Penegakan

Pelanggaran code of conduct dapat dilaporkan ke maintainer via email. Semua laporan akan ditangani serius dan rahasia.

---

## 🎁 Penghargaan untuk Kontributor

Setiap kontributor akan:

- ✅ Tercatat di [Contributors](https://github.com/ryuherdianto/sekolahcms/graphs/contributors) GitHub
- ✅ Disebut di CHANGELOG untuk feature/fix kontribusi
- ✅ Mendapat "Hall of Fame" badge di README

---

## ❓ Pertanyaan?

- 💬 [GitHub Discussions](https://github.com/ryuherdianto/sekolahcms/discussions) — Pertanyaan umum
- 🐛 [GitHub Issues](https://github.com/ryuherdianto/sekolahcms/issues) — Bug & feature
- 📧 [Email](mailto:rizha.yuherdianto@gmail.com) — Kontak langsung

Terima kasih sudah membaca! Kami menantikan kontribusi Anda. 🚀
