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
- Bagaimana  behavior dari customer terhadap pemesanan pada sebuah resto
- Bagaimana demografi dari sebuah lokasi. Jenis makanan apa yang lebih populer di suatu daerah
- Membantu restoran baru dalam menentukan tema, menu untuk lokasi tertentu untuk mendapatkan rekomendasi usaha kuliner apa yang sebaiknya 
dijalankan berdasar data yang ada
### Goals
- Mendapatkan informasi behavior dari customer 
- Memperoleh insight demografi dari sebuah lokasi. Jenis makanan apa yang lebih populer di suatu daerah
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
Pada dataset kali ini, kita hanya akan menggunakan 1 buah dataframe yaitu zomato.csv, dataset ini terdiri dari 23569 baris dan 17 kolom. berikut keterangan dataset yang saya jelaskan dalam bahasa indonesia:

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


Didalam dataset ini ditemukan beberapa anomaly data sebagai berikut :
- Ada missing value pada kolom **rate**
- Kolom **phone** ada missing value
- Ada missing value pada **location**
- **rest_type** ditemukan missing value
- **dish_liked** ditemukan missing value
- **cuisines ** ditemukan missing value
- **approx_cost(for two people)**	 ditemukan missing value, 
- Pada kolom rate tipe data masih object


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
- Data Cleansing Null Value :
  Menghapus missing value dengan fungsi dropna pada kolom yang terdapat missing value
- Menghapus kolom-kolom yang tidak digunakan karena secara pertimbangan bisnis kolom-kolom tersebut tidak berarti, kolom-kolom tersebut adalah 'url','dish_liked','phone'
- Hapus data duplicated dengan fungsi drop_duplicates agar tidak ada duplikasi dalam data
- Rubah tipe data yang sesuai seperti kolom rate, diubah menjadi float karena rating seharusnya diisi oleh angka
- Removing '/5' from Rates, agar data lebih mudah diolah
- Rubah Value Data Yes,No menjadi True/False pada kolom online_order dan book_table
- Melakukan perubahan terhadap nama kolom agar lebih rapih, yaitu pada kolom approx_cost(for two people) menjadi cost, listed_in(type) menjadi type dan listed_in(city) menjadi city 
- Dilakukan Text cleansing pada kolom reviews_list dengan metode :Lower casing, Removal of Punctuations,Removal of Stopwords dengan librari dari nltk.corpus, Removal of URLs, Spelling correction


**Pembagian Data Train dan Data Valid**
  Setelah semua data sudah terkumpul, kita perlu membagi data tersebut menjadi data latih dan data validasi, namun sebelum itu kita perlu menarik sample dari dataset yang sudah ada.

## EDA

