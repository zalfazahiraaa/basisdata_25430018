# Dokumen Kebutuhan Data - Koperasi Mahasiswa (kopma_018)

**Nama:** Zalfa Zahira Putri  
**NPM:** 25430018  
**Kelas:** A  
**Tanggal:** 13 Oktober 2026  


## 1. Latar Belakang dan Aktivitas Organisasi
Koperasi Mahasiswa (Kopma) merupakan unit usaha mahasiswa yang mengelola penjualan produk, pengadaan barang dari pemasok, keanggotaan, serta pencatatan transaksi secara terkomputerisasi. Pengelolaan data yang terintegrasi sangat diperlukan untuk meminimalisir risiko ketidakcocokan stok barang, memantau transaksi pembelian serta penjualan harian, dan mempermudah perhitungan Sisa Hasil Usaha (SHU) anggota. Oleh karena itu, dibutuhkan basis data relasional `kopma_018` yang komprehensif, aman, dan efisien.


## 2. Aktor dan Proses Bisnis

### Tabel Aktor Sistem
| ID Aktor | Nama Aktor | Peran dan Tanggung Jawab |
| :--- | :--- | :--- |
| **AKT-01** | Kasir / Staf Toko | Mengelola transaksi penjualan harian, mencatat detail barang keluar, dan memproses transaksi poin anggota. |
| **AKT-02** | Anggota Kopma | Melakukan pembelian barang, mengumpulkan poin belanja, dan menerima bagian Sisa Hasil Usaha (SHU). |
| **AKT-03** | Pengurus / Staf Inventaris | Mengelola data anggota, data petugas, data pemasok, pengadaan pembelian barang, serta memantau laporan keuangan. |

### Tabel Proses Bisnis (PB-xx)
| Kode PB | Nama Proses Bisnis | Deskripsi Ringkas | Aktor Terlibat |
| :--- | :--- | :--- | :--- |
| **PB-01** | Registrasi Anggota | Menginput data diri mahasiswa untuk terdaftar sebagai anggota resmi Kopma. | AKT-02, AKT-03 |
| **PB-02** | Pengelolaan Barang & Pemasok | Menambah dan memperbarui data barang, kategori, serta pencatatan data pemasok. | AKT-03 |
| **PB-03** | Transaksi Penjualan | Mencatat nota penjualan oleh kasir, melayani pembelian anggota/umum, dan memperbarui stok barang. | AKT-01, AKT-02 |
| **PB-04** | Transaksi Pembelian & Stok | Mencatat pengadaan barang masuk dari pemasok ke inventaris koperasi. | AKT-01, AKT-03 |
| **PB-05** | Pengelolaan Poin | Mencatat perolehan dan penukaran poin transaksi anggota setiap bulan. | AKT-01, AKT-02 |


## 3. Dokumen Sumber yang Dianalisis
1. **Formulir Pendaftaran Anggota:** Berkas fisik/digital berisi nomor anggota, NIM, nama lengkap, prodi, nomor HP, status keanggotaan, dan tanggal daftar.
2. **Nota / Struk Penjualan:** Bukti transaksi memuat nomor nota, tanggal penjualan, ID petugas (kasir), ID anggota, daftar barang, kuantitas, harga satuan, dan total bayar.
3. **Faktur Pembelian & Kartu Stok:** Dokumen pengadaan barang dari pemasok serta catatan mutasi stok minimum dan harga beli barang.


## 4. Entitas Kandidat dan Elemen Data (Berdasarkan ERD Fisik)

1. **Petugas (`petugas`)**
   * Elemen Data: `id_petugas` (PK), `kode_petugas`, `nama_petugas`, `peran_petugas`.
2. **Anggota (`anggota`)**
   * Elemen Data: `id_anggota` (PK), `no_anggota`, `nim_anggota`, `nama_anggota`, `prodi_anggota`, `no_hp_anggota`, `status_anggota`, `tgl_daftar_anggota`.
3. **Pemasok (`pemasok`)**
   * Elemen Data: `id_pemasok` (PK), `nama_pemasok`, `telepon_pemasok`, `alamat_pemasok`.
