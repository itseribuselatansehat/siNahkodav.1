# 🚢 PANDUAN DEPLOYMENT & SOLUSI LAYAR PUTIH: SI NAKHODA DI HOSTINGER

Dokumen ini menjelaskan **penyebab utama layar putih (blank screen)** pada domain `https://sinahkoda.puskesmasseribuselatan.com/` dan **cara memperbaikinya dalam 1 menit**, serta cara sinkronisasi otomatis GitHub ke Hostinger.

---

## 🔍 PENYEBAB LAYAR PUTIH DI `sinahkoda.puskesmasseribuselatan.com`

Saat kami melakukan pengecekan langsung ke server Hostinger Anda melalui curl:
```bash
curl -sL https://sinahkoda.puskesmasseribuselatan.com/
```
Server mengembalikan tag script berikut:
```html
<script type="module" src="/src/main.tsx"></script>
```

**Artinya:** 
Folder `public_html` di Hostinger saat ini berisi **source code mentah** (file `.tsx`), **BUKAN hasil kompilasi produksi** (folder `dist/`).
- Web server Hostinger standar (LiteSpeed/Apache) **tidak bisa menjalankan file TypeScript `.tsx` secara langsung**.
- Browser memerlukan file JavaScript murni (`.js`) dan CSS (`.css`) yang dihasilkan dari perintah `npm run build` yang berada di dalam folder **`dist/`**.

---

## ⚡ CARA 1 (SOLUSI TERCEPAT - 1 MENIT): UPLOAD ISI FOLDER `dist/` VIA FILE MANAGER

Ini adalah cara paling cepat agar `https://sinahkoda.puskesmasseribuselatan.com/` langsung tampil normal:

1. Di komputer lokal Anda (atau download build yang sudah kami buat):
   Jalankan:
   ```bash
   npm run build
   ```
2. Anda akan melihat folder **`dist/`**. Di dalamnya terdapat:
   - `index.html` (berisi `<script src="/assets/index-....js">`)
   - Folder `assets/` (file `.js` dan `.css` hasil kompilasi)
   - `.htaccess` (konfigurasi routing SPA Hostinger)
   - `favicon.ico`, `icon.svg`, `pwa-*.png`, `manifest.webmanifest`
3. Buka **hPanel Hostinger** (`hpanel.hostinger.com`):
   - Masuk ke menu **File Manager** untuk domain `sinakhoda.puskesmasseribuselatan.com`.
   - Buka folder **`public_html/`**.
   - Hapus file-file mentah (`src/`, `package.json`, `tsconfig.json`, `server.ts`, dll) agar bersih.
   - Upload seluruh **ISI** dari folder `dist/` langsung ke dalam `public_html/`.
4. Refresh browser Anda (atau tekan `Ctrl + F5`):
   Website `https://sinahkoda.puskesmasseribuselatan.com/` **langsung tampil sempurna!**

---

## 🔄 CARA 2 (OTOMATIS 100%): MENGGUNAKAN GITHUB ACTIONS & HOSTINGER GIT

Agar setiap kali Anda melakukan update kode di GitHub, Hostinger otomatis memperbarui website tanpa perlu build manual:

Kami telah menyiapkan file `/.github/workflows/deploy.yml` yang:
1. Menerima push kode di branch `main`.
2. Menjalankan `npm run build` di cloud GitHub Actions.
3. Otomatis mem-publish hasil `dist/` ke branch bernama **`deploy`**.

### Langkah Setting di Hostinger Git (Hanya 1 Kali):
1. Buka **hPanel Hostinger** -> cari menu **Git** (di bawah *Advanced*).
2. Jika sudah ada repository sebelumnya, hapus atau ubah pengaturannya:
   - **Repository URL**: `https://github.com/USERNAME_ANDA/si-nahkoda.git`
   - **Branch**: Ganti menjadi **`deploy`** *(PENTING: Jangan pilih `main`, pilih `deploy`)*
   - **Install directory**: `public_html`
3. Klik **Deploy** / **Save**.
4. Salin **Webhook URL** yang diberikan oleh Hostinger.
5. Di repository GitHub Anda -> **Settings** -> **Webhooks** -> **Add webhook**:
   - Masukkan Webhook URL Hostinger.
   - Content type: `application/json`.
   - Event: *Just the push event*.
6. **Selesai!**
   - Setiap Anda push kode baru ke branch `main`, GitHub Actions akan otomatis mengkompilasi aplikasi dan mengupdate branch `deploy`.
   - Hostinger Git akan otomatis menarik file dari branch `deploy` ke `public_html`.
   - Website selalu aktif dan tidak akan pernah mengalami layar putih lagi!

---

## 🚀 CARA 3: MENGGUNAKAN GITHUB ACTIONS DENGAN FTP DIRECT DEPLOY

Jika ingin GitHub Actions meng-upload langsung file `dist/` ke Hostinger via FTP:

1. Buka **hPanel Hostinger** -> **Files** -> **Akun FTP**.
2. Catat:
   - Host FTP (contoh: `ftp.puskesmasseribuselatan.com` atau IP server)
   - Username FTP
   - Password FTP
3. Di repository GitHub Anda -> **Settings** -> **Secrets and variables** -> **Actions**:
   - Tambahkan Secret:
     - `FTP_SERVER` = Host FTP Anda
     - `FTP_USERNAME` = Username FTP Anda
     - `FTP_PASSWORD` = Password FTP Anda
4. Setiap push ke `main`, GitHub Actions akan mengunggah isi `dist/` langsung ke `public_html/`.

---

## 💻 CARA 4: MENGGUNAKAN TERMINAL / SSH HOSTINGER

Jika Anda menggunakan paket Hostinger dengan akses SSH / Terminal:
1. Buka Terminal di hPanel atau via PuTTY:
   ```bash
   cd domains/sinakhoda.puskesmasseribuselatan.com/public_html
   npm install
   npm run build
   cp -r dist/* .
   ```
2. Aplikasi langsung aktif seketika.

---

## 🌐 MENGHUBUNGKAN GOOGLE SPREADSHEET (GAS) DI HOSTINGER

Setelah website aktif di Hostinger:
1. Buka `https://sinahkoda.puskesmasseribuselatan.com/`.
2. Klik tombol **"LAPOR Si Nahkoda"**.
3. Klik **Login Admin** (di Navbar atau Sidebar, atau tekan `Ctrl + Shift + A`):
   - NIP: `12345`
   - Password: `admin`
4. Masuk ke tab **Integrasi Database**:
   - Masukkan URL Google Apps Script Web App Anda.
   - Klik **"Tarik Data Pegawai dari Spreadsheet"**.
5. Data seluruh pegawai langsung tersinkronisasi realtime di domain resmi Anda!
