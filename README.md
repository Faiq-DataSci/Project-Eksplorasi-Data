# 📊 Eksplorasi Data

## Analisis Pengaruh Suhu Udara dan Produksi Padi terhadap Harga Beras di Indonesia

Repository ini merupakan project Mata Kuliah **Eksplorasi Data** yang membahas analisis hubungan antara **Suhu Udara** dan **Produksi Padi** terhadap **Harga Beras di Indonesia** menggunakan Python.

Selain analisis hubungan antar variabel, project ini juga membandingkan beberapa metode **Forecasting** untuk memprediksi data time series.

---

## 📚 Mata Kuliah

**Eksplorasi Data**

---

# 🎯 Tujuan Project

- Menganalisis hubungan Suhu Udara terhadap Harga Beras.
- Menganalisis hubungan Produksi Padi terhadap Harga Beras.
- Mengukur hubungan kedua variabel secara bersama-sama menggunakan Korelasi Ganda.
- Membandingkan beberapa metode forecasting berdasarkan nilai MAE dan MSE.

---

# 📂 Struktur Repository

```text
CODE PROJECT EKSPLORASI DATA
│
├── .venv/
│
├── Penjelasan/
│   ├── Single Moving Average.md
│   ├── Double Moving Average.md
│   ├── Single Exponential Smoothing.md
│   ├── Double Exponential Smoothing.md
│   └── Korelasi Ganda.md
│
├── Correlasi.ipynb
├── Harga_Beras.ipynb
├── Produksi_Padi.ipynb
├── Suhu_Udara.ipynb
│
├── Data.xlsx
├── Hasil Korelasi Ganda.xlsx
│
└── README.md
```

---

# 🗂️ Penjelasan File

| File                          | Keterangan                                |
| ----------------------------- | ----------------------------------------- |
| **Data.xlsx**                 | Dataset penelitian                        |
| **Harga_Beras.ipynb**         | Analisis dan forecasting data harga beras |
| **Suhu_Udara.ipynb**          | Analisis data suhu udara                  |
| **Produksi_Padi.ipynb**       | Analisis data produksi padi               |
| **Correlasi.ipynb**           | Analisis Korelasi Ganda                   |
| **Hasil Korelasi Ganda.xlsx** | Output hasil perhitungan korelasi         |
| **Penjelasan/**               | Dokumentasi materi dan metode analisis    |

---

# 📈 Metode Forecasting

Project ini mengimplementasikan beberapa metode peramalan.

## 🔹 Single Moving Average (3 Bulan)

Menghitung rata-rata dari tiga periode sebelumnya sebagai nilai ramalan periode berikutnya.

Output:

- Forecast
- Error
- MAE
- MSE
- Grafik

---

## 🔹 Single Moving Average (5 Bulan)

Menggunakan lima periode sebelumnya sehingga menghasilkan prediksi yang lebih halus dibanding SMA 3 Bulan.

Output:

- Forecast
- Error
- MAE
- MSE
- Grafik

---

## 🔹 Double Moving Average

Melakukan proses Moving Average sebanyak dua kali sehingga mampu mengikuti pola data yang memiliki kecenderungan (trend).

Output:

- Nilai St
- Nilai S't
- Nilai a
- Nilai b
- Forecast
- MAE
- MSE

---

## 🔹 Single Exponential Smoothing

Memberikan bobot lebih besar pada data terbaru menggunakan parameter α (alpha).

Output:

- Nilai Smoothing
- Forecast
- MAE
- MSE

---

## 🔹 Double Exponential Smoothing

Digunakan untuk data yang memiliki trend dengan menghitung dua kali proses smoothing.

Output:

- Single Smoothing
- Double Smoothing
- Nilai a
- Nilai b
- Forecast
- MAE
- MSE

---

# 📊 Analisis Korelasi Ganda

Analisis dilakukan menggunakan tiga variabel:

- **Y : Harga Beras**
- **X₁ : Suhu Udara**
- **X₂ : Produksi Padi**

Hasil analisis meliputi:

- Korelasi Pearson
- Korelasi Berganda (R)
- Koefisien Determinasi (R²)
- F Hitung
- F Tabel
- Keputusan Hipotesis

---

# 📉 Evaluasi Model

Seluruh metode forecasting dibandingkan menggunakan:

- Mean Absolute Error (MAE)
- Mean Squared Error (MSE)

Metode terbaik dipilih berdasarkan nilai MAE dan MSE terkecil.

---

# 🛠️ Library

```python
pandas
numpy
matplotlib
scipy
scikit-learn
openpyxl
```

Install:

```bash
pip install pandas numpy matplotlib scipy scikit-learn openpyxl
```

---

# 💻 Cara Menjalankan

1. Clone repository.

```bash
git clone https://github.com/Faiq-DataSci/Project-Eksplorasi-Data.git
```

2. Masuk ke folder project.

```bash
cd Project-Eksplorasi-Data
```

3. Install library.

```bash
pip install -r requirements.txt
```

4. Jalankan notebook sesuai analisis yang diinginkan menggunakan **Jupyter Notebook** atau **Visual Studio Code**.

---

# 👨‍💻 Author

**Faiq**

Mata Kuliah Eksplorasi Data

Program Studi Sains Data

2026