4. **Barang (`barang`)**
   * Elemen Data: `id_barang` (PK), `kode_barang`, `nama_barang`, `kategori_barang`, `harga_jual_barang`, `stok_barang`, `stok_min_barang`.
5. **Penjualan (`penjualan`)**
   * Elemen Data: `id_penjualan` (PK), `no_nota_penjualan`, `tgl_penjualan`, `id_petugas` (FK), `id_anggota` (FK), `bayar_penjualan`.
6. **Detail Penjualan (`detail_penjualan`)**
   * Elemen Data: `id_penjualan` (PK, FK), `id_barang` (PK, FK), `qty_detail_penjualan`, `harga_satuan_detail_penjualan`.
7. **Pembelian (`pembelian`)**
   * Elemen Data: `id_pembelian` (PK), `no_faktur_pembelian`, `tgl_pembelian`, `id_pemasok` (FK), `id_petugas` (FK), `status_pembelian`.
8. **Detail Pembelian (`detail_pembelian`)**
   * Elemen Data: `id_pembelian` (PK, FK), `id_barang` (PK, FK), `qty_detail_pembelian`, `harga_beli_detail_pembelian`.
9. **Transaksi Poin (`transaksi_poin`)**
   * Elemen Data: `id_transaksi_poin` (PK), `id_penjualan` (FK), `jenis_transaksi_poin`, `jumlah_transaksi_poin`.


## 5. Aturan Bisnis (Tabel AB-xx)

| Kode AB | Pernyataan Aturan Bisnis | Implikasi Pada Basis Data |
| :--- | :--- | :--- |
| **AB-01** | Setiap anggota Kopma harus memiliki nomor anggota dan NIM yang unik. | Kolom `no_anggota` dan `nim_anggota` menggunakan batasan `UNIQUE` dan `NOT NULL`. |
| **AB-02** | Setiap transaksi penjualan wajib dilayani oleh petugas dan tercatat nomor notanya. | Kolom `id_petugas` dan `no_nota_penjualan` dikonfigurasi sebagai *Foreign Key* dan `NOT NULL`. |
| **AB-03** | Satu transaksi penjualan dan pembelian dapat mencakup banyak jenis barang. | Menggunakan tabel relasi `detail_penjualan` dan `detail_pembelian`. |
| **AB-04** | Stok barang dan stok minimum wajib dikontrol agar tidak bernilai negatif. | Kolom stok diberi batasan nilai minimal dan tipe data `INT`. |


## 6. Kebutuhan Informasi (Tabel KI-xx)

| Kode KI | Kebutuhan Informasi | Sumber Data (Tabel) |
| :--- | :--- | :--- |
| **KI-01** | Omzet & jumlah nota per hari/bulan. | `penjualan`, `detail_penjualan`[cite: 5] |
| **KI-02** | Lima barang terlaris per bulan. | `barang`, `detail_penjualan`, `penjualan`[cite: 5] |
| **KI-03** | Barang di bawah stok minimum. | `barang`[cite: 5] |
| **KI-04** | Sepuluh anggota belanja terbesar. | `anggota`, `penjualan`, `detail_penjualan`[cite: 5] |
| **KI-05** | Poin diperoleh & ditukar per bulan. | `anggota`, `penjualan`, `transaksi_poin`[cite: 5] |
| **KI-06** | Sepuluh anggota poin terbanyak. | `anggota`, `penjualan`, `transaksi_poin`[cite: 5] |


## 7. Matriks CRUD

| Entitas / Tabel | PB-01 (Registrasi) | PB-02 (Barang) | PB-03 (Penjualan) | PB-04 (Pembelian) | PB-05 (Poin) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **`petugas`** | - | - | **R** | **R** | - |
| **`anggota`** | **C, R, U** | - | **R** | - | **R** |
| **`pemasok`** | - | **C, R, U** | - | **R** | - |
| **`barang`** | - | **C, R, U, D** | **R, U** | **R, U** | - |
| **`penjualan`** | - | - | **C, R** | - | **R** |
| **`detail_penjualan`**| - | - | **C, R** | - | - |
| **`pembelian`** | - | - | - | **C, R** | - |
| **`detail_pembelian`**| - | - | - | **C, R** | - |
| **`transaksi_poin`**| - | - | **R** | - | **C, R** |


