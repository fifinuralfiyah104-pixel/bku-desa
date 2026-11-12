# Catatan Kas Pribadi Desa

Aplikasi web sederhana untuk mencatat, memantau, dan melaporkan arus kas berdasarkan Pos Dana. Aplikasi berjalan di browser, menyimpan data secara lokal, dan menyediakan backup/restore dalam format JSON.

## Fitur

- Dashboard ringkasan kas dan saldo tiap Pos Dana.
- Grafik pengeluaran bulanan.
- Grafik penerimaan dan pengeluaran per Pos Dana.
- Grafik pie/donut pengeluaran per Pos Dana dengan warna berbeda.
- Daftar Pos Dana dengan saldo dan persentase realisasi.
- Riwayat transaksi kas.
- Jenis transaksi:
  - Kas masuk.
  - Kas keluar.
  - Transfer antar-Pos Dana.
  - Pelunasan talangan.
- Dukungan dana pihak lain pada transaksi kas keluar:
  - Talangan.
  - Kontribusi.
- Validasi nominal, tanggal, nomor bukti, Pos Dana, dan relasi transaksi.
- Nomor bukti otomatis untuk transaksi masuk, keluar, dan transfer.
- Lampiran gambar atau PDF pada transaksi.
- Kompresi otomatis gambar sebelum disimpan.
- Pembuatan bukti transaksi dengan QR Code.
- Cetak kuitansi dan laporan.
- Template laporan:
  - BKU standar.
  - Ringkasan bulanan.
  - Detail transaksi.
- Ekspor BKU ke CSV.
- Backup dan restore data dalam format JSON.
- Favicon aplikasi.
- Tampilan responsif untuk desktop dan perangkat mobile.

## Teknologi

- HTML5.
- CSS utility melalui Tailwind CSS CDN.
- JavaScript vanilla.
- Chart.js untuk grafik.
- QRCode.js untuk QR Code bukti transaksi.
- Font Awesome untuk ikon.
- Google Fonts Inter.
- `localStorage` browser untuk penyimpanan data lokal.

## Struktur Data

Aplikasi menggunakan tiga penyimpanan utama di `localStorage`:

| Key | Isi |
|---|---|
| `bku_profil` | Profil desa dan identitas pengelola/pemeriksa. |
| `kas_pos_dana` | Daftar Pos Dana. |
| `kas_transaksi` | Daftar transaksi kas. |

Data tidak dikirim ke server aplikasi. Data hanya tersimpan pada browser dan perangkat tempat aplikasi digunakan.

## Menjalankan Aplikasi

### Cara cepat

1. Pastikan file `index.html` tersedia.
2. Buka `index.html` di browser modern.
3. Isi profil melalui menu **Pengaturan**.
4. Buat Pos Dana.
5. Catat transaksi masuk atau keluar.

### Menggunakan server lokal

Server lokal lebih stabil untuk fitur browser tertentu, termasuk pembacaan file dan service worker.

Contoh menggunakan Python:

```bash
python -m http.server 8080
```

Kemudian buka:

```text
http://localhost:8080
```

Contoh menggunakan Node.js:

```bash
npx serve .
```

## Alur Penggunaan

### 1. Mengatur profil

Buka **Pengaturan**, lalu isi:

- Desa/wilayah.
- Kecamatan.
- Kabupaten.
- Tahun anggaran empat digit.
- Nama dan identitas pemeriksa.
- Nama dan identitas saksi internal.
- Nama dan identitas pengelola/bendahara.

Simpan profil sebelum mencatat transaksi.

### 2. Membuat Pos Dana

Buka tab **Pos Dana**, lalu pilih **Pos Dana Baru**. Isi nama Pos Dana, tanggal, dan penyetor/sumber dana.

Pos Dana digunakan untuk memisahkan saldo berdasarkan sumber atau tujuan penggunaan uang.

### 3. Mencatat transaksi

Pilih **Catat Transaksi**, lalu isi:

- Jenis transaksi.
- Tanggal.
- Nomor bukti.
- Pos Dana.
- Uraian.
- Nominal.
- Penerima atau sumber dana.
- Lampiran jika diperlukan.

Untuk transaksi keluar dengan dana pihak lain, buka rincian dana pihak lain dan isi nama serta nominalnya.

### 4. Transfer antar-Pos Dana

Pilih jenis **TRANSFER**, kemudian pilih Pos Dana asal dan Pos Dana tujuan yang berbeda. Transfer mengurangi saldo Pos Dana asal dan menambah saldo Pos Dana tujuan.

### 5. Talangan dan pelunasan

Transaksi keluar dengan jenis dana pihak lain **TALANGAN** akan muncul sebagai kewajiban. Gunakan transaksi **PELUNASAN** untuk mengurangi kewajiban tersebut.

### 6. Melihat laporan

Gunakan tab **Laporan Kas** untuk melihat BKU berdasarkan periode atau Pos Dana. Laporan dapat dicetak atau diekspor menjadi CSV.

## Backup dan Restore

### Backup

1. Klik tombol **Backup**.
2. Browser mengunduh file JSON.
3. Simpan file pada lokasi aman.

### Restore

1. Klik tombol **Pulihkan**.
2. Pilih file JSON hasil backup.
3. Periksa konfirmasi penggantian data.
4. Konfirmasi restore.

