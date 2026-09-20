# Parkir Mall System

Sistem manajemen parkir berbasis PHP (mysqli) + MySQL, dijalankan di XAMPP (`htdocs/parkir_mall/`).
WIREFRAME / MOCKUP UI, UX DESIGN (USER EXPERIENCE), FLOWCHART / USERFLOW dan ERD & BASIS DATA
https://sidiqferynur.github.io/muckupui/

## Struktur File

### Autentikasi & Akun
| File | Fungsi |
|---|---|
| `login.php` | Halaman login (petugas/owner/user) |
| `proses_login.php` | Proses validasi login |
| `logout.php` | Logout & hancurkan session |
| `register.php` | Form registrasi user baru |
| `proses_register.php` | Proses simpan registrasi |
| `buat_password.php` | Buat/reset password |

### Dashboard per Role
| File | Fungsi |
|---|---|
| `petugas.php` | Dashboard petugas — input kendaraan masuk, transaksi harian |
| `dashboard_user.php` | Dashboard user/customer — cek slot, reservasi, riwayat |
| `owner.php` | Dashboard owner — laporan & monitoring |
| `admin.php` | Dashboard admin — kelola master data |

### Transaksi Parkir
| File | Fungsi |
|---|---|
| `transaksi.php` | Proses transaksi parkir (masuk/keluar kendaraan) |
| `cetak_karcis.php` | Cetak karcis masuk kendaraan (nomor #T<id_parkir>) |
| `cetak_struk.php` | Cetak struk pembayaran saat kendaraan keluar |
| `riwayat_struk.php` | Riwayat gabungan transaksi reguler + reservasi (UNION) |
| `rekap_transaksi.php` | Rekap transaksi untuk laporan |

### Reservasi
| File | Fungsi |
|---|---|
| `reservasi.php` | Form reservasi online (sisi user) |
| `reservasi_petugas.php` | Konfirmasi reservasi oleh petugas (input kode manual) |

### CRUD Master Data
| File | Fungsi |
|---|---|
| `crud_kendaraan.php` | Kelola data kendaraan terdaftar |
| `crud_area_parkir.php` | Kelola area/slot parkir |
| `crud_tarif.php` | Kelola tarif parkir |
| `edit_tarif.php` | Edit tarif tertentu |
| `crud_user.php` | Kelola akun user |

### Laporan & Log
| File | Fungsi |
|---|---|
| `laporan_keuangan.php` | Laporan keuangan |
| `log_aktivitas.php` | Pencatatan log aktivitas |
| `view_log.php` | Tampilan log aktivitas |

### Lain-lain
| File | Fungsi |
|---|---|
| `index.php` | Landing page publik (status slot, reservasi online, tarif, FAQ, ulasan) |
| `koneksi.php` | Koneksi database (mysqli) |
| `footer.php` | Footer/partial include |
| `proses_rating.php` | Proses simpan ulasan/rating |
| `qris.png` | Gambar QRIS untuk pembayaran |

## Alur Karcis & Struk

1. Kendaraan masuk dicatat di `petugas.php` → insert ke `tb_kendaraan` (jika belum ada) & `tb_parkir`.
2. `cetak_karcis.php?id=<id_parkir>` menampilkan karcis masuk dengan nomor **#T<id_parkir>**, plat nomor, jenis kendaraan, waktu masuk. Tombol "Keluar" kembali ke `petugas.php`.
3. Saat kendaraan keluar & bayar, `cetak_struk.php` mencetak struk pembayaran — merujuk `id_parkir` yang sama agar nomor karcis tetap konsisten dari masuk sampai keluar.
4. Riwayat gabungan (transaksi reguler + reservasi) bisa dilihat di `riwayat_struk.php`, diurutkan ASC berdasarkan `waktu_masuk`.

## Database

Nama database: `parkir`

Tabel utama:
- `tb_kendaraan` — data master kendaraan (plat, jenis, warna, pemilik)
- `tb_parkir` — transaksi parkir (id_parkir, id_kendaraan, waktu_masuk, waktu_keluar, biaya_total, status)
- `tb_area_parkir`, `tb_tarif`, `tb_reservasi`, `tb_transaksi`, `tb_user`, `tb_log_aktivitas`, `tb_faq`, `tb_ulasan`

> Catatan: `dashboard_user.php` memakai skema tabel berbeda (`reservasi`, `area_parkir`) dari skema `tb_*` yang dipakai halaman petugas.

## Catatan Deployment

Proyek ada di dua tempat terpisah dan **datanya tidak sinkron otomatis**:
- **Localhost** (`localhost/parkir_mall/`) — development, database MySQL lokal via XAMPP.
- **InfinityFree** (`mallsmk.infinityfree.me`) — hosting online, database terpisah.

Perubahan data/skema di satu sisi harus di-export/import manual ke sisi lain lewat phpMyAdmin.
