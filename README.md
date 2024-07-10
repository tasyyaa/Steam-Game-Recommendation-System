# Laporan Proyek Machine Learning - Tasya Putri Aliya
## Steam Game Recommendation System
Laporan ini dibentuk sebagai syarat untuk menyelesaikan salah satu Course Dicoding yaitu Machine Learning Terapan

## Project Overview
Recommender System (RSs) merupakan sebuah sistem yang digunakan untuk membantu users untuk menemukan item baru atau servis, termasuk buku, musik, transportasi, atau bahkan orang lain berdasarkan informasi dari user tersebut atau item yang direkomendasikan [[1]](https://ieeexplore.ieee.org/abstract/document/1423975/?casa_token=1YmZ_-GXuzMAAAAA:WbxnfNXofelZPRJ-a857zUxrysorWgpX4rAGuQyrXzwuxEtdujnCJnofm5t1UMOr6ODTl5HB1qsH0A). Sistem ini memberikan peran yang penting dalam membuat keputusan, membantu user untuk memaksimalkan profit [[2]](https://www.sciencedirect.com/science/article/pii/S0020025507004641?casa_token=poi0dE2cFwIAAAAA:TCf2ey4J7PSRiyAA5Ks47yhwJ3Ppy0MGl7T9RfLbKZqxfph7SQOoyvTElf0dKdNGy2byKFvwouA), atau meminimalisasi risiko [[3]](https://link.springer.com/chapter/10.1007/978-3-642-42054-2_8). Saat ini RSs sudah digunakan pada banyak perusahaan yang berfokus terhadap layanan jasa dan penjualan, seperti Google, Twitter, Linkedin, dan Netflix [[4]](https://dl.acm.org/doi/abs/10.1145/2507157.2507160).

Selain aplikasi-aplikasi yang disebutkan sebelumnya, terdapat aplikasi lain yang juga menggunakan Recommender System ini untuk meningkatkan pengalaman berbelanja user. Menurut data yang dikeluarkan oleh Steam, user aktif Steam sudah meningkat pada platform hingga 47 Juta, dan user aktif per bulannya adalah 90 Juta, serta peningkatan perbulan dari user dengan pembelian yang valid adalah 1.6 Juta [[5]](https://steamcommunity.com/groups/steamworks/announcements/detail/1697194621363928453). Salah satu alasan utama mengapa Steam bisa tumbuh sangat cepat karena adanya kemampuan mesin pencarian yang sangat baik yang dimention oleh Steam di dalam reportnya. Saat ini, Steam sedang mengembangkan Recommender System yang bekerja dengan Machine Learning untuk dapat menemukan games yang sesuai dengan preferensi penggemar, Steam sendiri juga sedang membangun lebih banyak fitur live dan apresiasi serta terus melakukan evaluasi desain keseluruhan dari toko tersebut.

Di sisi lain, sistem rekomendasi terkadang tidak berfungsi sebaik yang diharapkan. Salah satu alasan untuk ini adalah Efek Matius [[6]](https://link.springer.com/chapter/10.1007/978-3-030-44900-1_9#citeas)[[7]](https://www.science.org/doi/abs/10.1126/science.159.3810.56), yang berarti yang kaya semakin kaya dan yang miskin semakin miskin, selalu muncul di bidang ilmu sosial, juga dapat diterapkan pada pasar game. Game-game yang dikembangkan oleh perusahaan pengembang game besar menerima lebih banyak anggaran untuk iklan sehingga mereka bisa sangat populer. Game-game populer akan muncul di posisi teratas pada halaman web toko dan menarik lebih banyak pengguna untuk membeli [[5]](https://link.springer.com/chapter/10.1007/978-3-030-44900-1_9#citeas). Oleh karena itu, diperlukan suatu sistem rekomendasi yang akan memanfaatkan data dengan optimal dengan banyak variabel-variabel lain yang mungkin dapat menyelesaikan permasalahan tersebut.

## Business Understanding
Berdasarkan latar belakang project yang telah disusun, maka akan didapatkan problem statements, goals, dan solution statements dari project ini.

### Problem Statements
Berdasarkan latar belakang yang ada berikut merupakan problem statements yang disusun dalam proyek ini:
1. Berdasarkan data mengenai games yang ada, bagaimana membuat rekomendasi dengan Content Based Filtering?
2. Berdasarkan data users yang dimiliki, bagaimana Steam dapat membuat Collaborative Filtering untuk menampilkan rekomendasi games?
   
### Goals
Berdasarkan latar belakang dan problem statements yang telah dirumuskan, berikut merupakan goals dari proyek ini:
1. Menghasilkan sejumlah rekomendasi games yang dipersonalisasi pengguna menggunakan teknik Content Based Filtering.
2. Menghasilkan sejumlah rekomendasi games sesuai dengan pengguna dengan menggunakan Collaborative Filtering.
   
### Solution Statements
Berdasarkan problem statements dan goals yang telah dibuat, maka terdapat solution statements untuk menyelesaikannya pada proyek ini, yaitu:
1. Melakukan data understanding dan preparation sebelum menggunakan data ke dalam model agar dataset sudah bersih dan layak untuk digunakan.
2. Membangun dan mengevaluasi model yang dibangun untuk Content Based Filtering menggunakan gabungan TF-IDF Metrix dan K-Nearest Neighbor
3. Membangun dan mengevaluasi model yang dibangun untuk Collaborative Filtering menggunakan User-Based dan Nearest Neighbor

   
## Data Understanding
Dataset yang digunakan pada proyek Steam Games Recommendation ini merupakan dataset yang dimiliki oleh [Anton Kozyriev](https://www.kaggle.com/antonkozyriev) yang dirilisnya di platform Kaggle dan terakhir diupdate pada tahun 2024 yang dapat diakses pada link berikut [Game Recommendation on Steam](https://www.kaggle.com/datasets/antonkozyriev/game-recommendations-on-steam).
Pada tahap data understanding ini dilakukan beberapa tahapan yang bertujuan untuk dapat memehami lebih lanjut mengenai dataset yang sedang digunakan. Pertama-tama adalah penjelasan mengenai setiap variabel atau feature yang akan digunakan yang terdapat pada dataset ini

### Entitas pada Dataset
Dataset terdiri atas 3 entitas utama, yaitu:
1. games.csv: sebuah tabel informasi permainan (atau add-on) tentang peringkat, harga dalam dolar AS $, tanggal rilis, dll. Informasi tambahan yang tidak berbentuk tabel tentang permainan, seperti deskripsi dan tag, terdapat dalam file metadata yang berisi
2. users.csv: sebuah tabel informasi publik profil pengguna: jumlah produk yang dibeli dan ulasan yang dipublikasikan;
3. recommendations.csv: sebuah tabel ulasan pengguna yang berisi apakah pengguna merekomendasikan suatu produk. Tabel ini mewakili hubungan banyak-banyak antara entitas permainan dan entitas pengguna.

Terdapat satu metadata dari games yang didalamnya berisi deskripsi dan genre dari games, seperti berikut:
1. games_meta.json: sebuah metadata yang berisi deskripsi yang dimiliki game serta list dari genre yang dimiliki oleh games. Entitas ini terpisah dari entitas games karena entitas ini memiliki tags yang berbentuk list berupa tags sehingga tidak dapat dijadikan .csv

Dari entitas-entitas tersebut terdapat variabel-variabel yang ada di dalamnya, berikut variabel-variabel tersebut:
#### Variabel-Variabel dalam games.csv
1. **app_id**: ID dari game yang ada
2. **title**: Nama dari game
3. **date_release**: Tanggal game rilis
4. **win**: Apakah dapat dimainkan di Windows
5. **mac**: Apakah dapat dimainkan di Mac
6. **linux**: Apakah dapat dimainkan di linux
7. **rating**: rating dari games
8. **positive ratio**: Berapa skor ratio penilaian postif dari user berdasarkan seluruh penilaian
9. **user_reviews**: Review yang diberikan oleh user ke game
10. **price_final**: Harga akhir setelah adanya pemotongan diskon
11. **price_original**: Harga asli dari game
12. **discount**: Diskon yang dimiliki oleh suatu game
13. **steam_deck**: Apakah game dapat dimainkan pada steam deck

#### Variabel-Variabel dalam users.csv
1. **user_id**: ID dari user yang ada
2. **products**: Jumlah produk yang dibeli oleh users
3. **review**: Jumlah ulasan yang telah dipublikasikan

#### Variabel-Variabel dalam recommendations.csv
1. **app_id**: ID dari game yang ada
2. **helpful**: Jumlah user lain yang menganggap review user membantu
3. **funny**: Jumlah user lain yang menganggap review user lucu
4. **date**: Tanggal review dipublikasikan
5. **is_recommended**: Apakah user merekomendasikan game tersebut
6. **hours**: Berapa jam user memainkan game
7. **user_id**: ID dari user yang ada
8. **review_id**: ID dari review yang ada

#### Variabel-variabel dalam games_meta.json
1. **app_id**: ID dari aplikasi yang ada
2. **description**: Deskripsi dari game
3. **tags**: Genre game

### Univariate Explanatory Data Analysis
Merupakan suatu analisis untuk jenis analisis data yang melibatkan pemeriksaan dan deskripsi satu variabel yang bertujuan untuk memahami dan menggambarkan karakteristik dasar dari variabel tersebut. Berikut merupakan hasil dari univariate explanatory data analysis:

### games variabel
Pada proses ini akan dilakukan analisis dari variabel dan jenis datanya serta karakteristik dari entitas games

![image](https://github.com/tasyyaa/Steam-Game-Recommendation-System/assets/100066633/9479bbef-165d-4ca3-aff1-49be0a9a5370)
![image](https://github.com/tasyyaa/Steam-Game-Recommendation-System/assets/100066633/6fa50653-6927-4b5d-a517-4ea40d4a7959)

Berdasarkan hasil di atas dapat dilihat bahwa games memiliki 13 variabel yang di dalamnya terdapat nama games, tanggal rilis, harga, dan lain-lain. Dapat juga dilihat bahwa terdapat 50.872 games yang ada pada data dengan contoh bentuk data seperti yang ditunjukkan di gambar. Namun, dapat dilihat bahwa data tidak mengandng informasi genre games yang akan digunakan di dalam proyek, oleh karena itu diperlukan penggabungan dengan dataset lain yaitu games_meta.json

#### games_meta variabel
Pada proses ini dilakukan analisis dari variabel dan jenis data dari entitas games_meta.json

![image](https://github.com/tasyyaa/Steam-Game-Recommendation-System/assets/100066633/5c947cde-9ae2-40b0-9c72-2b96d400098d)
![image](https://github.com/tasyyaa/Steam-Game-Recommendation-System/assets/100066633/21daca14-ffff-4fe1-be13-b0f10985e560)

Berdasarkan gambar yang telah ditautkan dapat dilihat bahwa dataset ini merupakan dataset dari games yang memiliki deskripsi dan genre dari games. Genre ini akan digunakan sebagai salah satu variabel untuk menemukan rekomendasi berdasarkan content based filtering. 

#### Recommendations Variabel
Proses ini akan dilakukan analisis dari variabel dan jenis data pada entitas recommendations untuk melihat karakteristiknya dan apa yang dapat digunakan dalam sistem rekomendasi nanti

![image](https://github.com/tasyyaa/Steam-Game-Recommendation-System/assets/100066633/05b35cb0-2677-481e-be98-56a0a34c000b)
![image](https://github.com/tasyyaa/Steam-Game-Recommendation-System/assets/100066633/91ecbb20-3cb0-4962-a77b-806c0c4bcb31)

Berdasarkan hasil di atas, salah satu variabel yang dapat digunakan untuk sistem rekomendasi adalah variabel "is_recommended" yang menunjukkan apakah user merekomendasikan games tersebut ke orang lain. Variabel tersebut dapat digunakan dalam collaborative filtering

#### Users variabel
Proses ini akan mencakup analisis dari entitas user untuk melihat karakteristik variabel di dalamnya.

![image](https://github.com/tasyyaa/Steam-Game-Recommendation-System/assets/100066633/22655249-4de0-417f-92b4-2fb452e0bef5)
![image](https://github.com/tasyyaa/Steam-Game-Recommendation-System/assets/100066633/5941d9cf-e738-4dd3-a8a2-6d98bd3038e8)
![image](https://github.com/tasyyaa/Steam-Game-Recommendation-System/assets/100066633/780530a4-7f4e-4b62-9ab7-1a94b513d805)

Berdasarkan hasil di atas dapat dilihat bahwa entitas ini hanya memiliki 3 variabel, satu variabel penting yang dapat digunakan pada entitas ini adalah user_id yang menjadi foreign key pada data recommendations. user_id dapat digunakan untuk melakukan collaborative filtering bersama variabel is_recommended.

## Data Preparation
Data preparation merupakan proses yang melibatkan beberapa langkah untuk mengubah data mentah menjadi format yang sesuai dan siap digunakan dalam analisis atau model prediktif. Pada proyek ini digunakan beberapa metode data preparation dimulai dari yang paling standar yaitu mencari missing value hingga feature scaling.

### Menggabungkan games dan games_meta

Tahapan ini dilakukan untuk menggabungkan data games dan games_meta. Seperti yang sudah dijelaskan pada bagian Univariate Explanatory Data Analysis bahwa data games tidak memiliki genre dan genre tersebut terdapat pada games_meta, maka dari itu digabungkanlah kedua data tersebut agar menjadi satu data yang memberikan informasi lengkap mengenai dataset mulai dari ID, nama, deskripsi, tanggal rilis, harga, kompatibalitasnya, dan genre dari games. Data genre ini penting untuk digunakan dalam Content Based Filtering

![image](https://github.com/tasyyaa/Steam-Game-Recommendation-System/assets/100066633/529a8b9d-6262-4598-9576-d92aff1eaa2d)

Dapat dilihat pada gambar di atas bahwa terdapat dataframe baru bernama games_merge yang berisi seluruh variabel dari suatu games

### Mengatasi Missing Value

Mengatasi missing value merupakan metode untuk mencari value yang hilang atau kosong. Kosong ini dapat berbentuk tidak ada data, atau NaN. Pemeriksaan ini bertujuan untuk memastikan kualitas dari data dengan memastikan data sudah sesuai. Pemeriksaan menggunakan isnull() untuk melihat value kosong. Pada tahap ini akan dilakukan pemeriksaan missing value pada ketiga datafram yang ada

![image](https://github.com/tasyyaa/Steam-Game-Recommendation-System/assets/100066633/aebeff46-d6d2-4648-aeb2-49dfe8ab45b2)
![image](https://github.com/tasyyaa/Steam-Game-Recommendation-System/assets/100066633/11b9571f-2b98-4d1a-8586-0f440d5241ca)
![image](https://github.com/tasyyaa/Steam-Game-Recommendation-System/assets/100066633/ebd244cb-f37c-49cd-8528-376bf55f901d)

Berdasarkan hasil di atas dapat dilihat bahwa tidak ada missing value pada ketiga dataframe sehingga tidak perlu untuk melakukan tindakan untuk missing value

### Mengubah user dan games_merge menjadi list dan Menyisakan data unique

Pada tahap ini dilakukan pengubahan dataframe menjadi list dan kemudian melakukan encoding untuk app_id serta user_id hanya pada data yang unique. Tahapan ini dilakukan untuk digunakan sebagai input dalam collaborative filtering dan sebagai data untuk melakukan train ataupun testing pada collaborative filtering. Alasan utama perlu dilakukannya encoding adalah karena algoritma tersebut bekerja dengan data numerik sehingga harus dipastikan bahwa data berbentuk numerik selain itu encoding juga diperlukan agar representasi data lebih kompak, dan memberikan konsistensi di dalam data apabila terdapat nama games atau nama user yang sama, jika dilakukan encoding maka masing-masing akan memiliki ID yang unik. Pada tahap ini dataframe recommendations_merge hanya akan menggunakan data sampel sebesar 100.000 data karena data awal dari recommendations terlalu besar yaitu sekitar 1,4 juta dan memori dari colab tidak sanggup menampungnya.

### Membagi data menjadi train dan testing data

Tahap selanjutnya adalah membagi data menjadi train dan testing data. Data splitting adalah proses membagi dataset menjadi dua subset terpisah yaitu training dataset dan testing dataset. Training dataset dibuat untuk digunakan dalam melatih model sedangkan testing dataset dibuat untuk menguji performa model terhadap data yang belum pernah dilihatnya. Proses ini perlu dilakukan karena untuk dapat melihat kinerja model apakah model sudah berjalan baik atau masih overfitting maupun underfitting. Pada proyek ini tahap pertama sebelum melakukan data splitting adalah mengacak dataframe recommendations_merge sehingga tidak berurutan kemudian melakukan pembagian menjadi 80% untuk train dataset dan 20% untuk validation atau testing dataset. Data yang sudah dibagi ini akan digunakan dalam metode collaborative filtering. Berdasarkan hasil pembagian didapatkan **8000** data untuk training data dan **2000** data untuk testing data.

1. **TF-IDF Vectorizer**
TF-IDF atau Term Frequency-Inverse Document Frequency merupakan teknik yang digunakan dalam text mining dan information retrieval untuk merepresentasikan teks sebagai vektor numerik, yang mencerminkan pentingnya kata-kata dalam dokumen. TF-IDF sendiri memiliki beberapa komponen, yaitu:
- **Term Frequency (TF)**
  Untuk mengukur seberapa sering suatu kata muncul dalam dokumen tertentu dengan rumus
  ![image](https://github.com/tasyyaa/Steam-Game-Recommendation-System/assets/100066633/2d86db7a-649c-45fa-9ade-70eccbee9cde)

- **Inverse Document Frequence (IDF)**
  Untuk mengukur seberapa penting suatu kata secara global dalam kumpulan data dengan rumus
  ![image](https://github.com/tasyyaa/Steam-Game-Recommendation-System/assets/100066633/7fa3b8dc-34da-4b6f-88c2-fd3d709ea00c)

-**TF-IDF**
  Mengkombinasikan kedua metrik untuk memberikan bobot pada kata dengan rumus
  ![image](https://github.com/tasyyaa/Steam-Game-Recommendation-System/assets/100066633/bc671e0a-771c-48b6-96a3-53b519ea768f)

**Kelebihan**
1. Sederhana dan efektif
2. Mengurangi pengaruh kata umum
3. Independen dari pengguna lain

**Kelemahan**
1. Tidak memahami konteks dari kata
2. Mengabaikan urutan kata
3. Skalabilitas yang sangat memakan waktu dan memori pada dataset besar

**Kegunaan** TF-IDF dalam Content Based Filtering ini adalah untuk mengubah list tags dari games menjadi vektor fitur yang memungkinkan sistem untuk menghitung kesamaan antara games berdasarkan tags tersebut. Pada proyek ini **penggunaan** TF-IDF dilakukan dengan mengubah list dari tags yang dimiliki oleh datafram games_merge menjadi String dan setelah dimasukkan ke **TfidVectorizer()** dalam TF-IDF tags tersebut terbagi menjadi list dari kumpulan seluruh jenis genre dan memberikan matrix TF-IDF berupa nilai suatu judul game dengan suatu tags atau genre. Genre yang awalnya berupa gabungan list pada matrix akan ditampilkan menjadi masing-masing string untuk tiap genre yang ada. 

![image](https://github.com/tasyyaa/Steam-Game-Recommendation-System/assets/100066633/0b3b42c8-8321-4e88-91f9-71880ffdb51b)

Melihat gambar di atas berikut merupakan cuplikan dari sampel games dengan hasil pembobotan genre terhadap games tersebut. Dapat dilihat bahwa pada games **Little Adventure 2** nilai dari genre **platformer** bukan 0 tetapi **0.2535** yang berarti games tersebut memiliki tags atau genre **platformer**. Semakin tinggi nilai dari TF-IDF menunjukkan bahwa game tersebut semakin condong terhadap genre tersebut. Total dari nilai pada satu genre adalah 1.

## Modeling dan Result
Pada tahap ini dilakukan pembuatan model dari Machine Learning yang bertujuan untuk membentuk model sistem rekomendasi untuk game yang terdapat di Steam dengan baik. Akan dilakukan dua pembuatan sistem rekomendasi, yaitu pembuatan sistem rekomendasi untuk mendapatkan rekomendasi games berdasarkan genre yang sesuai dari suatu game menggunakan **Content Based Filtering**, serta pembuatan sistem rekomendasi untuk mendapatkan rekomendasi games berdasarkan games yang sesuai dengan games yang telah direkomendasikan oleh user pada review Steam menggunakan **Collaborative Filtering**. Kedua model ini memiliki tahapan yang sedikit berbeda dengan bentuk input data yang berbeda. Untuk melihat lebih jelasnya sebagai berikut:

### Model Development dengan Content Based Filtering
**Content-Based Filtering** merupakan salah satu teknik dalam sistem rekomendasi yang memberikan rekomendasi kepada pengguna berdasarkan karakteristik atau fitur dari item yang mereka sukai sebelumnya. Cara kerja dari teknik ini adalah fitur ekstraksi yaitu melakukan ekstraksi dari fitur yang terdapat dalam suatu item, yang pada games ini adalah tags kemudian dilakukan penilaian kesamaan fitur antara tags pada games tersebut dengan tags-tags yang terdapat pada games lain dengan games yang memiliki tags terdekat akan menjadi hasil rekomendasi. 

**Keuntungan Content-Based Filtering**
1. Setiap user mendapatkan rekomendasi yang sesuai dengan preferensi mereka yang unik
2. Independen dari pengguna lain (tidak memerlukan data pengguna lain)
3. Transparan yaitu mudah untuk menjelaskan mengapa suatu item direkomendasikan berdasarkan fitur yang serupa

**Kelemahan Content-Based Filtering**
1. Cenderung merekomendasikan item yang sangat mirip dengan apa yang disukai pengguna sehingga kurang mengeksplorasi hal baru
2. Kualitas rekomendasi tergantung pada fitur yang tersedia

**Algoritma yang digunakan**

#### K-Nearest Neighbors
**K-Nearest Neighbor (K-NN)** merupakan algoritma yang pada umumnya digunakan pada sistem klasifikasi atau regresi, namun pada konteks sistem rekomendasi, K-NN digunakan untuk menemukan item yang paling mirip dengan item yang telah disukai oleh pengguna berdasarkan atribut atau fitur mereka.

**Kelebihan**
1. Sederhana dan intuitif
2. Tidak membutuhkan pelatihan yang rumit
3. Fleksibilitas dalam metrik kesamaan

**Kelemahan**
1. Kinerja pada data besar bisa menurun
2. Lambat dan membutuhkan banyak memori
3. Tidak menangkan hubungan yang kompleks

**Kegunaan** KNN ini pada sistem rekomendasi dapat digunakan pada beberapa bagian, yaitu representasi fitur seperti TF-IDF, pengukuran kesamaan, dan identifikasi tetangga terdekat. Pada proyek ini **penggunaan** KNN dilakukan pada bidang pengukuran kesamaan dan identifikasi tetangga terdekat. Sebelumnya telah dilakukan representasi fitur dengan menggunakan TF-IDF, hasil dari representasi fitur ini kemudian digunakan oleh KNN untuk mengukur kesamaan antar fitur menggunakan metrik Cosine Similarity pada KNN. Cosine Similarity adalah metrik kesamaan yang digunakan untuk mengukur seberapa mirip dua item berdasarkan fitur atau atribut mereka. Perhitungan dari cosine similarity seperti berikut
![image](https://github.com/tasyyaa/Steam-Game-Recommendation-System/assets/100066633/c685881e-2505-47df-84a9-2e26c2995bb4)

Setelah dilakukan pengukuran kesamaan menggunakan KNN dengan metrik cosine similarity kemudian dilakukan identifikasi tetangga terdekat yaitu sebanyak lima tetangga terdekat dari tags yang akan digunakan.

#### Mendapatkan Rekomendasi
Tahapan ini adalah tahapan untuk melihat hasil rekomendasi yang diberikan oleh model yang telah dibuat. Untuk mendapatkan rekomendasi hanya diperlukan function model yang telah dibuat sebelumnya dan memasukkan parameter berupa title yang menjadi games yang akan dicari rekomendasi kesamaannya berdasarkan tagsnya. Hasil yang diperoleh seperti berikut:

![image](https://github.com/tasyyaa/Steam-Game-Recommendation-System/assets/100066633/51cf9c3b-03a0-4640-991e-6ea18da8404e)

Nama games dan title dari game yang akan menjadi input model rekomendasi

![image](https://github.com/tasyyaa/Steam-Game-Recommendation-System/assets/100066633/a093e3c0-9d5c-4eff-b31f-dc41fab5e405)

Merupakan hasil dari rekomendasi berdasarkan input sebelumnya, dapat dilihat bahwa tags yang dimiliki menyerupai tags pada input dan jika dilihat lebih saksama games rekomendasi merupakan games dengan seri yang sama dengan games input.

### Model Development dengan Collaborative Filtering
**Collaborative Filtering** adalah salah satu pendekatan dalam sistem rekomendasi yang menggunakan perilaku atau preferensi pengguna untuk memprediksi preferensi yang lebih baik untuk pengguna lain. Pendekatan ini tidak bergantung pada atribut atau karakteristik item (seperti dalam content-based filtering), tetapi lebih fokus pada hubungan antar pengguna dan item berdasarkan data historis preferensi. Pada proyek ini fokus dari collaborative filtering adalah data games yang direkomendasikan oleh user dan akan mencari games lain yang sesuai dengan games yang telah direkomendasikan tersebut. Teknik ini akan menggunakan user-based collaborative filtering dengan menghitung kesamaan antara user berdasarkan data rekomendasi gamenya dengan user lain yang memiliki rekomendasi yang sama memberikan rekomendasi game apa selain game tersebut.

**Kelebihan Collaborative Filtering**
1. Efektif untuk long tain atau merekomendasikan item yang jarang dilihat atau dieksplorasi oleh pengguna
2. Memperhitungkan perubahan preferensi

**Kelemahan Collaborative Filtering**
1. Kesulitan dalam memberikan rekomendasi untuk pengguna baru atau item baru yang belum memiliki data historis yang cukup.
2. Masalah yang timbul ketika data preferensi pengguna sangat jarang, yang dapat mengurangi keakuratan prediksi.
3. Over Specialization atau rekomendasi yang terlalu spesifik

#### Collaborative Filtering berbasis Embedding
Pada proyek ini digunakan Tensorflow dan Keras untuk mengimplementasikan sebuah model dalam sistem rekomendasi. Pada algoritma ini terdapat tiga komponen penting yang harus diperhatikan, yaitu:
1. **Embedding Layers**
   Terdapat dua jenis embedding layer: untuk pengguna (users) dan untuk games. Embedding layer bertujuan untuk memetakan pengguna dan item ke dalam ruang laten (embedding space) yang memiliki dimensi embedding_size. Ini memungkinkan model untuk mempelajari representasi yang lebih kompak dan berdimensi rendah dari pengguna dan item.

2. **Input dan Output**
**Input**: Model ini menerima input dalam bentuk tensor inputs dengan dua kolom. Kolom pertama adalah indeks pengguna (inputs[:, 0]) dan kolom kedua adalah indeks game (inputs[:, 1]).
**Output**: Model menghitung dot product antara embedding vektor pengguna dan embedding vektor game (dot_user_games). Kemudian ditambahkan dengan bias yang sesuai (user_bias dan games_bias). Hasilnya kemudian melewati fungsi aktivasi sigmoid (tf.nn.sigmoid(x)) untuk menghasilkan prediksi probabilitas.

3. **Optimasi dan Metrics**
   Model menggunakan Binary Crossentropy sebagai loss function (tf.keras.losses.BinaryCrossentropy()) karena umumnya digunakan untuk masalah rekomendasi biner (misalnya, apakah pengguna akan menyukai atau tidak suatu game).
Adam optimizer digunakan untuk mengoptimalkan model dengan learning rate 0.001.
Metrics yang digunakan adalah Root Mean Squared Error (RMSE), yang merupakan metrik umum untuk mengukur akurasi dari model rekomendasi.

**Kelebihan**
1. Dapat menangani jumlah pengguna dan item yang besar
2. Model mampu mempelajari hubungan non-linear
3. Embedding vektor yang dipelajari dapat memberikan insight tentang hubungan antara pengguna dan item

**Kelemahan**
1. Sulit memberikan rekomendasi untuk pengguna atau item baru
2. Masalah muncul ketika data preferensi pengguna sangat jarang

#### Tahap Training
Dilakukan training model tersebut sebanyak 100 epochs dengan 500 batch kepada data yang sudah displit sebelumnya dengan metriks RMSE dan optimasi menggunakan Adam Optimizer

#### Mendapatkan Rekomendasi
Setelah mendapatkan model maka akan dilakukan penggunaan model tersebut ke input yang akan diambil. Pada kali ini input berupa user_id yang didapatkan secara random dan akan memberikan hasil **Top-3** games yang paling terdekat dengan games yang direkomendasikan oleh user_id. Untuk mendapatkan rekomendasi hanya perlu menggunakan fungsi yang telah dibuat sebelumnya

Berikut merupakan hasil model rekomendasi
![image](https://github.com/tasyyaa/Steam-Game-Recommendation-System/assets/100066633/989f4ebb-df44-4cca-9421-1522b6436cda)

Berdasarkan hasil di atas dapat dilihat dari user yang terpilih random bahwa dia telah merekomendasikan game **thief** dengan tags **stealth, action, adventure, dan First Person** dan didapatkan hasil rekomendasi games yang menyerupai games yang telah direkomendasikan oleh user, yaitu dua game teratas merupakan game dengan nama yang memiliki kata yang sama yaitu **thief**, kemudian rekomendasi ketiga tidak memiliki kata yang sama, namun memiliki tags yang meyerupai game sebelumnya.


## Evaluation
Proses ini merupakan proses untuk menilai kinerja dari model Machine Learning yang telah dilatih dan sangat penting untuk dilakukan untuk memastikan bahwa model bekerja dengan baik pada data yang belum dilihat sebelumnya yaitu menggunakan data testing. Metrik yang digunakan pada evaluasi kali ini adalah metrik evaluasi **Root Mean Squared Error (RMSE)** untuk menilai hasil yang didapatkan oleh Collaborative Filtering. Berikut merupakan penjelasan dari **Root Mean Square Error**

- **Root Mean Squared Error (RMSE)**
  Root Mean Squared Error (RMSE) adalah salah satu metrik evaluasi yang umum digunakan untuk mengukur seberapa baik suatu model regresi (termasuk model rekomendasi) dapat memprediksi nilai kontinu. RMSE mengukur akar rata-rata dari kuadrat selisih antara nilai prediksi model dengan nilai yang diamati atau sebenarnya. Secara matematis, RMSE dapat dihitung dengan rumus berikut:

  ![image](https://github.com/tasyyaa/Steam-Game-Recommendation-System/assets/100066633/df9c981f-80db-4e3f-8b0a-8384491856f0)

dengan n merupakan jumlah sampel dalam data, y merupakan nilai sebenarnya dari sampel ke-i, dan yhat merupakan nilai prediksi dari model untuk sampel ke-i. Dalam proyek ini penggunaan metrik ini digunakan untuk mengevaluasi seberapa baik model untuk sistem rekomendasi dapat memprediksi preferensi pengguna terhadap item yang direkomendasikan. Metrik ini hanya digunakan pada collaborative filtering dan mungkin hasilnya tidak akan cukup bagus karena pada dasarnya dataset ini tidak memberikan jawaban sebenarnya dari rekomendasi seharusnya atau Machine Learning menggunakan unsupervised learning. Hasil dari evaluasi ditunjukkan pada bagian visualisasi metrix di bawah

- **Visualisasi Metrix**
  ![image](https://github.com/tasyyaa/Steam-Game-Recommendation-System/assets/100066633/760c7af3-1012-4c3c-ba88-1098577c1fc4)

Berdasarkan hasil visualisasi tersebut didapatkan bahwa model dikatakan **overfitting** karena hasil testing yang jauh berbeda dan lebih besar dibandingkan hasil training. Hal ini menandakan bahwa model masih belum dapat bekerja dengan baik saat bertemu dengan dataset baru yang belum pernah dilihat sebelumnya. Namun, jika dilihat berdasarkan hasil yang didapatkan sebelumnya dapat dikatakan bahwa model masih berjalan dengan baik namun tidak sempurna.

Berdasarkan hasil yang diperoleh dari pengerjaan proyek ini maka didapatkan beberapa hal, yaitu:
1. Pembuatan sistem rekomendasi pada dataset ini dengan menggunakan Content Based Filtering dapat dilakukan dengan mencari rekomendasi games yang menyerupai games lain berdasarkan kemiripan dari tags atau genre dari games tersebut. Data games ini terdapat pada games.csv dan games_meta.json dan menggunakan algoritma TF-IDF sebagai representasi fitur dan penggunaan K-Nearest Neighbor sebagai penghitung jarak kesamaan dan mencari titik terdekat.
2. Pembuatan sistem rekomendasi pada dataset ini dengan menggunakan Collaborative Filtering dapat dilakukan dengan membuat rekomendasi untuk user berdasarkan games yang telah dia rekomendasikan pada review yang dia publish sesuai dengan data user lain yang melakukan rekomendasi game yang sama. Algoritma yang digunakan adalah Collaborative Filtering dengan basis Embedding dan menggunakan Root Mean Squared Error sebagai metriks akurasi.

## Referensi
[1] G. Adomavicius and A. Tuzhilin, "Toward the next generation of recommender systems: a survey of the state-of-the-art and possible extensions," in IEEE Transactions on Knowledge and Data Engineering, vol. 17, no. 6, pp. 734-749, June 2005, doi: 10.1109/TKDE.2005.99.

[2] Long-Sheng Chen, Fei-Hao Hsu, Mu-Chen Chen, Yuan-Chia Hsu,
Developing recommender systems with the consideration of product profitability for sellers, Information Sciences, Volume 178, Issue 4, 2008, Pages 1032-1048, ISSN 0020-0255, https://doi.org/10.1016/j.ins.2007.09.027.

[3] Bouneffouf, D., Bouzeghoub, A., Ganarski, A.L. (2013). Risk-Aware Recommender Systems. In: Lee, M., Hirose, A., Hou, ZG., Kil, R.M. (eds) Neural Information Processing. ICONIP 2013. Lecture Notes in Computer Science, vol 8226. Springer, Berlin, Heidelberg. https://doi.org/10.1007/978-3-642-42054-2_8

[4] Harald Steck. 2013. Evaluation of recommendations: rating-prediction and ranking. In Proceedings of the 7th ACM conference on Recommender systems (RecSys '13). Association for Computing Machinery, New York, NY, USA, 213–220. https://doi.org/10.1145/2507157.2507160

[5] https://steamcommunity.com/groups/steamworks/announcements/detail/1697194621363928453

[6] Gong, J., Ye, Y., Stefanidis, K. (2020). A Hybrid Recommender System for Steam Games. In: Flouris, G., Laurent, D., Plexousakis, D., Spyratos, N., Tanaka, Y. (eds) Information Search, Integration, and Personalization. ISIP 2019. Communications in Computer and Information Science, vol 1197. Springer, Cham. https://doi.org/10.1007/978-3-030-44900-1_9

[7] Robert K. Merton ,The Matthew Effect in Science.Science159,56-63(1968).DOI:10.1126/science.159.3810.56