Restore menggantikan data aplikasi yang sedang tersimpan di browser. Selalu buat backup terbaru sebelum melakukan restore.

### Format backup

Contoh struktur tingkat atas:

```json
{
  "app": "BKU_BENDAHARA_DESA",
  "version": "3.0",
  "exportedAt": "2026-11-08T00:00:00.000Z",
  "profil": {},
  "posDana": [],
  "transaksi": []
}
```

Validasi restore mencakup:

- Struktur array Pos Dana dan transaksi.
- ID unik.
- Format tanggal `YYYY-MM-DD`.
- Nominal bilangan bulat positif.
- Referensi Pos Dana yang tersedia.
- Nomor bukti yang tidak duplikat.
- Relasi talangan dan pelunasan.
- Batas lampiran.

## Data Dummy

File `data-dummy-bku-2026.json` berisi contoh data untuk pengujian:

- 10 Pos Dana.
- 120 transaksi.
- Profil desa contoh.
- Transaksi masuk dan keluar dengan nomor bukti unik.

Untuk menggunakannya:

1. Buka aplikasi.
2. Klik **Pulihkan**.
3. Pilih `data-dummy-bku-2026.json`.
4. Konfirmasi restore.

Jangan gunakan data dummy sebagai data operasional nyata.

## Batasan Penyimpanan

Aplikasi menggunakan `localStorage`, yang memiliki kapasitas terbatas dan berbeda antar-browser. Lampiran gambar atau PDF berukuran besar dapat memenuhi kapasitas penyimpanan.

Praktik yang disarankan:

- Lakukan backup secara berkala.
- Hindari menyimpan lampiran yang tidak diperlukan.
- Gunakan gambar berukuran wajar.
- Simpan backup di luar browser.
- Jangan menghapus data browser sebelum membuat backup.

## Validasi dan Keamanan Data

- Input HTML ditampilkan melalui escaping pada bagian yang dirender dinamis.
- Nominal dibatasi sebagai `Number.isSafeInteger`.
- ID, tanggal, nomor bukti, dan referensi transaksi divalidasi.
- Data backup divalidasi sebelum menggantikan data aktif.
- Saat penyimpanan gagal, aplikasi berusaha mengembalikan nilai penyimpanan sebelumnya.

Aplikasi ini belum menggunakan autentikasi, enkripsi, database server, atau sinkronisasi antarperangkat. Jangan menyimpan data yang memerlukan perlindungan khusus pada perangkat bersama.

## Ketergantungan Eksternal

Aplikasi memuat beberapa library dari CDN:

- Tailwind CSS.
- Chart.js.
- QRCode.js.
- Font Awesome.
- Google Fonts.

Koneksi internet diperlukan agar library CDN tersebut dapat dimuat ketika aplikasi pertama kali dibuka atau saat cache belum tersedia.

## Pengembangan

File utama aplikasi saat ini adalah:

```text
index.html
favicon.png
favicon.ico
data-dummy-bku-2026.json
```

Sebagian besar tampilan, logika, validasi, dan state aplikasi berada di dalam `index.html`.

Sebelum mengubah kode:

1. Buat backup data dari aplikasi.
2. Simpan salinan `index.html`.
3. Uji alur tambah/edit/hapus Pos Dana.
4. Uji transaksi masuk, keluar, transfer, talangan, dan pelunasan.
5. Uji ekspor lalu impor kembali backup.
6. Periksa dashboard dan laporan setelah setiap perubahan.

## Pemeriksaan Sintaks JavaScript

JavaScript aplikasi tertanam di dalam `index.html`. Pemeriksaan sintaks dapat dilakukan dengan mengekstrak blok `<script>` terlebih dahulu, kemudian menjalankan:

```bash
node --check file-script.js
```

Pemeriksaan ini hanya memvalidasi sintaks JavaScript, bukan perilaku UI atau integritas data.

## Troubleshooting

### Data tidak muncul

- Pastikan membuka browser dan profil yang sama.
- Periksa apakah data tersimpan di browser tersebut.
- Jangan gunakan mode private/incognito untuk data penting.
- Pulihkan dari file backup.

### Restore gagal

- Pastikan file berformat JSON.
- Gunakan file hasil tombol **Backup** atau `data-dummy-bku-2026.json`.
- Pastikan JSON tidak diedit sehingga struktur, ID, nominal, atau tanggal menjadi tidak valid.
- Pastikan ukuran file tidak melebihi batas aplikasi.

### Grafik tidak muncul

- Pastikan koneksi internet tersedia untuk memuat Chart.js.
- Refresh halaman setelah library selesai dimuat.
- Periksa Console browser jika aplikasi dijalankan dalam mode pengembangan.

### QR Code atau ikon tidak muncul

- Pastikan koneksi internet tersedia untuk memuat QRCode.js atau Font Awesome.
- Jalankan aplikasi melalui server lokal jika membuka langsung dengan `file://` menimbulkan pembatasan browser.

## Lisensi

Aplikasi ini menggunakan [MIT License](LICENSE).

Lisensi MIT mengizinkan penggunaan, penyalinan, modifikasi, distribusi, sublicensing, dan penjualan salinan aplikasi dengan syarat pemberitahuan hak cipta dan teks lisensi tetap disertakan. Aplikasi disediakan tanpa jaminan.
