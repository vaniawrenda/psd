# Laporan Proyek Sains Data Baru


## Pendahuluan 

### Latar Belakang

Beras Rojolele merupakan salah satu varietas padi lokal unggulan yang dihasilkan oleh petani di Jawa Tengah. Dikenal dengan tekstur pulen, aroma harum, dan butiran besar, beras ini menjadi salah satu pilihan favorit masyarakat. Seiring dengan perkembangan ekonomi dan perubahan gaya hidup, permintaan terhadap beras Rojolele terus mengalami peningkatan yang stabil. Namun, kenaikan permintaan ini membawa tantangan bagi para produsen dan distributor. Faktor-faktor seperti ketidakpastian dalam rantai pasokan, perubahan iklim yang memengaruhi hasil panen, serta kebijakan pemerintah terkait impor dan ekspor beras dapat menyebabkan fluktuasi harga yang tidak terduga. Selain itu, dinamika pasar global turut memengaruhi kestabilan harga beras di pasar lokal.  
  
Untuk mengatasi tantangan ini, peramalan harga beras Rojolele berdasarkan data historis menjadi alat penting dalam menjaga keseimbangan antara penawaran dan permintaan. Peramalan yang akurat dapat membantu produsen dan distributor mengantisipasi perubahan harga di masa depan serta meminimalkan risiko kerugian akibat fluktuasi harga. Dengan demikian, upaya untuk menjaga stabilitas harga beras Rojolele di pasar dapat dilakukan lebih efektif dan mendukung keberlanjutan bisnis sekaligus memenuhi kebutuhan konsumen dengan lebih baik.  

### Tujuan 

Tujuan utama dari analisis ini membantu produsen dan distributor untuk mengetahui kenaikan harga beras berdasarkan data historis sehingga harga beras dapat dikendalikan di pasaran.  Dengan menggunakan peramalan harga yang akurat berdasarkan data historis, diharapkan dapat meminimalkan risiko kerugian akibat fluktuasi harga yang tidak terduga. Selain itu, analisis ini bertujuan untuk menjaga keseimbangan antara penawaran dan permintaan, serta memastikan ketersediaan beras Rojolele yang berkualitas untuk memenuhi kebutuhan konsumen secara efektif. 


### Rumusan Masalah


Dalam upaya menjaga stabilitas harga dan memenuhi permintaan pasar, para produsen dan distributor beras Rojolele menghadapi beberapa masalah utama. Salah satu tantangan terbesar adalah fluktuasi harga yang disebabkan oleh berbagai faktor eksternal, seperti ketidakpastian dalam rantai pasokan, perubahan iklim, dan kebijakan pemerintah. Ketidakpastian ini sering kali mengakibatkan kesulitan dalam merencanakan produksi dan distribusi, yang pada gilirannya dapat mengganggu ketersediaan beras Rojolele di pasar. Oleh karena itu, perlu adanya pendekatan yang lebih efektif dalam peramalan harga agar produsen dan distributor dapat membuat keputusan yang lebih baik dalam menghadapi dinamika pasar yang cepat berubah.



## Metodologi

### Data Understanding 

#### Sumber Data 
Data yang digunakan dalam penelitian ini merupakan data sekunder yang diperoleh dari website PHIPS Nasional (Pusat Informasi Harga Pangan Strategis Nasional) di www.bi.go.id/hargapangan. PHIPS Nasional adalah sebuah platform online yang dikelola oleh Bank Indonesia, yang menyediakan informasi historis mengenai harga pangan di seluruh provinsi di Indonesia. Pemantauan harga PIHPS Nasional telah mencakup empat jenis pasar, yakni pasar tradisional, pasar modern, pedagang besar, dan produsen. Dalam proyek ini, digunakan data historis harga beras dari tahun 2020 hingga 2024, dengan periode bulanan, yang diambil dari seluruh pasar modern di Jawa Timur. 

#### Deskripsi Dataset

Dataset ini terdiri dari 8 fitur atau kolom dan 56 record atau baris. Atribut-atirbut dalam dataset ini antara lain:
1.	Bulan: Periode bulan ketika data harga beras dikumpulkan.
2.	Tahun: Berisi informasi mengenai tahun pengumpulan data harga beras. 
3.	Beras Kualitas Bawah I: Berisi harga beras dengan kualitas rendah tingkat I, dinyatakan dalam satuan rupiah per kilogram.
4.	Beras Kualitas Bawah II: Berisi harga beras dengan kualitas rendah tingkat II, dinyatakan dalam satuan rupiah per kilogram.
5.	Beras Kualitas Medium I: Berisi harga beras dengan kualitas menengah tingkat I, dinyatakan dalam satuan rupiah per kilogram.
6.	Beras Kualitas Medium II: Berisi harga beras dengan kualitas menengah tingkat II, dinyatakan dalam satuan rupiah per kilogram.
7.	Beras Kualitas Super I: Berisi harga beras dengan kualitas tinggi tingkat I, dinyatakan dalam satuan rupiah per kilogram.
8.	Beras Kualitas Super II: Berisi harga beras dengan kualitas tinggi tingkat II, dinyatakan dalam satuan rupiah per kilogram.
