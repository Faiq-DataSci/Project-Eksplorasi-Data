# Penjelasan Program Analisis Korelasi Ganda (Multiple Correlation)

## 1. Import Library

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

from scipy.stats import pearsonr
from scipy.stats import f
from sklearn.linear_model import LinearRegression
```

### Penjelasan

Program menggunakan beberapa library untuk mendukung proses analisis data, yaitu:

- **Pandas (`pd`)** digunakan untuk membaca, mengelola, dan mengolah data dalam bentuk DataFrame.
- **NumPy (`np`)** digunakan untuk melakukan operasi numerik dan perhitungan matematis.
- **Matplotlib (`pyplot`)** digunakan untuk membuat visualisasi data.
- **SciPy (`pearsonr`)** digunakan untuk menghitung koefisien korelasi Product Moment Pearson.
- **SciPy (`f`)** digunakan untuk memperoleh nilai distribusi F pada uji signifikansi.
- **Scikit-learn (`LinearRegression`)** digunakan untuk membentuk garis regresi pada scatter plot.

---

## 2. Membaca File Excel

```python
file_excel = "Data.xlsx"
sheet = "Data"

df = pd.read_excel(file_excel, sheet_name=sheet)
```

### Penjelasan

Kode di atas membaca data dari file **Data.xlsx** pada sheet **Data**, kemudian menyimpannya ke dalam variabel **df**.

```python
print(df)
```

Perintah tersebut digunakan untuk menampilkan seluruh data yang akan dianalisis.

---

## 3. Mengambil Variabel

```python
Y = df.iloc[:,1]
X1 = df.iloc[:,2]
X2 = df.iloc[:,3]
```

### Penjelasan

Program mengambil tiga variabel penelitian berdasarkan posisi kolom.

- **Y** → Variabel terikat (Dependent Variable)
- **X1** → Variabel bebas pertama (Independent Variable)
- **X2** → Variabel bebas kedua (Independent Variable)

---

## 4. Menghitung Jumlah Data

```python
n = len(df)
```

### Penjelasan

Menghitung jumlah observasi atau sampel yang digunakan dalam penelitian.

Rumus:

\[
n=\text{Jumlah seluruh data}
\]

Nilai ini digunakan pada proses pengujian statistik.

---

## 5. Menghitung Korelasi Product Moment Pearson

```python
r_yx1, p1 = pearsonr(Y, X1)
r_yx2, p2 = pearsonr(Y, X2)
r_x1x2, p3 = pearsonr(X1, X2)
```

### Penjelasan

Program menghitung koefisien korelasi Pearson untuk tiga pasangan variabel.

- Korelasi antara **Y dan X1**
- Korelasi antara **Y dan X2**
- Korelasi antara **X1 dan X2**

Rumus Korelasi Pearson:

\[
r=
\frac{
n\sum XY-\sum X\sum Y
}{
\sqrt{
(n\sum X^2-(\sum X)^2)
(n\sum Y^2-(\sum Y)^2)
}
}
\]

Nilai korelasi berada pada rentang:

\[
-1 \le r \le 1
\]

Interpretasi:

- Mendekati **1** → hubungan positif sangat kuat.
- Mendekati **0** → hubungan sangat lemah atau tidak ada hubungan.
- Mendekati **−1** → hubungan negatif sangat kuat.

---

## 6. Menghitung Korelasi Ganda (Multiple Correlation)

```python
R = np.sqrt(
    (
        (r_yx1**2)
        +(r_yx2**2)
        -2*r_yx1*r_yx2*r_x1x2
    )
    /
    (1-r_x1x2**2)
)
```

### Penjelasan

Program menghitung **koefisien korelasi ganda (R)** untuk mengetahui kekuatan hubungan variabel **X1** dan **X2** secara bersama-sama terhadap variabel **Y**.

Rumus:

\[
R=
\sqrt{
\frac{
r*{yx1}^{2}
+r*{yx2}^{2}
-2r*{yx1}r*{yx2}r*{x1x2}
}
{
1-r*{x1x2}^{2}
}
}
\]

Keterangan:

- \(r\_{yx1}\) = korelasi Y dengan X1.
- \(r\_{yx2}\) = korelasi Y dengan X2.
- \(r\_{x1x2}\) = korelasi antara X1 dan X2.

Semakin besar nilai **R**, semakin kuat hubungan kedua variabel bebas terhadap variabel terikat.

---

## 7. Menentukan Parameter Uji F

```python
k = 2
alpha = 0.05
```

### Penjelasan

Menentukan parameter pengujian statistik.

- **k = 2** → jumlah variabel bebas.
- **α = 0,05** → taraf signifikansi 5%.

---

## 8. Menghitung Nilai F Hitung

```python
F_hitung = (R**2/k)/((1-R**2)/(n-k-1))
```

### Penjelasan

Menghitung statistik **F Hitung** untuk menguji signifikansi korelasi ganda.

Rumus:

\[
F=
\frac{R^{2}/k}
{(1-R^{2})/(n-k-1)}
\]

Semakin besar nilai **F Hitung**, semakin besar kemungkinan hubungan antar variabel bersifat signifikan.

---

## 9. Menghitung Derajat Bebas

```python
df1 = k
df2 = n-k-1
```

### Penjelasan

Derajat bebas digunakan untuk menentukan nilai **F Tabel**.

Rumus:

\[
df_1=k
\]

\[
df_2=n-k-1
\]

---

## 10. Menghitung Nilai F Tabel

```python
F_tabel = f.ppf(1-alpha, df1, df2)
```

### Penjelasan

Mengambil nilai **F Tabel** berdasarkan:

- Taraf signifikansi.
- Derajat bebas pembilang.
- Derajat bebas penyebut.

Nilai ini digunakan sebagai pembanding dengan **F Hitung**.

---

## 11. Menentukan Tingkat Hubungan

```python
def hubungan(r):
```

### Penjelasan

Fungsi ini mengelompokkan tingkat hubungan berdasarkan nilai korelasi.

| Nilai Korelasi | Interpretasi  |
| -------------- | ------------- |
| 0,00 – 0,19    | Sangat Rendah |
| 0,20 – 0,39    | Rendah        |
| 0,40 – 0,59    | Sedang        |
| 0,60 – 0,79    | Kuat          |
| 0,80 – 1,00    | Sangat Kuat   |

---

## 12. Menentukan Keputusan Hipotesis

```python
if F_hitung > F_tabel:
```

### Penjelasan

Kriteria pengujian:

Jika

\[
F*{hitung}>F*{tabel}
\]

maka:

- **H₀ ditolak**
- Hubungan antar variabel signifikan.

Sebaliknya,

\[
F*{hitung}\le F*{tabel}
\]

maka:

- **H₀ diterima**
- Hubungan antar variabel tidak signifikan.

---

## 13. Membuat Tabel Hasil

```python
hasil = pd.DataFrame(...)
```

### Penjelasan

Program menyusun hasil analisis ke dalam sebuah DataFrame yang berisi:

- Jumlah data.
- Korelasi Y terhadap X1.
- Korelasi Y terhadap X2.
- Korelasi X1 terhadap X2.
- Nilai R.
- Nilai F Hitung.
- Nilai F Tabel.
- Interpretasi hubungan.
- Keputusan uji hipotesis.

---

## 14. Menyimpan Hasil ke Excel

```python
with pd.ExcelWriter("Hasil Korelasi Ganda.xlsx") as writer:
```

### Penjelasan

Program menyimpan hasil analisis ke dalam file **Hasil Korelasi Ganda.xlsx**.

Terdapat dua sheet:

- **Data** → data penelitian.
- **Perhitungan** → hasil analisis korelasi.

---

## 15. Visualisasi Heatmap Korelasi

```python
corr = df.iloc[:,1:].corr()
```

### Penjelasan

Menghitung matriks korelasi antar variabel kemudian menampilkannya dalam bentuk **heatmap**.

Heatmap memudahkan identifikasi kekuatan hubungan antar variabel.

- Warna merah menunjukkan korelasi positif yang lebih kuat.
- Warna biru menunjukkan korelasi negatif yang lebih kuat.

---

## 16. Scatter Plot Y terhadap X1

```python
plt.scatter(X1,Y)
```

### Penjelasan

Menampilkan hubungan antara variabel **X1** dan **Y**.

Program juga membuat garis regresi menggunakan:

```python
LinearRegression()
```

Garis merah menunjukkan kecenderungan hubungan antara kedua variabel.

---

## 17. Scatter Plot Y terhadap X2

```python
plt.scatter(X2,Y)
```

### Penjelasan

Menampilkan hubungan antara variabel **X2** dan **Y**.

Garis regresi digunakan untuk memperlihatkan pola hubungan kedua variabel.

---

## 18. Diagram Batang (Bar Chart)

```python
nama = ["r(Y,X1)","r(Y,X2)","R"]
```

### Penjelasan

Diagram batang digunakan untuk membandingkan nilai:

- Korelasi Y dengan X1.
- Korelasi Y dengan X2.
- Korelasi ganda (R).

Nilai korelasi juga ditampilkan di atas masing-masing batang agar lebih mudah dibaca.

---

## 19. Menampilkan Hasil Akhir

```python
print(f"Jumlah Data (n) : {n}")
```

### Penjelasan

Program menampilkan ringkasan hasil analisis, meliputi:

- Jumlah data.
- Korelasi Y dengan X1.
- Korelasi Y dengan X2.
- Korelasi X1 dengan X2.
- Nilai R.
- Nilai F Hitung.
- Nilai F Tabel.
- Tingkat hubungan.
- Keputusan hipotesis.

---

# Kesimpulan

Program ini digunakan untuk melakukan **analisis korelasi ganda (Multiple Correlation)** antara satu variabel terikat (**Y**) dan dua variabel bebas (**X1** dan **X2**). Analisis dimulai dengan menghitung koefisien korelasi Pearson pada setiap pasangan variabel, kemudian menghitung **koefisien korelasi ganda (R)** untuk mengetahui kekuatan hubungan kedua variabel bebas secara simultan terhadap variabel terikat. Selanjutnya dilakukan **uji F** untuk menguji signifikansi hubungan tersebut. Hasil analisis disajikan dalam bentuk tabel, disimpan ke file Excel, dan divisualisasikan melalui **heatmap korelasi**, **scatter plot** dengan garis regresi, serta **diagram batang**, sehingga memudahkan interpretasi hubungan antar variabel dan pengambilan keputusan berdasarkan hasil pengujian statistik.
