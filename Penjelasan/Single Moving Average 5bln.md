# Penjelasan Program Single Moving Average (SMA) 5 Bulan

## 1. Import Library

```python
import pandas as pd
import matplotlib.pyplot as plt
```

### Penjelasan

Program menggunakan dua library utama, yaitu:

- **Pandas (`pd`)** digunakan untuk membaca, mengelola, dan mengolah data dalam bentuk tabel (DataFrame).
- **Matplotlib (`pyplot`)** digunakan untuk membuat grafik perbandingan antara data aktual dan hasil peramalan.

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
print("================ DATA AWAL ================")
display(df.style.hide(axis="index"))
```

Perintah tersebut digunakan untuk menampilkan data awal tanpa menampilkan nomor indeks sehingga tabel terlihat lebih rapi.

---

## 3. Menentukan Periode Single Moving Average

```python
periode = 5
```

### Penjelasan

Variabel **periode** menentukan jumlah data historis yang digunakan untuk menghitung rata-rata bergerak.

Pada program ini digunakan:

- **Periode = 5 bulan**

Artinya, prediksi harga pada suatu bulan dihitung berdasarkan rata-rata harga dari **lima bulan sebelumnya**.

---

## 4. Menghitung Single Moving Average

```python
df["St"] = (
    df["Harga Beras (Rp) (Y)"]
    .rolling(window=periode)
    .mean()
    .shift(1)
)
```

### Penjelasan

Kode ini menghitung nilai **Single Moving Average (SMA)**.

Fungsi yang digunakan meliputi:

- **rolling(window=5)** membuat jendela perhitungan sebanyak lima data.
- **mean()** menghitung rata-rata dari lima data tersebut.
- **shift(1)** menggeser hasil rata-rata satu periode sehingga digunakan sebagai prediksi pada periode berikutnya.

Contoh:

| Bulan    | Harga  |
| -------- | ------ |
| Januari  | 14.000 |
| Februari | 14.200 |
| Maret    | 14.100 |
| April    | 14.300 |
| Mei      | 14.400 |
| Juni     | ?      |

Prediksi bulan Juni:

\[
S_t=\frac{14.000+14.200+14.100+14.300+14.400}{5}=14.200
\]

Nilai tersebut menjadi hasil prediksi untuk bulan Juni.

---

## 5. Menghitung Error

### Error

```python
df["Xt-St"] = df["Harga Beras (Rp) (Y)"] - df["St"]
```

### Penjelasan

Menghitung selisih antara nilai aktual dengan hasil prediksi.

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

### Absolute Percentage Error (APE)

```python
df["APE (%)"] = (df["|Xt-St|"] / df["Harga Beras (Rp) (Y)"]) * 100
```

### Penjelasan

Menghitung persentase kesalahan hasil prediksi terhadap data aktual.

Rumus:

\[
APE=\frac{|X_t-S_t|}{X_t}\times100\%
\]

Semakin kecil nilai APE maka semakin baik hasil peramalannya.

---

## 6. Menampilkan Hasil Perhitungan

```python
print("\n===== HASIL SINGLE MOVING AVERAGE 5 BULAN =====")
display(df.style.hide(axis="index"))
```

### Penjelasan

Program menampilkan tabel hasil perhitungan yang berisi:

- Data aktual
- Nilai Single Moving Average (St)
- Error
- Absolute Error
- Squared Error
- Absolute Percentage Error (APE)

---

## 7. Menghitung Nilai Evaluasi

```python
hasil = df.dropna()
```

### Penjelasan

Baris yang masih memiliki nilai kosong (**NaN**) dihapus terlebih dahulu.

Hal ini dilakukan karena lima data pertama belum dapat dihitung nilai SMA akibat belum tersedia lima data historis sebelumnya.

---

### Mean Absolute Error (MAE)

```python
mae = hasil["|Xt-St|"].mean()
```

Rumus:

\[
MAE=\frac{\sum|X_t-S_t|}{n}
\]

MAE menunjukkan rata-rata besarnya kesalahan prediksi.

Semakin kecil nilai MAE, maka semakin baik tingkat akurasi model.

---

### Mean Squared Error (MSE)

```python
mse = hasil["(Xt-St)^2"].mean()
```

Rumus:

\[
MSE=\frac{\sum(X_t-S_t)^2}{n}
\]

MSE memberikan penalti yang lebih besar terhadap kesalahan yang bernilai besar.

Semakin kecil nilai MSE menunjukkan hasil peramalan yang semakin baik.

---

### Mean Absolute Percentage Error (MAPE)

```python
mape = hasil["APE (%)"].mean()
```

Rumus:

\[
MAPE=\frac{\sum APE}{n}
\]

MAPE menunjukkan rata-rata persentase kesalahan prediksi.

Interpretasi umum nilai MAPE:

| Nilai MAPE | Kategori    |
| ---------- | ----------- |
| < 10%      | Sangat Baik |
| 10–20%     | Baik        |
| 20–50%     | Cukup       |
| > 50%      | Kurang Baik |

---

## 8. Menampilkan Nilai Evaluasi

```python
print("\n===== HASIL EVALUASI =====")
print(f"MAE  : {mae:.2f}")
print(f"MSE  : {mse:.2f}")
print(f"MAPE : {mape:.2f}%")
```

### Penjelasan

Program menampilkan hasil evaluasi model berupa:

- **MAE (Mean Absolute Error)**
- **MSE (Mean Squared Error)**
- **MAPE (Mean Absolute Percentage Error)**

Ketiga metrik tersebut digunakan untuk menilai tingkat akurasi metode Single Moving Average periode 5 bulan.

---

## 9. Visualisasi Data

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

Menampilkan grafik garis yang menggambarkan data harga beras aktual.

- Marker berbentuk lingkaran (`o`)
- Ketebalan garis 2

---

### Grafik Hasil Peramalan

```python
plt.plot(
    df["Bulan"],
    df["St"],
    marker='s',
    linewidth=2,
    label="SMA 5 Bulan"
)
```

### Penjelasan

Menampilkan grafik hasil prediksi menggunakan metode **Single Moving Average periode 5 bulan**.

- Marker berbentuk persegi (`s`)
- Ketebalan garis 2

---

### Pengaturan Grafik

```python
plt.title("Single Moving Average 5 Bulan Harga Beras")
plt.xlabel("Bulan")
plt.ylabel("Harga Beras (Rp)")
plt.xticks(rotation=45)
plt.grid(True)
plt.legend()
```

### Penjelasan

Bagian ini berfungsi untuk:

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

- **tight_layout()** digunakan agar seluruh elemen grafik tersusun rapi dan tidak saling bertumpuk.
- **show()** digunakan untuk menampilkan grafik ke layar.

---

# Kesimpulan

Program ini menerapkan metode **Single Moving Average (SMA) dengan periode 5 bulan** untuk melakukan peramalan harga beras. Peramalan dilakukan dengan menghitung rata-rata harga dari lima bulan sebelumnya sebagai prediksi untuk bulan berikutnya. Setelah nilai prediksi diperoleh, program menghitung nilai error beserta metrik evaluasi berupa **MAE**, **MSE**, dan **MAPE** untuk mengukur tingkat akurasi model. Selanjutnya, hasil peramalan divisualisasikan dalam bentuk grafik sehingga memudahkan pengguna dalam membandingkan data aktual dengan hasil prediksi menggunakan metode **Single Moving Average 5 Bulan**.
