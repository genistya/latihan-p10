# DEBUG LOG - KATALOG KARYA (LATIHAN P10)

## BUG-01: Logo Navbar Rusak (G1)
- **Gejala:** Logo di navbar tidak tampil; muncul ikon gambar rusak.
- **Langkah Reproduksi:** Buka `index.html` di browser, periksa navbar bagian atas.
- **Hipotesis 1:** Berkas `logo.svg` hilang atau tidak ada dalam direktori. (GUGUR - berkas tersedia di folder `assets/`)
- **Hipotesis 2:** Path lokasi berkas pada atribut `src` di tag `<img>` salah. (TERBUKTI)
- **Bukti DevTools:** Tab Network menunjukkan status `404 Not Found` saat meminta `GET img/logo.svg`.
- **Perbaikan:** `index.html` baris 12 — mengubah `src="img/logo.svg"` menjadi `src="assets/logo.svg"`.
- **Uji Ulang:** Halaman dimuat ulang, logo tampil sempurna dan tab Network menunjukkan status `200 OK`.

## BUG-02: Kartu Karya Tersusun Satu Kolom (G2)
- **Gejala:** Kartu karya tersusun satu kolom memanjang ke bawah pada layar lebar (> 992 px).
- **Langkah Reproduksi:** Buka `index.html` pada browser tampilan desktop.
- **Hipotesis 1:** Class grid pembagi kolom milik Bootstrap (`col-lg-4`) salah tulis. (GUGUR - penulisan class pada fungsi `kartu()` sudah benar)
- **Hipotesis 2:** Elemen kontainer galeri tidak memiliki class `row` milik Bootstrap. (TERBUKTI)
- **Bukti DevTools:** Tab Elements menunjukkan `<div id="galeri" class="g-3">` tanpa class `row`.
- **Perbaikan:** `index.html` baris 30 — menambahkan class `row` menjadi `<div id="galeri" class="row g-3">`.
- **Uji Ulang:** Kartu karya terbagi rapi menjadi 3 kolom pada tampilan desktop.

## BUG-03: Filter Kategori Tidak Berfungsi & TypeError (G3)
- **Gejala:** Memilih opsi kategori tidak mengubah daftar karya dan Console menampilkan galat.
- **Langkah Reproduksi:** Pilih opsi "Video" atau "Animasi" pada dropdown filter kategori.
- **Hipotesis 1:** Fungsi `saring()` mengalami kesalahan logika pemfilteran array. (GUGUR)
- **Hipotesis 2:** Selektor ID elemen DOM pada `document.querySelector` tidak cocok dengan ID di HTML. (TERBUKTI)
- **Bukti DevTools:** Tab Console menunjukkan `Uncaught TypeError: Cannot read properties of null (reading 'addEventListener')` di `main.js:49`.
- **Perbaikan:** `main.js` baris 49 — mengganti `document.querySelector('#filter-kategori')` menjadi `filterKategori`.
- **Uji Ulang:** Memilih kategori berhasil menyaring daftar karya tanpa ada galat di Console.

## BUG-04: Teks Jumlah Karya Tidak Dinamis (G4)
- **Gejala:** Teks "Menampilkan ... karya" tidak berubah saat hasil saringan berkurang.
- **Langkah Reproduksi:** Ketik kata kunci di kotak cari atau ubah filter kategori.
- **Hipotesis 1:** Elemen `<p id="jumlah">` tidak diperbarui oleh fungsi `tampil()`. (GUGUR)
- **Hipotesis 2:** Fungsi `tampil()` menggunakan properti panjang array asli (`KARYA.length`), bukan panjang array hasil saringan (`hasil.length`). (TERBUKTI)
- **Bukti DevTools:** Inspeksi fungsi `tampil()` di `main.js` menunjukkan penggunaan `KARYA.length` secara statis.
- **Perbaikan:** `main.js` baris 35 — mengganti `KARYA.length` pertama menjadi `hasil.length`.
- **Uji Ulang:** Teks jumlah berubah secara dinamis sesuai jumlah karya yang tersaring.

## BUG-05: Tombol Detail Mati Setelah Pencarian (G5)
- **Gejala:** Tombol Detail berfungsi saat pertama dimuat, tetapi mati setelah melakukan pencarian/filter.
- **Langkah Reproduksi:** Ketik "Animasi" pada kotak cari, lalu klik tombol "Detail".
- **Hipotesis 1:** Elemen modal/detail `<div id="detail">` tersembunyi secara permanen. (GUGUR)
- **Hipotesis 2:** Event listener pada tombol detail hilang karena elemen kartu diregenerasi ulang tanpa dipasangi listener baru. (TERBUKTI)
- **Bukti DevTools:** Tab Elements -> Event Listeners pada tombol detail hilang setelah dipanggilnya fungsi `tampil()`.
- **Perbaikan:** `main.js` baris 34 — menambahkan pemanggilan `pasangDetail()` di dalam fungsi `tampil()`.
- **Uji Ulang:** Tombol detail tetap dapat diklik dan menampilkan informasi karya meskipun setelah pencarian.

## BUG-06: Pencarian Sensitif Huruf Besar/Kecil (G6)
- **Gejala:** Pencarian tidak menemukan karya bila diketik dengan huruf kecil semua.
- **Langkah Reproduksi:** Ketik kata kunci "animasi" (huruf kecil) di kotak cari.
- **Hipotesis 1:** Event `input` pada elemen pencarian tidak memicu pemanggilan fungsi. (GUGUR)
- **Hipotesis 2:** Fungsi `saring()` tidak mengonversi teks input dan judul karya ke format *lowercase*. (TERBUKTI)
- **Bukti DevTools:** Pengujian variabel di breakpoint tab Sources menunjukkan perbandingan String bersifat *case-sensitive*.
- **Perbaikan:** `main.js` baris 10-14 — menambahkan `.toLowerCase()` pada variabel `kata` dan properti `k.judul`.
- **Uji Ulang:** Pencarian dengan huruf kecil "animasi" berhasil menampilkan data karya terkait.