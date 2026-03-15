# Proyeksi Klaim Asuransi Kesehatan Individu AXA Financial Indonesia

**Pendekatan Ensemble: Prophet & Amazon Chronos dengan Dekomposisi Geografis**

**Kompetisi:** Mathematical Challenge Festival ITB 2026
**Studi Kasus oleh:** AXA Financial Indonesia
**Best Public Score:** MAPE 4.473

Repositori ini berisi dokumentasi lengkap, kode pemrograman, dan laporan teknis untuk pemodelan peramalan klaim asuransi kesehatan AXA Financial Indonesia periode Agustus–Desember 2025.

---

## Ringkasan Proyek

Proyek ini merupakan solusi untuk kompetisi data science MCF ITB 2026 dengan studi kasus dari AXA Financial Indonesia. Tujuan utama adalah membangun model prediktif untuk meramalkan tiga variabel target klaim asuransi kesehatan individu secara bulanan:

- **Claim Frequency** — jumlah klaim per bulan
- **Claim Severity** — rata-rata biaya per klaim
- **Total Claim** — total nominal klaim yang disetujui

Target prediksi mencakup periode **Agustus–Desember 2025** (Kaggle submission) dan **Januari–Desember 2026** (rekomendasi bisnis).

Solusi ini mengusulkan arsitektur model **LocationDecomposed_Triple** yang menggabungkan kekuatan dekomposisi statistik (Prophet) dan *foundation model* deret waktu berbasis Transformer (Amazon Chronos). Inovasi utama terletak pada pemisahan strategi peramalan berdasarkan wilayah (Indonesia, Singapura, Malaysia) untuk menangani volatilitas biaya ekstrem (*severity*) di lokasi luar negeri.

---

## Hasil Utama

| Indikator | Nilai |
|---|---|
| Best Public MAPE Score | **4.473** |
| Metodologi Utama | *Zero-shot forecasting* dengan strategi kuantil adaptif |
| Arsitektur Model Terbaik | LocationDecomposed_Triple |
| Periode Data Training | Januari 2024 – Juli 2025 (19 bulan) |

---

## Struktur Folder

```
├── dataset/                # Folder berisi dataset mentah (format .xlsx/.csv)
├── notebook/               # Jupyter Notebook (.ipynb) pemodelan utama
├── reports/                # Makalah/Laporan Teknis (format PDF)
└── README.md               # Dokumentasi proyek
```

---

## Struktur Notebook

Notebook utama `MCF_ITB_2026_MERGED_FINAL.ipynb` disusun dengan alur sebagai berikut:

```
MCF_ITB_2026_MERGED_FINAL.ipynb
│
├── 0. Install & Import
├── 1. Load Data
├── 2. Exploratory Data Analysis (EDA)
│   ├── 2.1 Basic Check — Struktur Data
│   ├── 2.2 Analisis Univariat & Multivariat
│   └── 2.3 Analisis Gabungan Klaim & Polis
├── 3. Preprocessing
│   ├── 3.1 Date Parsing & Type Casting
│   ├── 3.2 Missing Value Handling
│   ├── 3.3 Outlier Handling (Winsorization P99)
│   └── 3.4 Merge Klaim + Polis
├── 4. Feature Engineering
│   ├── 4.1 Claim-Level Features
│   ├── 4.2 Policy-Level Features
│   ├── 4.3 Build Monthly Time Series
│   ├── 4.4 Time-Series Feature Engineering
│   ├── 4.5 Visualisasi Target Bulanan
│   ├── 4.6 Analisis Tren
│   └── 4.7 Dekomposisi Lokasi Geografis
├── 5. Pembuatan Model
│   ├── 5.1 Model Registry
│   ├── 5.2 Model 1: Prophet (Baseline)
│   ├── 5.3 Model 2: Amazon Chronos-Bolt
│   ├── 5.4 Blend Model 1+2: ChronosBlend
│   ├── 5.5 LocationDecomposed
│   └── 5.6 Model Terbaik: LocationDecomposed_Triple
├── 6. Cross Validasi & Evaluasi Model
│   ├── 6.1 Walk-Forward Backtest
│   ├── 6.2 Perbandingan Semua Model
│   ├── 6.3 Analisis Faktor
│   └── 6.4 Visualisasi Hasil Forecasting
├── 7. Hasil Prediksi & Rekomendasi
│   ├── 7.1 Prediksi Agustus–Desember 2025
│   ├── 7.2 Prediksi Tahun 2026
│   └── 7.3 Rekomendasi Bisnis
└── 8. Submission Kaggle
```

