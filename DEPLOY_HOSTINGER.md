# 🚢 PANDUAN DEPLOYMENT SI NAKHODA: GITHUB KE HOSTINGER

Aplikasi **SI NAKHODA** (*Sistem Informasi Navigasi Kinerja dan Optimalisasi Data Pegawai*) telah dikonfigurasi dan dioptimasi **100% Siap Deploy** dari GitHub langsung ke hosting **Hostinger** (baik **Hostinger Web / Cloud Hosting** maupun **Hostinger Node.js / VPS**).

---

## 📋 DAFTAR ISI
1. [Langkah 1: Push Source Code ke GitHub](#langkah-1-push-source-code-ke-github)
2. [METODE 1 (Direkomendasikan): Hostinger Web Hosting via GitHub Actions (Otomatis Sync)](#metode-1-hostinger-web-hosting-via-github-actions-otomatis-sync)
3. [METODE 2: Hostinger Web Hosting via Fitur Git hPanel](#metode-2-hostinger-web-hosting-via-fitur-git-hpanel)
4. [METODE 3: Upload Manual via File Manager Hostinger](#metode-3-upload-manual-via-file-manager-hostinger)
5. [METODE 4: Hostinger VPS / Node.js Application Manager (Full-Stack)](#metode-4-hostinger-vps--nodejs-application-manager)
6. [Sinkronisasi Database Google Spreadsheet (GAS) di Hostinger](#sinkronisasi-database-google-spreadsheet-gas-di-hostinger)

---

## 1. Langkah 1: Push Source Code ke GitHub

Jika Anda belum membuat repository di GitHub, lakukan langkah berikut di komputer / terminal:

```bash
# 1. Masuk ke folder project
git init

# 2. Tambahkan semua file
git add .

# 3. Buat commit pertama
git commit -m "feat: SI NAKHODA v2.4 siap deploy Hostinger"

# 4. Buat branch main
git branch -M main

# 5. Hubungkan ke repository GitHub Anda (ganti URL dengan repo Anda)
git remote add origin https://github.com/USERNAME_ANDA/si-nahkoda.git

# 6. Push kode ke GitHub
git push -u origin main
```

---

## 2. METODE 1: Hostinger Web Hosting via GitHub Actions (Otomatis Sync)

Project ini sudah dilengkapi file workflow otomatis: `/.github/workflows/deploy.yml`. Setiap kali Anda melakukan `git push` ke branch `main`, GitHub Actions akan otomatis melakukan:
1. `npm install`
2. `npm run build` (menghasilkan folder `dist/` + file `.htaccess`)
3. Mengunggah folder `dist/` langsung ke `public_html/` Hostinger melalui FTP.

### Cara Mengaktifkan:
1. Buka **hPanel Hostinger** Anda (`hpanel.hostinger.com`).
2. Masuk ke menu **Files** -> **Akun FTP** (FTP Accounts).
3. Catat 3 informasi penting:
   - **Host FTP / IP**: (misal: `ftp.domainanda.com` atau alamat IP server)
   - **Username FTP**: (misal: `u123456789`)
   - **Password FTP**: (password akun FTP Anda)
4. Buka repository Anda di **GitHub** -> tab **Settings** -> pilih **Secrets and variables** -> **Actions**.
5. Klik tombol **New repository secret**, tambahkan 3 secret berikut:
   - Name: `FTP_SERVER` | Value: *(Host FTP Anda)*
   - Name: `FTP_USERNAME` | Value: *(Username FTP Anda)*
   - Name: `FTP_PASSWORD` | Value: *(Password FTP Anda)*
6. Selesai! Sekarang, setiap kali Anda melakukan update kode dan push ke GitHub, website di Hostinger akan **otomatis ter-update dalam 1-2 menit**.

---

## 3. METODE 2: Hostinger Web Hosting via Fitur Git hPanel

Jika Anda lebih memilih menggunakan fitur bawaan Hostinger:
1. Buka **hPanel Hostinger** -> cari menu **Git** (di bawah kategori *Advanced*).
2. Di bagian **Deploy a repository**:
   - **Repository URL**: Masukkan link GitHub Anda (misal: `https://github.com/USERNAME_ANDA/si-nahkoda.git`).
   - **Branch**: `main`
   - **Install path**: Kosongkan atau isi `public_html`.
3. Klik tombol **Create**.
4. Hostinger akan memberikan **Webhook URL**.
5. Buka repository GitHub Anda -> **Settings** -> **Webhooks** -> **Add webhook**:
   - Masukkan Payload URL dari Hostinger.
   - Content type: `application/json`.
   - Pilih *Just the push event*.
   - Klik **Add webhook**.
6. Sekarang setiap push ke GitHub akan otomatis men-trigger pull di Hostinger.

---

## 4. METODE 3: Upload Manual via File Manager Hostinger

Jika ingin langsung mencoba secara instan tanpa konfigurasi webhook:
1. Jalankan build di komputer lokal:
   ```bash
   npm install
   npm run build
   ```
2. Anda akan mendapatkan folder **`dist/`**.
3. Di dalam folder `dist/`, pastikan terdapat:
   - `index.html`
   - Folder `assets/`
   - File `.htaccess` (sudah otomatis ter-copy dari folder `public/`)
   - `favicon.ico`, `manifest.webmanifest`, dll.
4. Buka **hPanel Hostinger** -> **File Manager** -> buka folder **`public_html/`**.
5. Upload seluruh **isi** folder `dist/` langsung ke dalam `public_html/`.
6. Akses domain Anda di browser: `https://domainanda.com`.

> **Catatan Penting:** File `.htaccess` yang telah disediakan menjamin bahwa routing SPA (Single Page Application) berjalan lancar tanpa error 404 saat halaman di-refresh.

---

## 5. METODE 4: Hostinger VPS / Node.js Application Manager

Jika paket Hostinger Anda adalah **Hostinger Cloud / Business dengan Node.js** atau **Hostinger VPS**:

### A. Hostinger Node.js Application Manager (hPanel):
1. Buka hPanel -> **Node.js**.
2. Klik **Create Application**:
   - **Node.js version**: 20.x atau 18.x
   - **Application root**: `/public_html` (atau folder aplikasi Anda)
   - **Application startup file**: `server.ts` atau `dist/server.js`
3. Jalankan `npm install` dan `npm run build` pada terminal console hPanel.
4. Klik **Start / Restart Application**.

### B. Hostinger VPS (Ubuntu / Debian dengan PM2):
Project ini sudah dilengkapi file konfigurasi PM2: `ecosystem.config.cjs`.
```bash
# Clone repository
git clone https://github.com/USERNAME_ANDA/si-nahkoda.git /var/www/si-nahkoda
cd /var/www/si-nahkoda

# Install dependencies & Build
npm install
npm run build

# Jalankan via PM2
pm2 start ecosystem.config.cjs
pm2 save
pm2 startup
```

---

## 6. 🌐 SINKRONISASI DATABASE SPREADSHEET (GAS) DI HOSTINGER

Agar data pegawai, lapor diri, dan koordinat tersinkronisasi langsung dengan Google Spreadsheet resmi Puskesmas:

1. Buka website SI NAKHODA Anda di Hostinger: `https://domainanda.com`.
2. Klik tombol **"LAPOR Si Nahkoda"** di halaman awal.
3. Di dalam aplikasi, klik menu **Login Admin** (bisa melalui Navbar atas, Sidebar kiri, atau tekan shortcut keyboard `Ctrl + Shift + A`).
4. Masukkan kredensial Admin:
   - **NIP / Kode**: `12345`
   - **Password**: `admin`
5. Masuk ke menu **Pengaturan Sistem**:
   - Masukkan URL **Google Apps Script Web App** Anda pada kolom Webhook.
   - Klik **"Tarik Data Pegawai dari Spreadsheet"** untuk memastikan koneksi aktif.
6. Seluruh pegawai kini dapat mencari nama mereka dan melakukan lapor diri langsung dari smartphone maupun PC!

---
*SI NAKHODA v2.4 • Puskesmas Kecamatan Kepulauan Seribu Selatan • Menjangkau Kepulauan dengan Hati*
