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
Pada univariate data analysis, kita akan melihat 2 barplot:
- Barplot Pertama
  ![Barplot Pertama](https://raw.githubusercontent.com/farelarden/Dicoding-SIB/main/15.JPG))

    Pada barplot pertama, saya menganalisa rating yang berasal dari rating dataset. Ternyata banyak pengguna yang memberi penilian 0 pada buku - buku yang mereka telah baca. Penilian 0 dari 10 tetaplah valid, sehingga kita tidak dapat menganggap nilai 0 ini sebagai nilai NaN.
- Barplot Kedua
  ![Barplot Kedua](https://raw.githubusercontent.com/farelarden/Dicoding-SIB/main/17.JPG)) 
    Pada barplot kedua, saya menganalisa tahun terbitnya buku, Ternyata banyak sekali buku yang terbit pada tahun 2002.
**Multivariate Data Analysis**
Pada multivariate data analysis, saya menggunakan pairplot pada rating dataset.
![pairplot](https://raw.githubusercontent.com/farelarden/Dicoding-SIB/main/18.JPG))

## Data Preparation
Pada proses data preparation, saya hanya melakukan 2 hal sebelum masuk ke content dan collaborative based filtering:
- **Dropna**
  Dropna perlu digunakan dalam proses data preparation untuk membuang seluruh row yang memiliki NaN values. Sebuah model tidak dapat melakukan training apabila terdapat nilai NaN pada data latih. 
- **Drop Duplicates**
  Drop dulicates digunakan dalam proses data preparation untuk membuang data - data yang terduplikasi. Adanya data yang terduplikasi membuat model berlatih menggunakan data yang  berulang.

**Content Based Filtering**
Pada content Based Filtering, data preparation yang diperlukan ada 2, yaitu:
- **Dataframe dari buku menjadi sebuah list**
  Perubahan dari dataseries menjadi list dipenuhi dengan menggunakan .tolist() method. Proses ini diperlukan karena list ini akan digunakan pada tahap selanjutnya menjadi dictionary baru yg akan menjadi landasan pada sistem rekomendasi
- **Memasukkan List ke Dictionary**
  Setelah kita membuat list, kita perlu membuat dictionary yang digunakan untuk memnentukan pasangan key-value pada book_ISBN, book_title, book_author, dan book_year_of_publication. 

**Collaborative Based Filtering**
Data preparation yang diperlukan pada sistem collaborative based filtering dimulai dengan menyandikan user_id pada rating_dataset dan ISBN pada book_dataset menjadi integer.

Setelah disandikan, jumlah dari user_id dan ISBN tersebut akan disimpan pada num_users dan num_book.

- **Pembagian Data Train dan Data Valid**
  Setelah semua data sudah terkumpul, kita perlu membagi data tersebut menjadi data latih dan data validasi, namun sebelum itu kita perlu menarik sample dari dataset yang sudah ada.


## Modeling
**Content Based Filtering**
Pada content Based Filtering, kita akan menggunakan TF-IDF Vectorizer untuk membangun sistem rekomendasi berdasarkan penulis buku.

TF-IDF yang merupakan kepanjangan dari Term Frequency-Inverse Document Frequency memiliki fungsi untuk mengukur seberapa pentingnya suatu kata terhadap kata - kata lain dalam dokumen.
Kita umumnya menghitung skor untuk setiap kata untuk menandakan pentingnya dalam dokumen dan corpus. Metode sering digunakan dalam Information Retrieval dan Text Mining.

![TF-IDF initialization](https://raw.githubusercontent.com/farelarden/Dicoding-SIB/main/19.JPG))
Pada code di atas, saya mendefinisikan tf = TfidfVectorizer() dan mendapatkan kata - kata penting dalam kolom book_author yang berasal dari attribut .get_feature_names() dari tf.

Kemudian dari string yang didapat akan dimasukkan ke dalam matriks. Pada proyek ini, saya menggunakan tfidf_matrix sebagai matriks.

Dalam sistem rekomendasi, kita perlu mencari cara supaya item yang kita rekomendasikan tidak terlalu jauh dari data pusat, oleh karena itu kita butuh derajat kesamaan pada item, dalam proyek ini, buku dengan derajat kesamaan antar buku dengan cosine similarity.

Kemudian kita membutuhkan fungsi author_recommendation di mana atribut argpartition berguna untuk mengambil sejumlah nilai k, dalam fungsi ini 5 tertinggi dari tingkat kesamaan yang berasal dari dataframe cosine_sim_df. Jumlah rekomendasi yang akan diberikan nantinya akan ditentukan oleh nilai k pada fungsi author_recommendation.

**book[book.book_title.eq(books_that_have_been_read)]**

Kode di atas dibutuhkan untuk mencari buku yang memiliki kemiripan dengan buku yang sudah kita baca.

**recommendations = author_recommendations(books_that_have_been_read, cosine_sim_df, book[['book_title', 'book_author']])**

Setelah kita menjalankan kode di atas, kita akan mendapatkan 5 buku rekomendasi yang berasal dari penulis yang sama.
Berikut adalah hasil rekomendasi untuk buku "The Diaries of Adam and Eve":

![recommendedBooks](https://raw.githubusercontent.com/farelarden/Dicoding-SIB/main/23.JPG))

Terlihat dari buku - buku yang direkomendasikan berasal dari penulis buku yang sama. Buku yang memiliki penulis yang sama memiliki kesamaan konten, gaya penulisan, dan bahasa membuat pembaca nyaman dan tidak perlu beradaptasi dengan perubahan gaya penulisan dan bahasa yang ada.

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