---

## Dataset

| File | Deskripsi |
|---|---|
| `Data_Klaim.csv` | Data klaim asuransi kesehatan individual |
| `Data_Polis.csv` | Data polis dan demografis tertanggung |
| `sample_submission.csv` | Template format submission Kaggle |

Data mencakup periode **Januari 2024 – Juli 2025** (19 bulan).

---

## Panduan Penggunaan (Google Colab & Drive)

Notebook ini dirancang untuk dijalankan di Google Colab dengan dataset yang tersimpan di Google Drive. Ikuti langkah-langkah berikut untuk melakukan replikasi.

### 1. Persiapan Folder Drive

Pastikan struktur folder di Google Drive Anda adalah sebagai berikut:

```
/content/drive/MyDrive/AXA_Competition/dataset/
```

Letakkan file dataset berikut di dalam folder tersebut:

- `Train_Data_Claim.xlsx`
- `Train_Data_Polis.xlsx`

### 2. Menjalankan Notebook

1. Buka file `.ipynb` di Google Colab.
2. Aktifkan GPU (opsional, namun disarankan untuk pemrosesan Amazon Chronos).
3. Jalankan sel pertama untuk *mounting* Drive:

```python
from google.colab import drive
drive.mount('/content/drive')
```

4. Arahkan `BASE_PATH` ke folder dataset yang sesuai:

```python
BASE_PATH = '/content/drive/MyDrive/AXA_Competition/dataset/'
```

5. Jalankan seluruh sel secara berurutan dari atas ke bawah. Hasil submission akan tersimpan otomatis dalam format CSV sesuai template Kaggle.

### 3. Instalasi Dependensi

Jalankan cell instalasi pada awal notebook untuk menginstal seluruh dependensi secara otomatis:

```bash
pip install "numpy==1.26.4"
pip install "torch==2.5.1" "torchvision==0.20.1"
pip install "transformers==4.44.0"
pip install chronos-forecasting prophet statsforecast
```

Daftar lengkap library utama yang digunakan:

| Library | Versi | Kegunaan |
|---|---|---|
| `numpy` | 1.26.4 | Komputasi numerik |
| `pandas` | — | Manipulasi data |
| `torch` | 2.5.1 | Backend Chronos |
| `transformers` | 4.44.0 | Arsitektur transformer |
| `chronos-forecasting` | latest | Amazon Chronos-Bolt |
| `prophet` | latest | Meta Prophet |
| `holidays` | latest | Kalender libur Indonesia |
| `scipy` | — | Uji statistik |
| `seaborn` / `matplotlib` | — | Visualisasi |

---

## Metodologi Pemodelan

### 1. Preprocessing

Penanganan *outlier* menggunakan teknik **Winsorization pada persentil ke-99 (P99)** di level klaim individual. Langkah ini terbukti krusial: tanpa winsorisasi diperoleh MAPE sebesar 8.479, sedangkan setelah winsorisasi MAPE turun menjadi 4.473.

### 2. Feature Engineering

Ekstraksi komponen temporal (bulan, kuartal), rasio *cashless*, segmentasi demografi, serta pembangunan deret waktu bulanan dari data klaim dan polis yang telah digabungkan.

