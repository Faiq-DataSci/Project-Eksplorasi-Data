# Penjelasan Program Double Exponential Smoothing (DES) Brown

## 1. Import Library

```python
import pandas as pd
import matplotlib.pyplot as plt
```

### Penjelasan

Program menggunakan dua library utama, yaitu:

- **Pandas (`pd`)** digunakan untuk membaca, mengolah, dan menganalisis data dalam bentuk DataFrame.
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

Kode di atas membaca data dari file **Data.xlsx** pada sheet **Harga_Beras**, kemudian menyimpannya ke dalam variabel **df**.

```python
print("================ DATA AWAL ================")
display(df.style.hide(axis="index"))
```

Perintah tersebut digunakan untuk menampilkan data awal tanpa menampilkan nomor indeks sehingga tampilan tabel menjadi lebih rapi.

---

## 3. Menentukan Parameter Alpha (α)

```python
alpha = 0.2
```

### Penjelasan

Nilai **alpha (α)** merupakan konstanta pemulusan (_smoothing constant_) pada metode **Double Exponential Smoothing Brown**.

Pada program ini digunakan:

\[
\alpha = 0,2
\]

Nilai alpha berada pada rentang:

\[
0 < \alpha < 1
\]

Interpretasi nilai alpha:

- **Alpha kecil (mendekati 0)** → hasil peramalan lebih stabil karena lebih dipengaruhi data lama.
- **Alpha besar (mendekati 1)** → hasil peramalan lebih responsif terhadap perubahan data terbaru.

Karena menggunakan **α = 0,2**, model menghasilkan pemulusan yang lebih halus.

---

## 4. Membuat Kolom Perhitungan

```python
df["S't"] = 0.0
df["S''t"] = 0.0
df["a"] = 0.0
df["b"] = 0.0
df["Forecast"] = None
```

### Penjelasan

Program membuat beberapa kolom baru yang digunakan selama proses perhitungan, yaitu:

- **S't** → hasil pemulusan pertama (_Single Smoothing_)
- **S''t** → hasil pemulusan kedua (_Double Smoothing_)
- **a** → komponen level
- **b** → komponen tren
- **Forecast** → hasil peramalan

---

## 5. Menentukan Nilai Awal

```python
df.loc[0, "S't"] = df.loc[0, "Harga Beras (Rp) (Y)"]
df.loc[0, "S''t"] = df.loc[0, "Harga Beras (Rp) (Y)"]
```

### Penjelasan

Nilai awal untuk proses pemulusan diambil dari data aktual pertama.

Rumus:

\[
S'\_1 = X_1
\]

\[
S''\_1 = X_1
\]

Keterangan:

- \(X_1\) = data aktual pertama.

Langkah ini diperlukan karena pada periode pertama belum tersedia data sebelumnya.

---

## 6. Menghitung Single Smoothing

```python
df.loc[i, "S't"] = (
    alpha * df.loc[i, "Harga Beras (Rp) (Y)"] +
    (1-alpha) * df.loc[i-1, "S't"]
)
```

### Penjelasan

Tahap pertama adalah menghitung nilai **Single Exponential Smoothing**.

Rumus:

\[
S'_t=\alpha X_t+(1-\alpha)S'_{t-1}
\]

Keterangan:

