# 🚢 PANDUAN DEPLOYMENT & SOLUSI LAYAR PUTIH: SI NAKHODA DI HOSTINGER
**Domain:** `https://sinahkoda.puskesmasseribuselatan.com/`

Dokumen ini menjelaskan **penyebab utama layar putih (blank screen)** dan memberikan **solusi perbaikan langsung** agar aplikasi tampil sempurna di Hostinger.

---

## 🔍 PENYEBAB LAYAR PUTIH DI `sinahkoda.puskesmasseribuselatan.com`

Sesuai informasi dari tim teknis Hostinger:
> *"Hasil pemeriksaan: DNS, SSL, server, malware, PHP, dan resource hosting normal. Masalahnya ada pada deployment aplikasi React/Vite: file sumber seperti `src/main.tsx` masih disajikan, bukan hasil build produksi."*

### Mengapa ini terjadi?
1. Git repository standar awalnya mengabaikan folder `dist/` (karena `.gitignore`).
2. Saat Anda menghubungkan Hostinger Git ke branch `main`, Hostinger meng-clone file mentah (`src/`, `package.json`, root `index.html`) ke dalam `public_html/`.
3. Web server Hostinger (LiteSpeed/Apache) menyajikan `src/main.tsx` sebagai teks biasa (`text/plain`). Browser menolak mengeksekusi TypeScript mentah sehingga layar tetap putih.

---

## 🛠️ APA YANG SUDAH KAMI PERBAIKI UNTUK ANDA:

1. **Folder `dist/` Sekarang Disertakan di Repository Git:**
   - Kami telah menghapus `dist/` dari `.gitignore` dan melakukan kompilasi produksi penuh (`npm run build`).
   - Setiap file JavaScript (`.js`), CSS (`.css`), icon, dan manifest PWA sudah tersedia lengkap di folder `dist/`.

2. **File `.htaccess` Cerdas Dual-Mode di Root & Dist:**
   - Kami membuat file `/.htaccess` di root repository.
   - File `.htaccess` ini secara otomatis mendeteksi: Jika repo di-clone utuh ke `public_html`, server akan **secara transparan mengarahkan semua lalu lintas ke `dist/` dan `dist/index.html`** tanpa perlu konfigurasi tambahan!

3. **Fallback Otomatis di Root `index.html`:**
   - Jika server statis mencoba menyajikan root `index.html`, script deteksi otomatis akan langsung memuat `/dist/` sehingga tidak ada layar putih.

---

## ⚡ 2 CARA MUDAH AGAR WEBSITE LANGSUNG TAMPIL NORMAL:

### PILIHAN 1 (TERCEPAT & INSTAN 1 MENIT) — Lewat File Manager Hostinger
Jika Anda ingin website langsung aktif detik ini juga:
1. Buka **hPanel Hostinger** (`hpanel.hostinger.com`).
2. Masuk ke menu **File Manager** -> Buka folder **`public_html/`**.
3. Hapus file-file mentah lama yang ada di dalam `public_html/` (seperti folder `src/`, `node_modules/`, `package.json`, dll).
4. Masuk ke folder **`dist/`** yang ada di repository proyek ini, lalu upload seluruh **ISI** dari folder `dist/` ke dalam `public_html/`:
   - `index.html` (hasil build Vite)
   - folder `assets/` (file `.js` dan `.css`)
   - `.htaccess`
   - `manifest.webmanifest`, `registerSW.js`, `favicon.ico`, `icon.svg`, gambar pwa
5. Buka kembali `https://sinahkoda.puskesmasseribuselatan.com/` di browser (tekan `Ctrl + F5` untuk hard refresh).
6. **Website langsung tampil sempurna dan responsif!**

---

### PILIHAN 2 (OTOMATIS VIA HOSTINGER GIT)
Karena kami sudah mengaktifkan `dist/` dan `.htaccess` cerdas di repository:
1. Buka **hPanel Hostinger** -> **Git** (di bawah menu *Advanced*).
2. Lakukan **Deploy** / **Pull** ulang repository terbaru Anda.
3. Karena file `/.htaccess` dan folder `dist/` sekarang ikut ditarik oleh Hostinger:
   - Server Hostinger otomatis mendeteksi folder `dist/` dan menyajikan aplikasi produksi secara langsung.
4. Buka `https://sinahkoda.puskesmasseribuselatan.com/` -> website langsung aktif normal.

*(Opsional: Anda juga dapat menggunakan file workflow `/.github/workflows/deploy.yml` yang otomatis mem-publish hasil build ke branch `deploy` setiap kali ada commit baru).*

---

## 🌐 MENGHUBUNGKAN GOOGLE SPREADSHEET (GAS) DI HOSTINGER

Setelah website aktif:
1. Akses `https://sinahkoda.puskesmasseribuselatan.com/`.
2. Klik tombol **"LAPOR Si Nahkoda"** di halaman awal.
3. Masuk ke **Menu Login Admin** (di Navbar pojok kanan atas atau tekan shortcut `Ctrl + Shift + A`):
   - NIP: `12345`
   - Password: `admin`
4. Di Dashboard Admin, buka tab **Integrasi Database**:
   - Masukkan URL Web App Google Apps Script Anda (dari menu *Deploy > New deployment* di Google Spreadsheet).
   - Klik **"Tarik Data Pegawai dari Spreadsheet"**.
5. Data seluruh pegawai langsung tersinkronisasi realtime ke sistem SI NAKHODA!
