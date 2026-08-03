````markdown
# CASHFLOW SYSTEM LABORATORY

## Dashboard Terintegrasi untuk Pengelolaan Denda, Uang Kas, dan Arus Kas Laboratorium

Cashflow System Laboratory merupakan aplikasi web berbasis Google Apps Script dan Google Sheets yang dikembangkan untuk membantu pengelolaan administrasi keuangan internal laboratorium.

Aplikasi ini mengintegrasikan pencatatan denda, pembayaran uang kas, pengeluaran operasional, dan ringkasan arus kas ke dalam satu dashboard. Sistem menyediakan tampilan publik untuk transparansi informasi serta panel khusus Bendahara untuk mengelola transaksi.

---

## DAFTAR ISI

1. Pendahuluan dan Grounding Masalah
2. Live Portal dan Kredensial Pengujian
3. Arsitektur Sistem dan Alur Data
4. Formulasi Pemodelan Nilai dan Logika Bisnis
5. Analisis Permasalahan Teknikal dan Solusi Terapan
6. Batasan Arsitektur dan Peluang Revisi Lanjutan
7. Lisensi dan Isolasi Kode

---

# 1. PENDAHULUAN DAN GROUNDING MASALAH

Pencatatan pelanggaran di lingkungan laboratorium sebelumnya dilakukan menggunakan media pencatatan umum, termasuk melalui percakapan WhatsApp. Pendekatan tersebut cukup praktis untuk komunikasi sementara, tetapi kurang sesuai sebagai media penyimpanan data operasional.

Informasi yang dikirim melalui percakapan dapat tertumpuk oleh pesan lain, sulit dicari kembali, atau terlupakan sebelum dipindahkan ke catatan utama. Risiko tersebut menjadi lebih besar karena asisten laboratorium memiliki aktivitas operasional yang cukup padat.

Selain pencatatan denda, laboratorium juga mulai menjalankan sistem uang kas. Pengelolaan denda dan kas melalui media yang berbeda berpotensi menghasilkan data yang tersebar dan menyulitkan proses rekapitulasi.

Permasalahan lainnya adalah keterbatasan informasi mengenai kondisi kas laboratorium. Meskipun terdapat kepercayaan terhadap Bendahara, anggota tetap memerlukan gambaran mengenai dana yang tersedia agar dapat merencanakan kegiatan secara lebih realistis.

Cashflow System Laboratory dikembangkan untuk menjawab kebutuhan tersebut melalui satu dashboard yang mencakup:

- Pencatatan denda asisten.
- Pemantauan pembayaran uang kas.
- Pencatatan pengeluaran laboratorium.
- Perhitungan total pemasukan.
- Perhitungan total pengeluaran.
- Perhitungan net cashflow.
- Visualisasi data keuangan secara terintegrasi.

Sistem ini tidak dimaksudkan sebagai aplikasi akuntansi formal. Aplikasi berfungsi sebagai sistem administrasi dan transparansi keuangan internal dalam ruang lingkup operasional laboratorium.

---

# 2. LIVE PORTAL DAN KREDENSIAL PENGUJIAN

## Live Demo

Aplikasi dapat diakses melalui tautan berikut:

(https://script.google.com/macros/s/AKfycbw7HnlfVyeeGyfuVxCObtueOLP8bMlA8cwNgcSLyD-b_vkaXbXG1jIRLDP2b7AAui8b/exec)

Versi yang ditampilkan merupakan aplikasi demo yang telah dipisahkan dari aplikasi utama. Data dan kredensial di dalamnya disediakan untuk kebutuhan pengujian serta demonstrasi portofolio.

## Akun Demo Bendahara

```
Username: bendahara
Password: asistensultan
```

## Skema Akses

Sistem memiliki dua tingkat akses utama.

### Pengunjung Publik

Pengunjung dapat:

- Melihat riwayat denda.
- Melihat status pembayaran kas.
- Melihat catatan pengeluaran.
- Melihat total pemasukan.
- Melihat total pengeluaran.
- Melihat net cashflow.
- Melihat grafik dan ringkasan transaksi.

### Bendahara

Bendahara memperoleh akses tambahan untuk:

- Menambahkan catatan denda.
- Memvalidasi pembayaran denda.
- Mengubah status pembayaran kas.
- Menambahkan catatan pengeluaran.
- Menghapus catatan pengeluaran.
- Memantau keseluruhan transaksi melalui dashboard administratif.

---

# 3. ARSITEKTUR SISTEM DAN ALUR DATA

Cashflow System Laboratory menggunakan arsitektur web sederhana berbasis layanan Google.

Google Apps Script berfungsi sebagai backend sekaligus media deployment aplikasi. Google Sheets digunakan sebagai database operasional, sedangkan antarmuka pengguna dibangun menggunakan HTML, JavaScript, Tailwind CSS, Chart.js, dan Lucide Icons.

## Diagram Arsitektur

```
+----------------------------+
|          PENGGUNA          |
|                            |
|  Pengunjung   Bendahara    |
+-------------+--------------+
              |
              v
+----------------------------+
|       ANTARMUKA WEB        |
|                            |
|  Dashboard                 |
|  Tabel Transaksi           |
|  Grafik                    |
|  Form Administrasi         |
+-------------+--------------+
              |
              v
+----------------------------+
|     GOOGLE APPS SCRIPT     |
|                            |
|  Pengambilan Data          |
|  Pengolahan Data           |
|  Validasi Transaksi        |
|  Operasi Tambah dan Ubah   |
+-------------+--------------+
              |
              v
+----------------------------+
|        GOOGLE SHEETS       |
|                            |
|  Data Asisten              |
|  Data Denda                |
|  Data Kas                  |
|  Data Pengeluaran          |
+----------------------------+
```

## Alur Data

Alur penggunaan sistem secara umum adalah sebagai berikut:

1. Pengguna membuka aplikasi melalui browser.
2. Antarmuka mengirim permintaan data ke Google Apps Script.
3. Backend membaca data dari Google Sheets.
4. Data denda, kas, dan pengeluaran diolah menjadi ringkasan.
5. Hasil pengolahan dikirim ke antarmuka.
6. Dashboard menampilkan tabel, indikator, dan grafik.
7. Bendahara dapat melakukan perubahan melalui panel administratif.
8. Perubahan disimpan kembali ke Google Sheets.
9. Dashboard diperbarui berdasarkan data terbaru.

## Struktur Database

Sistem menggunakan empat kelompok data utama:

| Data | Fungsi |
|------|--------|
| Data Asisten | Menyimpan daftar asisten dan status keaktifannya |
| Data Denda | Menyimpan pelanggaran, nominal, status, dan waktu pembayaran |
| Data Kas | Menyimpan tagihan kas bulanan dan status pelunasannya |
| Data Pengeluaran | Menyimpan penggunaan dana untuk kebutuhan laboratorium |

Pemilihan Google Sheets sebagai database mempertimbangkan kemudahan akses, fleksibilitas pengelolaan, dan kesesuaian dengan skala penggunaan internal laboratorium.

---

# 4. FORMULASI PEMODELAN NILAI DAN LOGIKA BISNIS

Logika bisnis sistem dibangun berdasarkan pemisahan antara dana yang telah diterima, dana yang masih menjadi tunggakan, dan dana yang telah digunakan.

## Pemasukan Denda

Denda hanya dihitung sebagai pemasukan ketika status pembayaran telah divalidasi menjadi "LUNAS".

Denda dengan status "BELUM" tetap ditampilkan sebagai kewajiban pembayaran, tetapi tidak dimasukkan ke dalam kas aktual.

## Pemasukan Uang Kas

Pembayaran kas yang telah berstatus "LUNAS" dihitung sebagai pemasukan laboratorium.

Kas yang belum dibayar ditampilkan sebagai tunggakan agar Bendahara dan anggota dapat memantau kewajiban yang belum diselesaikan.

## Pengeluaran

Setiap transaksi pengeluaran yang dicatat Bendahara akan mengurangi posisi kas laboratorium.

Pengeluaran disertai informasi mengenai:

- Waktu transaksi.
- Keperluan penggunaan dana.
- Nominal pengeluaran.
- Identitas transaksi.

## Net Cashflow

Net cashflow menggambarkan selisih antara seluruh pemasukan yang telah tervalidasi dengan seluruh pengeluaran yang telah dicatat.

Secara umum:

```
Net Cashflow = Total Pemasukan - Total Pengeluaran
```

Total pemasukan berasal dari:

- Denda yang telah dibayar.
- Uang kas yang telah dibayar.

Net cashflow positif menunjukkan bahwa total pemasukan masih lebih besar daripada pengeluaran. Net cashflow negatif menunjukkan bahwa pengeluaran telah melebihi pemasukan yang tercatat.

Indikator ini digunakan sebagai gambaran awal kondisi dana laboratorium, bukan sebagai laporan akuntansi formal.

## Persentase Pembayaran

Dashboard menampilkan persentase pembayaran untuk membantu pengguna memahami tingkat penyelesaian kewajiban.

Persentase dihitung berdasarkan perbandingan antara nilai yang telah dibayar dengan total kewajiban yang tercatat.

Visualisasi tersebut membantu pengguna membaca kondisi pembayaran tanpa harus menghitung setiap transaksi secara manual.

---

# 5. ANALISIS PERMASALAHAN TEKNIKAL DAN SOLUSI TERAPAN

Pengembangan aplikasi melibatkan beberapa permasalahan teknis yang umum ditemukan pada aplikasi berbasis spreadsheet.

## Konsistensi Tipe Data

Data dari Google Sheets tidak selalu diterima dalam tipe yang seragam. Nilai nominal dapat terbaca sebagai angka, teks, atau sel kosong.

Untuk menjaga konsistensi perhitungan, sistem melakukan normalisasi data sebelum nominal digunakan dalam proses agregasi.

Pendekatan ini mencegah kesalahan perhitungan akibat perbedaan tipe data.

## Konsistensi Struktur Spreadsheet

Backend bergantung pada struktur kolom yang digunakan dalam Google Sheets. Ketidaksesuaian urutan kolom dapat menyebabkan data dibaca pada atribut yang salah.

Untuk mengurangi risiko tersebut, setiap sheet menggunakan struktur data yang tetap dan dipisahkan berdasarkan jenis transaksi.

## Agregasi Data

Dashboard tidak hanya menampilkan data mentah, tetapi juga mengolahnya menjadi:

- Total pembayaran.
- Total tunggakan.
- Total pengeluaran.
- Persentase pembayaran.
- Tren pemasukan.
- Peringkat transaksi.
- Net cashflow.

Proses agregasi dirancang agar data kosong atau tidak valid tidak langsung menyebabkan seluruh dashboard gagal dimuat.

## Pembaruan Data

Setelah Bendahara menambah atau mengubah transaksi, data pada dashboard perlu diperbarui agar tetap sesuai dengan database.

Sistem menangani kebutuhan tersebut dengan menyinkronkan perubahan dari frontend ke Google Sheets dan menampilkan kembali data terbaru kepada pengguna.

## Penanganan Kesalahan

Setiap proses pengambilan dan perubahan data memiliki mekanisme penanganan kesalahan.

Ketika proses gagal, sistem memberikan notifikasi kepada pengguna melalui elemen antarmuka, sehingga kegagalan tidak hanya muncul sebagai error teknis pada browser.

## Tampilan Responsif

Antarmuka dirancang agar dapat digunakan melalui komputer maupun perangkat dengan ukuran layar lebih kecil.

Tabel, navigasi, form, dan kartu informasi disusun menggunakan layout responsif untuk mempertahankan keterbacaan pada beberapa ukuran layar.

---

# 6. BATASAN ARSITEKTUR DAN PELUANG REVISI LANJUTAN

Aplikasi ini dikembangkan untuk kebutuhan operasional dengan skala data yang relatif terbatas. Oleh karena itu, terdapat beberapa batasan yang perlu diperhatikan.

## Ketergantungan terhadap Google Sheets

Google Sheets memberikan kemudahan dalam pengelolaan data, tetapi tidak memiliki seluruh kemampuan database relasional.

Pada jumlah data dan pengguna yang lebih besar, proses pembacaan seluruh data dapat menjadi kurang efisien.

Pengembangan lanjutan dapat mempertimbangkan penggunaan database yang menyediakan:

- Query yang lebih terstruktur.
- Relasi antardata.
- Kontrol transaksi.
- Pengelolaan akses yang lebih rinci.
- Performa yang lebih stabil pada volume besar.

## Pengelolaan Hak Akses

Sistem saat ini menggunakan skema akses sederhana antara pengunjung publik dan Bendahara.

Pengembangan lanjutan dapat menambahkan beberapa peran, seperti:

- Administrator.
- Bendahara.
- Ketua laboratorium.
- Asisten.
- Auditor internal.

Setiap peran dapat memiliki hak akses yang berbeda sesuai tanggung jawabnya.

## Riwayat Perubahan

Sistem belum menyediakan audit trail yang lengkap untuk setiap perubahan data.

Fitur berikut dapat ditambahkan:

- Identitas pengguna yang membuat transaksi.
- Identitas pengguna yang mengubah status.
- Waktu perubahan.
- Riwayat nilai sebelum dan sesudah perubahan.
- Alasan penghapusan atau koreksi transaksi.

## Bukti Pembayaran

Validasi pembayaran masih dilakukan berdasarkan konfirmasi Bendahara.

Pengembangan berikutnya dapat menambahkan:

- Unggah bukti pembayaran.
- Tautan bukti transaksi.
- Status verifikasi.
- Catatan hasil pemeriksaan.
- Waktu validasi.

## Pengelolaan Periode

Dashboard saat ini berfokus pada keseluruhan data yang tersedia.

Fitur filter dapat dikembangkan berdasarkan:

- Bulan.
- Tahun.
- Periode kepengurusan.
- Kategori transaksi.
- Nama asisten.
- Status pembayaran.

## Pelaporan

Peluang pengembangan berikutnya adalah penyediaan laporan yang dapat diunduh dalam format:

- PDF.
- Spreadsheet.
- Rekap bulanan.
- Laporan per anggota.
- Laporan pemasukan dan pengeluaran.

## Skalabilitas

Arsitektur saat ini sesuai untuk penggunaan internal dan kebutuhan demonstrasi portofolio.

Apabila sistem digunakan dalam skala yang lebih besar, pengembangan dapat diarahkan pada:

1. Pemisahan frontend dan backend.
2. Penggunaan database khusus.
3. Autentikasi berbasis akun.
4. Pengujian otomatis.
5. Pengelolaan versi API.
6. Penyimpanan log aktivitas.
7. Penggunaan sistem deployment yang terpisah antara demo dan produksi.

---

# 7. LISENSI DAN ISOLASI KODE

Repositori ini digunakan sebagai bagian dari dokumentasi portofolio pengembangan perangkat lunak.

Kode sumber dapat dipelajari untuk memahami struktur aplikasi, integrasi Google Apps Script, pengelolaan data spreadsheet, dan penyusunan dashboard operasional.

Data yang digunakan pada versi demo telah dipisahkan dari aplikasi utama. Pemisahan ini dilakukan agar pengguna dapat mencoba fitur aplikasi tanpa memengaruhi data operasional laboratorium.

## Lingkungan Demo

Versi demo digunakan untuk:

- Demonstrasi portofolio.
- Pengujian antarmuka.
- Pengujian fitur Bendahara.
- Simulasi pencatatan transaksi.
- Evaluasi alur penggunaan aplikasi.

## Lingkungan Utama

Aplikasi utama dan database operasional laboratorium tidak disertakan dalam repositori publik.

Pemisahan tersebut bertujuan menjaga isolasi antara:

- Data demonstrasi.
- Data operasional.
- Konfigurasi pengembangan.
- Konfigurasi penggunaan utama.

## Lisensi

Hak cipta aplikasi dan dokumentasi berada pada pengembang.

```
Copyright © 2026 Dicky Alfian Irvansyah.
All rights reserved.
```

Penggunaan, modifikasi, atau distribusi ulang kode perlu memperoleh izin dari pemilik repositori, kecuali apabila pada masa mendatang repositori dilengkapi dengan lisensi terbuka yang menyatakan ketentuan berbeda.

---

# PENUTUP

Cashflow System Laboratory dikembangkan sebagai solusi terhadap pencatatan administrasi keuangan laboratorium yang sebelumnya tersebar pada beberapa media.

Aplikasi menyatukan data denda, kas, dan pengeluaran ke dalam satu sistem sehingga informasi lebih mudah dicatat, ditelusuri, dan divisualisasikan.

Proyek ini menunjukkan penerapan beberapa kompetensi pengembangan perangkat lunak, meliputi:

- Analisis permasalahan operasional.
- Perancangan alur data.
- Integrasi frontend dan backend.
- Pengelolaan database berbasis spreadsheet.
- Pengembangan dashboard interaktif.
- Pengolahan dan visualisasi data.
- Pengelolaan akses pengguna.
- Penanganan kesalahan aplikasi.
- Penyusunan antarmuka responsif.

Sistem masih memiliki ruang pengembangan, terutama pada aspek skalabilitas, audit trail, pengelolaan bukti transaksi, dan pemisahan hak akses yang lebih rinci.
````