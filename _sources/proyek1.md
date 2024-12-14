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

# Laporan Proyek 1

## Prediksi Pergerakan Harga Ethereum (ETH) Berdasarkan Data Historis untuk Mendukung Strategi Investasi yang Lebih Efektif
## Pendahuluan 

### Latar Belakang

<p style="text-indent: 50px; text-align: justify;">Cryptocurrency telah berkembang pesat sebagai aset digital yang memanfaatkan teknologi blockchain untuk mencatat transaksi secara aman dan transparan. Salah satu cryptocurrency yang paling terkenal dan memiliki potensi besar adalah Ethereum (ETH), yang diluncurkan pada tahun 2015 oleh Vitalik Buterin. Ethereum bukan hanya sekadar cryptocurrency, tetapi juga platform blockchain yang memungkinkan pengembangan aplikasi terdesentralisasi (dApps) dan kontrak pintar (smart contracts). Ethereum memungkinkan transaksi yang lebih cepat, lebih murah, dan lebih fleksibel dibandingkan dengan blockchain lainnya, menjadikannya sebagai salah satu platform paling banyak digunakan dalam dunia cryptocurrency.</p>

<p style="text-indent: 50px; text-align: justify;">Untuk membantu investor dalam mengelola fluktuasi harga dan merencanakan strategi investasi yang lebih efektif, penting untuk mengembangkan model prediksi harga Ethereum yang dapat memberikan wawasan lebih dalam mengenai potensi pergerakan harga di masa depan. Dengan memanfaatkan data historis harga Ethereum, analisis tren, dan faktor-faktor yang mempengaruhi pasar, model prediksi ini dapat membantu investor dalam mengambil keputusan yang lebih terinformasi, mengurangi risiko, dan meningkatkan peluang keuntungan.</p>
  
<p style="text-indent: 50px; text-align: justify;">Seiring dengan meningkatnya adopsi Ethereum di berbagai sektor, baik dalam industri keuangan, teknologi, maupun sektor lainnya, pemahaman yang lebih baik tentang pergerakan harga Ethereum menjadi semakin penting. Oleh karena itu, pengembangan model prediksi harga Ethereum yang berbasis data historis menjadi sangat relevan untuk mendukung investor dalam menghadapi tantangan dan memanfaatkan peluang di pasar cryptocurrency yang sangat dinamis.</p>

### Rumusan Masalah

<p style="text-indent: 50px; text-align: justify;">Bagaimana data historis dapat digunakan untuk memprediksi pergerakan harga Ethereum (ETH) dan memberikan gambaran yang lebih jelas tentang potensi harga di masa depan? Selain itu, bagaimana analisis data historis dapat membantu investor memahami faktor-faktor yang mempengaruhi fluktuasi harga Ethereum dan merencanakan keputusan investasi yang lebih efektif dan terinformasi?</p>

### Tujuan 

<p style="text-indent: 50px; text-align: justify;">Penelitian ini bertujuan untuk mengeksplorasi penggunaan data historis dalam memprediksi pergerakan harga Ethereum (ETH) untuk membantu investor membuat keputusan investasi yang lebih baik. Selain itu, penelitian ini juga bertujuan untuk mengembangkan model prediksi yang dapat memberikan wawasan yang lebih dalam mengenai faktor-faktor yang mempengaruhi harga Ethereum, serta membantu investor merencanakan strategi investasi yang lebih matang dan mengurangi risiko yang terkait dengan fluktuasi harga.</p>



## Metodologi

### Data Understanding 

#### Sumber Data 
<p style="text-indent: 50px; text-align: justify;">Data yang digunakan dalam proyek ini merupakan data yang diperoleh dari platform Yahoo Finance, yang menyediakan informasi historis mengenai harga berbagai aset keuangan, termasuk cryptocurrency. Yahoo Finance adalah sumber data terpercaya yang banyak digunakan oleh investor dan analis untuk mendapatkan data harga, volume perdagangan, serta indikator pasar lainnya. Dalam proyek ini, digunakan data historis harga Cardano (ADA) <a href="https://finance.yahoo.com/quote/ETH-USD/history/" target="_blank" rel="noopener noreferrer">Yahoo Finance Cardano</a>
 dari tahun 2020 hingga 2024, dengan frekuensi harian. Data ini mencakup informasi mengenai harga pembukaan (open), harga tertinggi (high), harga terendah (low), harga penutupan (close), serta volume perdagangan. Informasi ini diambil untuk mendukung analisis dan pengembangan model prediksi harga Cardano yang akurat dan berbasis data.</p>

```{code-cell} python
# import library
import pandas as pd
```

```{code-cell} python
# Membaca data CSV
# Membaca data
df = pd.read_csv('https://raw.githubusercontent.com/vaniawrenda/dataset/refs/heads/main/etherium.csv')

# Mengubah kolom 'Tanggal' menjadi format datetime
df['Date'] = pd.to_datetime(df['Date'])

# Mengatur kolom 'Date' sebagai indeks
df.set_index('Date', inplace=True)

# Mensortir data berdasarkan tanggal
df = df.sort_values(by='Date')
print(df.head())
```

#### Deskripsi Data

