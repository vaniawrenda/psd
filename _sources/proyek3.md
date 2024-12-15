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


<p style="text-indent: 50px; text-align: justify;">Hasil korelasi yang ditampilkan dalam heatmap ini menggambarkan hubungan antar fitur dalam data. Nilai korelasi berkisar antara -1 hingga 1, di mana 1 menunjukkan hubungan yang sangat kuat dan positif, 0 menunjukkan tidak ada hubungan, dan -1 menunjukkan hubungan yang sangat kuat namun negatif. Dari heatmap ini, dapat dilihat bahwa fitur Open, High, Low, Close, dan Adj Close memiliki korelasi yang sangat tinggi (mendekati 1), yang mengindikasikan bahwa nilai-nilai dari fitur-fitur tersebut saling berhubungan erat dan bergerak searah.  Di sisi lain, fitur Volume memiliki korelasi yang lebih rendah dengan fitur harga lainnya, dengan nilai berkisar antara 0.62 hingga 0.66. Ini menunjukkan bahwa meskipun ada hubungan positif antara *Volume* dan harga, hubungan tersebut tidak sekuat korelasi antar harga. Penafsiran ini penting untuk memahami interaksi antar fitur dalam analisis data, membantu mengurangi redundansi informasi dalam pemodelan, serta membantu dalam menentukan fitur mana yang paling berpengaruh dalam analisis lebih lanjut.</P>

### Pra-pemrosesan Data (Data Preprocessing)

#### a. Menghapus Fitur yang tidak relevan
<p style="text-indent: 50px; text-align: justify;">Pada tahap perhitungan matriks korelasi, ditemukan bahwa fitur ‘volume’ tidak relevan atau tidak memiliki pengaruh terhadap fitur lainnya, sehingga fitur ‘volume’ akan dihilangkan. Selain itu, fitur ‘Adj Close’ juga dihapus karena nilainya identik dengan fitur ‘Close’.</p>

```{code-cell} python
# Menghapus kolom yang tidak digunakan
df = df.drop(columns=['Volume', 'Adj Close'])
df.head()
```

#### b. Rekayasa Fitur

<p style="text-indent: 50px; text-align: justify;"> Dalam penelitian ini, tujuan utamanya adalah memprediksi nilai Close Cardano (ADA) untuk hari berikutnya. Oleh karena itu, diperlukan variabel baru yang akan berfungsi sebagai target prediksi. Fitur ini memberikan gambaran mengenai potensi penurunan harga, yang dapat dimanfaatkan oleh investor untuk membeli aset pada nilai yang lebih rendah, sehingga meningkatkan peluang keuntungan ketika harga Cardano kembali mengalami kenaikan.</p>

```{code-cell} python
df['Close Target'] = df['Close'].shift(-1)

df = df[:-1]
df.head()
```
### c. Forecasting

<p style="text-indent: 50px; text-align: justify;">Pada tahap ini, parameter FORECAST_STEPS digunakan untuk menentukan jumlah periode atau langkah ke depan yang akan diprediksi dalam metode multi-step forecasting. Nilainya diatur menjadi 5, yang berarti model akan memproyeksikan nilai target untuk 5 hari atau periode mendatang. Parameter ini dapat disesuaikan berdasarkan kebutuhan analisis atau tujuan prediksi. Dalam pendekatan Iterative Multi-Step Forecasting, setiap prediksi yang dihasilkan akan menjadi input untuk langkah prediksi berikutnya. Proses ini dimulai dengan memprediksi langkah pertama, kemudian hasilnya digunakan sebagai data input untuk langkah kedua, dan seterusnya hingga mencapai jumlah langkah yang telah ditentukan oleh FORECAST_STEPS. Pendekatan ini memungkinkan prediksi bertahap untuk beberapa periode ke depan.</p>

```{code-cell} python
# Parameter untuk Multi-Step Forecasting
FORECAST_STEPS = 5  # Jumlah langkah ke depan yang ingin diprediksi
```

```{code-cell} python
# Membuat target untuk n langkah ke depan
for i in range(1, FORECAST_STEPS + 1):
    df[f'Close Target+{i}'] = df['Close'].shift(-i)

# Menghapus baris yang memiliki nilai NaN pada target
df = df[:-FORECAST_STEPS]
```

#### d. Normalisasi Data

