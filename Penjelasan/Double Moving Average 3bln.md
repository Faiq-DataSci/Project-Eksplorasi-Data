# Penjelasan Program Double Moving Average (DMA) 3 Bulan

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

Program menggunakan tiga library utama, yaitu:

- **Pandas (`pd`)** digunakan untuk membaca dan mengolah data dalam bentuk DataFrame.
- **Matplotlib (`pyplot`)** digunakan untuk membuat grafik hasil peramalan.
- **Scikit-learn (`sklearn.metrics`)** digunakan untuk menghitung nilai evaluasi model, yaitu:
  - **Mean Absolute Error (MAE)**
  - **Mean Squared Error (MSE)**
  - **Mean Absolute Percentage Error (MAPE)**

---

## 2. Membaca File Excel

```python
df = pd.read_excel(
    "Data.xlsx",
    sheet_name="Harga_Beras"
)
```

### Penjelasan

Kode di atas membaca data dari file **Data.xlsx** pada sheet **Harga_Beras**, kemudian menyimpannya ke dalam variabel **df**.

```python
print("===== DATA AWAL =====")
display(df)
```

Perintah tersebut digunakan untuk menampilkan seluruh data yang akan digunakan dalam proses perhitungan Double Moving Average.

---

## 3. Menentukan Periode Double Moving Average

```python
periode = 3
```

### Penjelasan

Variabel **periode** menentukan jumlah data historis yang digunakan untuk menghitung rata-rata bergerak.

Pada program ini digunakan:

- **Periode = 3 bulan**

Artinya setiap nilai Moving Average dihitung berdasarkan rata-rata tiga bulan sebelumnya.

---

## 4. Menghitung Moving Average Pertama (M1)

```python
df["M1"] = (
    df["Harga Beras (Rp) (Y)"]
    .rolling(window=periode)
    .mean()
)
```

### Penjelasan

Langkah pertama dalam metode Double Moving Average adalah menghitung **Moving Average pertama (M1)**.

Rumus:

\[
M1*t=\frac{X_t+X*{t-1}+X\_{t-2}}{3}
\]

Keterangan:

- \(X_t\) = data aktual
- \(M1_t\) = Moving Average pertama

Nilai ini merupakan rata-rata dari tiga data terakhir.

---

## 5. Menghitung Moving Average Kedua (M2)

```python
df["M2"] = (
    df["M1"]
    .rolling(window=periode)
    .mean()
)
```

### Penjelasan

Setelah diperoleh nilai **M1**, langkah berikutnya adalah menghitung **Moving Average kedua (M2)** dengan cara merata-ratakan kembali nilai M1.

Rumus:

\[
M2*t=\frac{M1_t+M1*{t-1}+M1\_{t-2}}{3}
\]

Moving Average kedua digunakan untuk mengurangi fluktuasi data sehingga menghasilkan pola yang lebih halus.

---

## 6. Menghitung Nilai a

```python
df["a"] = (2 * df["M1"]) - df["M2"]
```

### Penjelasan

Nilai **a** merupakan estimasi level (nilai dasar) dari data.

Rumus:

\[
a_t=2M1_t-M2_t
\]

Nilai ini menggambarkan posisi rata-rata data setelah dilakukan proses pemulusan dua kali.

---

## 7. Menghitung Nilai b

```python
df["b"] = (2 / (periode - 1)) * (df["M1"] - df["M2"])
```

### Penjelasan

Nilai **b** digunakan untuk memperkirakan arah atau tren perubahan data.

Karena periode yang digunakan adalah **3**, maka rumus menjadi:

\[
b_t=\frac{2}{3-1}(M1_t-M2_t)
\]

atau

\[
b_t=M1_t-M2_t
\]

Nilai ini menunjukkan besarnya perubahan rata-rata dari satu periode ke periode berikutnya.

---

## 8. Menghitung Forecast

```python
df["Forecast"] = df["a"] + df["b"]
```

### Penjelasan

Forecast digunakan untuk memprediksi nilai pada periode berikutnya.

Karena prediksi dilakukan satu periode ke depan (\(m=1\)), maka rumusnya adalah:

\[
F\_{t+1}=a_t+b_t
\]

Hasil forecast merupakan kombinasi antara nilai level (**a**) dan nilai tren (**b**).

---

## 9. Menampilkan Hasil Perhitungan

```python
print("\n===== HASIL DOUBLE MOVING AVERAGE =====")
display(df)
```

### Penjelasan

Program menampilkan seluruh hasil perhitungan yang meliputi:

- Data aktual
- Moving Average pertama (M1)
- Moving Average kedua (M2)
- Nilai a
- Nilai b
- Forecast