- Onine VS Offline Order <br>
![image](https://user-images.githubusercontent.com/84785795/190883317-7b22b92e-2354-44a6-91c9-d5ae98ee892f.png)<br>

Sebanyak 70 persen customer lebih memilih order secara online dan sisanya offline<br>

- Book Table Vs No<br>

![image](https://user-images.githubusercontent.com/84785795/190883335-bb24059c-b73c-4547-bbc6-218748f69929.png)<br>
Dari grafik dapat dilihat behavior customer lebih banyak yang pesan tanpa booking table terlebih dahulu<br>

- Customer Order Behavior By Restoran Type<br>
![image](https://user-images.githubusercontent.com/84785795/190883343-f3ccd8af-f42f-4a5a-b6a3-861a4471cba3.png)<br>

Secara keseluruhan Dari grafik dapat dilihat customer lebih banyak melakukan pembelian secara online sebanyak 69.2% dibanding offline sekitar 30.8 %, dan tipe restoran paling banyak disukai yaitu Casual Dining kemudian disusul dengan Quick Bites, Cafe dan Delivery.<br>

- Customer Order Offline dilihat dari Tipe Restoran<br>
![image](https://user-images.githubusercontent.com/84785795/190883358-276c145e-c5ad-4d6b-a6d2-02e772ac3f6a.png)<br>

Customer yang melakukan pembelian offline paling banyak berada di daerah Whitefield, Indiranagar, JP Nagar, Marathalli dan Basavanaguni dengan tipe restoran paling banyak adalah Casual Dinning<br>

- Customer Order Online dilihat dari Tipe Restoran<br>
![image](https://user-images.githubusercontent.com/84785795/190883387-d8d3c604-31c7-467d-9862-c7a9b37559cf.png)<br>

Customer yang melakukan pembelian online paling banyak berada di daerah Whitefield, HSR, Indiranagar, BTM dan Marathalli Dengan tipe restoran di dominasi oleh Casual Dinning dan Quiq Bites<br>

- Food Type<br>
![image](https://user-images.githubusercontent.com/84785795/190883406-7d7faea1-181b-40be-ace4-dc5ea14d7649.png)<br>

Customer paling sering melakukan delivery order<br>

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

|                                        |                                          cuisines | Mean Rating | cost |              location |                  city |
|---------------------------------------:|--------------------------------------------------:|------------:|-----:|----------------------:|----------------------:|
|       Byg Brewski Brewing Company      | Continental, North Indian, Italian, South Indi... |        5.00 |  1.6 |         Sarjapur Road |             Bellandur |
|                Flechazo                |           Asian, Mediterranean, North Indian, BBQ |        4.87 |  1.4 |            Whitefield |            Whitefield |
|        Ab'S - Absolute Barbecues       |        European, Mediterranean, North Indian, BBQ |        4.86 |  1.6 |                   BTM |                   BTM |
|               Biergarten               |        Continental, European, BBQ, Chinese, Asian |        4.83 |  2.4 |            Whitefield |            Whitefield |
|               Biergarten               | Continental, North Indian, Chinese, European, ... |        4.83 |  2.1 | Koramangala 5th Block | Koramangala 6th Block |
|                Communiti               |                           Continental, BBQ, Salad |        4.67 |  1.5 |        Residency Road |          Brigade Road |
|                Hammered                |   North Indian, Thai, Japanese, Continental, Cafe |        4.65 |  1.3 |       Cunningham Road |          Lavelle Road |
| Brew And Barbeque - A Microbrewery Pub |             Continental, North Indian, BBQ, Steak |        4.64 |  1.4 |          Marathahalli |          Marathahalli |
|             The Globe Grub             |         Continental, North Indian, Asian, Italian |        4.48 |  1.3 |          Marathahalli |          Marathahalli |
|               Jalsa Gold               |                    North Indian, Mughlai, Italian |        4.48 |  1.3 |          Marathahalli |          Marathahalli |


**Collaborative Based Filtering**
Model yang akan dipakai dalam Collaborative Based Filtering adalah RecommenderNet.
Selanjutnya kita melakukan proses compile pada model dengan binary crossentropy sebagai loss function, adam sebagai optimizer, dan RMSE sebagai metrik dari model.

Setelah proses compile sudah selesai, kita akan melatih model dengan batch_size 5 dan 20 epochs.

Untuk mendapatkan rekomendasi, kita perlu menambahkan beberapa kode tambahan, dimulai dengan mengambil resto_type_id secara acak dari rating_dataset. Dari resto_type_id ini kita perlu mengetahui resto apa saja yang pernah dikunjungi dan yang belum dikunjungi, sehingga kita hanya dapat merekomendasikan resto-resto yang belum dikunjungi.

Setelah itu kita akan mendapatkan rekomendasi sesuai dengan resto_type_id yang didapatkan.

Hasil rekomendasi buku untuk resto_type_id = 60 adalah:
<br>
Showing recommendations for resto_type_id: 60
===========================
Resto with high ratings from user
--------------------------------
House Of Commons : Continental, Asian, North Indian . Rating: 4.8 .Resto Type: Bar , Location:  Koramangala 5th Block . Dish Like: Cocktails, Chaap, Tandoori Chicken, Mocktails, Beer, Ghee Roast Mutton, Martini
One For The Road : Continental, North Indian, Finger Food . Rating: 4.5 .Resto Type: Bar , Location:  Nagawara . Dish Like: Cocktails, Pasta, Pizza, Mocktails, Thai Crispy Chicken, Beer, Chicken Wings
The Old Fashioned Bar : Finger Food, North Indian, Continental . Rating: 4.5 .Resto Type: Bar , Location:  Koramangala 6th Block . Dish Like: Cocktails, Long Island Iced Tea, Draught Beer, Masala Peanuts, Pizza, Mocktails, Chicken Tikka
Myu Bar at Gilly's Redefined : American, North Indian, Pizza, Finger Food, Continental, Italian . Rating: 4.4 .Resto Type: Microbrewery &  Bar , Location:  Koramangala 4th Block . Dish Like: Mai Thai, Sangria, Cocktails, Pizza, Biryani, Beer, Chicken Nachos
Drunken Monkey : Beverages, Fast Food . Rating: 4.4 .Resto Type: Beverage Shop &  Quick Bites , Location:  Bannerghatta Road . Dish Like: Chicken Tikka Sandwich, Hot Chocolate Brownie, Red Velvet Shake, Chicken Sandwiches, French Fries, Cheese Sandwich, Irish Cream Coffee
--------------------------------
Top 5 resto recommendation
--------------------------------
The Globe Grub : Continental, North Indian, Asian, Italian Resto Type: Casual Dining , Location:  BTM . Dish Like: Prawn, Shahi Tukda, Barfi, Veg Mushroom, Pizza, Momos, Masala Lemonade
Samaroh : North Indian Resto Type: Casual Dining , Location:  Bellandur . Dish Like: Kulfi, Rajasthani Thali, Sweet Paan, Buttermilk, Rasmalai, Chaat, Lassi
Balle-Licious Kitchen : North Indian Resto Type: Delivery , Location:  HSR . Dish Like: Dal Makhani, Butter Chicken, Reshmi Kebab, Shahi Paneer, Butter Naan, Shahi Tukda, Chilli Chicken
Little Chef : Chinese Resto Type: Casual Dining , Location:  Richmond Road . Dish Like: Dragon Chicken, Hunan Chicken
Quench Bar : Seafood, Mexican, Continental, Finger Food Resto Type: Bar , Location:  Domlur . Dish Like: Rooftop Ambience, Decor
<br>

System rekomendasi diatas menampilkan rekomendasi type resto yang belum pernah dikunjungi oleh investor/bukan pilihan investor. dalam kasus ini investor ingin membuka usaha dan mendapatkan rekomendasi sesuai tipe restoran yang dia sukai, maka dapat menggunakan Showing recommendations for users.

|       |                   resto_name |                  resto_type |                                  resto_dish_liked |                                    resto_cuisines |        resto_location |            resto_city |                                     resto_address | resto_rate |
|------:|-----------------------------:|----------------------------:|--------------------------------------------------:|--------------------------------------------------:|----------------------:|----------------------:|--------------------------------------------------:|-----------:|
|  4388 |             House Of Commons |                         Bar | Cocktails, Chaap, Tandoori Chicken, Mocktails,... |                  Continental, Asian, North Indian | Koramangala 5th Block |                   BTM | 122/B, Jyothi Nivas Road, 5th Block, Koramanga... |        4.8 |
| 10658 |             One For The Road |                         Bar | Cocktails, Pasta, Pizza, Mocktails, Thai Crisp... |            Continental, North Indian, Finger Food |              Nagawara |          Kalyan Nagar | 3rd Floor, Elements Mall, Thanisandra Main Roa... |        4.5 |
| 12553 |        The Old Fashioned Bar |                         Bar | Cocktails, Long Island Iced Tea, Draught Beer,... |            Finger Food, North Indian, Continental | Koramangala 6th Block | Koramangala 4th Block | 470, 80 Feet Road, Koramangala 6th Block, Bang... |        4.5 |
|  4691 | Myu Bar at Gilly's Redefined |          Microbrewery & Bar | Mai Thai, Sangria, Cocktails, Pizza, Biryani, ... | American, North Indian, Pizza, Finger Food, Co... | Koramangala 4th Block |                   BTM | 413, 100 Feet Road, Koramangala 4th Block, Ban... |        4.4 |
|  588  |               Drunken Monkey | Beverage Shop & Quick Bites | Chicken Tikka Sandwich, Hot Chocolate Brownie,... |                              Beverages, Fast Food |     Bannerghatta Road |     Bannerghatta Road | 54/55, 3rd Cross, Omkar Nagar, Arekere Gate, B... |        4.4 |
<br>


## Evaluation

**Content Based Filtering**
Pada Content Based Filtering, saya menggunakan akurasi sebagai metrik evaluasi.
Akurasi pada Content Based Filtering adalah:
Jumlah resto yang direkomendasikan sesuai dengan location resto / Jumlah resto yang direkomendasikan

Di bawah ini adalah hasil dari akurasi dari model sistem rekomendasi, di manaJumlah resto yang direkomendasikan sesuai denganlocation resto / Jumlah resto yang direkomendasikan.

![image](https://user-images.githubusercontent.com/84785795/190883168-72c4a931-7df7-43f9-81b1-2d844ac78298.png)


Kelebihan pada evaluasi metrik akurasi terletak pada kesederhanaannya untuk dipahami, karena metrik akurasi hanyalah jumlah yang benar dibandingkan dengan keseluruhan jawaban.

Sedangkan kekurangan pada evaluasi metrik adalah terletak pada kesederhanannya pula, karena metrik akurasi hanya menghitung jumlah yang benar dibandingkan dengan keseluruhan jawaban dan tidak memperhitungkan aspek lainnya.

**Collaborative Based Filtering**
Pada Collaborative Based Filtering, kita memiliki evaluasi metrik RMSE atau root-mean-square error.RMSE adalah ukuran yang sering digunakan untuk perbedaan antara nilai (nilai sampel atau populasi) yang diprediksi oleh model atau penduga dan nilai yang diamati.Berbeda dengan MSE, RMSE adalah hasil dari akar MSE membuat RMSE memiliki nilai yang lebih kecil dibandingkan dengan MSE.

Berikut adalah rumus RMSE:

![image](https://user-images.githubusercontent.com/84785795/190883262-238e2c93-461c-4bfb-939e-e0fadb827a98.png)


Dimana,
At = Nilai data Aktual
Ft = Nilai hasil peramalan
N= banyaknya data
∑ = Summation (Jumlahkan keseluruhan  nilai)

RMSE memiliki nilai yang lebih kecil daripada MSE, dengan adanya nilai kecil ini dapat menjadi kelebihan dan kekurangan tersendiri. Kelebihan dari nilai kecil adalah kita tidak perlu takut karena nilai error nya kecil dan kita dapat langsung masuk ke tahap selanjutnya, namun dengan nilai error yang kecil, kita dapat menjadi terlalu percaya diri dengan modelnya tanpa melihat posibilitas overfitting atau undefitting.

RMSE terlebih dahulu kita definisikan pada bagian metrik dalam model, kemudian kita visualisasikan lewat grafik.

![image](https://user-images.githubusercontent.com/84785795/190883289-81a03796-2735-4d85-8cca-cb78913d2739.png)


proses training model cukup smooth dan model konvergen pada epochs sekitar 20. Dari proses ini, kita memperoleh nilai error akhir sebesar sekitar 0.12 dan error pada data validasi sebesar 0.13. Nilai tersebut cukup bagus untuk sistem rekomendasi
