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

# Laporan Project 1


## Pendahuluan 

### Latar Belakang

<p style="text-indent: 50px; text-align: justify;">Ethereum (ETH) adalah mata uang kripto terdesentralisasi yang memanfaatkan teknologi blockchain untuk menjalankan kontrak pintar dan aplikasi terdesentralisasi (dApps). Ethereum terkenal karena kemampuannya mendukung pengembangan platform DeFi dan NFT, yang membuatnya populer di kalangan investor dan pengemban.</p>

<p style="text-indent: 50px; text-align: justify;">Namun, harga Ethereum, seperti halnya cryptocurrency lainnya, sangat fluktuatif, dipengaruhi oleh faktor seperti perkembangan teknologi, adopsi pasar, kebijakan pemerintah, dan sentimen pasar global. Fluktuasi harga ini sering kali membuat investor kesulitan dalam membuat keputusan investasi yang tepat.</p>
  
<p style="text-indent: 50px; text-align: justify;">Untuk membantu investor mengatasi ketidakpastian harga Ethereum, teknologi prediksi dapat digunakan untuk memperkirakan pergerakan harga di masa depan. Dengan menganalisis data historis, pendekatan ini dapat mengurangi risiko dan mendukung pengambilan keputusan investasi yang lebih tepat.</p>

### Rumusan Masalah

<p style="text-indent: 50px; text-align: justify;">Ethereum menghadapi fluktuasi harga yang dipengaruhi oleh berbagai faktor eksternal, seperti sentimen pasar dan kebijakan ekonomi global. Oleh karena itu, diperlukan pendekatan yang lebih efektif untuk memprediksi pergerakan harga Ethereum agar investor dapat mengantisipasi perubahan harga yang cepat dan membuat keputusan yang lebih baik.</p>

### Tujuan 

<p style="text-indent: 50px; text-align: justify;">Tujuan utama dari proyek ini adalah untuk memprediksi harga Ethereum di masa depan berdasarkan data historis harga, sehingga dapat membantu investor mengurangi risiko dan membuat keputusan investasi yang lebih informasional dalam pasar yang sangat volatil</p>



## Metodologi

### Data Understanding 

#### a. Sumber Data 
<p style="text-indent: 50px; text-align: justify;">Data yang digunakan dalam proyek ini diperoleh dari platform Yahoo Finance, yang dapat diakses di https://finance.yahoo.com/quote/ETH-USD/. Platform ini menawarkan informasi tentang harga Ethereum (ETH) terhadap dolar AS (USD) dalam berbagai periode waktu, termasuk harga penutupan, perubahan harga harian, dan fitur analisis pasar lainnya. Untuk proyek ini, digunakan data harga Ethereum dalam format CSV, dengan rentang waktu dari 10 April 2020 hingga 5 Desember 2024.</p>

```{code-cell} python
# import library
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import MinMaxScaler
from sklearn.linear_model import LinearRegression, Ridge
from sklearn.tree import DecisionTreeRegressor
from sklearn.metrics import mean_squared_error, r2_score, mean_absolute_percentage_error
import seaborn as sns
import matplotlib.pyplot as plt
```

```{code-cell} python
# Membaca data CSV
df = pd.read_csv('https://raw.githubusercontent.com/vaniawrenda/dataset/refs/heads/main/etherium.csv')
pd.options.display.float_format = '{:.0f}'.format
print(df.head())
```
<p style="text-indent: 50px; text-align: justify;">
Selanjutnya, untuk memastikan kolom Date diproses dengan benar, kita mengonversinya ke format datetime. Hal ini memungkinkan perbandingan dan analisis berdasarkan waktu yang lebih akurat. Selanjutnya, menjadikan Date sebagai indeks akan mempermudah pencarian data berdasarkan tanggal, sementara penyortiran data memastikan urutannya sesuai dengan waktu yang benar. </p>

```{code-cell} python
# mengubah kolom 'Date' dalam format datetime
df['Date'] = pd.to_datetime(df['Date'])

# Mengatur kolom 'Date' sebagai indeks
df.set_index('Date', inplace=True)

# Mensortir data berdasarkan kolom Date dari terkecil ke terbesar
df = df.sort_values(by='Date')
df.head()
```

#### b. Deskripsi Data
Dataset ini memiliki 6 fitur atau kolom dan terdiri dari 1802 baris data. Berikut adalah penjelasan masing-masing atribut:

- Date: Tanggal yang mencatat harga aset koin (format YYYY-MM-DD)
- Open: Harga pembukaan aset koin pada tanggal tersebut
- High: Harga tertinggi yang tercatat pada tanggal tersebut
- Low: Harga terendah yang tercatat pada tanggal tersebut
- Close: Harga penutupan aset koin pada tanggal tersebut
- Adj Close: Harga penutupan yang telah disesuaikan dengan pembagian aset, dividen, dan aksi korporasi lainnya
- Volume: Jumlah transaksi aset koin yang terjadi pada tanggal tersebut


Melihat ringkasan DataFrame.

