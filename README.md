# Laporan Proyek Sistem Rekomendasi Produk (Saranin)

## Project Overview

Pelanggan supermarket modern cenderung memiliki gaya hidup yang serba cepat dan kurang memiliki waktu atau keinginan untuk menjelajahi seluruh rak demi menemukan produk yang mereka butuhkan.

Banyak pelanggan merasa kebingungan dalam menentukan produk apa yang akan dibeli, terutama jika mereka tidak memiliki daftar belanja yang jelas. Hal ini menyebabkan pengalaman belanja menjadi tidak efisien dan memakan waktu.

> Solusi yang ditawarkan:  
> Sistem rekomendasi berbasis machine learning untuk memberikan saran produk personalisasi berdasarkan preferensi pelanggan lain yang serupa.

## Business Understanding

### Problem Statements
1. Bagaimana supermarket dapat mengatasi kesulitan pelanggan dalam menemukan produk dengan cepat?

2. Bagaimana supermarket dapat membantu pelanggan yang kebingungan memilih produk tanpa daftar belanja?

3. Bagaimana menjembatani kebutuhan belanja cepat dengan kenyataan pencarian produk manual?

### Goals
- Meningkatkan Efisiensi Belanja
- Meningkatkan Kenyamanan Pelanggan
- Menyediakan Rekomendasi yang Dipersonalisasi

### Solution Statements
Membangun sistem rekomendasi dengan dua pendekatan:
- Collaborative Filtering (fitur utama):
Rekomendasi berbasis pola pembelian pengguna lain yang serupa.
- Content-Based Filtering (fitur tambahan):
Rekomendasi berdasarkan histori pembelian pribadi seperti produk beserta kategori.

## Data Understanding

### Informasi Dataset
Dataset dibuat secara mandiri menggunakan Mockaroo dan terdiri dari 4 file:
- Product.csv : berisi detail informasi produk
- User.csv : berisi detail informasi dari user / pelanggan
- Transaction.csv : berisi history transaksi sebelumnya.
- Detailed Transaction.csv : berisi detail barang apa saja, hingga jumlah barang yang dibeli dari pengguna.

Link Dataset:
https://drive.google.com/drive/folders/1d1LoIhTe6YeUDUK9ExnwjSBBqVenNa9h?usp=sharing 

### Jumlah Baris dan Kolom
- Product : 225 Baris dan 5 Kolom
- User : 60 Baris dan 4 Kolom
- Transaction : 250 Baris dan 3 Kolom
- Detailed Transaction : 1000 Baris dan 5 Kolom

### Uraian fitur
Product :
- id_product : kolom ini digunakan untuk identifier unik dari masing-masing produk.
- name : kolom ini digunakan untuk menyimpan informasi nama produk.
- category : kolom ini digunakan untuk menyimpan informasi kategori produk.
- harga : kolom ini digunakan untuk menyimpan informasi harga produk.
- ketersediaan_stock : kolom ini digunakan untuk menyimpan informasi ketersediaan produk dengan tipe data boolean.

User :
- id_user : kolom ini digunakan untuk identifier unik dari masing-masing user / pelanggan.
- fullname : kolom ini digunakan menyimpan nama panjang pengguna.
- email : kolom ini digunakan untuk menyimpan informasi email dari masing-masing pengguna.
- gender : kolom ini digunakan untuk menyimpan informasi gender / jenis kelamin pelanggan.

Transaction :
- id_transaksi : kolom ini digunakan untuk identifier unik dari setiap transaksi yang dilakukan user.
- id_user : kolom ini digunakan untuk menyimpan identifier unik dari user yang melakukan transaksi sebelumnya.
- date : kolom yang digunakan untuk menyimpan informasi waktu seperti tanggal dan jam pada waktu transaksi.

Detailed Transaction : 
- id_transaksi : kolom ini merujuk dari identifier unik dari transaksi.
- quantity : kolom ini menyimpan informasi jumlah barang yang dibeli oleh pelanggan.
- id_product : kolom ini menyimpan informasi identifier unik dari produk yang dibeli.
- harga : kolom ini menyimpan informasi harga beli per produk nya.
- total : kolom ini menyimpan harga total dari setiap jenis produk yang dibeli oleh user.

## Data Preparation

### Data Preprocessing
- Mengecek info dataset yang bertujuan untuk menampilkan ringkasan struktur data dan keberadaan nilai yang hilang dalam dataset, sebelum melakukan analisis atau pemrosesan dataset lebih lanjut.
- Mengecek Detail Shape yang bertujuan untuk menampilkan jumlah baris dan kolom.
- Melakukan Pengecekan Duplicated yang bertujuan untuk menghitung dan menampilkan jumlah baris yang merupakan duplicated dalam dataset.
- Melakukan Penghapusan Duplicated yang bertujuan untuk menghapus baris duplikat pada dataset.
- Mengecek Detail Isna yang bertujuan untuk menghitung jumlah nilai yang hilang (missing values) di setiap kolom pada dataset.
- Melakukan Imputasi dengan Nilai Mean pada Kolom Numerik yang bertujuan untuk mengganti (imputasi) nilai-nilai yang hilang pada dataset.
- Melakukan Penggabungan antar Dataset yang bertujuan untuk menggabungkan dataset agar menjadi satu kesatuan.