## 8. Kamus Data Awal

| Nama Tabel | Nama Kolom | Tipe Data | Batasan / Constraint | Penanggung Jawab |
| :--- | :--- | :--- | :--- | :--- |
| **`petugas`** | `id_petugas` | SMALLINT | PRIMARY KEY, AUTO_INCREMENT | Pengurus Kopma |
| | `kode_petugas` | CHAR(3) | NOT NULL, UNIQUE | Pengurus Kopma |
| | `nama_petugas` | VARCHAR(100)| NOT NULL | Pengurus Kopma |
| | `peran_petugas`| ENUM | NOT NULL | Pengurus Kopma |
| **`anggota`** | `id_anggota` | INT | PRIMARY KEY, AUTO_INCREMENT | Pengurus Kopma |
| | `no_anggota` | VARCHAR(20) | NOT NULL, UNIQUE | Pengurus Kopma |
| | `nim_anggota`| CHAR(20) | NOT NULL, UNIQUE | Pengurus Kopma |
| | `nama_anggota`| VARCHAR(100)| NOT NULL | Pengurus Kopma |
| | `prodi_anggota`| VARCHAR(50) | NOT NULL | Pengurus Kopma |
| | `no_hp_anggota`| VARCHAR(15) | NULL | Pengurus Kopma |
| | `status_anggota`| ENUM | NOT NULL | Pengurus Kopma |
| | `tgl_daftar_anggota`| DATE | DEFAULT CURRENT_DATE | Pengurus Kopma |
| **`pemasok`** | `id_pemasok` | INT | PRIMARY KEY, AUTO_INCREMENT | Staf Inventaris |
| | `nama_pemasok` | VARCHAR(100)| NOT NULL | Staf Inventaris |
| | `telepon_pemasok`| VARCHAR(15) | NULL | Staf Inventaris |
| | `alamat_pemasok`| TEXT | NULL | Staf Inventaris |
| **`barang`** | `id_barang` | INT | PRIMARY KEY, AUTO_INCREMENT | Staf Inventaris |
| | `kode_barang` | VARCHAR(20) | NOT NULL, UNIQUE | Staf Inventaris |
| | `nama_barang` | VARCHAR(100)| NOT NULL | Staf Inventaris |
| | `kategori_barang`| VARCHAR(50) | NOT NULL | Staf Inventaris |
| | `harga_jual_barang`| DECIMAL(10,2)| NOT NULL, DEFAULT 0 | Staf Inventaris |
| | `stok_barang` | INT | NOT NULL, DEFAULT 0 | Staf Inventaris |
| | `stok_min_barang`| INT | NOT NULL, DEFAULT 0 | Staf Inventaris |
| **`penjualan`**| `id_penjualan` | INT | PRIMARY KEY, AUTO_INCREMENT | Kasir |
| | `no_nota_penjualan`| VARCHAR(30) | NOT NULL, UNIQUE | Kasir |
| | `tgl_penjualan` | DATE | DEFAULT CURRENT_DATE | Kasir |
| | `id_petugas` | SMALLINT | FOREIGN KEY (`petugas`), NOT NULL | Kasir |
| | `id_anggota` | INT | FOREIGN KEY (`anggota`), NULL | Kasir |
| | `bayar_penjualan`| DECIMAL(10,2)| NOT NULL | Kasir |
| **`detail_penjualan`**| `id_penjualan` | INT | PK, FK (`penjualan`) | Kasir |
| | `id_barang` | INT | PK, FK (`barang`) | Kasir |
| | `qty_detail_penjualan`| INT | NOT NULL | Kasir |
| | `harga_satuan_detail_penjualan`| DECIMAL(10,2)| NOT NULL | Kasir |
| **`pembelian`**| `id_pembelian` | INT | PRIMARY KEY, AUTO_INCREMENT | Staf Inventaris |
| | `no_faktur_pembelian`| VARCHAR(30) | NOT NULL, UNIQUE | Staf Inventaris |
| | `tgl_pembelian`| DATE | DEFAULT CURRENT_DATE | Staf Inventaris |
| | `id_pemasok` | INT | FOREIGN KEY (`pemasok`), NOT NULL | Staf Inventaris |
| | `id_petugas` | SMALLINT | FOREIGN KEY (`petugas`), NOT NULL | Staf Inventaris |
| | `status_pembelian`| ENUM | NOT NULL | Staf Inventaris |
| **`detail_pembelian`**| `id_pembelian` | INT | PK, FK (`pembelian`) | Staf Inventaris |
| | `id_barang` | INT | PK, FK (`barang`) | Staf Inventaris |
| | `qty_detail_pembelian`| INT | NOT NULL | Staf Inventaris |
| | `harga_beli_detail_pembelian`| DECIMAL(10,2)| NOT NULL | Staf Inventaris |
| **`transaksi_poin`**| `id_transaksi_poin`| INT | PRIMARY KEY, AUTO_INCREMENT | Kasir |
| | `id_penjualan` | INT | FOREIGN KEY (`penjualan`), NOT NULL | Kasir |
| | `jenis_transaksi_poin`| ENUM | NOT NULL | Kasir |
| | `jumlah_transaksi_poin`| INT | NOT NULL | Kasir |


