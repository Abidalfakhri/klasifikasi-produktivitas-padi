# Klasifikasi Produktivitas Padi di Sumatera Berdasarkan Data Iklim

Tugas Proyek Kelompok Mata Kuliah Kecerdasan Buatan

Anggota Kelompok:
1. M. Abid Alfakhri
2. Rekhi Wirya Adjie
3. Juan Carlos Simanungkalit
4. Michael Garcia Ginting
## Deskripsi

Proyek ini mengklasifikasikan produktivitas padi tingkat provinsi di Pulau Sumatera ke dalam dua kelas, **Tinggi** dan **Rendah**, berdasarkan provinsi, tahun, curah hujan, kelembapan, dan suhu rata-rata.

Produktivitas dihitung dari produksi dibagi luas panen (ton/ha). Kelas **Tinggi** diberikan jika produktivitas ≥ median (4,15 ton/ha), selain itu **Rendah**. Kolom produksi dan luas panen tidak dipakai sebagai fitur karena dipakai untuk membentuk label.

Empat algoritma dibandingkan dengan Stratified 5-Fold Cross-Validation:

- K-Nearest Neighbors (KNN)
- Gaussian Naive Bayes
- Decision Tree
- Support Vector Machine (SVM, kernel RBF)

## Dataset

- **Nama:** Dataset Tanaman Padi Sumatera
- **Sumber:** Kaggle, [judul dataset dan nama pembuat], [URL Kaggle]
- **Sumber data asli:** produksi dan luas panen dari Badan Pusat Statistik (BPS); curah hujan, kelembapan, dan suhu dari BMKG
- **Cakupan:** 8 provinsi Sumatera, tahun 1993–2020
- **Ukuran:** 224 baris awal, 221 baris setelah pembersihan
- **Berkas:** `Data_Tanaman_Padi_Sumatera_version_1.csv`

| Kolom | Keterangan |
|---|---|
| Provinsi | Nama provinsi |
| Tahun | Tahun pencatatan |
| Produksi | Produksi padi (ton) |
| Luas Panen | Luas panen (hektare) |
| Curah hujan | Curah hujan (mm) |
| Kelembapan | Kelembapan udara (%) |
| Suhu rata-rata | Suhu rata-rata (°C) |

Catatan: data merupakan kompilasi pihak ketiga di Kaggle, bukan unduhan langsung dari BPS/BMKG. Hak cipta dan lisensi data tetap mengikuti sumber aslinya.

## Alur Proyek

1. Data collection
2. Preprocessing (imputasi median, standardisasi, one-hot encoding)
3. Pembagian data stratifikasi 70/15/15 (train/validation/test)
4. Training dan pemilihan hyperparameter (berdasarkan F1 pada validation)
5. Evaluasi dengan 5-Fold CV dan test set
6. Analisis fitur, kesalahan, dan performa per provinsi
7. Deployment sederhana (model disimpan dengan joblib)

## Hasil Utama

Stratified 5-Fold CV (data train + validation, F1 rata-rata ± simpangan baku):

| Model | F1 |
|---|---|
| KNN (K=3) | 0,909 ± 0,045 |
| SVM (RBF) | 0,892 ± 0,049 |
| Decision Tree | 0,803 ± 0,071 |
| Naive Bayes | 0,392 ± 0,060 |

Pada test set (34 data), SVM dan Decision Tree mencapai F1 0,971 dan KNN 0,914. Selisih ini hanya dua sampel sehingga tidak cukup kuat untuk menyimpulkan keunggulan. Berdasarkan permutation importance, tahun dan provinsi adalah fitur paling berpengaruh, jauh di atas variabel iklim.


## Cara Menjalankan

1. Clone repositori ini atau buka notebook di Google Colab.
2. Pastikan berkas CSV ada di repositori. Pada sel pemuatan data, `URL_DATA` harus mengarah ke alamat raw CSV di repositori ini:
   `https://raw.githubusercontent.com/[username]/[nama-repo]/main/Data_Tanaman_Padi_Sumatera_version_1.csv`
3. Pasang pustaka yang dibutuhkan:
   ```
   pip install numpy pandas matplotlib seaborn scikit-learn joblib
   ```
4. Jalankan seluruh sel secara berurutan (`random_state = 42`, hasil dapat diulang).

## Keterbatasan dan Etika

- Data bersifat observasional pada tingkat provinsi, bukan petani individual atau kabupaten. Hasil tidak menunjukkan hubungan sebab-akibat.
- Pembagian data acak pada data deret waktu dapat membuat skor lebih optimistis daripada memprediksi tahun mendatang.
- Setiap provinsi hanya punya sekitar 28 data, sehingga metrik per provinsi (akurasi 0,79–0,93) hanya indikasi.
- Model hanya alat bantu analisis dan tidak menggantikan pertimbangan ahli pertanian.
