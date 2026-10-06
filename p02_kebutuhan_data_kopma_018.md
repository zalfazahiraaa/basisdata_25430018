# Dokumen Kebutuhan Data - Koperasi Mahasiswa (kopma_018)

**Nama:** Zalfa  
**NPM:** 25430018  
**Kelas:** A  
**Tanggal:** 7 Oktober 2026  


## 1. Latar Belakang dan Aktivitas Organisasi
Koperasi Mahasiswa (Kopma) merupakan unit usaha mahasiswa yang mengelola penjualan produk, keanggotaan, serta transaksi pembelian inventaris. Pengelolaan data manual menggunakan kertas berisiko tinggi memicu ketidakcocokan stok barang, pencatatan transaksi yang terlewat, dan kesulitan dalam menghitung Sisa Hasil Usaha (SHU) anggota. Oleh karena itu, dibutuhkan basis data relasional `kopma_018` yang terstruktur, aman, dan efisien.


## 2. Aktor dan Proses Bisnis

### Tabel Aktor Sistem
| ID Aktor | Nama Aktor | Peran dan Tanggung Jawab |
| :--- | :--- | :--- |
| **AKT-01** | Kasir / Staf Toko | Mengelola transaksi penjualan harian, menginput barang, dan mencatat pembayaran. |
| **AKT-02** | Anggota Kopma | Melakukan pembelian barang dan menerima bagian Sisa Hasil Usaha (SHU). |
| **AKT-03** | Pengurus Kopma | Mengelola inventaris barang, data anggota, serta memantau laporan keuangan bulanan. |

### Tabel Proses Bisnis (PB-xx)
| Kode PB | Nama Proses Bisnis | Deskripsi Ringkas | Aktor Terlibat |
| :--- | :--- | :--- | :--- |
| **PB-01** | Registrasi Anggota | Menginput data mahasiswa untuk terdaftar sebagai anggota resmi Kopma. | AKT-02, AKT-03 |
| **PB-02** | Pengelolaan Produk | Menambah, mengubah, dan memperbarui stok serta harga produk di toko Kopma. | AKT-03 |
| **PB-03** | Transaksi Penjualan | Mencatat pembelian barang oleh pelanggan/anggota dan memperbarui stok barang secara otomatis. | AKT-01, AKT-02 |
| **PB-04** | Pelaporan Keuangan | Menyusun rekapitulasi penjualan harian dan perhitungan SHU untuk pengurus. | AKT-03 |


## 3. Dokumen Sumber yang Dianalisis
1. **Formulir Pendaftaran Anggota:** Berkas fisik berisi NIM, nama, email, nomor telepon, dan status keanggotaan.
2. **Nota / Struk Penjualan:** Bukti transaksi memuat kode transaksi, tanggal, ID kasir, daftar produk, jumlah, dan total bayar.
3. **Buku Induk Inventaris Barang:** Catatan persediaan produk yang memuat ID barang, nama produk, kategori, harga beli, harga jual, dan stok.


## 4. Entitas Kandidat dan Elemen Data

1. **Anggota (`anggota`)**
   * Elemen Data: `id_anggota` (PK), `nim`, `nama_lengkap`, `email`, `nomor_telepon`, `tanggal_daftar`.
2. **Kategori (`kategori`)**
   * Elemen Data: `id_kategori` (PK), `nama_kategori`.
3. **Produk (`produk`)**
   * Elemen Data: `id_produk` (PK), `kode_produk`, `nama_produk`, `harga`, `stok`, `id_kategori` (FK).
4. **Penjualan (`penjualan`)**
   * Elemen Data: `id_penjualan` (PK), `id_anggota` (FK), `tanggal_transaksi`, `total_bayar`, `metode_pembayaran`.
5. **Detail Penjualan (`detail_penjualan`)**
   * Elemen Data: `id_detail` (PK), `id_penjualan` (FK), `id_produk` (FK), `jumlah`, `subtotal`.


## 5. Aturan Bisnis (Tabel AB-xx)

| Kode AB | Pernyataan Aturan Bisnis | Implikasi Pada Basis Data |
| :--- | :--- | :--- |
| **AB-01** | Setiap anggota Kopma harus memiliki NIM dan email yang unik. | Kolom `nim` dan `email` menggunakan batasan `UNIQUE` dan `NOT NULL`. |
| **AB-02** | Setiap produk harus memiliki kategori barang yang jelas. | Kolom `id_kategori` pada tabel `produk` dikonfigurasi sebagai *Foreign Key*. |
| **AB-03** | Satu transaksi penjualan dapat terdiri dari banyak jenis produk. | Menggunakan tabel relasi `detail_penjualan` untuk menjembatani tabel `penjualan` dan `produk`. |
| **AB-04** | Metode pembayaran hanya mendukung opsi 'tunai', 'qris', atau 'transfer'. | Kolom `metode_pembayaran` dikonfigurasi menggunakan `ENUM('tunai', 'qris', 'transfer')`. |
| **AB-05** | Stok produk tidak boleh negatif dan harga produk harus bernilai minimal 0. | Kolom `stok` dan `harga` diberi batasan `NOT NULL` serta default `0`. |


## 6. Kebutuhan Informasi (Tabel KI-xx)