Dataset ini terdiri dari 6 fitur atau kolom dan 1802 record atau baris. Atribut dalam dataset ini antara lain:
- Date: Kolom ini mencatat tanggal setiap data harga. Formatnya adalah YYYY-MM-DD (tahun-bulan-hari), yang menunjukkan data harian dari 1 Januari 2020 hingga seterusnya.
- Open:Harga pembukaan Cardano (ADA) pada awal hari perdagangan. Nilai ini menunjukkan harga pertama yang tercatat ketika pasar mulai aktif pada hari tersebut.
-	High: Harga tertinggi yang dicapai oleh Cardano (ADA) selama hari perdagangan. Nilai ini menunjukkan level maksimum yang dicapai oleh aset pada hari itu.
- Low:Harga terendah yang dicapai oleh Cardano (ADA) selama hari perdagangan. Nilai ini menunjukkan level minimum yang dicapai oleh aset pada hari itu.
- Close: Harga penutupan Cardano (ADA) pada akhir hari perdagangan. Nilai ini adalah harga terakhir yang tercatat sebelum pasar tutup.
- Adj Close: Harga penutupan yang telah disesuaikan untuk faktor-faktor tertentu, seperti dividen, aksi korporasi, atau perubahan lainnya. Dalam konteks cryptocurrency, nilai ini biasanya sama dengan harga penutupan kecuali ada penyesuaian tertentu.
- Volume: Jumlah total unit Cardano (ADA) yang diperdagangkan selama hari tersebut. Volume menunjukkan tingkat aktivitas perdagangan dan dapat digunakan untuk mengukur minat pasar pada aset tersebut.

Melihat ringkasan DataFrame.

```{code-cell} python
df.info()
print('Ukuran data ', df.shape)
```

```{code-cell} python
df.dtypes
```
Berdasarkan hasil output diatas, DataFrame memiliki 1802 baris dengan indeks yang dimulai dari 0. 

<b>Jenis Data</b>
1. Open: Data numerik (kontinu), menunjukkan harga pembukaan Cardano (ADA) pada awal hari perdagangan. Nilainya dapat berupa pecahan desimal untuk mencerminkan perubahan harga dengan presisi tinggi.
2. High: Data numerik (kontinu), mencatat harga tertinggi yang dicapai Cardano (ADA) selama hari perdagangan. Nilainya kontinu karena dapat memiliki pecahan desimal.
3. Low: Data numerik (kontinu), menunjukkan harga terendah yang dicapai Cardano (ADA) selama hari perdagangan. Data ini juga bersifat kontinu karena dapat memiliki pecahan desimal.
4. Close: Data numerik (kontinu), menunjukkan harga penutupan Cardano (ADA) pada akhir hari perdagangan. Nilainya kontinu dan digunakan untuk menganalisis perubahan harga harian.
6. Adj Close: Data numerik (kontinu), menunjukkan harga penutupan yang telah disesuaikan untuk mencerminkan peristiwa seperti dividen atau perubahan struktur pasar. Data ini penting untuk analisis historis yang lebih akurat.
7. Volume: Data numerik (diskrit), menunjukkan jumlah unit Cardano (ADA) yang diperdagangkan selama hari tersebut. Karena volume dihitung dalam bilangan bulat (jumlah unit), data ini bersifat diskrit.


#### Eksplorasi Data

<p style="text-indent: 50px; text-align: justify;">Mengecek apakah terdapat missing value pada data.</p>

```{code-cell} python
df.isnull().sum()
```

```{code-cell} python
df.describe()
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

##### Tren tiap fitur
<p style="text-indent: 50px; text-align: justify;">Selanjutnya, untuk menganalisis dinamika harga Cardano (ADA), kita akan menampilkan tren setiap fitur yang ada dalam data historis. Tren ini mencakup pergerakan harga pembukaan (Open), harga tertinggi (High), harga terendah (Low), harga penutupan (Close), serta volume perdagangan (Volume) dari waktu ke waktu. </p>

```{code-cell} python
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np  # Tambahkan ini untuk memperbaiki error

# Membuat subplot otomatis berdasarkan jumlah kolom dalam dataframe
plt.figure(figsize=(9, int(np.ceil(len(df.columns) / 3))*3))

for i, col in enumerate(df.columns):
    plt.subplot(int(np.ceil(len(df.columns) / 3)), 3, i + 1)
    sns.lineplot(data=df, x='Date', y=col)
    plt.title(f'Trend of {col}')
    plt.xticks(rotation=45)

plt.tight_layout()
plt.show()
```

##### Korelasi antar fitur 

<p style="text-indent: 50px; text-align: justify;">Korelasi antar fitur dalam data historis harga Cardano (ADA) merujuk pada hubungan atau keterkaitan antara dua atau lebih variabel dalam dataset. Dalam konteks ini, fitur-fitur seperti harga pembukaan (Open), harga tertinggi (High), harga terendah (Low), harga penutupan (Close), dan volume perdagangan (Volume) dapat saling mempengaruhi satu sama lain. Analisis korelasi membantu kita untuk memahami sejauh mana perubahan pada satu fitur dapat berhubungan dengan perubahan pada fitur lainnya</p>

```{code-cell} python
correlation_matrix = df.corr()

plt.figure(figsize=(7, 3))
sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm', linewidths=0.5)
plt.title('Heatmap Korelasi Antar Fitur')
plt.show()
```