- \(S'\_t\) = hasil pemulusan pertama
- \(X_t\) = data aktual
- \(S'\_{t-1}\) = hasil smoothing sebelumnya

---

## 7. Menghitung Double Smoothing

```python
df.loc[i, "S''t"] = (
    alpha * df.loc[i, "S't"] +
    (1-alpha) * df.loc[i-1, "S''t"]
)
```

### Penjelasan

Tahap kedua adalah melakukan pemulusan kembali terhadap hasil smoothing pertama.

Rumus:

\[
S''_t=\alpha S'\_t+(1-\alpha)S''_{t-1}
\]

Hasil pemulusan kedua digunakan untuk memperoleh estimasi level dan tren data.

---

## 8. Menghitung Nilai a (Level)

```python
df.loc[i, "a"] = (
    2 * df.loc[i, "S't"] -
    df.loc[i, "S''t"]
)
```

### Penjelasan

Nilai **a** menunjukkan estimasi level data.

Rumus:

\[
a_t=2S'\_t-S''\_t
\]

Komponen ini menggambarkan posisi rata-rata data setelah proses pemulusan.

---

## 9. Menghitung Nilai b (Trend)

```python
df.loc[i, "b"] = (
    alpha / (1-alpha)
) * (
    df.loc[i, "S't"] -
    df.loc[i, "S''t"]
)
```

### Penjelasan

Nilai **b** menunjukkan estimasi tren data.

Rumus:

\[
b_t=\frac{\alpha}{1-\alpha}(S'\_t-S''\_t)
\]

Komponen ini menunjukkan arah perubahan data dari waktu ke waktu.

---

## 10. Menghitung Forecast

```python
df.loc[i, "Forecast"] = (
    df.loc[i, "a"] +
    df.loc[i, "b"]
)
```

### Penjelasan

Forecast digunakan untuk memprediksi nilai satu periode ke depan.

Karena dilakukan prediksi satu langkah ke depan (\(m=1\)), maka rumusnya:

\[
F\_{t+1}=a_t+b_t
\]

Hasil forecast merupakan gabungan dari komponen level dan tren.

---

## 11. Menghitung Error

### Error

```python
df["Xt-Ft"] = (
    df["Harga Beras (Rp) (Y)"] -
    df["Forecast"]
)
```

### Penjelasan

Menghitung selisih antara data aktual dan hasil peramalan.

Rumus:

\[
Error=X_t-F_t
\]

---

### Absolute Error

```python
df["|Xt-Ft|"] = df["Xt-Ft"].abs()
```

### Penjelasan

Mengubah seluruh nilai error menjadi positif.

Rumus:

\[
|X_t-F_t|
\]

---

### Squared Error

```python
df["(Xt-Ft)^2"] = df["Xt-Ft"]**2
```

### Penjelasan

Menghitung kuadrat dari setiap nilai error.

Rumus:

\[
(X_t-F_t)^2
\]

---

### Absolute Percentage Error (APE)

```python
df["APE (%)"] = (
    df["|Xt-Ft|"] /
    df["Harga Beras (Rp) (Y)"]
) * 100
```

### Penjelasan

Menghitung persentase kesalahan hasil peramalan.

Rumus:

\[
APE=\frac{|X_t-F_t|}{X_t}\times100\%
\]

Semakin kecil nilai APE maka semakin baik hasil prediksi.

---

## 12. Menampilkan Hasil Perhitungan

```python
print("\n===== HASIL DOUBLE EXPONENTIAL SMOOTHING =====")
display(df.style.hide(axis="index"))
```

### Penjelasan

Program menampilkan tabel hasil perhitungan yang meliputi:

- Data aktual
- Single Smoothing
- Double Smoothing
- Nilai level (a)
- Nilai tren (b)
- Forecast
- Error
- Absolute Error
- Squared Error
- APE

---

## 13. Menghitung Nilai Evaluasi

```python
hasil = df.dropna()
```

### Penjelasan

Baris yang masih memiliki nilai kosong (**NaN**) dihapus sebelum proses evaluasi agar hanya data yang memiliki hasil forecast yang dihitung.

---

### Mean Absolute Error (MAE)

```python
mae = hasil["|Xt-Ft|"].mean()
```

Rumus:

\[
MAE=\frac{\sum|X_t-F_t|}{n}
\]

Semakin kecil nilai MAE maka semakin baik tingkat akurasi model.

---

### Mean Squared Error (MSE)

```python
mse = hasil["(Xt-Ft)^2"].mean()
```

Rumus:

\[
MSE=\frac{\sum(X_t-F_t)^2}{n}
\]

MSE memberikan penalti lebih besar terhadap kesalahan yang besar.

---

### Mean Absolute Percentage Error (MAPE)

```python
mape = hasil["APE (%)"].mean()
```

Rumus:

\[
MAPE=\frac{\sum APE}{n}
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
print(f"MAE  : {mae:.2f}")
print(f"MSE  : {mse:.2f}")
print(f"MAPE : {mape:.2f}%")
```

### Penjelasan

Program menampilkan tiga ukuran evaluasi model, yaitu:

- **MAE (Mean Absolute Error)**
- **MSE (Mean Squared Error)**
- **MAPE (Mean Absolute Percentage Error)**

Ketiga metrik tersebut digunakan untuk menilai tingkat akurasi metode Double Exponential Smoothing.

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

Menampilkan grafik harga beras aktual.

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
    label=f"DES Brown (α={alpha})"
)
```

### Penjelasan

Menampilkan grafik hasil peramalan menggunakan metode **Double Exponential Smoothing Brown** dengan **α = 0,2**.

- Marker berbentuk persegi (`s`)
- Ketebalan garis 2

---

### Pengaturan Grafik

```python
plt.title("Double Exponential Smoothing Harga Beras")
plt.xlabel("Bulan")
plt.ylabel("Harga Beras (Rp)")
plt.xticks(rotation=45)
plt.grid(True)
plt.legend()
```

### Penjelasan

Bagian ini digunakan untuk:

- Memberikan judul grafik.
- Memberikan nama sumbu X dan Y.
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

Program ini menerapkan metode **Double Exponential Smoothing (DES) Brown** dengan **nilai alpha (α) sebesar 0,2** untuk melakukan peramalan harga beras. Metode ini melakukan proses pemulusan sebanyak dua kali, yaitu **Single Smoothing (S't)** dan **Double Smoothing (S''t)**, sehingga mampu mengakomodasi pola data yang memiliki kecenderungan tren. Selanjutnya dihitung komponen **level (a)** dan **tren (b)** untuk menghasilkan nilai **Forecast** satu periode ke depan. Akurasi hasil peramalan dievaluasi menggunakan **MAE**, **MSE**, dan **MAPE**, kemudian divisualisasikan dalam bentuk grafik sehingga memudahkan perbandingan antara data aktual dan hasil prediksi menggunakan metode **Double Exponential Smoothing Brown**.