```{code-cell} python
# Import library yang dibutuhkan
# Import library yang dibutuhkan
from sklearn.preprocessing import MinMaxScaler
import pandas as pd

# Inisialisasi scaler untuk fitur dan target
scaler_features = MinMaxScaler()
scaler_target = MinMaxScaler()

# Normalisasi fitur
features = ['Open', 'High', 'Low', 'Close']
df_features_normalized = pd.DataFrame(
    scaler_features.fit_transform(df[features]),
    columns=features,
    index=df.index
)

# Normalisasi target
target_columns = [f'Close Target+{i}' for i in range(1, FORECAST_STEPS + 1)]
df_target_normalized = pd.DataFrame(
    scaler_target.fit_transform(df[target_columns]),
    columns=target_columns,
    index=df.index
)

# Menggabungkan kembali dataframe yang sudah dinormalisasi
df_normalized = pd.concat([df_features_normalized, df_target_normalized], axis=1)
```
<p style="text-indent: 50px; text-align: justify;"> Normalisasi data pada fitur dan target dilakukan untuk menyelaraskan nilai-nilai dalam dataset agar berada dalam rentang tertentu, biasanya antara 0 dan 1. Dalam konteks ini, MinMaxScaler digunakan untuk menormalisasi fitur seperti (Open, High, Low, Close) serta target (Close Target). Proses normalisasi fitur dilakukan dengan scaler_features.fit_transform(), sedangkan normalisasi target menggunakan scaler_target.fit_transform(). Hasil normalisasi tersebut kemudian digabungkan menjadi satu dataframe bernama df_normalized menggunakan pd.concat(), sehingga data siap untuk digunakan dalam pengembangan model prediksi harga Cardano. Normalisasi ini berperan penting dalam memastikan skala data yang seragam, sehingga model dapat memproses data secara lebih optimal dan menghasilkan prediksi yang lebih andal. </P>

### Modelling 

#### a. Pembagian Data
<p style="text-indent: 50px; text-align: justify;"> Selanjutnya, data dibagi menjadi data training dan data testing menggunakan train_test_split, dengan 80% data digunakan untuk training dan 20% untuk testing. Proses ini dilakukan dengan opsi shuffle=False agar urutan data tetap terjaga sesuai dengan urutan aslinya. Setelah pembagian, data training (X_train dan y_train) digunakan untuk melatih model, sementara data testing (X_test dan y_test) digunakan untuk menguji performa model yang telah dilatih. </p>


```{code-cell} python
from sklearn.model_selection import train_test_split  # Tambahkan ini

# Mengatur fitur (X) dan target (y)
X = df_normalized[features]
y = df_normalized[target_columns]

# Membagi data menjadi training dan testing
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.4, shuffle=False)
```
#### b. Penyusunann Model
<p style="text-indent: 50px; text-align: justify;">Pada tahap ini, dilakukan percobaan dengan menggunakan tiga model utama, yaitu Support Vector Regression (SVR), Decision Tree, dan SVR dengan Decision Tree. Selain itu, untuk meningkatkan akurasi dan kinerja model, diterapkan juga teknik ensemble menggunakan metode bagging.</p>

```{code-cell} python
from sklearn.linear_model import LinearRegression, Ridge
from sklearn.tree import DecisionTreeRegressor
from sklearn.metrics import mean_squared_error, mean_absolute_percentage_error
import numpy as np
import matplotlib.pyplot as plt

# List model regresi
models = {
    "Linear Regression": LinearRegression(),
    "Decision Tree": DecisionTreeRegressor(random_state=32),
    "Ridge Regression": Ridge(alpha=1.0)
}

# Dictionary untuk menyimpan hasil evaluasi
results = {}

# Iterasi setiap model
for name, model in models.items():
    # Latih model
    model.fit(X_train, y_train)

    # Prediksi pada data uji
    y_pred = model.predict(X_test)

    # Evaluasi untuk setiap target hari ke depan
    mse_list = []
    mape_list = []
    for i in range(FORECAST_STEPS):
        mse = mean_squared_error(y_test.iloc[:, i], y_pred[:, i])
        mape = mean_absolute_percentage_error(y_test.iloc[:, i], y_pred[:, i]) * 100
        mse_list.append(mse)
        mape_list.append(mape)

    # Simpan hasil evaluasi rata-rata
    results[name] = {
        "Average RMSE": np.sqrt(np.mean(mse_list)),
        "Average MAPE": np.mean(mape_list)
    }

    # Kembalikan hasil prediksi ke skala asli
    y_pred_original = scaler_target.inverse_transform(y_pred)
    y_test_original = scaler_target.inverse_transform(y_test)

    # Plot hasil prediksi untuk setiap hari
    plt.figure(figsize=(15, 6))
    for i in range(FORECAST_STEPS):
        plt.plot(
            y_test.index, y_test_original[:, i], label=f"Actual Target+{i+1}", linestyle="dashed"
        )
        plt.plot(
            y_test.index, y_pred_original[:, i], label=f"Predicted Target+{i+1}", alpha=0.7
        )

    # Tambahkan detail plot
    plt.title(f'Actual vs Predicted Values ({name})')
    plt.xlabel('Tanggal')
    plt.ylabel('Kurs')
    plt.legend()
    plt.grid(True)

    # Tampilkan plot
    plt.show()

# Tampilkan hasil evaluasi
print("HASIL EVALUASI MODEL")
for model, metrics in results.items():
    print(f"{model}:")
    print(f"  Average RMSE: {metrics['Average RMSE']:.2f}")
    print(f"  Average MAPE: {metrics['Average MAPE']:.2f}%")

# Cari model dengan Average MAPE terbaik (nilai terkecil)
best_model_name = min(results, key=lambda x: results[x]["Average MAPE"])
best_model = models[best_model_name]

```