---

## 10. Menghapus Data Kosong

```python
hasil = df.dropna()
```

### Penjelasan

Baris yang masih memiliki nilai kosong (**NaN**) dihapus terlebih dahulu.

Hal ini terjadi karena perhitungan Moving Average memerlukan beberapa data awal sehingga nilai M1, M2, maupun Forecast belum dapat dihitung pada beberapa periode pertama.

---

## 11. Menghitung Mean Absolute Error (MAE)

```python
mae = mean_absolute_error(
    hasil["Harga Beras (Rp) (Y)"],
    hasil["Forecast"]
)
```

### Penjelasan

MAE digunakan untuk menghitung rata-rata kesalahan absolut antara data aktual dan hasil prediksi.

Rumus:

\[
MAE=\frac{\sum |X_t-F_t|}{n}
\]

Semakin kecil nilai MAE maka semakin baik tingkat akurasi model.

---

## 12. Menghitung Mean Squared Error (MSE)

```python
mse = mean_squared_error(
    hasil["Harga Beras (Rp) (Y)"],
    hasil["Forecast"]
)
```

### Penjelasan

MSE menghitung rata-rata kuadrat kesalahan prediksi.

Rumus:

\[
MSE=\frac{\sum(X_t-F_t)^2}{n}
\]

Kesalahan yang besar akan memberikan pengaruh lebih besar terhadap nilai MSE.

Semakin kecil nilai MSE menunjukkan hasil peramalan yang semakin baik.

---

## 13. Menghitung Mean Absolute Percentage Error (MAPE)

```python
mape = mean_absolute_percentage_error(
    hasil["Harga Beras (Rp) (Y)"],
    hasil["Forecast"]
) * 100
```

### Penjelasan

MAPE digunakan untuk mengetahui rata-rata persentase kesalahan hasil prediksi.

Rumus:

\[
MAPE=\frac{1}{n}\sum\left|\frac{X_t-F_t}{X_t}\right|\times100\%
\]

Interpretasi nilai MAPE:

| Nilai MAPE | Kategori    |
| ---------- | ----------- |
| < 10%      | Sangat Baik |
| 10–20%     | Baik        |
| 20–50%     | Cukup       |
| > 50%      | Kurang Baik |

---

## 14. Menampilkan Nilai Evaluasi

```python
print("\n===== HASIL EVALUASI =====")
print(f"MAE : {mae:.2f}")
print(f"MSE : {mse:.2f}")
print(f"MAPE : {mape:.2f}%")
```

### Penjelasan

Program menampilkan hasil evaluasi model berupa:

- **MAE**
- **MSE**
- **MAPE**

Ketiga metrik tersebut digunakan untuk mengukur tingkat akurasi metode Double Moving Average.

---

## 15. Visualisasi Data

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

Menampilkan grafik garis yang menggambarkan harga beras aktual.

- Marker berbentuk lingkaran (`o`)
- Ketebalan garis 2

---

### Grafik Forecast

```python
plt.plot(
    df["Bulan"],
    df["Forecast"],
    marker='s',
    linewidth=2,
    label="Forecast DMA"
)
```

### Penjelasan

Menampilkan grafik hasil prediksi menggunakan metode **Double Moving Average**.

- Marker berbentuk persegi (`s`)
- Ketebalan garis 2

---

### Pengaturan Grafik

```python
plt.title("Double Moving Average Harga Beras")
plt.xlabel("Bulan")
plt.ylabel("Harga Beras (Rp)")
plt.xticks(rotation=45)
plt.grid(True)
plt.legend()
```

### Penjelasan

Bagian ini digunakan untuk:

- Memberikan judul grafik.
- Memberikan nama pada sumbu X dan sumbu Y.
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

- **tight_layout()** mengatur tata letak grafik agar lebih rapi.
- **show()** digunakan untuk menampilkan grafik hasil peramalan.

---

# Kesimpulan

Program ini menerapkan metode **Double Moving Average (DMA) dengan periode 3 bulan** untuk melakukan peramalan harga beras. Berbeda dengan Single Moving Average, metode DMA melakukan proses pemulusan sebanyak dua kali melalui perhitungan **Moving Average pertama (M1)** dan **Moving Average kedua (M2)**. Selanjutnya dihitung nilai **a** sebagai estimasi level dan **b** sebagai estimasi tren untuk menghasilkan nilai **Forecast** pada periode berikutnya. Akurasi model dievaluasi menggunakan **MAE**, **MSE**, dan **MAPE**, kemudian hasil peramalan divisualisasikan dalam bentuk grafik sehingga memudahkan analisis perbandingan antara data aktual dan hasil prediksi.
