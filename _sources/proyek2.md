---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: '0.13'
    jupytext_version: '1.11.5'
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# Laporan Project 2

## Pemodelan Prediksi Harga Beras Berdasarkan Tren Historis


## PENDAHULUAN

### Latar Belakang

<p style="text-indent: 50px; text-align: justify;">Beras Rojolele merupakan salah satu varietas padi lokal unggulan yang dihasilkan oleh petani di Jawa Tengah. Dengan visi menjadi perusahaan pengolahan beras terkemuka yang senantiasa memberikan kualitas beras terbaik untuk konsumen. Misi beras Rojolele adalah konsisten menawarkan beras berkualitas tinggi dengan cita rasa pulen dan aroma harum, serta menjaga stabilitas harga yang bertujuan untuk mendukung kesejahteraan petani lokal dan meningkatkan kualitas hidup konsumen.</p>

<p style="text-indent: 50px; text-align: justify;">Seiring dengan perkembangan ekonomi dan perubahan gaya hidup, permintaan terhadap beras Rojolele terus mengalami peningkatan yang stabil. Namun, kenaikan permintaan ini membawa tantangan bagi para produsen dan distributor. Faktor seperti kebijakan pemerintah terkait impor dan ekspor beras dapat menyebabkan fluktuasi harga yang tidak terduga.</p>
  
<p style="text-indent: 50px; text-align: justify;">Untuk mengatasi tantangan ini, peramalan harga beras Rojolele berdasarkan data historis menjadi alat penting dalam menjaga keseimbangan antara penawaran dan permintaan. Peramalan yang akurat dapat membantu produsen dan distributor mengantisipasi perubahan harga di masa depan serta meminimalkan risiko kerugian akibat fluktuasi harga. Dengan demikian, upaya untuk menjaga stabilitas harga beras Rojolele di pasar dapat dilakukan lebih efektif dan mendukung keberlanjutan bisnis sekaligus memenuhi kebutuhan konsumen dengan lebih baik.</p>

### Rumusan Masalah

<p style="text-indent: 50px; text-align: justify;">Dalam upaya menjaga stabilitas harga dan memenuhi permintaan, beras Rojolele menghadapi beberapa masalah utama. Salah satu tantangan terbesar adalah fluktuasi harga yang disebabkan oleh kebijakan pemerintah terkiat impor dan ekspor beras. Oleh karena itu, perlu adanya pendekatan yang lebih efektif dalam peramalan harga agar produsen dan distributor dapat membuat keputusan yang lebih baik dalam menghadapi dinamika pasar yang cepat berubah.</p>

### Tujuan 

<p style="text-indent: 50px; text-align: justify;">Tujuan utama dari analisis ini adalah untuk memprediksi harga beras pada hari berikutnya berdasarkan data historis, sehingga dapat meminimalkan risiko kerugian akibat fluktuasi harga yang tidak terduga.</p>


## METODOLOGI

### Data Understanding 

#### a. Sumber Data 
<p style="text-indent: 50px; text-align: justify;">Data yang digunakan dalam proyek ini merupakan data sekunder yang diperoleh dari website PHIPS Nasional (Pusat Informasi Harga Pangan Strategis Nasional). PHIPS Nasional adalah sebuah platform online yang dikelola oleh Bank Indonesia, yang menyediakan informasi historis mengenai harga pangan di seluruh provinsi di Indonesia. Pemantauan harga PIHPS Nasional telah mencakup empat jenis pasar, yakni pasar tradisional, pasar modern, pedagang besar, dan produsen. Dalam proyek ini, digunakan data historis harga beras dari tahun 2020 hingga 2024, dengan periode harian, yang diambil dari seluruh pasar modern di Jawa Timur.</p>


```{code-cell} python
# import library
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import MinMaxScaler
from sklearn.linear_model import LinearRegression
from sklearn.svm import SVR
from sklearn.neighbors import KNeighborsRegressor
from sklearn.ensemble import BaggingRegressor
from sklearn.metrics import mean_squared_error, r2_score, mean_absolute_percentage_error
import seaborn as sns
import matplotlib.pyplot as plt
```

```{code-cell} python
#Mengambil dan menampilkan data
df = pd.read_csv('https://raw.githubusercontent.com/vaniawrenda/dataset/refs/heads/main/dataset.csv')
pd.options.display.float_format = '{:.0f}'.format
print(df.head())
```

#### b. Deskripsi Data

Dataset ini terdiri dari 2 fitur atau kolom dan 1218 record atau baris. Atribut dalam dataset ini antara lain:
1.  Date: Tanggal harga beras dengan format yyyy-mm-dd
2.	Harga: Berisi harga beras Rojolele dalam satuan rupiah per kilogram.

Melihat ringkasan DataFrame.

```{code-cell} python
df.info()
print('Ukuran data ', df.shape)
```
Berdasarkan hasil output diatas, DataFrame memiliki 1218 baris dengan indeks yang dimulai dari 0. 

```{code-cell} python
df.dtypes
```
<b>Jenis Data</b>
1. Date: Data saat ini disajikan dalam bentuk string, namun akan diubah menjadi tipe data datetime pada tahap eksplorasi untuk memudahkan analisis waktu. 
2. Harga Beras (kg): Merupakan data numerik (Kontinu), karena harga dapat memiliki nilai pecahan dan dapat diukur dengan presisi yang lebih tinggi.

