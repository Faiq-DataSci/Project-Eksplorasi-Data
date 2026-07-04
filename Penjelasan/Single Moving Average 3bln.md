# Penjelasan Program Single Moving Average (SMA) 3 Bulan

## 1. Import Library

```python
import pandas as pd
import matplotlib.pyplot as plt
```

### Penjelasan

Program menggunakan dua library utama:

- **Pandas (`pd`)** digunakan untuk membaca, mengolah, dan menganalisis data berbentuk tabel (DataFrame).
- **Matplotlib (`pyplot`)** digunakan untuk membuat grafik hasil peramalan.

---

## 2. Membaca File Excel

```python
df = pd.read_excel(
    "Data.xlsx",
    sheet_name="Harga_Beras"
)
```

### Penjelasan

Data dibaca dari file **Data.xlsx** pada sheet **Harga_Beras** kemudian disimpan ke dalam variabel **df**.

```python
print("================ DATA AWAL ================")
display(df.style.hide(axis="index"))
```

Kode tersebut menampilkan isi data tanpa menampilkan nomor indeks sehingga tabel lebih rapi.

---

## 3. Menentukan Periode Single Moving Average

```python
periode = 3
```

### Penjelasan

Variabel **periode** menentukan jumlah data sebelumnya yang digunakan untuk menghitung rata-rata bergerak.

Pada program ini digunakan:

- **Periode = 3 bulan**

Artinya nilai prediksi bulan berikutnya dihitung dari rata-rata tiga bulan sebelumnya.

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

Bagian ini menghitung nilai **Single Moving Average (SMA)**.

Fungsi yang digunakan:

- **rolling(window=3)** membuat jendela perhitungan sebanyak 3 data.
- **mean()** menghitung rata-rata dari ketiga data tersebut.
- **shift(1)** menggeser hasil rata-rata satu periode ke bawah agar digunakan sebagai prediksi periode berikutnya.

Contoh:

| Bulan    | Harga  |
| -------- | ------ |
| Januari  | 14.000 |
| Februari | 14.200 |
| Maret    | 14.100 |
| April    | ?      |

Prediksi bulan April:

\[
S_t = \frac{14.000 + 14.200 + 14.100}{3}=14.100
\]

---

## 5. Menghitung Error

### Error

```python
df["Xt-St"] = df["Harga Beras (Rp) (Y)"] - df["St"]
```

Menghitung selisih antara data aktual dengan hasil prediksi.

Rumus:

\[
Error = X_t - S_t
\]

---

### Absolute Error

```python
df["|Xt-St|"] = df["Xt-St"].abs()
```

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

Menghitung kuadrat error.

Rumus:

\[
(X_t-S_t)^2
\]

---

### Absolute Percentage Error (APE)

```python
df["APE (%)"] = (df["|Xt-St|"] / df["Harga Beras (Rp) (Y)"]) * 100
```

Menghitung persentase kesalahan prediksi.

Rumus:

\[
APE=\frac{|X_t-S_t|}{X_t}\times100\%
\]

Semakin kecil nilai APE maka hasil peramalan semakin baik.

---

## 6. Menampilkan Hasil Perhitungan

```python
print("\n===== HASIL SINGLE MOVING AVERAGE 3 BULAN =====")
display(df.style.hide(axis="index"))
```

### Penjelasan

Program menampilkan tabel yang berisi:

- Data aktual
- Nilai hasil SMA
- Error
- Absolute Error
- Squared Error
- APE

---

## 7. Menghitung Nilai Evaluasi

```python
hasil = df.dropna()
```

### Penjelasan

Menghapus baris yang masih memiliki nilai kosong (`NaN`).

Hal ini dilakukan karena tiga data pertama belum memiliki hasil SMA.

---

### Mean Absolute Error (MAE)

```python
mae = hasil["|Xt-St|"].mean()
```

Rumus:

\[
MAE=\frac{\sum |X_t-S_t|}{n}
\]

MAE menunjukkan rata-rata besar kesalahan prediksi.

Semakin kecil MAE maka model semakin baik.

---

### Mean Squared Error (MSE)

```python
mse = hasil["(Xt-St)^2"].mean()
```

Rumus:

\[
MSE=\frac{\sum(X_t-S_t)^2}{n}
\]

MSE memberikan penalti lebih besar terhadap kesalahan yang besar.

Semakin kecil nilai MSE maka model semakin akurat.

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

Interpretasi umum:

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

Program menampilkan tiga ukuran evaluasi yaitu:

- **MAE**
- **MSE**
- **MAPE**

Ketiga nilai tersebut digunakan untuk mengukur tingkat akurasi metode Single Moving Average.

---

## 9. Visualisasi Data

```python
plt.figure(figsize=(10,5))
```

Membuat ukuran grafik sebesar **10 × 5 inci**.

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

Menampilkan garis harga beras aktual.

- Marker lingkaran (`o`)
- Ketebalan garis 2

---

### Grafik Hasil Prediksi

```python
plt.plot(
    df["Bulan"],
    df["St"],
    marker='s',
    linewidth=2,
    label="Single Moving Average"
)
```

Menampilkan garis hasil peramalan menggunakan metode Single Moving Average.

---

### Pengaturan Grafik

```python
plt.title("Single Moving Average Harga Beras")
plt.xlabel("Bulan")
plt.ylabel("Harga Beras (Rp)")
plt.xticks(rotation=45)
plt.grid(True)
plt.legend()
```

Pengaturan ini berfungsi untuk:

- Memberikan judul grafik.
- Memberi nama sumbu X dan Y.
- Memutar label bulan sebesar 45° agar mudah dibaca.
- Menampilkan garis bantu (grid).
- Menampilkan legenda.

---

### Menampilkan Grafik

```python
plt.tight_layout()
plt.show()
```

- **tight_layout()** mengatur jarak antar elemen grafik agar tidak saling bertumpuk.
- **show()** menampilkan grafik ke layar.

---

# Kesimpulan

Program ini menerapkan metode **Single Moving Average (SMA) dengan periode 3 bulan** untuk melakukan peramalan harga beras. Proses dimulai dengan membaca data dari file Excel, kemudian menghitung nilai rata-rata bergerak sebagai hasil prediksi. Selanjutnya dihitung nilai error beserta metrik evaluasi berupa **MAE**, **MSE**, dan **MAPE** untuk mengukur tingkat akurasi model. Terakhir, hasil peramalan divisualisasikan dalam bentuk grafik sehingga memudahkan perbandingan antara data aktual dan hasil prediksi.