## 9. Kebutuhan Non-Fungsional Data
1. **Volume Data:** Mampu menangani hingga **2.000 data anggota**, **5.000 jenis barang**, dan kisaran **50.000 transaksi penjualan/pembelian** per tahun.
2. **Retensi Data:** Data transaksi dicatat dan disimpan secara aktif minimal **3 tahun** untuk keperluan audit keuangan serta evaluasi pembagian Sisa Hasil Usaha (SHU).
3. **Privasi dan Keamanan Basis Data:** Hak akses pengelolaan dan perubahan data terbatas pada akun resmi pengelola (`mhs_018` / `dev_018`), sementara akun publik atau tamu dibatasi hanya untuk pembacaan katalog barang.
4. **Ketersediaan (*Availability*):** Sistem basis data dirancang untuk beroperasi secara lokal via XAMPP/MariaDB secara stabil tanpa mengalami *down-time* saat jam operasional toko koperasi berlangsung.
5. **Kinerja Kueri (*Performance*):** Waktu respons untuk pencarian data barang dan rekapitulasi laporan transaksi harian dipastikan berjalan cepat di bawah 1 detik melalui penerapan *Primary Key* dan *Foreign Key* yang optimal.


## 10. Isu Kualitas Data yang Diantisipasi
1. **Duplikasi Identitas Anggota:** Diantisipasi dengan menerapkan batasan *Constraint* `UNIQUE` pada kolom `nim_anggota` dan `no_anggota`.
2. **Inkonsistensi Data Barang & Pemasok:** Diantisipasi melalui normalisasi tabel secara ketat serta relasi *Foreign Key* yang menghubungkan tabel `barang`, `pemasok`, dan `kategori`.
3. **Stok Barang Bernilai Negatif:** Diantisipasi dengan validasi tipe data *integer* murni serta batasan logika kueri pada aplikasi kasir agar stok tidak melebihi batas fisik yang ada.
4. **Data Transaksi Yatim (*Orphan Records*):** Diantisipasi dengan penerapan aturan relasi kaskade pada tabel detail penjualan maupun detail pembelian terhadap tabel induknya.
5. **Kesalahan Input Nominal Keuangan:** Diantisipasi menggunakan tipe data presisi tinggi yaitu `DECIMAL(10,2)` untuk seluruh kolom harga, subtotal, dan total pembayaran transaksi.


## 11. Bukti Git

* **Link Repositori:** https://github.com/zalfazahiraaa/basisdata_25430018
* **Tangkapan Layar Git Log:**

![Bukti Git Log Modul 2 Kopma](<img/tangkapan_layar_git_log_kopma.png>)