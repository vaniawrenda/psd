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

# Projek 2


## Pendahuluan 

### Latar Belakang

<p style="text-indent: 50px; text-align: justify;">Beras Rojolele merupakan salah satu varietas padi lokal unggulan yang dihasilkan oleh petani di Jawa Tengah. Dikenal dengan tekstur pulen, aroma harum, dan butiran besar, beras ini menjadi salah satu pilihan favorit masyarakat. Dengan visi menjadi perusahaan pengolahan beras terkemuka yang senantiasa memberikan kualitas beras terbaik untuk konsumen. Misi beras Rojolele adalah konsisten menawarkan beras berkualitas tinggi dengan cita rasa pulen dan aroma harum, serta menjaga stabilitas harga dan ketersediaan produk, yang bertujuan untuk mendukung kesejahteraan petani lokal dan meningkatkan kualitas hidup konsumen.</p>

<p style="text-indent: 50px; text-align: justify;">Seiring dengan perkembangan ekonomi dan perubahan gaya hidup, permintaan terhadap beras Rojolele terus mengalami peningkatan yang stabil. Namun, kenaikan permintaan ini membawa tantangan bagi para produsen dan distributor. Faktor-faktor seperti ketidakpastian dalam rantai pasokan, perubahan iklim yang memengaruhi hasil panen, serta kebijakan pemerintah terkait impor dan ekspor beras dapat menyebabkan fluktuasi harga yang tidak terduga.</p>
  
<p style="text-indent: 50px; text-align: justify;">Untuk mengatasi tantangan ini, peramalan harga beras Rojolele berdasarkan data historis menjadi alat penting dalam menjaga keseimbangan antara penawaran dan permintaan. Peramalan yang akurat dapat membantu produsen dan distributor mengantisipasi perubahan harga di masa depan serta meminimalkan risiko kerugian akibat fluktuasi harga. Dengan demikian, upaya untuk menjaga stabilitas harga beras Rojolele di pasar dapat dilakukan lebih efektif dan mendukung keberlanjutan bisnis sekaligus memenuhi kebutuhan konsumen dengan lebih baik.</p>

### Rumusan Masalah

<p style="text-indent: 50px; text-align: justify;">Dalam upaya menjaga stabilitas harga dan memenuhi permintaan pasar, para produsen dan distributor beras Rojolele menghadapi beberapa masalah utama. Salah satu tantangan terbesar adalah fluktuasi harga yang disebabkan oleh berbagai faktor eksternal, seperti ketidakpastian dalam rantai pasokan, perubahan iklim, dan kebijakan pemerintah. Ketidakpastian ini sering kali mengakibatkan kesulitan dalam merencanakan produksi dan distribusi, yang pada gilirannya dapat mengganggu ketersediaan beras Rojolele di pasar. Oleh karena itu, perlu adanya pendekatan yang lebih efektif dalam peramalan harga agar produsen dan distributor dapat membuat keputusan yang lebih baik dalam menghadapi dinamika pasar yang cepat berubah.</p>

### Tujuan 

<p style="text-indent: 50px; text-align: justify;">Tujuan utama dari analisis ini membantu produsen dan distributor untuk mengetahui kenaikan harga beras berdasarkan data historis sehingga harga beras dapat dikendalikan di pasaran.  Dengan menggunakan peramalan harga yang akurat berdasarkan data historis, diharapkan dapat meminimalkan risiko kerugian akibat fluktuasi harga yang tidak terduga. Selain itu, analisis ini bertujuan untuk menjaga keseimbangan antara penawaran dan permintaan, serta memastikan ketersediaan beras Rojolele yang berkualitas untuk memenuhi kebutuhan konsumen secara efektif.</p>



## Metodologi

### Data Understanding 

#### Sumber Data 
<p style="text-indent: 50px; text-align: justify;">Data yang digunakan dalam proyek ini merupakan data sekunder yang diperoleh dari website [PHIPS Nasional](https://www.bi.go.id/hargapangan) (Pusat Informasi Harga Pangan Strategis Nasional) . PHIPS Nasional adalah sebuah platform online yang dikelola oleh Bank Indonesia, yang menyediakan informasi historis mengenai harga pangan di seluruh provinsi di Indonesia. Pemantauan harga PIHPS Nasional telah mencakup empat jenis pasar, yakni pasar tradisional, pasar modern, pedagang besar, dan produsen. Dalam proyek ini, digunakan data historis harga beras dari tahun 2019 hingga 2024, dengan periode mingguan, yang diambil dari seluruh pasar modern di Jawa Timur.</p>

```{code-cell} python
# import library
import pandas as pd
```

```{code-cell} python
# Membaca data CSV
df = pd.read_csv('https://raw.githubusercontent.com/vaniawrenda/dataset/refs/heads/main/dataset.csv')
pd.options.display.float_format = '{:.0f}'.format
print(df.head())
```

#### Deskripsi Data

Dataset ini terdiri dari 2 fitur atau kolom dan 1479 record atau baris. Atribut dalam dataset ini antara lain:
1.  Date: Tanggal harga beras dengan format yyyy-mm-dd
2.	Harga: Berisi harga beras Rojolele dalam satuan rupiah per kilogram.

Melihat ringkasan DataFrame.

```{code-cell} python
df.info()
print('Ukuran data ', df.shape)
```
Berdasarkan hasil output diatas, DataFrame memiliki 1479 baris dengan indeks yang dimulai dari 0. 

```{code-cell} python
df.dtypes
```
<b>Jenis Data</b>
1. Date: Data saat ini disajikan dalam bentuk string, namun akan diubah menjadi tipe data datetime pada tahap eksplorasi untuk memudahkan analisis waktu. 
2. Harga Beras (kg): Merupakan data numerik (Kontinu), karena harga dapat memiliki nilai pecahan dan dapat diukur dengan presisi yang lebih tinggi.

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