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

# Laporan Proyek Sains Data Baru


## Pendahuluan 

### Latar Belakang

Beras Rojolele merupakan salah satu varietas padi lokal unggulan yang dihasilkan oleh petani di Jawa Tengah. Dikenal dengan tekstur pulen, aroma harum, dan butiran besar, beras ini menjadi salah satu pilihan favorit masyarakat. 
Dengan visi menjadi perusahaan pengolahan beras terkemuka yang senantiasa memberikan kualitas beras terbaik untuk konsumen. Misi beras Rojolele adalah konsisten menawarkan beras berkualitas tinggi dengan cita rasa pulen dan aroma harum, serta menjaga stabilitas harga dan ketersediaan produk, yang bertujuan untuk mendukung kesejahteraan petani lokal dan meningkatkan kualitas hidup konsumen.

Seiring dengan perkembangan ekonomi dan perubahan gaya hidup, permintaan terhadap beras Rojolele terus mengalami peningkatan yang stabil. Namun, kenaikan permintaan ini membawa tantangan bagi para produsen dan distributor. Faktor-faktor seperti ketidakpastian dalam rantai pasokan, perubahan iklim yang memengaruhi hasil panen, serta kebijakan pemerintah terkait impor dan ekspor beras dapat menyebabkan fluktuasi harga yang tidak terduga.
  
Untuk mengatasi tantangan ini, peramalan harga beras Rojolele berdasarkan data historis menjadi alat penting dalam menjaga keseimbangan antara penawaran dan permintaan. Peramalan yang akurat dapat membantu produsen dan distributor mengantisipasi perubahan harga di masa depan serta meminimalkan risiko kerugian akibat fluktuasi harga. Dengan demikian, upaya untuk menjaga stabilitas harga beras Rojolele di pasar dapat dilakukan lebih efektif dan mendukung keberlanjutan bisnis sekaligus memenuhi kebutuhan konsumen dengan lebih baik.  

### Tujuan 

Tujuan utama dari analisis ini membantu produsen dan distributor untuk mengetahui kenaikan harga beras berdasarkan data historis sehingga harga beras dapat dikendalikan di pasaran.  Dengan menggunakan peramalan harga yang akurat berdasarkan data historis, diharapkan dapat meminimalkan risiko kerugian akibat fluktuasi harga yang tidak terduga. Selain itu, analisis ini bertujuan untuk menjaga keseimbangan antara penawaran dan permintaan, serta memastikan ketersediaan beras Rojolele yang berkualitas untuk memenuhi kebutuhan konsumen secara efektif. 


### Rumusan Masalah


Dalam upaya menjaga stabilitas harga dan memenuhi permintaan pasar, para produsen dan distributor beras Rojolele menghadapi beberapa masalah utama. Salah satu tantangan terbesar adalah fluktuasi harga yang disebabkan oleh berbagai faktor eksternal, seperti ketidakpastian dalam rantai pasokan, perubahan iklim, dan kebijakan pemerintah. Ketidakpastian ini sering kali mengakibatkan kesulitan dalam merencanakan produksi dan distribusi, yang pada gilirannya dapat mengganggu ketersediaan beras Rojolele di pasar. Oleh karena itu, perlu adanya pendekatan yang lebih efektif dalam peramalan harga agar produsen dan distributor dapat membuat keputusan yang lebih baik dalam menghadapi dinamika pasar yang cepat berubah.



## Metodologi

### Data Understanding 

#### Sumber Data 
Data yang digunakan dalam proyek ini merupakan data sekunder yang diperoleh dari website [PHIPS Nasional](https://www.bi.go.id/hargapangan) (Pusat Informasi Harga Pangan Strategis Nasional) . PHIPS Nasional adalah sebuah platform online yang dikelola oleh Bank Indonesia, yang menyediakan informasi historis mengenai harga pangan di seluruh provinsi di Indonesia. Pemantauan harga PIHPS Nasional telah mencakup empat jenis pasar, yakni pasar tradisional, pasar modern, pedagang besar, dan produsen. Dalam proyek ini, digunakan data historis harga beras dari tahun 2019 hingga 2024, dengan periode mingguan, yang diambil dari seluruh pasar modern di Jawa Timur. 

```{code-cell} python
# import library
import pandas as pd
```

```{code-cell} python
# Baca data CSV
df = pd.read_csv('https://raw.githubusercontent.com/vaniawrenda/dataset/main/dataset.csv')
print(df.head())
```

#### Deskripsi Dataset

Dataset ini terdiri dari 2 fitur atau kolom dan 1479 record atau baris. Atribut dalam dataset ini antara lain:
1.  Date: Tanggal harga beras dengan format yyyy-mm-dd
2.	Harga: Berisi harga beras Rojolele dalam satuan rupiah per kilogram.

Melihat ringkasan Data Frame.

```{code-cell} python
df.info()
print('Ukuran data ', df.shape)
```
Berdasarkan hasil output diatas, DataFrame memiliki 1479 baris dengan indeks yang dimulai dari 0 hingga 1478. 

```{code-cell} python
df.dtypes
```
###### Jenis Data
1. Date: Data kolom ini disajikan dalam bentuk string, namun pada eksplorasi data akan dirubah menjadi datetime
2. Harga Beras (kg): Numerik (Kontinu), karena harga dapat memiliki nilai pecahan dan dapat diukur dengan presisi yang lebih tinggi.



