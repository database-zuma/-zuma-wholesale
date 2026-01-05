# Form Order Wholesale - Project Documentation

## Status: Dalam Pengembangan

---

## File Utama
- **Form HTML**: `Form_Order_GoogleSheets (3).html`
- **Google Sheet**: [Link](https://docs.google.com/spreadsheets/d/1776Q559LGP-X4RyqPpvxxSpqZNWJ_CILSy5wVZrqGoo/)

---

## Kolom Google Sheet (FormOrderData)
| SKU | Nama Produk | Series | Gender | Harga | ImageURL |
|-----|-------------|--------|--------|-------|----------|
| SJ2AWG9 | LADIES WEDGES 9 AQUA HAZE | WEDGES | LADIES | 159,000 | https://drive.google.com/file/d/xxx/view |
| SJ1ACA1 | MEN CLASSIC 1 JET BLACK | CLASSIC | MEN | 99,000 | https://drive.google.com/file/d/yyy/view |
| BB2BZHK1 | BABY GIRLS HELLO KITTY 1 PINK | HELLOKITTY | BABY | 139,000 | https://drive.google.com/file/d/zzz/view |

> **Catatan ImageURL**: Gunakan link sharing Google Drive. Format: `https://drive.google.com/file/d/FILE_ID/view`

**CSV URL (Published):**
```
https://docs.google.com/spreadsheets/d/e/2PACX-1vSOWaNo-hDRWdP1u_czDNR55PvrAByQqGpMV1WgXPSdfs-PGVxc2-vyGEdXkuFEGNTjONdrWIpwmrOv/pub?gid=1738516088&single=true&output=csv
```

---

## Fitur yang Sudah Diimplementasi

### 1. Informasi Dokumen
- **Tanggal**: Auto-fill hari ini
- **No. Form Order**: Auto-generate `FO-Wholesale-Zuma-DDMMYYYY-XXX`
  - Nomor urut mulai dari 001 setiap hari
  - Reset ke 001 jika tanggal berubah
  - Disimpan di localStorage

### 2. Informasi Customer
- Nama Customer
- Alamat (text area)
- Provinsi (dropdown 38 provinsi Indonesia)
- Kabupaten/Kota (dropdown, otomatis sesuai provinsi)
- Kecamatan (dropdown, otomatis sesuai kabupaten)
- No. HP

### 3. Detail Order
- Search SKU / Nama Produk (autocomplete)
- Auto-fill: Nama Produk, Series, Gender
- **Diskon Otomatis**:
  - LADIES & MEN: **40%**
  - BABY: **30%**
- Tampilan: Price/Normal (coret abu), Diskon % (merah), Harga Diskon (hijau)
- **Foto Produk**: Preview foto dari Google Drive (hanya di form, tidak di PDF)
  - **Zoom**: Klik foto untuk memperbesar (popup modal)
  - Hover effect saat mouse di atas foto
  - Tekan ESC atau klik di luar untuk tutup
- Qty (Box)
- Subtotal otomatis

### 4. Tanda Tangan Digital (3 kolom)
- **Customer** - Wajib untuk generate PDF
- **SPV Wholesale** - Optional
- **Branch Manager BD & Wholesale** - Optional
- Jika belum TTD, tampil "(Belum ditandatangani)" di PDF

### 5. Arsip Order & Sistem Approval
- Tombol "Lihat Arsip Order"
- Simpan otomatis ke localStorage setiap generate PDF
- **Filter Arsip (klik tombol untuk filter):**
  - 📋 Semua - tampilkan semua order
  - 🟡 Menunggu TTD - order yang belum lengkap TTD
  - 🟢 Lengkap - order yang sudah lengkap TTD
- Info jumlah order ditampilkan
- **Sistem Approval Berurutan:**
  1. Customer TTD saat generate PDF (wajib)
  2. SPV bisa TTD dari arsip setelah Customer TTD
  3. Manager bisa TTD dari arsip setelah SPV TTD
- Klik order di arsip untuk membuka detail dan TTD
- Status tracking:
  - 🟡 Kuning = Menunggu TTD (SPV/Manager belum lengkap)
  - 🟢 Hijau = Lengkap (semua sudah TTD)
- Detail: Customer ✅ | SPV ✅/❌ | Manager ✅/❌
- Tombol aksi dinamis:
  - "TTD SPV" jika SPV belum TTD
  - "TTD Manager" jika SPV sudah TTD, Manager belum
  - "Lihat Detail" jika semua sudah TTD
- Generate ulang PDF dengan TTD terbaru

### 6. Logo & PDF Output
- **Setup Logo**: Gunakan `setup-logo.html` untuk upload logo (disimpan di localStorage)
- **PDF Layout**:
  - Logo kiri, judul CV. UNTUNG BESAR BERSAMA rata tengah
  - Info dokumen (Tanggal, No. FO)
  - Info customer (Nama, Alamat lengkap, HP)
  - Tabel order: No, SKU, Nama Produk, Gender, Price (coret abu), Diskon (merah), Harga Diskon (hijau), Qty, Subtotal
  - Total order
  - Catatan/Notes
  - 3 kolom tanda tangan

### 7. Hapus Order
- Tombol hapus (ikon trash) di setiap item arsip
- Konfirmasi sebelum menghapus
- Hapus juga bisa dari modal detail order

---

## Catatan Teknis

### CORS Proxy
Untuk akses Google Sheets dari file lokal, menggunakan CORS proxy:
```javascript
const GOOGLE_SHEET_URL = 'https://api.codetabs.com/v1/proxy?quest=' + encodeURIComponent(GOOGLE_SHEET_CSV);
```

### Data Wilayah
- 38 Provinsi Indonesia lengkap
- Kabupaten/Kota utama per provinsi
- Kecamatan per kabupaten
- Cascading dropdown (Provinsi → Kabupaten → Kecamatan)

### Penyimpanan Lokal (localStorage)
- `fo_last_date`: Tanggal terakhir untuk reset nomor urut
- `fo_sequence`: Nomor urut form order
- `fo_archive`: Array arsip order
- `fo_logo_base64`: Logo base64 untuk PDF UBB (disetup via setup-logo.html)
- `fo_logo_mbb_base64`: Logo base64 untuk PDF MBB (disetup via setup-logo-mbb.html)
- `fo_users`: Array user accounts (username, password, role, name)
- `fo_current_user`: User yang sedang login
- `fo_trash`: Array order yang dihapus (recycle bin)

---

## TODO / Pengembangan Selanjutnya
- [x] ~~Sistem approval berurutan (Customer → SPV → Manager)~~
- [x] ~~Filter arsip berdasarkan status TTD~~
- [x] ~~Logo pada PDF~~
- [x] ~~Fitur hapus order~~
- [x] ~~Price pada tabel PDF~~
- [x] ~~Foto produk dari Google Drive (preview di form)~~
- [x] ~~Zoom foto produk (klik untuk memperbesar)~~
- [x] ~~Dual company support (UBB/MBB)~~
- [x] ~~Trash/Recycle bin untuk order yang dihapus~~
- [x] ~~Sistem autentikasi dengan login/password~~
- [x] ~~Role-based access control (Customer/SPV/Manager/Admin)~~
- [x] ~~Admin panel untuk kelola user~~
- [ ] Test koneksi Google Sheets (pastikan data ter-load)
- [ ] Backup arsip ke Google Sheets (agar bisa diakses multi-device)
- [ ] Export arsip ke Excel

---

## ✅ FITUR FOTO PRODUK DARI SHEET TERPISAH - SELESAI

### ImageData Sheet (Terpisah dari FormOrderData)

**Masalah yang diselesaikan:**
- Data produk di-sync dari Master sheet via Apps Script
- Jika ImageURL ditaruh di FormOrderData, akan hilang saat sync

**Solusi yang diimplementasi:**
- Sheet terpisah **"ImageData"** dengan kolom: `SKU | ImageURL`
- Kode load dari 2 sumber: FormOrderData + ImageData
- **Prioritas**: ImageData > FormOrderData (jika SKU ada di ImageData, gunakan URL dari sana)

**Progress implementasi kode:**
1. ✅ Variable `IMAGE_DATA_CSV` dan `IMAGE_DATA_URL` (line ~598-599)
2. ✅ Fungsi `loadImageData()` untuk load dari sheet ImageData (line ~733)
3. ✅ Merge imageDataMap dengan product data - menggunakan `finalImageUrl = imageDataMap[sku] || imageUrl` (line ~842)

---

## 📋 LANGKAH SETUP UNTUK USER

### Cara Setup Sheet ImageData:
1. **Buka Google Sheets** yang sama dengan FormOrderData
2. **Buat sheet baru** dengan nama "ImageData"
3. **Isi header kolom** di baris 1: `SKU | ImageURL`
4. **Isi data** mulai baris 2:
   - Kolom A: SKU produk (harus sama persis dengan SKU di FormOrderData)
   - Kolom B: URL gambar dari Google Drive
5. **Publish sheet ImageData ke CSV**:
   - File → Share → Publish to web
   - Pilih sheet "ImageData"
   - Format: CSV
   - Klik "Publish" dan copy URL-nya
6. **Paste URL ke file HTML**:
   - Buka `Form_Order_GoogleSheets (3).html`
   - Cari line ~598: `const IMAGE_DATA_CSV = 'PASTE_IMAGE_DATA_CSV_URL_HERE';`
   - Ganti `PASTE_IMAGE_DATA_CSV_URL_HERE` dengan URL CSV yang di-copy

### Format Google Drive URL yang didukung:
- `https://drive.google.com/file/d/FILE_ID/view`
- `https://drive.google.com/open?id=FILE_ID`
- URL akan otomatis dikonversi ke thumbnail

---

## ✅ DUAL COMPANY SUPPORT (UBB/MBB) - SELESAI

### Fitur:
- Dua entitas perusahaan: UBB (CV. Untung Besar Bersama) dan MBB (CV. Makmur Besar Bersama)
- SPV memilih entitas saat TTD
- PDF menggunakan logo dan info perusahaan yang dipilih

### File terkait:
- `setup-logo.html` - Setup logo untuk UBB
- `setup-logo-mbb.html` - Setup logo untuk MBB

### Implementasi:
- Konstanta `COMPANY_INFO` berisi detail kedua perusahaan
- Radio button untuk pilih entitas di halaman detail order
- Pilihan entitas disimpan ke arsip order (`selectedCompany`)
- PDF di-generate sesuai entitas yang dipilih

---

## ✅ TRASH / RECYCLE BIN - SELESAI

### Fitur:
- Order yang dihapus masuk ke Trash, bukan dihapus permanen
- Bisa restore order dari Trash
- Bisa hapus permanen atau kosongkan semua Trash

### Implementasi:
- Tombol "Sampah" di modal Arsip
- Data disimpan di localStorage key `fo_trash`
- Fungsi: `openTrash()`, `closeTrash()`, `restoreOrder()`, `permanentDelete()`, `emptyTrash()`

---

## ✅ SISTEM AUTENTIKASI - SELESAI

### Fitur:
- Login dengan username dan password
- 4 role: Customer, SPV, Manager, Admin
- Pembatasan akses berdasarkan role

### Default Users:
| Username | Password | Role | Nama |
|----------|----------|------|------|
| admin | admin123 | admin | Administrator |
| spv | spv123 | spv | SPV Wholesale |
| manager | manager123 | manager | Branch Manager |

### Permission Matrix:
| Aksi | Customer | SPV | Manager | Admin |
|------|----------|-----|---------|-------|
| Buat Order | ✅ | ✅ | ✅ | ✅ |
| TTD Customer | ✅ | ✅ | ✅ | ✅ |
| TTD SPV | ❌ | ✅ | ❌ | ✅ |
| TTD Manager | ❌ | ❌ | ✅ | ✅ |
| Admin Panel | ❌ | ❌ | ❌ | ✅ |

### Admin Panel:
- Tambah user baru (Customer, SPV, Manager, Admin)
- Lihat daftar user
- Reset password user
- Hapus user (kecuali default users)

### Pembatasan Customer:
- Customer **tidak bisa hapus** order yang sudah di-TTD SPV atau Manager
- Customer **bisa hapus** order yang belum di-TTD (untuk revisi)
- Tombol hapus disembunyikan untuk customer pada order yang sudah di-TTD

---

## ✅ HEADER LOGO ZUMA - SELESAI

### Implementasi:
- Logo ZUMA di header form (kiri)
- File: `ZUMA_FINAL LOGO_UPDATED-04.png`
- Ukuran: 150px
- Warna: Putih (CSS filter: brightness(0) invert(1))
- Layout: Logo kiri | Judul tengah | User info & Logout kanan

### File Logo tersedia:
- `ZUMA_FINAL LOGO_UPDATED-04.png` - Logo + tulisan ZUMA (warna navy)
- `ZUMA_FINAL LOGO_UPDATED-08.png` - Logo saja (warna putih)
- `Penerapan Warna Logo_page-0001.jpg` - Logo horizontal (warna navy)

---

## ✅ DEPLOYMENT GITHUB PAGES - SELESAI

### URL Live:
**https://database-zuma.github.io/-zuma-wholesale/Form%20Order%20Wholesale.html**

### Repository:
https://github.com/database-zuma/-zuma-wholesale

### Default Users:
| Username | Password | Role |
|----------|----------|------|
| admin | admin123 | Admin |
| spv | spv123 | SPV Wholesale |
| manager | manager123 | Branch Manager |
| cs | cs123 | Customer Service |

### Catatan:
- Data (arsip order, logo) disimpan di localStorage browser masing-masing
- Setiap device/browser perlu setup logo sendiri
- Default users otomatis tersedia di semua device

---

*Last updated: 2026-01-05*
