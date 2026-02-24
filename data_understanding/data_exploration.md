---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.11.5
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# Data Exploration

Eksplorasi data (data exploration). Pada tahap ini, data dianalisis menggunakan statistik deskriptif dan visualisasi untuk melihat pola, distribusi, serta hubungan antarvariabel. Eksplorasi ini bertujuan untuk menemukan informasi awal yang tersembunyi dalam data, seperti kecenderungan nilai tertentu, adanya ketidakseimbangan kelas, atau korelasi antarvariabel. Hasil eksplorasi sangat penting karena dapat menjadi dasar dalam menentukan metode analisis atau model yang akan digunakan.

# Tujuan Data Exploration

Berikut adalah tujuan Data Exploration (Eksplorasi Data):
1. Mengetahui distribusi data, seperti pola penyebaran nilai (normal, miring/skewed, dll.).
2. Mengidentifikasi pola atau tren tertentu yang muncul dalam dataset.
3. Menganalisis hubungan antarvariabel, termasuk korelasi atau keterkaitan antar fitur.
4. Mendeteksi ketidakseimbangan data (imbalanced data), terutama pada kasus klasifikasi.
5. Menemukan outlier atau anomali awal yang dapat memengaruhi hasil analisis.
6. Mengidentifikasi variabel yang berpotensi berpengaruh signifikan terhadap target
7. Menjadi dasar dalam menentukan metode analisis atau pemodelan yang paling sesuai.

# Visualisasi Data
## Histogram 
Salah satu cara untuk kita mengetahui distribusi data ialah menggunakan Histogram, Histogram ialah grafik yang digunakan untuk menampilkan distribusi frekuensi data numerik dalam bentuk batang (bar) yang saling berdempetan. Histogram menunjukkan bagaimana data tersebar dalam beberapa interval nilai yang disebut bin atau kelas. <br>

Dalam histogram, sumbu horizontal (X) menunjukkan rentang nilai atau interval data, sedangkan sumbu vertikal (Y) menunjukkan jumlah frekuensi atau banyaknya data dalam setiap interval tersebut. Berbeda dengan diagram batang biasa, pada histogram tidak terdapat jarak antar batang karena data yang ditampilkan bersifat kontinu. Untuk Histogram tiap attribute sebagai berikut:



```{figure} ../image/hist_spl.PNG
---
width: 60%
align: center
---
Histogram Distribusi Frekuensi Fitur sepal_length
```

```{figure} ../image/hist_spw.PNG
---
width: 60%
align: center
---
Histogram Distribusi Frekuensi Fitur sepal_width
```

```{figure} ../image/hist_ptl.PNG
---
width: 60%
align: center
---
Histogram Distribusi Frekuensi Fitur petal_length
```

```{figure} ../image/hist_ptw.PNG
---
width: 60%
align: center
---
Histogram Distribusi Frekuensi Fitur petal_width
```
```{figure} ../image/hist_sp.PNG
---
width: 60%
align: center
---
Histogram Distribusi Frekuensi Fitur petal_width
```

## Scatter Plot

Lalu selanjutnya untuk membantu mengidentifikasi pola maupun tren ada Scatter Plot, Scatter plot adalah grafik yang digunakan untuk menampilkan hubungan antara dua variabel numerik dalam bentuk titik-titik pada bidang koordinat.

Pada scatter plot:
1. Sumbu X mewakili variabel pertama.
2. Sumbu Y mewakili variabel kedua.
3. Setiap titik merepresentasikan satu pasangan data (x, y).

Tujuan utama scatter plot adalah untuk melihat pola hubungan atau korelasi antara dua variabel.

Fungsi Scatter Plot:

1. Mengetahui hubungan antarvariabel (positif, negatif, atau tidak ada hubungan).
2. Melihat pola atau tren data, misalnya linear atau non-linear.
3. Mendeteksi outlier (data yang menyimpang jauh dari pola umum).
4. Membantu analisis regresi atau korelasi. 

### Scatter Plot sepal_length dan sepal_width

```{figure} ../image/sp_spl.PNG
---
width: 60%
align: center
---
Scatter Plot Fitur sepal_length dan sepal width
```
Berdasarkan scatter plot antara sepal_length dan sepal_width, terlihat bahwa nilai korelasi sebesar r = -0,11 menunjukkan hubungan negatif yang sangat lemah. Hal ini berarti peningkatan sepal_length tidak memiliki hubungan linear yang signifikan terhadap perubahan sepal_width.

### Scatter Plot petal dan petal_width

```{figure} ../image/sp_positif.PNG
---
width: 60%
align: center
---
Scatter Plot Fitur petal_length dan petal_width
```
Berikut jika diimplementasikan di Python:
```{code-cell}
:tags: [hide-input]
import pandas as pd
import matplotlib.pyplot as plt
df=pd.read_csv("../IRIS.csv")
print("Korelasi petal_length dan petal_width: ",round(df['petal_width'].corr(df['petal_length']), 2))
plt.scatter(df['petal_length'], df['petal_width'])
plt.xlabel('petal_length')
plt.ylabel('petal_width')
plt.title('Scatter Plot petal_length vs petal_width')
plt.show()

```
Berdasarkan scatter plot antara petal_length dan petal_width, terlihat adanya korelasi positif yang sangat kuat dengan nilai koefisien korelasi sebesar r = 0,96. Hal ini menunjukkan bahwa semakin tinggi nilai petal_length, maka petal_width juga cenderung meningkat secara linear. Nilai korelasi yang mendekati 1 mengindikasikan hubungan linear yang sangat kuat antara kedua variabel tersebut.

# Outlier

Outlier adalah nilai data yang menyimpang jauh dari sebagian besar data lainnya dalam suatu dataset. Outlier sering disebut juga sebagai data pencilan karena posisinya berada di luar pola umum distribusi data. Secara sederhana, outlier adalah data yang nilainya terlalu tinggi atau terlalu rendah dibandingkan data lain yang sejenis.

Ada beberapa metode untuk menentukan Outlier, untuk sekarang saya menggunakan metode Local Outlier Factor untuk menentukan Outliar pada Iris Flower Dataset.

```{figure} ../image/outlier.PNG
---
width: 60%
align: center
---
Outlier
```
```{code-cell} 
import pandas as pd
import numpy as np
from sklearn.neighbors import LocalOutlierFactor

df = pd.read_csv("../IRIS.csv")
X = df.select_dtypes(include=[np.number]).values
lof = LocalOutlierFactor(
    n_neighbors=20,
    contamination=0.10,
    metric='euclidean'
)

y_pred = lof.fit_predict(X)
df['LOF_Score'] = -lof.negative_outlier_factor_
df['Outlier'] = y_pred

outlier_lines = list(df[df['Outlier'] == -1].index + 1)

print("Total Outlier:", len(outlier_lines))
print("Nomor Baris Outlier:")
print(outlier_lines)
```
Dari metode Local Outlier Factor didapat 15 data sebagai outlier