### 3. Location Decomposition

Pemisahan deret waktu berdasarkan segmen geografis lokasi rumah sakit (IDN, SGP, MYS) untuk mengisolasi volatilitas tinggi pada klaim luar negeri, khususnya Singapura. Klaim Singapura menyumbang sekitar 47% dari total nilai klaim meskipun hanya mencakup 22% dari frekuensi, dengan rata-rata *severity* 3,2x lebih tinggi dibandingkan klaim domestik.

### 4. Pembuatan Model

Tiga model utama dikembangkan secara bertahap:

**Model 1 — Prophet (Baseline)**
Model deret waktu berbasis dekomposisi aditif dengan konfigurasi `changepoint_prior_scale = 0.05` dan penentuan *changepoint* manual pada Februari 2024, April 2024, dan Januari 2025.

**Model 2 — Amazon Chronos-Bolt**
*Foundation model* berbasis arsitektur T5 untuk *zero-shot forecasting*. Menghasilkan distribusi probabilistik melalui 9 kuantil (Q0.1–Q0.9) tanpa memerlukan proses pelatihan ulang pada data spesifik.

**Model 3 — LocationDecomposed_Triple (Best Model)**
*Ensemble equal-weight* dari tiga komponen: LocationDecomposed + Chronos-Bolt + Prophet, dengan dekomposisi deret waktu per segmen geografis. Model ini merupakan arsitektur final yang menghasilkan performa terbaik.

### 5. Evaluasi

Evaluasi internal dilakukan menggunakan *walk-forward backtest* dengan pembagian data sebagai berikut:

- **Train:** Januari 2024 – Januari 2025
- **Validate:** Februari – Juli 2025

---

## Hasil Evaluasi Model

| Model | Public MAPE Score |
|---|---|
| Prophet | 5.124 |
| Chronos-Bolt | 4.860 |
| ChronosBlend | 4.646 |
| **LocationDecomposed_Triple** | **4.473** |

---

## Temuan Utama

- Klaim Singapura menyumbang sekitar **47% total nilai klaim** meskipun hanya **22% dari frekuensi klaim**, dengan rata-rata *severity* 3,2x lebih tinggi dari klaim domestik.
- Winsorisasi P99 memberikan dampak signifikan terhadap akurasi model: MAPE tanpa winsorisasi mencapai 8.479, turun menjadi 4.473 setelah winsorisasi diterapkan.
- Ketiga variabel target (frekuensi, *severity*, dan total klaim) menunjukkan **tren menurun secara statistik** sepanjang periode observasi.
- Faktor paling dominan terhadap nilai *severity* klaim adalah proporsi klaim luar negeri (`pct_overseas`).

---

## Laporan Teknis

Penjelasan mendalam mengenai teori, analisis statistik deskriptif (EDA), dan justifikasi pemilihan model dapat ditemukan pada file berikut:

```
reports/Makalah_AXA_Financial_Indonesia_TeamName.pdf
```

---

## Kontributor

Proyek ini dikembangkan oleh tim berikut dalam kapasitas sebagai *Data Scientist* dan *Actuarial Modeler* pada kompetisi Mathematical Challenge Festival ITB 2026:

| No. | Nama |
|-----|------|
| 1 | Andi Nabil Safaraz |
| 2 | Aulia Kemal Syah |
| 3 | Naufal Dzakirsyah |

---

## Catatan Teknis

Model ini dikembangkan khusus untuk data deret waktu pendek (19 bulan) dengan memanfaatkan kemampuan *pre-trained* dari Amazon Chronos-Bolt. Seluruh eksperimen dan pengujian dilakukan dalam lingkungan Google Colab dengan akselerasi GPU. Pendekatan *zero-shot forecasting* dipilih untuk mengatasi keterbatasan jumlah observasi historis yang tidak mencukupi untuk pelatihan model berbasis *deep learning* secara penuh.
