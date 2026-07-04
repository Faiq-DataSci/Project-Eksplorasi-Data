# Penjelasan Program Single Exponential Smoothing (SES)

## 1. Import Library

```python
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.metrics import (
    mean_absolute_error,
    mean_squared_error,
    mean_absolute_percentage_error
)
```

### Penjelasan

Program menggunakan beberapa library, yaitu:

- **Pandas (`pd`)** digunakan untuk membaca dan mengolah data dalam bentuk DataFrame.
- **Matplotlib (`pyplot`)** digunakan untuk membuat grafik hasil peramalan.
- **Scikit-learn (`sklearn.metrics`)** digunakan untuk menghitung ukuran evaluasi model seperti:
  - Mean Absolute Error (MAE)
  - Mean Squared Error (MSE)
  - Mean Absolute Percentage Error (MAPE)

---

## 2. Membaca File Excel

```python
df = pd.read_excel(
    "Data.xlsx",
    sheet_name="Harga_Beras"
)
```

### Penjelasan

Kode di atas membaca data dari file **Data.xlsx** pada sheet **Harga_Beras** kemudian menyimpannya ke dalam variabel **df**.

```python
print("===== DATA AWAL =====")
display(df.style.hide(axis="index"))
```

Perintah tersebut digunakan untuk menampilkan data awal yang akan digunakan dalam proses peramalan.

---

## 3. Menentukan Nilai Alpha (α)

```python
alpha = 0.5
```

### Penjelasan

Nilai **alpha (α)** merupakan parameter pemulusan (_smoothing constant_) pada metode Single Exponential Smoothing.

Pada program ini digunakan:

\[
\alpha = 0,5
\]

Nilai alpha berada pada rentang:

\[
0 < \alpha < 1
\]

Interpretasi nilai alpha:

- **Alpha kecil (mendekati 0)** → hasil smoothing lebih halus dan lebih dipengaruhi data lama.
- **Alpha besar (mendekati 1)** → hasil smoothing lebih cepat mengikuti perubahan data terbaru.

Karena menggunakan **α = 0,5**, maka data lama dan data terbaru memiliki bobot yang sama besar.

---

## 4. Membuat Kolom Hasil Smoothing

```python
df["St"] = 0.0
```

### Penjelasan

Program membuat kolom baru bernama **St** yang akan digunakan untuk menyimpan hasil perhitungan Single Exponential Smoothing.

---

## 5. Menentukan Nilai Awal

```python
df.loc[0, "St"] = df.loc[0, "Harga Beras (Rp) (Y)"]
```

### Penjelasan

Nilai awal smoothing diambil dari data aktual pertama.

Rumus:

\[
S_1=X_1
\]

Keterangan:

- \(S_1\) = nilai smoothing pertama
- \(X_1\) = data aktual pertama

Langkah ini dilakukan karena pada periode pertama belum terdapat data sebelumnya.

---

## 6. Menghitung Single Exponential Smoothing

```python
for i in range(1, len(df)):
    df.loc[i, "St"] = (
        alpha * df.loc[i-1, "Harga Beras (Rp) (Y)"]
        + (1 - alpha) * df.loc[i-1, "St"]
    )
```

### Penjelasan

Program menghitung nilai smoothing untuk setiap periode menggunakan rumus Single Exponential Smoothing.

Rumus:

\[
S*t=\alpha X*{t-1}+(1-\alpha)S\_{t-1}
\]

Keterangan:

- \(S_t\) = nilai smoothing periode ke-t
- \(X\_{t-1}\) = data aktual periode sebelumnya
- \(S\_{t-1}\) = hasil smoothing periode sebelumnya
- \(\alpha\) = konstanta smoothing

Setiap nilai smoothing merupakan kombinasi antara data aktual sebelumnya dan hasil smoothing sebelumnya.

---

## 7. Menghitung Error

### Error

```python
df["Xt-St"] = df["Harga Beras (Rp) (Y)"] - df["St"]
```

### Penjelasan

Menghitung selisih antara data aktual dengan hasil smoothing.

Rumus:

\[
Error=X_t-S_t
\]

---

### Absolute Error

```python
df["|Xt-St|"] = df["Xt-St"].abs()
```

### Penjelasan

Mengubah seluruh nilai error menjadi positif.

Rumus:

\[
|X_t-S_t|
\]

---

### Squared Error

```python
df["(Xt-St)^2"] = df["Xt-St"] ** 2
```

### Penjelasan

Menghitung kuadrat dari setiap nilai error.

Rumus:

\[
(X_t-S_t)^2
\]

---

## 8. Menampilkan Hasil Perhitungan

```python
print("\n===== HASIL SINGLE EXPONENTIAL SMOOTHING =====")
display(df.style.hide(axis="index"))
```

### Penjelasan

Program menampilkan tabel hasil perhitungan yang meliputi:

- Data aktual
- Nilai smoothing (St)
- Error
- Absolute Error
- Squared Error

