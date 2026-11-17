# BobasoLaundry — Sistem Manajemen Laundry

Aplikasi web interaktif untuk mengelola pesanan dan pelanggan usaha laundry. Dibuat sebagai **Final Project Web Design: Interactive Web Application Development** menggunakan **HTML5, CSS3, dan Vanilla JavaScript** (tanpa framework).

> **Anggota kelompok:** isi nama dan NIM anggota di sini
> **Mata kuliah / dosen:** isi di sini

---

## Daftar Isi

1. [Deskripsi](#deskripsi)
2. [Fitur](#fitur)
3. [Screenshot](#screenshot)
4. [Teknologi](#teknologi)
5. [Struktur Folder](#struktur-folder)
6. [Cara Menjalankan](#cara-menjalankan)
7. [Cara Penggunaan](#cara-penggunaan)
8. [Penjelasan Implementasi JavaScript](#penjelasan-implementasi-javascript)
9. [Aturan Validasi](#aturan-validasi)
10. [Pemenuhan Ketentuan Project](#pemenuhan-ketentuan-project)
11. [Rencana Pengembangan](#rencana-pengembangan)

---

## Deskripsi

Banyak usaha laundry kecil masih mencatat pesanan secara manual di buku nota. Akibatnya status pengerjaan sulit dipantau, pembayaran yang belum lunas mudah terlewat, dan rekap pendapatan harus dihitung sendiri.

**BobasoLaundry** menyelesaikan masalah tersebut dengan satu aplikasi web yang:

- mencatat pesanan beserta berat, jenis layanan, status pengerjaan, dan status pembayaran,
- menghitung total harga otomatis (**berat × harga per kg**),
- menyimpan data pelanggan,
- menampilkan dashboard dan laporan secara otomatis dari data yang tersimpan.

Seluruh data disimpan di **Local Storage** browser, sehingga aplikasi berjalan tanpa server maupun database.

## Fitur

| Tampilan | Fitur |
|---|---|
| **Dashboard** | 4 kartu statistik (total pesanan, sedang diproses, selesai, pendapatan lunas), daftar 5 pesanan terbaru, tombol cepat "Buat Pesanan" |
| **Manajemen Pesanan** | Tambah, ubah, dan hapus pesanan; pencarian berdasarkan ID, nama pelanggan, atau layanan; filter status pengerjaan; total harga otomatis |
| **Data Pelanggan** | Tambah, ubah, dan hapus pelanggan; pencarian nama, telepon, atau alamat; jumlah riwayat pesanan per pelanggan |
| **Laporan** | Nilai seluruh pesanan, total sudah dibayar dan belum dibayar, jumlah transaksi, grafik batang status pengerjaan, rincian transaksi, tombol cetak laporan |

Fitur pendukung:

- Validasi form berlapis (HTML5 dan JavaScript)
- Notifikasi toast dan dialog konfirmasi sebelum menghapus data
- Pelanggan yang masih memiliki pesanan tidak dapat dihapus
- Tampilan responsif untuk desktop, tablet, dan mobile (sidebar menjadi drawer)
- Gaya cetak khusus (`@media print`) untuk halaman laporan
- Penutupan modal dengan tombol `Esc` atau klik di luar modal

## Screenshot

> Tambahkan screenshot hasil aplikasi ke folder `screenshots/`, lalu pastikan nama file sesuai dengan tautan di bawah.

| Dashboard | Manajemen Pesanan |
|---|---|
| ![Dashboard](screenshots/dashboard.png) | ![Pesanan](screenshots/orders.png) |

| Data Pelanggan | Laporan |
|---|---|
| ![Pelanggan](screenshots/customers.png) | ![Laporan](screenshots/reports.png) |

| Form Pesanan | Tampilan Mobile |
|---|---|
| ![Form](screenshots/order-form.png) | ![Mobile](screenshots/mobile.png) |

## Teknologi

- **HTML5** — semantic HTML (`aside`, `nav`, `header`, `main`, `section`, `article`, `footer`)
- **CSS3** — Flexbox, CSS Grid, variabel CSS (`:root`), media query responsif
- **JavaScript (Vanilla)** — DOM manipulation, event handling, array dan objek, template literal, `Intl.NumberFormat`
- **Web Storage API** — `localStorage` dengan format JSON

Tidak menggunakan React, Vue, Angular, maupun library JavaScript lainnya.

## Struktur Folder

```
bobasolaundry/
├── index.html      # Kerangka halaman, 4 tampilan, form, dan modal
├── style.css       # Seluruh gaya dan media query
├── app.js          # Data, CRUD, render, validasi, Local Storage
├── screenshots/    # Gambar untuk README
└── README.md
```

## Cara Menjalankan

Tidak ada instalasi atau build step.

**Opsi 1 — Langsung dari file**

1. Clone atau unduh repository ini.
2. Buka `index.html` dengan browser modern (Chrome, Edge, Firefox, Safari).

```bash
git clone https://github.com/<username>/<nama-repository>.git
cd <nama-repository>
```

**Opsi 2 — Dengan Live Server (disarankan saat pengembangan)**

1. Buka folder project di Visual Studio Code.
2. Pasang ekstensi **Live Server**.
3. Klik kanan `index.html`, lalu pilih **Open with Live Server**.

**Opsi 3 — Deploy ke GitHub Pages**

1. Buka **Settings → Pages** pada repository.
2. Pada **Source**, pilih branch `main` dan folder `/ (root)`.
3. Simpan, lalu buka alamat yang ditampilkan GitHub.

> **Catatan:** data tersimpan di Local Storage browser dan perangkat yang dipakai. Membuka aplikasi di browser atau perangkat lain akan menampilkan data awal (contoh) kembali.

## Cara Penggunaan

1. **Dashboard** — lihat ringkasan. Tombol **Buat Pesanan** membuka form pesanan baru.
2. **Data Pelanggan** — klik **Tambah Pelanggan**, isi nama, nomor telepon, dan alamat.
3. **Manajemen Pesanan** — klik **Tambah Pesanan**, pilih pelanggan dan layanan, isi berat serta harga per kg. Total harga dihitung otomatis.
4. **Cari dan filter** — ketik pada kolom pencarian dan/atau pilih status untuk menyaring tabel.
5. **Ubah atau hapus** — gunakan tombol ✎ (ubah) dan ⌫ (hapus) pada baris data. Penghapusan meminta konfirmasi.
6. **Laporan** — lihat rekap pembayaran dan status. Klik **Cetak Laporan** untuk mencetak.

### Data awal

Saat pertama kali dibuka, aplikasi berisi 4 pelanggan dan 4 pesanan contoh. Data ini akan diganti dengan data Anda begitu ada perubahan pertama.

Untuk mengembalikan data awal, hapus data situs di browser atau jalankan di Console:

```js
localStorage.removeItem("laundryku_data_v1"); location.reload();
```

## Penjelasan Implementasi JavaScript

Semua logika berada di `app.js`.

### Alur data

```
Aksi pengguna → Event handler → Ubah array db → save() → renderAll()
```

### Model data

```js
db = {
  customers: [{ id, name, phone, address }],
  orders:    [{ id, customerId, service, weight, price,
                status, payment, date, note }]
};
```

Total harga tidak disimpan, melainkan dihitung dari data: `orderTotal(o) = Math.round(weight * price)`.

### Fungsi penting

| Fungsi | Peran |
|---|---|
| `loadData()` / `save()` | Membaca dan menulis data ke Local Storage (JSON). Jika kosong atau rusak, memakai data awal |
| `renderAll()` | Memanggil seluruh fungsi render setelah data berubah |
| `renderDashboard()` | Menghitung statistik dan menampilkan 5 pesanan terbaru |
| `renderOrders()` | Menerapkan pencarian dan filter, lalu menggambar tabel pesanan |
| `renderCustomers()` | Menerapkan pencarian dan menggambar tabel pelanggan |
| `renderReports()` | Menghitung total, lunas, belum lunas, dan grafik status |
| `submitOrder()` / `submitCustomer()` | Validasi form, lalu membuat atau memperbarui data |
| `deleteOrder()` / `deleteCustomer()` | Konfirmasi, lalu menghapus data |
| `showView()` | Berpindah antar tampilan tanpa memuat ulang halaman |
| `toast()` | Menampilkan notifikasi singkat |
| `esc()` | Meng-escape karakter HTML sebelum dimasukkan ke `innerHTML` |

### CRUD pada pesanan

| Operasi | Implementasi |
|---|---|
| Create | `db.orders.push(item)` |
| Read | `db.orders.filter(...)` lalu `rows.map(...)` menjadi baris tabel |
| Update | `db.orders.map(o => o.id === id ? item : o)` |
| Delete | `db.orders.filter(o => o.id !== id)` |

### Pencarian dan filter

Kolom pencarian (`input`) dan filter status (`change`) memanggil `renderOrders()` setiap kali nilainya berubah. Pencarian mencocokkan ID, nama pelanggan, atau layanan, lalu hasilnya disaring lagi berdasarkan status.

## Aturan Validasi

**Pesanan**

- Pelanggan harus dipilih (tambahkan pelanggan terlebih dahulu jika belum ada)
- Berat dan harga per kg harus lebih dari 0
- Tanggal masuk wajib diisi

**Pelanggan**

| Field | Aturan |
|---|---|
| Nama | Wajib, minimal 2 karakter |
| Nomor telepon | Wajib, cocok dengan `/^(\+62\|0)[0-9]{8,13}$/` (contoh: `081234567890` atau `+6281234567890`) |
| Alamat | Wajib, minimal 3 karakter |

Validasi berjalan dua lapis: atribut HTML5 (`required`, `min`, `minlength`, `type`) diperiksa browser lebih dulu, kemudian JavaScript memeriksa aturan tambahan dan menampilkan pesan lewat toast.

## Pemenuhan Ketentuan Project

| Ketentuan | Implementasi |
|---|---|
| Minimal 4 tampilan | Dashboard, Manajemen Pesanan, Data Pelanggan, Laporan |
| CRUD | Pesanan dan pelanggan |
| Search dan filter | Pencarian pesanan dan pelanggan, filter status pesanan |
| Form validation | HTML5 dan JavaScript |
| Local Storage | `localStorage` dengan format JSON |
| Responsive design | Breakpoint 1000px (tablet) dan 700px (mobile) |
| DOM manipulation dan event handling | Render dinamis dan `addEventListener` |
| Dashboard / summary | Kartu statistik dan halaman Laporan |
| Notifikasi / konfirmasi | Toast, `confirm()`, dan `alert()` |
| UI/UX konsisten | Satu sistem warna dan komponen di seluruh halaman |
| HTML5, CSS3, Vanilla JS | Tanpa framework frontend |

## Rencana Pengembangan

- [ ] Grafik pendapatan dengan Chart.js
- [ ] Export data pesanan ke CSV
- [ ] Deployment ke GitHub Pages
- [ ] Pesan error langsung di bawah field form
- [ ] Pengecekan nomor telepon duplikat
- [ ] Ubah status pesanan langsung dari tabel

## Pembagian Tugas

| Anggota | Kontribusi |
|---|---|
| isi nama | isi bagian yang dikerjakan |
| isi nama | isi bagian yang dikerjakan |
| isi nama | isi bagian yang dikerjakan |

---

© 2026 BobasoLaundry · Final Project Web Design