```{code-cell} python
df.info()
print('Ukuran data ', df.shape)
```
Berdasarkan hasil output diatas, DataFrame memiliki 1802 baris dengan indeks yang dimulai dari 0. 

```{code-cell} python
df.dtypes
```
<b>Jenis Data</b>
Jenis Data

- Open: Merupakan data numerik dengan tipe data float64, karena harga pembukaan aset koin dapat memiliki nilai pecahan dan bersifat kontinu.
- High: Merupakan data numerik dengan tipe data float64, karena harga tertinggi yang dicapai dapat berupa nilai pecahan dan bersifat kontinu.
- Low: Merupakan data numerik dengan tipe data float64, karena harga terendah yang tercatat dapat berupa nilai pecahan dan bersifat kontinu.
- Close: Merupakan data numerik dengan tipe data float64, karena harga penutupan aset koin dapat memiliki nilai pecahan dan bersifat kontinu.
- Adj Close: Merupakan data numerik dengan tipe data float64, karena harga penutupan yang disesuaikan dapat berupa nilai pecahan dan bersifat kontinu.
- Volume: Merupakan data numerik dengan tipe data int64, karena jumlah aset koin yang diperdagangkan adalah bilangan bulat dan dapat dihitung secara diskrit.

#### C. Eksplorasi Data

<p style="text-indent: 50px; text-align: justify;">Sebelum melakukan eksplorasi data, mencari missing value</p>

```{code-cell} python
df.isnull().sum()
df
```

<p style="text-indent: 50px; text-align: justify;"> Selanjutnya, Pastikan bahwa kolom Harga Beras memiliki format yang tepat untuk analisis lebih lanjut. Dalam langkah ini, menggunakan fungsi pd.to_numeric() untuk mengonversi nilai dalam kolom tersebut menjadi tipe data float, yang memungkinkan untuk melakukan perhitungan matematis yang lebih akurat. Sebelum konversi, disini juga menggunakan metode str.replace(',', '') untuk menghapus tanda koma (,) dari string yang ada, karena nilai harga beras biasanya dituliskan dengan tanda koma sebagai pemisah ribuan.</p>

```{code-cell} python
#Mengonversi kolom Harga Beras dalam dataframe mnenjadi data float
df['Harga Beras'] = pd.to_numeric(df['Harga Beras'].str.replace(',', ''), errors='coerce')
print(df.head())
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

<p style="text-indent: 50px; text-align: justify;">Selanjutnya  melakukan rekayasa fitur, dimana membuat variabel baru yang merepresentasikan harga beras pada lima langkah waktu sebelumnya. Dengan menggunakan metode shift(), dapat menghasilkan kolom-kolom baru yang menyimpan nilai harga beras yang telah bergeser ke depan dalam waktu, sehingga model dapat menganalisis pola dan tren harga dari waktu ke waktu.</p>

```{code-cell} python
df['xt-5'] = df['Harga Beras'].shift(-5)
df['xt-4'] = df['Harga Beras'].shift(-4)
df['xt-3'] = df['Harga Beras'].shift(-3)
df['xt-2'] = df['Harga Beras'].shift(-2)
df['xt-1'] = df['Harga Beras'].shift(-1)
df['xt'] = df['Harga Beras']

df = df.dropna()
df = df.drop(columns=['Harga Beras'])
df.head()
```
<p style="text-indent: 50px; text-align: justify;">Visualisasi ini dibuat untuk menunjukkan perubahan harga beras dari waktu ke waktu, termasuk harga beras pada hari ke-5, ke-4, ke-3, ke-2, dan ke-1, serta harga saat ini. Dengan menggunakan grafik garis, kita dapat dengan jelas mengamati tren dan pola harga tersebut, sehingga membantu kita memahami bagaimana harga saat ini dipengaruhi oleh harga-harga di hari-hari sebelumnya.</p>

```{code-cell} python
df.plot()
```
<p style="text-indent: 50px; text-align: justify;">Selanjutnya melihat korelasi antara kolom satu dengan kolom lainnya.</p>

```{code-cell} python
import numpy as np
import pandas as pd
# Menghitung korelasi Pearson antara fitur dan target
correlations = {}
for col in df.columns[:-1]:
    correlation = np.corrcoef(df[col], df['xt'])[0, 1]
    correlations[col] = correlation

# Menampilkan hasil korelasi
for fitur, bobot_korelasi in correlations.items():
    print(f"Korelasi Pearson antara {fitur} dan target: {bobot_korelasi:.4f}")
```

<p style="text-indent: 50px; text-align: justify;">Dalam analisis ini, menghitung korelasi Pearson antara setiap fitur yang dihasilkan dari harga beras pada hari-hari sebelumnya dan target harga saat ini. Hasil korelasi menunjukkan bahwa terdapat hubungan yang sangat kuat antara harga beras pada hari ke-5 sebelumnya (koefisien korelasi 0.9970) hingga harga beras pada hari ke-1 sebelumnya (koefisien korelasi 0.9993) dengan harga saat ini.</P>

### Pra-pemrosesan Data (Data Preprocessing)
