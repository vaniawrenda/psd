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

# Laporan Proyek 3

## Prediksi Harga Cryptocurrency Cardano (ADA) Menggunakan Data Historis untuk Meningkatkan Strategi Investasi

## Pendahuluan 

### Latar Belakang

<p style="text-indent: 50px; text-align: justify;">Cryptocurrency adalah aset digital yang menggunakan teknologi blockchain untuk memastikan transaksi tercatat dengan aman dan transparan. Salah satu cryptocurrency yang menarik perhatian adalah Cardano (ADA), sebuah platform blockchain yang dibangun dengan pendekatan berbasis riset dan ilmiah. Cardano bertujuan untuk menciptakan ekosistem yang aman, terukur, dan berkelanjutan, serta mendukung aplikasi terdesentralisasi (dApps) dan kontrak pintar. Dengan menggunakan mekanisme konsensus Proof-of-Stake (PoS), Cardano menawarkan efisiensi energi yang lebih tinggi dibandingkan dengan cryptocurrency lain yang menggunakan Proof-of-Work (PoW). Seperti halnya cryptocurrency lainnya, harga Cardano (ADA) sangat fluktuatif dan dipengaruhi oleh berbagai faktor, termasuk sentimen pasar, perkembangan teknologi, kebijakan regulasi, dan kondisi ekonomi global. Perubahan harga yang cepat ini sering menjadi tantangan bagi investor dalam membuat keputusan investasi yang tepat.</p>

<p style="text-indent: 50px; text-align: justify;">Untuk membantu investor dalam mengelola fluktuasi harga dan membuat keputusan yang lebih terinformasi, penting untuk mengembangkan model prediksi harga Cardano. Dengan memanfaatkan data historis dan teknologi kecerdasan buatan, model prediksi ini dapat memberikan wawasan yang lebih mendalam tentang potensi pergerakan harga Cardano di masa depan. Hal ini dapat membantu investor dalam merencanakan strategi investasi yang lebih efektif, mengurangi risiko, dan meningkatkan peluang keuntungan.</p>
  
<p style="text-indent: 50px; text-align: justify;">Oleh karena itu, pengembangan model prediksi harga Cardano menjadi sangat relevan untuk membantu investor memahami dinamika harga Cardano secara lebih mendalam, mendukung strategi investasi yang lebih terinformasi dan berbasis data, serta meningkatkan kepercayaan terhadap aset ini dengan menyediakan wawasan yang dapat diandalkan terkait potensi pergerakan harga. Proyek ini dirancang untuk menjawab kebutuhan tersebut, dengan fokus pada pemanfaatan data historis untuk memberikan solusi praktis bagi tantangan yang dihadapi oleh investor di pasar cryptocurrency.</p>

### Rumusan Masalah

<p style="text-indent: 50px; text-align: justify;">Bagaimana data historis dapat digunakan untuk memprediksi pergerakan harga Cardano (ADA) dan memberikan wawasan yang lebih mendalam bagi investor dalam merencanakan strategi investasi yang lebih efektif? Selain itu, bagaimana pemanfaatan data historis dapat menghasilkan model prediksi yang akurat, yang dapat membantu investor dalam mengelola fluktuasi harga dan membuat keputusan investasi yang lebih terinformasi dan berbasis data?</p>

### Tujuan 

<p style="text-indent: 50px; text-align: justify;">Penelitian ini bertujuan untuk mengeksplorasi pemanfaatan data historis dalam memprediksi pergerakan harga Cardano (ADA) guna memberikan wawasan yang lebih mendalam bagi investor dalam merencanakan strategi investasi yang lebih efektif. Selain itu, penelitian ini juga bertujuan untuk mengembangkan model prediksi berbasis data historis yang akurat, sehingga dapat membantu investor dalam meminimalkan risiko dan membuat keputusan yang lebih strategis dan terencana. Dengan demikian, penelitian ini diharapkan dapat memberikan solusi praktis yang mendukung pengambilan keputusan yang lebih baik di pasar cryptocurrency.</p>



## Metodologi

### Data Understanding 

