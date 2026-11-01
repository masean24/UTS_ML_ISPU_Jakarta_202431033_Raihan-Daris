# Klasifikasi Kategori ISPU DKI Jakarta dengan KNN

Tugas UTS Pembelajaran Mesin (C31010503), Institut Teknologi PLN.
Proyek ini memprediksi kategori Indeks Standar Pencemar Udara (ISPU) harian di DKI Jakarta dari 6 parameter polutan memakai algoritma K-Nearest Neighbors (KNN).

| | |
|---|---|
| **Nama** | Raihan Daris Ramadhan |
| **NIM** | 202431033 |
| **Dosen** | Dr. Efy Yosrita, S.Si., M.Kom |
| **Video penjelasan** | [Tonton di YouTube](ISI_LINK_YOUTUBE) |

## Latar Belakang

Pemantauan kualitas udara DKI Jakarta menghasilkan data ISPU harian dari 5 stasiun. Data ini sering memiliki nilai kosong dan nilai ekstrem, sehingga kategori kualitas udara (BAIK, SEDANG, TIDAK SEHAT) tidak selalu mudah ditentukan. Proyek ini membangun model klasifikasi untuk memprediksi kategori tersebut dari polutan yang tersedia.

## Dataset

- **Sumber:** [Satu Data Jakarta, Data ISPU di Provinsi DKI Jakarta](https://satudata.jakarta.go.id/open-data/detail?kategori=dataset&page_url=data-indeks-standar-pencemar-udara-ispu-di-provinsi-dki-jakarta&data_no=1)
- **File:** `8759.csv`
- **Ukuran:** 3.501 baris dan 13 kolom
- **Periode data:** Januari 2024 sampai November 2025
- **Stasiun:** DKI1 Bundaran HI, DKI2 Kelapa Gading, DKI3 Jagakarsa, DKI4 Lubang Buaya, DKI5 Kebon Jeruk
- **Fitur:** PM10, PM2.5, SO2, CO, O3, NO2
- **Target:** `kategori` (BAIK, SEDANG, TIDAK SEHAT)

Masalah kualitas data yang ditemukan:

- 640 sel kosong dengan penanda `N/A`, `NULL`, dan `TIDAK ADA DATA`
- Banyak outlier, terutama pada ozon (169 data) dan NO2 (60 data)
- 13 variasi penulisan nama stasiun untuk 5 stasiun yang sebenarnya
- Kelas tidak seimbang (SEDANG sekitar 77%) dan kelas SANGAT TIDAK SEHAT hanya 1 baris

## Alur Pengerjaan

1. **Business Understanding:** merumuskan masalah, tujuan, dan metrik.
2. **EDA:** cek missing value, statistik deskriptif, boxplot, histogram, korelasi, dan rata-rata polutan per kategori.
3. **Preprocessing:**
   - menyamakan nama stasiun
   - mengubah penanda kosong menjadi NaN
   - membuang baris tanpa label
   - menggabungkan kelas SANGAT TIDAK SEHAT ke TIDAK SEHAT
   - membuang `max` dan `parameter_pencemar_kritis` untuk mencegah kebocoran data
   - split data latih dan uji 80:20 (stratified)
   - imputasi median, capping outlier dengan IQR, dan normalisasi Min-Max
4. **Modeling:** KNN dengan pemilihan nilai k lewat 5-fold cross validation.
5. **Evaluasi:** akurasi, macro F1, classification report, dan confusion matrix.
6. **Verifikasi hitungan manual:** hitungan KNN manual pada 15 data sampel dicocokkan dengan scikit-learn.

## Hasil

| Metrik | Nilai |
|---|---|
| k terbaik | 3 |
| Akurasi data uji | 92,24% |
| Macro F1 data uji | 0,861 |
| Baseline (kelas mayoritas) | 77,44% |
| Recall BAIK / SEDANG / TIDAK SEHAT | 79,8% / 96,7% / 73,5% |

Kelas TIDAK SEHAT paling sulit diprediksi karena jumlah datanya paling sedikit.

## Struktur Repositori

```
.
├── README.md
├── 8759.csv                                  # dataset
├── UTS_ML_ISPU_Jakarta_202431033.ipynb       # notebook utama
└── UTS-PM-KELAS-202431033-RaihanDarisRamadhan.docx   # lembar jawaban UTS
```

## Cara Menjalankan

1. Clone repositori ini.

   ```bash
   git clone https://github.com/USERNAME/NAMA_REPO.git
   cd NAMA_REPO
   ```

2. Pasang library yang dibutuhkan.

   ```bash
   pip install pandas numpy matplotlib seaborn scikit-learn jupyter
   ```

3. Buka notebook, lalu jalankan semua cell lewat menu Kernel, Restart & Run All.

   ```bash
   jupyter notebook UTS_ML_ISPU_Jakarta_202431033.ipynb
   ```

Pastikan `8759.csv` berada satu folder dengan notebook.

## Keterbatasan

- Pembagian data latih dan uji dilakukan secara acak, padahal data harian berurutan waktu.
- KNN sensitif terhadap ketidakseimbangan kelas.
- Pada hitungan manual dengan 12 data latih, hasilnya sensitif terhadap pemilihan tetangga (akurasi manual 2 dari 3 data uji).

## Teknologi

Python, pandas, NumPy, Matplotlib, Seaborn, scikit-learn, Jupyter Notebook.