---

## 9. Menyiapkan Data Evaluasi

```python
hasil = df.iloc[1:]
```

### Penjelasan

Baris pertama tidak digunakan dalam evaluasi karena nilai smoothing pertama merupakan nilai awal (inisialisasi), bukan hasil perhitungan metode SES.

---

## 10. Menghitung Mean Absolute Error (MAE)

```python
mae = hasil["|Xt-St|"].mean()
```

### Penjelasan

Menghitung rata-rata kesalahan absolut.

Rumus:

\[
MAE=\frac{\sum |X_t-S_t|}{n}
\]

Semakin kecil nilai MAE maka semakin baik hasil peramalan.

---

## 11. Menghitung Mean Squared Error (MSE)

```python
mse = hasil["(Xt-St)^2"].mean()
```

### Penjelasan

Menghitung rata-rata kuadrat kesalahan.

Rumus:

\[
MSE=\frac{\sum(X_t-S_t)^2}{n}
\]

Nilai error yang besar akan memberikan pengaruh lebih besar terhadap nilai MSE.

Semakin kecil nilai MSE maka model semakin akurat.

---

## 12. Menghitung Mean Absolute Percentage Error (MAPE)

```python
mape = (
    (hasil["|Xt-St|"] / hasil["Harga Beras (Rp) (Y)"])
    .mean()
) * 100
```

### Penjelasan

Menghitung rata-rata persentase kesalahan hasil prediksi.

Rumus:

\[
MAPE=\frac{1}{n}\sum\left(\frac{|X_t-S_t|}{X_t}\right)\times100\%
\]

Interpretasi nilai MAPE:

| Nilai MAPE | Kategori    |
| ---------- | ----------- |
| < 10%      | Sangat Baik |
| 10–20%     | Baik        |
| 20–50%     | Cukup       |
| > 50%      | Kurang Baik |

---

## 13. Menampilkan Nilai Evaluasi

```python
print("\n===== HASIL EVALUASI =====")
print(f"MAE  : {mae:.2f}")
print(f"MSE  : {mse:.2f}")
print(f"MAPE : {mape:.2f}%")
```

### Penjelasan

Program menampilkan hasil evaluasi berupa:

- **MAE**
- **MSE**
- **MAPE**

Ketiga metrik tersebut digunakan untuk mengetahui tingkat akurasi metode Single Exponential Smoothing.

---

## 14. Visualisasi Data

```python
plt.figure(figsize=(10,5))
```

### Penjelasan

Membuat area grafik dengan ukuran **10 × 5 inci**.

---

### Grafik Data Aktual

```python
plt.plot(
    df["Bulan"],
    df["Harga Beras (Rp) (Y)"],
    marker='o',
    linewidth=2,
    label="Data Aktual"
)
```

### Penjelasan

Menampilkan grafik harga beras aktual.

- Marker berbentuk lingkaran (`o`)
- Ketebalan garis 2

---

### Grafik Hasil Single Exponential Smoothing

```python
plt.plot(
    df["Bulan"],
    df["St"],
    marker='s',
    linewidth=2,
    label=f"SES (α = {alpha})"
)
```

### Penjelasan

Menampilkan grafik hasil peramalan menggunakan metode **Single Exponential Smoothing** dengan nilai **α = 0,5**.

- Marker berbentuk persegi (`s`)
- Ketebalan garis 2

---

### Pengaturan Grafik

```python
plt.title("Single Exponential Smoothing Harga Beras")
plt.xlabel("Bulan")
plt.ylabel("Harga Beras (Rp)")
plt.xticks(rotation=45)
plt.grid(True)
plt.legend()
```

### Penjelasan

Bagian ini digunakan untuk:

- Memberikan judul grafik.
- Memberikan nama sumbu X dan sumbu Y.
- Memutar label bulan sebesar **45°** agar mudah dibaca.
- Menampilkan garis bantu (grid).
- Menampilkan legenda grafik.

---

### Menampilkan Grafik

```python
plt.tight_layout()
plt.show()
```

### Penjelasan

- **tight_layout()** digunakan untuk mengatur tata letak grafik agar lebih rapi.
- **show()** digunakan untuk menampilkan grafik hasil peramalan.

---

# Kesimpulan

Program ini menerapkan metode **Single Exponential Smoothing (SES)** untuk melakukan peramalan harga beras dengan menggunakan **nilai alpha (α) sebesar 0,5**. Metode ini memberikan bobot yang lebih besar pada data terbaru dibandingkan data lama melalui proses pemulusan secara berulang. Setelah diperoleh nilai smoothing (**St**), program menghitung tingkat kesalahan menggunakan **MAE**, **MSE**, dan **MAPE** untuk mengevaluasi akurasi model. Selanjutnya, hasil peramalan divisualisasikan dalam bentuk grafik sehingga memudahkan perbandingan antara data aktual dan hasil prediksi menggunakan metode **Single Exponential Smoothing**.
