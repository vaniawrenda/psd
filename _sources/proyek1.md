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

<p style="text-indent: 50px; text-align: justify;"> 
Setelah dipastikan tidak ada missing value, langkah berikutnya adalah membuat visualisasi tren data untuk setiap kolom menggunakan matplotlib dan seaborn. Grafik garis dibuat dengan tanggal sebagai sumbu X dan nilai kolom sebagai sumbu Y untuk menunjukkan perubahan nilai seiring waktu.</p>

```{code-cell} python
import matplotlib.pyplot as plt
import seaborn as sns
for col in df:
    plt.figure(figsize=(7, 3))
    sns.lineplot(data=df, x='Date', y=col)
    plt.title(f'Trend of {col}')
    plt.xlabel('Date')
    plt.ylabel(col)
    plt.grid(True)
    plt.xticks(rotation=45)
    plt.show()
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

##### Korelasi antar fitur
<p style="text-indent: 50px; text-align: justify;">Selanjutnya, membuat heatmap digunakan untuk memahami hubungan antar fitur dalam dataset. Heatmap ini membantu mengidentifikasi korelasi kuat atau lemah antar fitur, sehingga memudahkan dalam memilih fitur yang relevan untuk analisis atau pembuatan model prediksi. Dengan demikian, dapat mengoptimalkan kinerja model dan menghindari potensi masalah seperti multikolinearitas.</p>

```{code-cell} python
correlation_matrix = df.corr()

plt.figure(figsize=(7, 3))
sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm', linewidths=0.5)
plt.title('Heatmap Korelasi Antar Fitur')
plt.show()
```
<p style="text-indent: 50px; text-align: justify;">Hasil korelasi pada heatmap menunjukkan bahwa fitur "Open," "High," "Low," "Close," dan "Adj Close" memiliki hubungan sangat kuat dengan nilai korelasi mendekati 1, menandakan keterkaitan yang tinggi. Sebaliknya, fitur "Volume" memiliki korelasi lemah (sekitar 0,26-0,27) terhadap fitur lainnya, sehingga perubahan pada "Volume" tidak terlalu memengaruhi fitur-fitur tersebut.</p>

### Data Prepocessing

#### a. Menghapus fitur yang tidak relevan 
<p style="text-indent: 50px; text-align: justify;">Dalam proses perhitungan matriks korelasi, ditemukan bahwa fitur 'Volume' tidak relevan atau tidak memiliki pengaruh signifikan terhadap fitur lainnya, sehingga fitur ini akan dihapus. Selain itu, fitur 'Adj Close' yang memiliki nilai identik dengan fitur 'Close' juga akan dihilangkan.</p>

```{code-cell} python
df = df.drop(columns=['Volume', 'Adj Close'])
df.head()
```

#### b. Rekayasa Fitur

<p style="text-indent: 50px; text-align: justify;">Dalam penelitian ini, fokusnya adalah memprediksi harga penutupan (Close) untuk hari berikutnya. Oleh karena itu, diperlukan penambahan variabel baru sebagai target. Variabel ini berguna untuk memahami potensi penurunan harga saham, sehingga investor dapat memanfaatkan prediksi tersebut untuk membeli aset saat harga sedang rendah, meningkatkan peluang keuntungan ketika harga kembali naik.</P>

```{code-cell} python
df['Close Target'] = df['Close'].shift(-1)

df = df[:-1]
df.head()
```