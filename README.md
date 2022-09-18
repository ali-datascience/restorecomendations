# Laporan Membuat Model Sistem Rekomendasi - Ali

## Project Overview
Bengaluru adalah tempat terbaik untuk pecinta kuliner. Jumlah restoran semakin hari semakin bertambah. Saat ini yang berdiri di sekitar 12.000 restoran. 
Dengan jumlah restoran yang begitu tinggi. Industri ini belum jenuh. Dan restoran baru buka setiap hari. 
Namun menjadi sulit bagi mereka untuk bersaing dengan restoran yang sudah mapan. 
Dataset ini bertujuan untuk menganalisis demografi lokasi dari sebuah resto. 
Yang paling penting itu akan membantu restoran baru dalam menentukan tema, menu, masakan dll untuk lokasi tertentu. 
Ini juga bertujuan untuk menemukan kesamaan antara lingkungan Bengaluru berdasarkan makanan.


## Business Understanding

### Problem Statement
- Ingin mengetahui bagaimana informasi behavior dari customer
- Bagaimana demografi dari sebuah lokasi. Jenis makanan apa yang lebih populer di suatu daerah
- Membantu restoran baru dalam menentukan tema, menu untuk lokasi tertentu untuk mendapatkan rekomendasi usaha kuliner apa yang sebaiknya 
dijalankan berdasar data yang ada
### Goals
- Mendapatkan informasi behavior dari customer
- Memperoleg insign demografi dari sebuah lokasi. Jenis makanan apa yang lebih populer di suatu daerah
- Membuat system  yang dapat merekomendasikan resto dari nama resto yang pernah dikunjungi sebelumnya dan dari tipe resto yang ingin dibuka.

### Solution Approach
Dalam melaksanakan proyek ini, saya menggunakan Content Based Filtering dan Collaborative Based Filtering

**Content Based Filtering**
Content Based Filtering adalah sistem rekomendasi yang merekomendasikan item sesuai dengan item yang disukai oleh pengguna di masa lampau.

Content Based mempelajari profil dan perilaku dari pengguna yang kemudian dari informasi tersebut dianalisa dan diproses sehingga menghasilkan sistem rekomendasi yang baik. Semakin banyak informasi yang diberikan ke sistem ini, maka sistem rekomendasi berbasis content based akan memiliki akurasi yang lebih baik.

Ada 2 informasi yang penting dalam sistem rekomendasi berbasis Content Based Filtering:
- Model preferensi pengguna
- Riwayat interaksi pengguna 

Kelebihan pada Content Based Filtering  dibandingkan dengan Collaborative Filtering:
Sistem rekomendasi dapat merekomendasikan item - item berdasarkan sejarah dari pengguna yang di mana setiap pengguna memiliki preferensinya masing - masing, sehingga Content Based Filtering dapat memberikan rekomendasi item yang subjektif dan tepat dengan pengguna.

Kekurangan pada Content Based Filtering  dibandingkan dengan Collaborative Filtering:
Sistem rekomendasi tidak dapat merekomendasikan item - item secara objektif, karena sistem rekomendasi Content Based Filtering memiliki bias terhadap riwayat profil dan perilaku pengguna.

Pada Content Based Filtering,  Tujuan dari projek ini adalah membuat sistem pemberi rekomendasi berbasis konten di mana ketika user akan menulis nama restoran, sistem Rekomendasi akan melihat ulasan restoran lain, 
dan Sistem akan merekomendasikan kepada user restoran lain dengan ulasan serupa dan mengurutkannya dari peringkat tertinggi.

**Collaborative Based Filtering**
Goal proyek Collaborative Filtered kali ini adalah menghasilkan rekomendasi sejumlah restoran yang sesuai dengan preferensi pengguna berdasarkan rating yang telah diberikan sebelumnya. Dari data rating pengguna, kita akan mengidentifikasi restoran-restoran yang mirip dan belum pernah dikunjungi oleh pengguna untuk direkomendasikan. 
Kita akan menggunakan teknik collaborative filtering untuk membuat rekomendasi ini..