| Kode KI | Kebutuhan Informasi | Sumber Data (Tabel) |
| :--- | :--- | :--- |
| **KI-01** | Daftar seluruh anggota aktif Kopma untuk pembagian SHU. | `anggota` |
| **KI-02** | Katalog persediaan stok dan harga produk Kopma. | `produk`, `kategori` |
| **KI-03** | Rekapitulasi transaksi penjualan harian beserta metode pembayarannya. | `penjualan` |
| **KI-04** | Laporan produk terlaris berdasarkan total jumlah penjualan. | `detail_penjualan`, `produk` |


## 7. Matriks CRUD

| Entitas / Tabel | PB-01 (Registrasi) | PB-02 (Produk) | PB-03 (Penjualan) | PB-04 (Pelaporan) |
| :--- | :---: | :---: | :---: | :---: |
| **`anggota`** | **C, R, U** | - | **R** | **R** |
| **`kategori`** | - | **C, R, U, D** | **R** | **R** |
| **`produk`** | - | **C, R, U, D** | **R, U** | **R** |
| **`penjualan`** | - | - | **C, R** | **R** |
| **`detail_penjualan`** | - | - | **C, R** | **R** |

*(Keterangan: **C** = Create, **R** = Read, **U** = Update, **D** = Delete)*


## 8. Kamus Data Awal

| Nama Tabel | Nama Kolom | Tipe Data | Batasan / Constraint | Penanggung Jawab Data |
| :--- | :--- | :--- | :--- | :--- |
| **`anggota`** | `id_anggota` | INT | PRIMARY KEY, AUTO_INCREMENT | Pengurus Kopma |
| | `nim` | VARCHAR(20) | NOT NULL, UNIQUE | Pengurus Kopma |
| | `nama_lengkap` | VARCHAR(100) | NOT NULL | Pengurus Kopma |
| | `email` | VARCHAR(100) | NOT NULL, UNIQUE | Pengurus Kopma |
| | `nomor_telepon` | VARCHAR(15) | NULL | Pengurus Kopma |
| | `tanggal_daftar` | DATE | DEFAULT CURRENT_DATE | Pengurus Kopma |
| **`kategori`** | `id_kategori` | INT | PRIMARY KEY, AUTO_INCREMENT | Staf Inventaris |
| | `nama_kategori` | VARCHAR(50) | NOT NULL, UNIQUE | Staf Inventaris |
| **`produk`** | `id_produk` | INT | PRIMARY KEY, AUTO_INCREMENT | Staf Inventaris |
| | `kode_produk` | VARCHAR(20) | NOT NULL, UNIQUE | Staf Inventaris |
| | `nama_produk` | VARCHAR(100) | NOT NULL | Staf Inventaris |
| | `harga` | DECIMAL(10,2)| NOT NULL, DEFAULT 0 | Staf Inventaris |
| | `stok` | INT | NOT NULL, DEFAULT 0 | Staf Inventaris |
| | `id_kategori` | INT | FOREIGN KEY (`kategori`) | Staf Inventaris |
| **`penjualan`**| `id_penjualan` | INT | PRIMARY KEY, AUTO_INCREMENT | Kasir |
| | `id_anggota` | INT | FOREIGN KEY (`anggota`), NULL | Kasir |
| | `tanggal_transaksi`| DATETIME | DEFAULT CURRENT_TIMESTAMP | Kasir |
| | `total_bayar` | DECIMAL(10,2)| NOT NULL | Kasir |
| | `metode_pembayaran`| ENUM | DEFAULT 'tunai' | Kasir |


## 9. Kebutuhan Non-Fungsional Data

1. **Volume Data:**
   * Mampu menangani hingga **2.000 data anggota**, **5.000 jenis produk**, dan kisaran **50.000 transaksi penjualan** per tahun.
2. **Retensi Data:**
   * Data transaksi disimpan secara aktif selama **3 tahun** untuk keperluan pembukuan keuangan dan evaluasi SHU.
3. **Privasi dan Keamanan Data:**
   * Hak akses edit basis data terbatas pada akun pengguna `mhs_018` / `dev_018`.
   * Akses publik/tamu (`tamu_018`) hanya diperbolehkan membaca (`SELECT`) data katalog produk.


## 10. Isu Kualitas Data yang Diantisipasi

1. **Duplikasi Identitas Anggota:**
   * *Antisipasi:* Batasan `UNIQUE` pada kolom `nim` dan `email`.
2. **Inkonsistensi Nama Kategori Produk:**
   * *Antisipasi:* Pemisahan tabel `kategori` dan penerapan relasi *Foreign Key* ke tabel `produk`.
3. **Stok Produk Bernilai Minus / Negatif:**
   * *Antisipasi:* Validasi logika kueri dan penggunaan tipe data integer tidak bernilai negatif.
4. **Transaksi Tanpa Detail Barang (*Orphan Records*):**
   * *Antisipasi:* Penggunaan klausa `ON DELETE CASCADE ON UPDATE CASCADE` pada tabel relasi `detail_penjualan`.


## 11. Bukti Git

* **Link Repositori:** https://github.com/zalfazahiraaa/basisdata_25430018
* **Tangkapan Layar Git Log:**

![Bukti Git Log Modul 2 Kopma](img/git_log_modul2.png)