#### c. Eksplorasi Data

<p style="text-indent: 50px; text-align: justify;">Sebelum melakukan eksplorasi data, kolom date akan dikonversi dari
format string menjadi tipe data datetime dan dijadikan sebagai indeks dari DataFrame. Selanjutnya, sebelum 
konversi disini menggunakan metode str.replace(',', '') untuk menghapus tanda koma (,) dari string yang ada, 
karena nilai harga beras biasanya dituliskan dengan tanda koma sebagai pemisah ribuan.</p>

```{code-cell} python
# Merubah kolom 'Date' dalam format datetime dengan dayfirst=True
df['Date'] = pd.to_datetime(df['Date'], dayfirst=True, errors='coerce')

# Mengatur kolom 'Date' sebagai indeks
df.set_index('Date', inplace=True)

# Menghapus tanda koma (pemisah ribuan) dan mengonversi kolom ini menjadi tipe float
df['Harga Beras'] = df['Harga Beras'].str.replace(',', '').astype(float)

# Menampilkan 5 baris pertama untuk memastikan
print(df.head())
```

```{code-cell} python
print(df.plot())
```

```{code-cell} python
# Mencari Missing Value
print(df.isnull().sum())
```

```{code-cell} python
print(df.describe())
```
Memberikan informasi statistik dekskriptif dari kolom numerik. 
1. count: Menghitung jumlah entri yang tidak kosong (valid) dalam kolom.
2. mean: Menghitung rata-rata dari semua nilai dalam kolom.
3. std: Menghitung standar deviasi, yang mengukur seberapa tersebar nilai-nilai dalam kolom dari rata-rata.
4. min: Menunjukkan nilai minimum atau terkecil dalam kolom.
5. 25%: Kuartil pertama, yang berarti 25% dari data memiliki nilai lebih rendah dari atau sama dengan nilai ini.
6. 50% (Median): Kuartil kedua, yang berarti nilai tengah dari data—50% dari data berada di bawah atau di atas nilai ini.
7. 75%: Kuartil ketiga, yang berarti 75% dari data berada di bawah atau sama dengan nilai ini.
8. max: Menunjukkan nilai maksimum atau tertinggi dalam kolom.


### Preprocessing

#### a. Slidding Window

<p style="text-indent: 50px; text-align: justify;">
Melakukan penambahan fitur lag untuk harga beras yang merepresentasikan harga pada hari-hari sebelumnya.
Kolom baru yang ditambahkan adalah harga-1, harga-2, dan harga-3, yang masing-masing menunjukkan 
harga beras satu, dua, dan tiga hari sebelumnya. Nilai-nilai NaN yang muncul akibat pergeseran (lag) 
dihapus menggunakan fungsi dropna, sehingga hanya data yang lengkap yang tersisa untuk analisis.
Setelah itu, kolom-kolom pada DataFrame diatur ulang, dengan kolom Harga Beras (harga saat ini) 
ditempatkan di posisi terakhir. Pengaturan ini bertujuan untuk mempermudah analisis prediktif harga menggunakan 
model berbasis waktu, karena variabel target (harga saat ini) akan berada di bagian akhir tabel.</p>

```{code-cell} python

# Membuat fitur lag untuk harga: harga-1, harga-2, harga-3
df['harga-1'] = df['Harga Beras'].shift(1)  # harga satu hari sebelumnya
df['harga-2'] = df['Harga Beras'].shift(2)  # harga dua hari sebelumnya
df['harga-3'] = df['Harga Beras'].shift(3)  # harga tiga hari sebelumnya

# Menghapus baris yang memiliki nilai NaN (karena shift menghasilkan NaN di awal dan akhir)
df.dropna(inplace=True)

# Menampilkan 5 baris pertama
print(df.head())

```
```{code-cell} python
df.shape
```

Dimensi DataFrame saat ini adalah (1215, 4), yang berarti DataFrame memiliki: 
1. 1215 baris: Setiap baris mewakili satu entri 
2. kolom: Terdapat empat variabel atau fitur yang dicatat untuk setiap baris.


#### b. Normalisasi Data



```{code-cell} python

# Inisialisasi scaler untuk fitur (input) dan target (output)
scaler_features = MinMaxScaler()
scaler_target = MinMaxScaler()

# Normalisasi fitur (harga-1, harga-2, harga-3)
df_features_normalized = pd.DataFrame(scaler_features.fit_transform(df[[  'harga-3', 'harga-2', 'harga-1']]),
                                      columns=[  'harga-3', 'harga-2', 'harga-1'],
                                      index=df.index)

# Normalisasi target (harga)
df_target_normalized = pd.DataFrame(scaler_target.fit_transform(df[['Harga Beras']]),
                                    columns=['Harga Beras'],
                                    index=df.index)

# Gabungkan kembali dataframe yang sudah dinormalisasi
df_normalized = pd.concat([df_features_normalized, df_target_normalized], axis=1)
df_normalized.head()

# Muat scaler dari file .pkl
import joblib

# Simpan scaler_features dan scaler_target ke file .pkl
joblib.dump(scaler_features, 'scaler-features.pkl')
joblib.dump(scaler_target, 'scaler-target.pkl')

```