## Data Understanding
Dataset untuk proyek ini dapat diunduh pada [tautan](https://www.kaggle.com/datasets/himanshupoddar/zomato-bangalore-restaurants)
Pada dataset kali ini, kita hanya akan menggunakan 1 buah dataframe yaitu zomato.csv, berikut keterangan dataset yang saya jelaskan dalam bahasa indonesia:

Berikut adalah deskripsi tiap kolom yang ada dari dataset

| Nama Kolom | Deskripsi |
| --- | --- |
|url|url resto|
|alamat|Alamat resto|
|nama|Nama resto|
|pesan_online|Pelayanan pembelian secara online|
|pesan_meja| Pelayanan pembelian meja|
|rating |Rata-rata rating resto|
|votes |Banyaknya rating yang diberikan pelanggan|
|telepon Nomor| telepon resto|
|lokasi |Lokasi resto|
|tipe_resto| Tipe resto|
|menu_favorit| Makanan yang paling disukai pelanggan|
|jenis_makanan| Jenis makanan di resto|
|dua_pelanggan| Banyaknya pelanggan yang memesan makanan untuk dua orang|
|review| Review pelanggan|
|menu| Menu di resto|
|tipe_makanan| Tipe makanan 



Berikut adalah visualisasi data yang berasal dari kedua dataframe tersebut:
Perlu diketahui pada visualisasi data yang diberikan di bawah ini berasal dari sample data dan bukan berasal dari keseluruhan data yang diberikan, disebabkan oleh besarnya data yang ada.
**Univariate Data Analysis**
Pada univariate data analysis, kita akan melihat barplot
  ![image](https://user-images.githubusercontent.com/84785795/190882489-f9936612-f214-44ce-ab40-886e429f6b87.png)  
  Pada barplot, saya menganalisa rating yang berasal dari rating dataset. Ternyata banyak juga pengguna yang memberi rating cukup baik.
  
**Multivariate Data Analysis**
Pada multivariate data analysis, saya menggunakan pairplot pada rating dataset.
![image](https://user-images.githubusercontent.com/84785795/190882530-8250b688-f636-45a6-9744-92254177eb93.png)


## Data Preparation
Pada proses data preparation, saya hanya melakukan beberapa hal sebelum masuk ke content dan collaborative based filtering:
- Data Cleansing Null Value
- Hapus data duplicated
- Rubah tipe data yang sesuai
- Removing '/5' from Rates
- Rubah Value Data Yes,No menjadi True/False
- Lower casing
- Removal of Punctuations
- Removal of Stopwords
- Removal of URLs
- Spelling correction

- **Pembagian Data Train dan Data Valid**
  Setelah semua data sudah terkumpul, kita perlu membagi data tersebut menjadi data latih dan data validasi, namun sebelum itu kita perlu menarik sample dari dataset yang sudah ada.


## Modeling
**Content Based Filtering**
Pada content Based Filtering, kita akan menggunakan TF-IDF Vectorizer untuk membangun sistem rekomendasi berdasarkan nama resto.

TF-IDF yang merupakan kepanjangan dari Term Frequency-Inverse Document Frequency memiliki fungsi untuk mengukur seberapa pentingnya suatu kata terhadap kata - kata lain dalam dokumen.
Kita umumnya menghitung skor untuk setiap kata untuk menandakan pentingnya dalam dokumen dan corpus. Metode sering digunakan dalam Information Retrieval dan Text Mining.

![image](https://user-images.githubusercontent.com/84785795/190882711-8d092146-46d4-4436-a35f-ab1220bc5ed5.png)

Pada code di atas, saya mendefinisikantfidf = TfidfVectorizer(analyzer='word', ngram_range=(1, 2), min_df=0, stop_words='english') dan mendapatkan kata - kata penting dalam kolomreviews_list.

Dalam sistem rekomendasi, kita perlu mencari cara supaya item yang kita rekomendasikan tidak terlalu jauh dari data pusat, oleh karena itu kita butuh derajat kesamaan pada item, dalam proyek ini, resto dengan derajat kesamaan antar resto dengan cosine similarity.

Kemudian kita membutuhkan fungsi recommend untuk menampilkan system rekomendasi kepada user.

Berikut adalah contoh output system rekomendasi yang kita buat

![image](https://user-images.githubusercontent.com/84785795/190882786-6ce107da-eabb-4fd5-b950-a08a904b8e8c.png)


**Collaborative Based Filtering**
Model yang akan dipakai dalam Collaborative Based Filtering adalah RecommenderNet.
Selanjutnya kita melakukan proses compile pada model dengan binary crossentropy sebagai loss function, adam sebagai optimizer, dan RMSE sebagai metrik dari model.

Setelah proses compile sudah selesai, kita akan melatih model dengan batch_size 5 dan 20 epochs.

Untuk mendapatkan rekomendasi, kita perlu menambahkan beberapa kode tambahan, dimulai dengan mengambil user_id secara acak dari rating_dataset. Dari user_id ini kita perlu mengetahui buku - buku apa saja yang pernah dibaca dan yang belum pernah dibaca, sehingga kita hanya dapat merekomendasikan buku - buku yang belum dibaca.

Setelah itu kita akan mendapatkan rekomendasi sesuai dengan user_id yang didapatkan.

Hasil rekomendasi buku untuk user_id = 278221 adalah:
![recommendedBooks](https://raw.githubusercontent.com/farelarden/Dicoding-SIB/main/26.JPG))

## Evaluation

**Content Based Filtering**
Pada Content Based Filtering, saya menggunakan akurasi sebagai metrik evaluasi.
Akurasi pada Content Based Filtering adalah:
Jumlah buku yang direkomendasikan sesuai dengan penulis buku / Jumlah buku yang direkomendasikan

Dalam mengaplikasi metrik akurasi pada kode adalah dengan membuat variabel books_that_have_been_read_row yang akan mengambil satu row dari buku yang pernah dibaca sebelumnya, dan juga membuat variabel books_that_have_been_read_author adalah penulis buku dari buku yang pernah dibaca sebelumnya.

Saya juga membuat variabel  book_recommendation_authors merupakan sebuah list yang terdiri dari penulis - penulis dari buku - buku yang direkomendasikan oleh sistem.
Kemudian saya membuat looping yang merupakan proses manual di mana setiap penulis dari buku yang direkomendasikan akan dicek, apabila sama, maka variabel real_author akan bertambah 1.

Kemudian di bawah ini adalah hasil dari akurasi dari model sistem rekomendasi, di mana jumlah buku yang direkomendasikan sesuai dengan penulis buku (Variabel real_author) / Jumlah buku yang direkomendasikan (5).

![Accuracy](https://raw.githubusercontent.com/farelarden/Dicoding-SIB/main/27.JPG)

Kelebihan pada evaluasi metrik akurasi terletak pada kesederhanaannya untuk dipahami, karena metrik akurasi hanyalah jumlah yang benar dibandingkan dengan keseluruhan jawaban.

Sedangkan kekurangan pada evaluasi metrik adalah terletak pada kesederhanannya pula, karena metrik akurasi hanya menghitung jumlah yang benar dibandingkan dengan keseluruhan jawaban dan tidak memperhitungkan aspek lainnya.

**Collaborative Based Filtering**
Pada Collaborative Based Filtering, kita memiliki evaluasi metrik RMSE atau root-mean-square error.RMSE adalah ukuran yang sering digunakan untuk perbedaan antara nilai (nilai sampel atau populasi) yang diprediksi oleh model atau penduga dan nilai yang diamati.Berbeda dengan MSE, RMSE adalah hasil dari akar MSE membuat RMSE memiliki nilai yang lebih kecil dibandingkan dengan MSE.

Berikut adalah rumus RMSE:
![RMSE](https://raw.githubusercontent.com/farelarden/Dicoding-SIB/main/22.JPG)
Dimana,
At = Nilai data Aktual
Ft = Nilai hasil peramalan
N= banyaknya data
∑ = Summation (Jumlahkan keseluruhan  nilai)

RMSE memiliki nilai yang lebih kecil daripada MSE, dengan adanya nilai kecil ini dapat menjadi kelebihan dan kekurangan tersendiri. Kelebihan dari nilai kecil adalah kita tidak perlu takut karena nilai error nya kecil dan kita dapat langsung masuk ke tahap selanjutnya, namun dengan nilai error yang kecil, kita dapat menjadi terlalu percaya diri dengan modelnya tanpa melihat posibilitas overfitting atau undefitting.

RMSE terlebih dahulu kita definisikan pada bagian metrik dalam model, kemudian kita visualisasikan lewat grafik.
![Grafik](https://raw.githubusercontent.com/farelarden/Dicoding-SIB/main/24.JPG))
