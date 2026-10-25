# 🚢 PANDUAN LENGKAP & SOLUSI INSTAN DEPLOYMENT HOSTINGER
**Domain:** `https://sinahkoda.puskesmasseribuselatan.com/`

Dokumen ini memberikan solusi langsung agar aplikasi **SI NAKHODA** tampil sempurna tanpa kendala layar putih atau macet di tulisan *"Memuat Sistem Navigasi & Posisi Pegawai..."*.

---

## 🔍 MENGAPA TAMPILAN MACET DI SCREENSHOT ANDA?

Pada screenshot yang Anda kirimkan:
- Anda mengakses URL: `https://sinahkoda.puskesmasseribuselatan.com/dist/`
- Tampilan macet di teks *"Memuat Sistem Navigasi & Posisi Pegawai..."*

### Penyebabnya:
1. **Server menyajikan file mentah (`src/main.tsx`), bukan JavaScript hasil kompilasi produksi.** Hostinger LiteSpeed menolak file TypeScript mentah sehingga aplikasi React tidak dapat me-render halaman.
2. **Aset path sebelumnya menggunakan path absolut (`/assets/...`).** Jika dibuka dari subfolder `/dist/`, browser mencari aset di root sehingga file JavaScript tidak ditemukan.

---

## 🛠️ APA YANG TELAH KAMI PERBAIKI DALAM KODE:

1. **Path Aset Relatif (`base: './'`):**
   - Konfigurasi Vite kini telah diubah ke path relatif (`./assets/...`).
   - Artinya, baik Anda mengakses `https://sinahkoda.puskesmasseribuselatan.com/` langsung ATAU via subfolder `https://sinahkoda.puskesmasseribuselatan.com/dist/`, seluruh file JS, CSS, dan icon akan terpanggil sempurna tanpa 404!
2. **Kompilasi Produksi Penuh (`dist/`):**
   - File `dist/index.html` dan seluruh folder `dist/assets/` sudah kami bangun ulang secara bersih.
3. **Konfigurasi `.htaccess` LiteSpeed:**
   - Menyertakan header `DirectoryIndex dist/index.html index.html` dan MIME type `application/javascript` resmi agar server Hostinger LiteSpeed tidak memperlakukan file script sebagai teks biasa.
4. **Pembersihan Redirect Loop:**
   - Menghapus script pengecekan manual yang sebelumnya memicu pengalihan URL yang tidak diinginkan.

---

## ⚡ CARA AGAR LANGSUNG TAMPIL (PILIH SALAH SATU):

### 🌟 CARA TERMUDAH & PALING CEPAT (1 Menit via File Manager Hostinger)

Ini adalah cara standar yang direkomendasikan langsung oleh Hostinger untuk aplikasi React/Vite:

1. Buka **hPanel Hostinger** (`hpanel.hostinger.com`) ➔ Masuk ke **File Manager**.
2. Masuk ke dalam folder **`public_html/`**.
3. **PENTING:** Hapus file mentah lama yang ada di dalam `public_html/` (seperti folder `src/`, file `package.json`, dll).
4. Ambil **seluruh isi** di dalam folder **`dist/`** proyek ini, lalu upload langsung ke dalam **`public_html/`**:
   - `index.html` (file hasil build produksi)
   - folder `assets/` (berisi file `.js` dan `.css`)
   - `.htaccess`
   - `manifest.webmanifest`, `favicon.ico`, `icon.svg`, gambar pwa
   *(Catatan: Jangan upload folder `dist`-nya, melainkan file-file yang ada di DALAM folder `dist` langsung ke `public_html`).*
5. Buka `https://sinahkoda.puskesmasseribuselatan.com/` di browser (tekan `Ctrl + F5` untuk hard refresh).
6. **Aplikasi SI NAKHODA langsung tampil sempurna!**

---

### 🔄 CARA KEDUA: VIA HOSTINGER GIT (SINKRONISASI GITHUB)

Jika Anda ingin menggunakan fitur Git di Hostinger:
1. Pastikan Anda melakukan **push** commit terbaru dari repository ini ke GitHub Anda.
2. Di hPanel Hostinger ➔ **Git** (di menu *Advanced*):
   - Klik tombol **Deploy** / **Pull** terbaru.
3. Karena folder `dist/` dan file `.htaccess` cerdas sekarang sudah disertakan di repository, web server Hostinger akan otomatis menyajikan file produksi yang ada di `dist/`.
4. Buka `https://sinahkoda.puskesmasseribuselatan.com/` (buka langsung alamat utamanya tanpa `/dist/`).

---

## 📱 MENGHUBUNGKAN GOOGLE APPS SCRIPT SETELAH TAMPIL:
1. Buka web SI NAKHODA ➔ Klik **"LAPOR Si Nahkoda"**.
2. Klik tombol **Login Admin** (atau tekan shortcut `Ctrl + Shift + A`):
   - NIP: `12345`
   - Password: `admin`
3. Masuk ke tab **Integrasi Database** ➔ Masukkan URL Web App Google Apps Script Anda.
4. Klik **"Tarik Data Pegawai dari Spreadsheet"** ➔ Data langsung tersinkronisasi realtime!