## Exploratory Data Analysis (EDA)
EDA dilakukan untuk memahami pola data dan distribusi awal:
- Frekuensi pembelian produk per kategori.
- Produk paling sering dibeli.
- Preferensi pengguna berdasarkan gender.
- Distribusi waktu transaksi.

## Modeling

### Collaborative Filtering - Neural Matrix Factorization (NMF)
Pada tahap ini, dibangun sistem rekomendasi produk supermarket berbasis Collaborative Filtering menggunakan Neural Matrix Factorization (NMF). Model ini mengandalkan embedding layer dan dense network untuk mempelajari interaksi laten antara pengguna dan produk.

Tujuan:
- Memprediksi preferensi pengguna berdasarkan histori.
- Memberikan rekomendasi yang dipersonalisasi.

Arsitektur Model:
- Input Layer: user_id, product_id.
- Embedding Layer: Untuk representasi laten.
- Concatenation Layer
- Dense Layer: Aktivasi ReLU, regulasi L2.
- Dropout Layer: Mencegah overfitting.
- Output Layer: Prediksi rating/preferensi.

Pelatihan:
- Early Stopping berdasarkan val_loss.
- Maksimum 50 epoch, berhenti saat validasi loss stagnan.

Hasil Training:
- Epoch 1: loss 11.0791 – val_loss 9.9718
- Epoch 10: loss 8.9544 – val_loss 7.8375
- Epoch 20: loss 3.3253 – val_loss 2.9393
- Epoch 26: loss 1.7108 – val_loss 2.2412 ← (Training berhenti di sini)
Model menunjukkan penurunan loss yang konsisten, yang mengindikasikan bahwa sistem belajar dengan baik terhadap pola interaksi user-item.

### Content-Based Filtering
Tahapan ini membangun sistem rekomendasi berbasis Content-Based Filtering, yaitu dengan menganalisis kemiripan antar produk berdasarkan fitur konten seperti name dan category
Metode:
- TF-IDF Vectorization: Mengubah teks menjadi fitur numerik.
- Cosine Similarity: Menghitung kedekatan antar produk.
- Penyimpanan model dalam bentuk .pkl.

Tahapan Implementasi:
1. Preprocessing teks pada nama dan kategori produk.
2. Membentuk TF-IDF Matrix (209 x 473).
3. Hitung Cosine Similarity Matrix (209 x 209).
4. Mapping product_id_enc ke indeks matrix.
5. Simpan model dalam format .pkl.

## Evaluation

### Collaborative Filtering
Evaluasi dilakukan untuk mengukur performa model rekomendasi berbasis interaksi pengguna menggunakan pendekatan Collaborative Filtering. Model bertujuan memprediksi kuantitas pembelian produk oleh user, sehingga sistem dapat memberikan rekomendasi produk yang paling relevan.

Visualisasi Loss:
- Train Loss dan Validation Loss menurun konsisten hingga epoch 22.
- Tidak terjadi overfitting signifikan.
Metrik Evaluasi:
- Mean Squared Error (MSE) = 2.2111  -> Cukup baik untuk ukuran prediksi kuantitas pembelian.

Contoh Rekomendasi:
1. Pilih satu user.
2. Identifikasi produk yang sudah dibeli.
3. Prediksi skor kepercayaan untuk produk lain.
4. Tampilkan Top 15 rekomendasi yang relevan.

### Content-Based Filtering
evaluasi berbeda dari model prediktif seperti Collaborative Filtering. Content-Based Filtering berfokus pada analisis fitur item (produk) dan mencocokkannya dengan preferensi pengguna berdasarkan riwayat interaksi atau pembelian sebelumnya.

Alasan Tidak Menggunakan Evaluasi Kuantitatif:
- Tidak tersedia ground truth eksplisit.
- Produk baru belum tentu muncul dalam histori pengguna.

Evaluasi Alternatif:
- Manual Verification: Memeriksa apakah produk yang direkomendasikan masuk akal berdasarkan histori user.
- User Testing (Opsional): Mengumpulkan feedback pengguna langsung mengenai relevansi rekomendasi.
- Analisis Qualitative: Kategori produk yang disarankan menunjukkan kemiripan tinggi.

## Kesimpulan
Proyek ini berhasil membangun dua sistem rekomendasi:
- Collaborative Filtering yang efektif mempelajari pola pembelian pengguna dan memberikan rekomendasi personalisasi.
- Content-Based Filtering sebagai cadangan yang dapat memberikan rekomendasi meskipun tidak ada histori interaksi pengguna lain.

Sistem rekomendasi ini diharapkan dapat meningkatkan pengalaman belanja pengguna di supermarket modern melalui efisiensi waktu dan kemudahan dalam memilih produk.