#### Sumber Data 
<p style="text-indent: 50px; text-align: justify;">Data yang digunakan dalam proyek ini merupakan data yang diperoleh dari platform Yahoo Finance, yang menyediakan informasi historis mengenai harga berbagai aset keuangan, termasuk cryptocurrency. Yahoo Finance adalah sumber data terpercaya yang banyak digunakan oleh investor dan analis untuk mendapatkan data harga, volume perdagangan, serta indikator pasar lainnya. Dalam proyek ini, digunakan data historis harga Cardano (ADA) <a href="https://finance.yahoo.com/quote/ADA-USD/history/" target="_blank" rel="noopener noreferrer">Yahoo Finance Cardano</a>
 dari tahun 2020 hingga 2024, dengan frekuensi harian. Data ini mencakup informasi mengenai harga pembukaan (open), harga tertinggi (high), harga terendah (low), harga penutupan (close), serta volume perdagangan. Informasi ini diambil untuk mendukung analisis dan pengembangan model prediksi harga Cardano yang akurat dan berbasis data.</p>

```{code-cell} python
# import library
import pandas as pd
```

```{code-cell} python
# Membaca data CSV
# Membaca data
df = pd.read_csv('https://raw.githubusercontent.com/vaniawrenda/dataset/refs/heads/main/cardano.csv')

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
1. Date: Data waktu yang telah disajikan dalam bentuk indeks bertipe datetime. Dengan menjadikannya indeks, analisis berbasis waktu seperti tren, musiman, atau pergerakan harga harian menjadi lebih efisien.
2. Open: Data numerik (kontinu), menunjukkan harga pembukaan Cardano (ADA) pada awal hari perdagangan. Nilainya dapat berupa pecahan desimal untuk mencerminkan perubahan harga dengan presisi tinggi.
3. High: Data numerik (kontinu), mencatat harga tertinggi yang dicapai Cardano (ADA) selama hari perdagangan. Nilainya kontinu karena dapat memiliki pecahan desimal.
4. Low: Data numerik (kontinu), menunjukkan harga terendah yang dicapai Cardano (ADA) selama hari perdagangan. Data ini juga bersifat kontinu karena dapat memiliki pecahan desimal.
5. Close: Data numerik (kontinu), menunjukkan harga penutupan Cardano (ADA) pada akhir hari perdagangan. Nilainya kontinu dan digunakan untuk menganalisis perubahan harga harian.
6. Adj Close: Data numerik (kontinu), menunjukkan harga penutupan yang telah disesuaikan untuk mencerminkan peristiwa seperti dividen atau perubahan struktur pasar. Data ini penting untuk analisis historis yang lebih akurat.
7. Volume: Data numerik (diskrit), menunjukkan jumlah unit Cardano (ADA) yang diperdagangkan selama hari tersebut. Karena volume dihitung dalam bilangan bulat (jumlah unit), data ini bersifat diskrit.


#### Eksplorasi Data

<p style="text-indent: 50px; text-align: justify;">Sebelum melakukan eksplorasi data, kolom date akan dikonversi dari format string menjadi tipe data datetime dan dijadikan sebagai indeks dari DataFrame.</p>

```{code-cell} python
df['Date'] = pd.to_datetime(df['Date'], dayfirst=True).dt.date
df.set_index('Date', inplace=True)
df.index = pd.to_datetime(df.index)
print(df.head())
```

<p style="text-indent: 50px; text-align: justify;"> Selanjutnya, Pastikan bahwa kolom Harga Beras memiliki format yang tepat untuk analisis lebih lanjut. Dalam langkah ini, menggunakan fungsi pd.to_numeric() untuk mengonversi nilai dalam kolom tersebut menjadi tipe data float, yang memungkinkan untuk melakukan perhitungan matematis yang lebih akurat. Sebelum konversi, disini juga menggunakan metode str.replace(',', '') untuk menghapus tanda koma (,) dari string yang ada, karena nilai harga beras biasanya dituliskan dengan tanda koma sebagai pemisah ribuan.</p>

```{code-cell} python
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
