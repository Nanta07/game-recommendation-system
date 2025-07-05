# Laporan Proyek Machine Learning: Game Recommendation System - Ananta Boemi Adji

## **1. Domain Project**
### Latar Belakang
Rekomendasi game menjadi semakin penting dalam industri gim yang sangat kompetitif. Dengan ribuan judul yang dirilis setiap tahun, pengguna seringkali mengalami kebingungan dalam memilih gim yang sesuai dengan preferensi mereka. Oleh karena itu, sistem rekomendasi berbasis Machine Learning bisa digunakan untuk membantu pengguna menemukan game yang mereka sukai dengan lebih efisien dan personal.

### Alasan Kepentingan Proyek Ini:
* Membantu pengguna menemukan game yang relevan dengan preferensi mereka.
* Meningkatkan pengalaman pengguna dan keterlibatan dalam platform distribusi game.
* Mendorong penjualan game dengan memberikan rekomendasi yang tepat sasaran.
* Mengurangi churn rate pengguna platform game.
* Memberikan insight kepada pengembang dan penerbit tentang pola minat pemain.

### Dataset
Dataset yang digunakan merupakan gabungan informasi tentang video game seperti genre, platform, publisher, rating, dan skor pengguna. Dataset ini bersumber dari Kaggle.

* **Judul Dataset**: [Video Game Sales with Ratings](https://www.kaggle.com/datasets/rush4ratio/video-game-sales-with-ratings)
* **Sumber**: Kaggle, diunggah oleh pengguna bernama rush4ratio

## **2. Business Understanding**
Sistem rekomendasi memainkan peran kunci dalam platform distribusi konten digital, termasuk industri game. Rekomendasi yang akurat mampu meningkatkan kepuasan pengguna, meningkatkan waktu keterlibatan, dan mendorong transaksi pembelian.

* **Pengguna**: Mendapatkan rekomendasi game yang sesuai minat mereka.
* **Platform distribusi game**: Meningkatkan retensi pengguna dan pendapatan.
* **Developer & publisher game**: Mendapatkan eksposur lebih besar terhadap game mereka.

### Problem Statements:
* Bagaimana memberikan rekomendasi game yang relevan berdasarkan metadata konten game?
* Bagaimana memberikan rekomendasi game berdasarkan pola interaksi pengguna dengan game?
* Sejauh mana performa masing-masing pendekatan (CBF dan CF) dalam menghasilkan rekomendasi yang relevan?

### Goals:
* Membangun dua sistem rekomendasi: Content-Based Filtering dan Collaborative Filtering.
* Mengevaluasi performa model dengan metrik Precision\@k dan Recall\@k.
* Menyediakan sistem rekomendasi interaktif berdasarkan input pengguna.

### Solution Statements:
Untuk mencapai tujuan tersebut, dilakukan pendekatan sebagai berikut:

1. **Data Collection**
   Menggunakan dataset dari Kaggle yang berisi metadata dan rating game.

2. **Data Exploration & Preprocessing**
   Melakukan pembersihan data, encoding fitur teks, dan penyusunan data user-item interaction.

3. **Model Building & Evaluation**
   * Content-Based Filtering menggunakan TF-IDF dan cosine similarity.
   * Collaborative Filtering menggunakan model LightFM dengan loss function `warp`.

## **3. Data Understanding & Exploratory Data Analysis (EDA)**
### Informasi Dataset
* **Sumber**: [Video Game Sales with Ratings – Kaggle](https://www.kaggle.com/datasets/rush4ratio/video-game-sales-with-ratings)
* **Jumlah data**: 16.719 baris dan 16 kolom

### Struktur Dataset
| Nama Kolom        | Deskripsi                              | Tipe Data | Contoh         |
| ----------------- | -------------------------------------- | --------- | -------------- |
| Name              | Nama video game                        | object    | GTA V          |
| Platform          | Platform tempat game dirilis           | object    | PS4            |
| Year\_of\_Release | Tahun rilis game                       | float64   | 2013.0         |
| Genre             | Genre dari game                        | object    | Action         |
| Publisher         | Penerbit game                          | object    | Rockstar Games |
| NA\_Sales         | Penjualan di Amerika Utara (juta unit) | float64   | 3.23           |
| EU\_Sales         | Penjualan di Eropa (juta unit)         | float64   | 2.45           |
| JP\_Sales         | Penjualan di Jepang (juta unit)        | float64   | 0.01           |
| Other\_Sales      | Penjualan di wilayah lain (juta unit)  | float64   | 0.47           |
| Global\_Sales     | Penjualan total global (juta unit)     | float64   | 6.16           |
| Critic\_Score     | Skor dari kritikus                     | float64   | 97.0           |
| Critic\_Count     | Jumlah ulasan dari kritikus            | float64   | 51.0           |
| User\_Score       | Skor dari pengguna                     | float64   | 9.0            |
| User\_Count       | Jumlah ulasan dari pengguna            | float64   | 300.0          |
| Rating            | Kategori usia dari ESRB                | object    | M              |
| Developer         | Nama pengembang game (jika ada)        | object    | Rockstar North |

### Ukuran dan Tipe Data
* **Dimensi dataset**: 16.719 baris × 16 kolom
* **Tipe data**:
  * Numerik (float & int): 10 kolom
  * Kategorikal / Teks: 6 kolom

### Kondisi Data
* Dataset memiliki **16.719 baris** dan **16 kolom**.
* Terdapat sejumlah fitur dengan **missing value yang signifikan**, khususnya pada:

  * `Critic_Score` (\~51%)
  * `Critic_Count` (\~51%)
  * `User_Score` (\~40%) – juga dalam format **object**, bukan numerik.
  * `User_Count` (\~54%)
  * `Developer` (\~40%)
  * `Rating` (\~41%)

**Insight**:
Informasi seperti rating dari pengguna dan kritikus tidak tersedia secara konsisten. Hal ini menjadi perhatian penting dalam preprocessing sebelum melakukan analisis lebih lanjut atau membangun model.

### Statistik Deskriptif
| Fitur         | Mean  | Std Dev | Min  | 25%   | 50%   | 75%   | Max    |
| ------------- | ----- | ------- | ---- | ----- | ----- | ----- | ------ |
| NA\_Sales     | 0.26  | 0.84    | 0.00 | 0.00  | 0.08  | 0.24  | 41.49  |
| EU\_Sales     | 0.15  | 0.50    | 0.00 | 0.00  | 0.02  | 0.12  | 29.02  |
| JP\_Sales     | 0.08  | 0.31    | 0.00 | 0.00  | 0.00  | 0.04  | 10.22  |
| Other\_Sales  | 0.05  | 0.19    | 0.00 | 0.00  | 0.01  | 0.04  | 10.57  |
| Global\_Sales | 0.54  | 1.56    | 0.01 | 0.06  | 0.17  | 0.47  | 82.74  |
| Critic\_Score | 68.97 | 13.91   | 13.0 | 61.0  | 70.0  | 79.0  | 98.0   |
| Critic\_Count | 23.81 | 13.14   | 3.0  | 13.0  | 21.0  | 33.0  | 51.0   |
| User\_Score\* | 6.43  | 1.51    | 0.0  | 5.6   | 6.9   | 7.8   | 9.8    |
| User\_Count   | 2,426 | 2,645   | 1.0  | 570.0 | 1,531 | 3,809 | 10,665 |

**Insight**:
Sebagian besar game memiliki **penjualan global di bawah 1 juta unit**, menunjukkan distribusi yang berat di bagian bawah (right-skewed). **Critic Score** dan **User Score** cenderung berada di kisaran menengah ke atas, menunjukkan penilaian yang umumnya positif terhadap game yang tersedia.

### Korelasi Antar Fitur
| Fitur         | Global\_Sales | NA\_Sales | EU\_Sales | JP\_Sales | Other\_Sales | Critic\_Score | User\_Score |
| ------------- | ------------- | --------- | --------- | --------- | ------------ | ------------- | ----------- |
| Global\_Sales | 1.00          | 0.94      | 0.90      | 0.63      | 0.75         | 0.27          | 0.24        |
| Critic\_Score | 0.27          | 0.26      | 0.26      | 0.19      | 0.24         | 1.00          | 0.58        |
| User\_Score   | 0.24          | 0.22      | 0.22      | 0.18      | 0.21         | 0.58          | 1.00        |

**Insight**:
* **Global\_Sales** sangat berkorelasi dengan **NA\_Sales** (0.94) dan **EU\_Sales** (0.90), menunjukkan kontribusi terbesar berasal dari dua wilayah tersebut.
* **Critic Score** dan **User Score** memiliki korelasi positif yang cukup baik (0.58), menandakan persepsi publik cenderung sejalan dengan kritikus.
* Korelasi skor review dengan penjualan relatif rendah → review bukan satu-satunya penentu suksesnya game.

### Distribusi Fitur
pada bagian EDA yang dilakukan saya melakukan beberapa analisis terkait hubungan antar feature yang terdapat dalam dataset agar bisa memberikan gambaran besar dari bagaimana karakteristik dari dataset yang digunakan.

#### Distribusi `User_Score`:
* Terdapat **noise** seperti nilai `'tbd'` → dikonversi menjadi `NaN` dan di-drop.
* Setelah dibersihkan, distribusi **mendekati normal** dengan puncak di sekitar skor **7**.

#### Distribusi `Critic_Score`:
* Cenderung **normal**, dengan skor dominan antara **60–85**.
* Skor sangat rendah dan sangat tinggi relatif jarang → distribusi lebih **stabil**.

#### Distribusi `Global_Sales`:
* Distribusi sangat **right-skewed (long-tail)**.
* Mayoritas game memiliki **penjualan <1 juta unit**.
* Sedikit game memiliki penjualan sangat tinggi → game blockbuster.

* **Review score** (User & Critic) lebih merata, namun penjualan sangat tidak merata.
* Hanya segelintir game yang benar-benar **meledak di pasaran**.

### Unik Value Fitur Kategorikal
| Fitur     | Jumlah Kategori | Contoh Dominan                       |
| --------- | --------------- | ------------------------------------ |
| Platform  | 31              | PS2 (2161), DS (2152), PS3 (1331)    |
| Genre     | 12              | Action (3370), Sports (2348)         |
| Developer | 1696            | Ubisoft, EA Canada, Capcom           |
| Publisher | 581             | Electronic Arts, Activision, Ubisoft |
| Rating    | 8               | E (3991), T (2961), M (1563)         |

* Platform populer: **PS2 dan DS** mendominasi.
* Genre paling umum: **Action** dan **Sports**.
* Beberapa developer/publisher besar mendominasi industri.
* Rating E dan T mendominasi, artinya banyak game disesuaikan untuk **semua umur**.

### Visualisasi Genre & Platform Populer
* **Genre**:
  * **Action** adalah genre dengan jumlah game terbanyak, diikuti oleh **Sports** dan **Misc**.
  * Menunjukkan tren pasar yang menyukai game dengan elemen dinamis dan kompetitif.

* **Platform**:
  * **PS2**, **DS**, dan **PS3** menjadi platform dengan jumlah game terbanyak.
  * Ini menunjukkan dominasi konsol generasi ke-6 dan ke-7 dalam industri distribusi game.

### Hasil informasi dari EDA
1. **Penjualan global sangat terlalu rata**, dengan hanya sebagian kecil game yang menjadi blockbuster.
2. **Review score** (User & Critic) cenderung positif dan stabil, namun tidak terlalu menentukan penjualan.
3. **Platform dan genre tertentu** sangat mendominasi pasar.
4. Banyak **missing value** pada fitur review, perlu perlakuan khusus saat preprocessing.
5. **Korelasi review dan penjualan cukup lemah**, penjualan lebih bergantung pada region dan mungkin faktor eksternal (marketing, IP populer, dll).

## 4. Data Preparation
Proses ini bertujuan untuk membersihkan dan mentransformasi data mentah menjadi format yang siap digunakan untuk pemodelan. Alur kerja ini mencakup pembersihan data, pembuatan dataset khusus untuk setiap model, rekayasa fitur, dan transformasi data.

### Pembersihan Data dan Penanganan Missing Values
Langkah pertama adalah memastikan kualitas data dasar dengan melakukan pembersihan dan konversi tipe data.

1.  **Menghapus Kolom Tidak Kritis**: Kolom dengan proporsi *missing value* yang sangat tinggi (`Critic_Score`, `Critic_Count`, `User_Count`, `Developer`) dihapus untuk menghindari noise dan menyederhanakan model.
2.  **Konversi Tipe Data `User_Score`**: Kolom `User_Score` diubah menjadi tipe numerik. Nilai teks seperti `'tbd'` otomatis diubah menjadi `NaN` (nilai kosong) pada proses ini.
3.  **Menghapus Baris dengan Nilai Kosong**: Setelah perubahan di atas, semua baris yang masih memiliki `NaN` pada kolom-kolom krusial (`Name`, `Genre`, `Publisher`, dll.) akan dihapus.
4.  **Konversi Tipe Data `Year_of_Release`**: Tipe data kolom ini diubah dari *float* menjadi *integer* untuk konsistensi.

**Insight**: Setelah tahap ini, dataset menjadi jauh lebih bersih dan siap untuk proses selanjutnya. Ukuran data berkurang menjadi **7.378 baris**, yang semuanya memiliki data lengkap pada kolom-kolom penting.

### Pembuatan Dataset untuk Modeling
Dari data yang sudah bersih, dibuat dua DataFrame terpisah yang disesuaikan untuk masing-masing pendekatan model.

#### A. Dataset Content-Based Filtering (df_cb)
* **Proses**: Data difilter untuk hanya menyertakan game dengan `User_Score >= 7.0`.
* **Tujuan**: Untuk fokus pada game-game berkualitas yang cenderung disukai pengguna, sehingga rekomendasi yang dihasilkan lebih relevan. DataFrame `df_cb` kemudian dibuat dengan memilih kolom-kolom yang relevan untuk analisis konten.

#### B. Dataset Collaborative Filtering (df_cf)
* **Proses**: `user_id` sintetis dibuat secara acak untuk game-game dengan `User_Score >= 7.0`.
* **Tujuan**: Karena tidak adanya data pengguna asli, langkah ini bertujuan untuk menciptakan dataset interaksi *user-item* yang diperlukan oleh model Collaborative Filtering seperti LightFM.

### Feature Engineering dan Transformasi Data
Langkah ini adalah inti dari persiapan data, di mana fitur-fitur baru dibuat dan diubah menjadi format yang dapat diproses oleh model.

#### A. Untuk Content-Based Filtering (CBF)
Fitur-fitur untuk `df_cb` direkayasa untuk menangkap kemiripan konten secara lebih efektif.

* **Normalisasi dan Kategorisasi Fitur Numerik**:
    * **Normalisasi**: `User_Score` dan `Global_Sales` dinormalisasi ke rentang [0, 1] menggunakan `MinMaxScaler`.
    * **Kategorisasi (Binning)**: Nilai-nilai tersebut kemudian dikelompokkan ke dalam kategori ('low', 'medium', 'high'). Ini membantu model menangkap pola umum tanpa terlalu terpengaruh oleh nilai absolut. Dengan ketentuan:
    - df_cb['User_Score_category']: Membagi skor pengguna menjadi 3 kategori ('low', 'medium', 'high') berdasarkan rentang nilai absolut. Digunakan pd.cut dengan batas [0, 5, 7, 10] untuk menyederhanakan analisis dan menangkap pola umum dari skor.
    - df_cb['Global_Sales_category']: Mengelompokkan nilai penjualan global yang telah dinormalisasi ke dalam 3 kuantil sama besar ('low', 'medium', 'high') menggunakan pd.qcut. Tujuannya agar distribusi kategori lebih seimbang.

* **Pembuatan Fitur Gabungan (`combined_features`)**:
    Sebuah fitur teks baru dibuat dengan menggabungkan beberapa metadata. Fitur tertentu diberi **bobot lebih tinggi** (dengan cara direpetisi) untuk meningkatkan pengaruhnya.

| Fitur                   | Bobot (Jumlah Repetisi) | Alasan Pembobotan                                         |
| ----------------------- | ----------------------- | ----------------------------------------------------------|
| `Genre`                 | 8                       | Merupakan indikator utama dari tipe permainan.            |
| `User_Score_category`   | 6                       | Mencerminkan persepsi kualitas umum dari pengguna.        |
| `Global_Sales_category` | 6                       | Menunjukkan tingkat popularitas game secara keseluruhan.  |
| `Platform`              | 5                       | Seringkali relevan dengan tipe game dan audiens.          |
| `Publisher`             | 4                       | Dapat mengindikasikan "gaya" atau kualitas produksi game. |
| `Rating`                | 3                       | Memberikan konteks tentang target audiens.                |
* **Reset Index**: Indeks DataFrame di-reset untuk memastikan sinkronisasi yang benar dengan matriks kemiripan (*cosine similarity*).

#### B. Vektorisasi Fitur dengan TF-IDF (CBF)
Ini adalah langkah persiapan akhir untuk model CBF, yang secara teknis merupakan bagian dari *feature engineering*.
* **Tujuan**: Mengubah kolom `combined_features` yang berisi teks menjadi representasi vektor numerik.
* **Proses**: `TfidfVectorizer` menghitung pentingnya sebuah kata dalam metadata game relatif terhadap keseluruhan dataset. Vektor yang dihasilkan inilah yang kemudian digunakan oleh model untuk menghitung kemiripan antar game.

#### Pembuatan Matriks Kemiripan (Hybrid Cosine Similarity)
Model CBF menggunakan matriks kemiripan yang dihitung melalui beberapa langkah:
1.  **TF-IDF Vectorization**: Fitur `combined_features` yang telah direkayasa sebelumnya diubah menjadi matriks vektor numerik menggunakan `TfidfVectorizer`.
2.  **Perhitungan Kemiripan**:
    * **Kemiripan Konten (Teks)**: Dihitung dari matriks TF-IDF menggunakan *cosine similarity*.
    * **Kemiripan Numerik**: Dihitung secara terpisah dari fitur `User_Score` dan `Global_Sales` yang sudah dinormalisasi.
3.  **Skor Kemiripan Gabungan**: Kedua matriks kemiripan tersebut digabungkan dengan bobot **80% untuk konten** dan **20% untuk numerik** untuk menghasilkan skor kemiripan akhir (`cosine_sim_combined`) yang lebih komprehensif.
4.  **Indexing**: Sebuah "kamus" (`indices`) dibuat untuk memetakan nama setiap game ke indeksnya dalam DataFrame, yang krusial untuk proses pencarian rekomendasi.

## **5. Modeling**
Pada tahap ini, dua pendekatan sistem rekomendasi dikembangkan dan dievaluasi: Content-Based Filtering (CBF) dan Collaborative Filtering (CF). Mengingat dataset yang tersedia lebih kaya akan metadata game daripada data interaksi pengguna, fokus utama pengembangan adalah pada model CBF. Model CF tetap dibangun sebagai pembanding untuk mengevaluasi potensi pendekatan berbasis interaksi.

### Pendekatan 1: Content-Based Filtering
Pendekatan ini merekomendasikan game berdasarkan kemiripan fitur atau metadata. Prosesnya mencakup pembangunan matriks kemiripan, pembuatan fungsi rekomendasi, dan evaluasi performa.

#### Fungsi Rekomendasi dan Evaluasi
Dua fungsi utama didefinisikan untuk model ini:
* **`run_cbf_recommendation`**: Fungsi ini menerima input judul game dari pengguna, lalu menggunakan matriks kemiripan untuk menemukan dan menampilkan daftar game teratas yang paling mirip.
* **`evaluate_cbf`**: Fungsi ini dibuat untuk mengukur performa model. Metrik yang digunakan adalah **Precision@k** dan **Recall@k**, dengan mendefinisikan "item relevan" sebagai game lain yang memiliki **Genre dan Publisher yang sama**.

#### Hasil dan Analisis Performa CBF
Setelah fungsi evaluasi dijalankan, didapatkan hasil sebagai berikut:
- Precision@5: 0.7436
- Recall@5: 0.2914

**Insight**:
* **Precision@5 sebesar 74.36%** menunjukkan bahwa dari setiap 5 rekomendasi yang diberikan, rata-rata sekitar 74% di antaranya sangat relevan (memiliki Genre dan Publisher yang sama). Ini membuktikan model sangat akurat dalam mengidentifikasi game dengan konten serupa.
* **Recall@5 sebesar 29.14%** menandakan bahwa sistem berhasil menangkap sekitar 29% dari seluruh item yang dianggap relevan. Hasil ini menunjukkan bahwa pendekatan berbasis konten yang telah disesuaikan (dengan pembobotan fitur) cukup kuat dan efektif.

### Pendekatan 2: Collaborative Filtering (LightFM)
Meskipun bukan fokus utama, model CF dibangun sebagai pembanding menggunakan data user sintetis.

#### Proses Pemodelan dan Evaluasi
1.  **Pembuatan Dataset**: Matriks interaksi *user-item* dibuat dari `df_cf` menggunakan `lightfm.data.Dataset`.
2.  **Pelatihan Model**: Model LightFM dilatih dengan *loss function* `warp` (Weighted Approximate-Rank Pairwise), yang cocok untuk mengoptimalkan urutan rekomendasi dari *implicit feedback*.
3.  **Evaluasi**: Performa model diukur menggunakan metrik `precision_at_k` dan `recall_at_k` yang disediakan oleh LightFM.

#### Hasil dan Analisis Performa CF
Hasil evaluasi pada data pelatihan menunjukkan:
- Precision@5: 0.2384
- Recall@5: 0.1267

**Insight**:
* Dengan **Precision@5 sebesar 23.84%** dan **Recall@5 sebesar 12.67%**, model ini menunjukkan kemampuan yang cukup baik dalam menangkap pola preferensi kolektif, bahkan dengan data pengguna yang disimulasikan.
* Meskipun metriknya lebih rendah dari CBF, ini wajar karena CF tidak menggunakan fitur eksplisit (seperti genre) untuk evaluasi, melainkan pola interaksi. Pendekatan ini memiliki potensi besar jika dilatih dengan data interaksi pengguna yang nyata.

#### Fungsi Rekomendasi CF
Fungsi `run_cf_recommendation` dibuat untuk memberikan rekomendasi berdasarkan selera kolektif.
* **Proses**: Fungsi ini mengidentifikasi semua pengguna (sintetis) yang menyukai game yang diinput, kemudian menganalisis game lain yang juga mereka sukai untuk menghasilkan daftar rekomendasi teratas.

### Demonstrasi Sistem - Game Recommendation System_Game Library
Bagian ini menunjukkan bagaimana sistem rekomendasi yang telah dibangun dapat digunakan secara interaktif. Terdapat dua komponen utama: sebuah *Game Library* untuk menjelajahi game yang tersedia dan fungsi pengujian langsung untuk kedua model.

### A. Game Library - Cek isi dataset
Untuk memudahkan pengguna dalam menemukan judul game yang valid untuk diuji, sebuah fungsi interaktif (`get_available_games_by_genre_paginated`) dibuat.

* **Fungsi**: Fungsi ini menampilkan sebuah *dropdown* menu yang memungkinkan pengguna untuk memfilter game berdasarkan **Genre**.
* **Tujuan**: Tujuannya adalah agar pengguna dapat dengan mudah menemukan judul game yang ada di dalam dataset untuk digunakan sebagai input pada fungsi rekomendasi CBF dan CF.
* **Tampilan**: Daftar game yang difilter akan ditampilkan dalam format tabel dengan navigasi halaman, lengkap dengan informasi *Publisher* dan *Rating*.

**Catatan**: Daftar game yang ditampilkan adalah game yang ada di kedua dataset (`df_cb` dan `df_cf`) untuk memastikan game tersebut dapat diuji pada kedua model.

### B. Pengujian Sistem Rekomendasi
Ini adalah tahap pengujian akhir di mana kedua fungsi rekomendasi dijalankan untuk menghasilkan daftar Top-N game berdasarkan input dari pengguna.

#### 1. Rekomendasi dengan Content-Based Filtering (CBF)
Mencoba fungsi `run_cbf_recommendation` dijalankan dengan input judul game **"Grand Theft Auto V"**.

**Hasil Output:**
| Pendekatan                | Nama Game yang Direkomendasikan       | User Score |
| :------------------------ | :------------------------------------ | :----------- |
| Content-Based             | Grand Theft Auto IV                   | 7.5          |
|                           | Mafia II                              | 7.7          |
|                           | NBA 2K11                              | 7.6          |
|                           | Red Dead Redemption                   | 8.8          |
|                           | Top Spin 3                            | 7.5          |
|                           | Grand Theft Auto V                    | 8.1          |
|                           | Red Dead Redemption: Undead Nightmare | 7.4          |
|                           | Way of the Samurai 3                  | 7.9          |
| ------------------------- | ------------------------------------- | ------------ |

**Analisis**: Hasil rekomendasi CBF sangat relevan dari segi konten, di mana sebagian besar game yang direkomendasikan memiliki genre *Action* dan *Open-World*, serta berasal dari publisher yang sama (Rockstar Games & 2K Games).

#### 2. Rekomendasi dengan Collaborative Filtering (CF)
Fungsi `run_cf_recommendation` juga dijalankan dengan input judul game **"Grand Theft Auto V"**.

**Hasil Output:**
| Pendekatan                | Nama Game yang Direkomendasikan       | User Score |
| :------------------------ | :------------------------------------ | :----------- |
| Collaborative             | NBA 2K7                               | 8.4          |
|                           | Plants vs. Zombies: Garden Warfare    | 7.3          |
|                           | The Lord of the Rings: Conquest       | 7.1          |
|                           | FIFA Soccer 10                        | 7.6          |
|                           | Tony Hawk's Pro Skater 3              | 7.5          |
| ------------------------- | ------------------------------------- | ------------ |
**Analisis**: Rekomendasi dari model CF lebih beragam dan tidak hanya terbatas pada kemiripan fitur eksplisit. Model ini menemukan game dari genre lain (seperti Sports dan Strategy) yang juga disukai oleh kelompok pengguna (sintetis) yang menyukai "Grand Theft Auto V", menunjukkan potensi penemuan game baru (*serendipity*).

**Catatan Akhir**:
Bisa terjadi kondisi di mana judul game yang dimasukkan tidak ditemukan. Hal ini wajar karena dataset yang digunakan tidak mencakup seluruh game yang ada, atau karena game tersebut tidak memenuhi kriteria (skor tinggi) untuk dimasukkan ke dalam pemodelan.

### **6. Evaluation**
#### **A. Metrik Evaluasi yang Digunakan**
Untuk mengukur performa sistem rekomendasi, digunakan dua metrik utama:

* **Precision@k**
    Mengukur proporsi dari item yang direkomendasikan (top-k) yang benar-benar relevan.
    **Formula:**
    $$
    \text{Precision@k} = \frac{\text{Jumlah item relevan dalam top-k rekomendasi}}{k}
    $$
    Precision yang tinggi menunjukkan bahwa sistem sangat akurat dalam memberikan rekomendasi yang sesuai.

* **Recall@k**
    Mengukur proporsi item relevan yang berhasil ditemukan dalam top-k rekomendasi dari semua item yang relevan.
    **Formula:**
    $$
    \text{Recall@k} = \frac{\text{Jumlah item relevan dalam top-k rekomendasi}}{\text{Total item relevan yang tersedia}}
    $$
    Recall yang tinggi menunjukkan bahwa sistem mampu mencakup sebagian besar item yang relevan bagi pengguna.

#### **B. Hasil Evaluasi**
|           Model         | Precision@5 | Recall@5   |
|:------------------------|:------------|:-----------|
| Content-Based Filtering | 0.7436      | 0.2914     |
| Collaborative Filtering | 0.2384      | 0.1267     |

##### **Interpretasi**
* **Content-Based Filtering (CBF):**
    Model ini menunjukkan **performa yang sangat kuat dan unggul**, dengan **Precision@5 sebesar 74.36%**. Ini membuktikan bahwa model sangat akurat dalam menemukan game yang secara kontekstual mirip (berdasarkan kombinasi dan pembobotan fitur seperti Genre dan Publisher). Pendekatan ini efektif untuk merekomendasikan item dengan karakteristik yang sama persis.

* **Collaborative Filtering (CF - LightFM):**
    Model ini menghasilkan **Precision@5 sebesar 23.84%** dan **Recall@5 sebesar 12.67%**. Meskipun secara angka lebih rendah dari CBF, hasil ini menunjukkan bahwa model **cukup mampu menangkap pola preferensi kolektif** antar pengguna. Performa yang lebih rendah ini wajar mengingat data pengguna yang digunakan adalah hasil simulasi (sintetis), bukan data interaksi nyata.

#### **C. Visualisasi Evaluasi**
Visualisasi perbandingan metrik menunjukkan bahwa model **Content-Based Filtering secara signifikan lebih unggul** dibanding Collaborative Filtering dalam hal Precision@5 dan Recall@5. Content-Based Filtering mampu mencapai nilai presisi sekitar 74.36% dan recall 29.14%, sedangkan Collaborative Filtering hanya sekitar 23.84% dan 12.67%. Hal ini menunjukkan bahwa untuk dataset ini, pendekatan berbasis kemiripan fitur metadata (dalam konteks project ini 'combined_features') yang telah di-engineer dengan baik jauh lebih efektif dalam menghasilkan rekomendasi yang relevan.

## 7. Project Summary and Results
### Ringkasan Tahapan Proyek
Proyek ini dimulai dari eksplorasi data untuk memahami karakteristik dataset, dilanjutkan dengan preprocessing intensif untuk menangani *missing values* dan melakukan rekayasa fitur. Dua model utama dibangun: **Content-Based Filtering** berbasis kemiripan metadata dan **Collaborative Filtering** berbasis interaksi pengguna sintetis menggunakan LightFM. Keduanya dievaluasi menggunakan metrik Precision dan Recall.

Tahapan utama yang dilakukan pada project ini adalah sebagai berikut:
1.  **Eksplorasi Data (EDA)**
    * Menganalisis struktur data, mengidentifikasi missing values yang signifikan pada data rating, serta memahami distribusi penjualan dan skor.

2.  **Preprocessing**
    * Menangani missing values dengan menghapus kolom dan baris yang tidak lengkap.
    * Melakukan normalisasi pada fitur numerik dan membuat fitur kategori dari `User_Score` dan `Global_Sales`.
    * Membuat `user_id` sintetis untuk memungkinkan implementasi Collaborative Filtering.

3.  **Modeling**
    * **Content-Based Filtering** menggunakan TF-IDF dan *hybrid cosine similarity* yang menggabungkan kemiripan teks (metadata dengan pembobotan) dan kemiripan numerik (skor dan penjualan).
    * **Collaborative Filtering** menggunakan model **LightFM** dengan loss function `warp`, yang dioptimalkan untuk rekomendasi berbasis ranking dari data interaksi implisit.

4.  **Evaluasi**
    * Kinerja model diukur menggunakan metrik **Precision@5** dan **Recall@5**.
    * Hasil evaluasi menunjukkan bahwa **Content-Based Filtering memberikan performa yang jauh lebih unggul** dalam proyek ini, dengan presisi 74.36% dibandingkan 23.84% untuk Collaborative Filtering.

### **Hubungan dengan Business Understanding**
Tujuan utama dari proyek ini adalah **meningkatkan retensi pengguna dan keterlibatan (engagement)** dengan cara menyediakan rekomendasi game yang relevan berdasarkan preferensi pengguna atau karakteristik game yang disukai. Evaluasi akhir proyek memberikan dampak nyata terhadap kebutuhan bisnis tersebut, Dari sisi bisnis, sistem rekomendasi yang baik adalah sistem yang dapat mendorong **engagement**, **durasi bermain**, dan **loyalitas** pengguna. Berdasarkan hasil evaluasi, model yang dikembangkan memberikan dampak langsung terhadap tujuan ini.

---

#### **Apakah sudah menjawab setiap problem statement?**
**Iya,** proyek ini berhasil menjawab ketiga problem statement yang telah diidentifikasi:

#### **Hasil dari Problem Statement**
1.  **Bagaimana memberikan rekomendasi game yang relevan berdasarkan metadata konten game?**
    Sistem **Content-Based Filtering (CBF)** berhasil diimplementasikan dengan menggabungkan dan memberi bobot pada berbagai fitur metadata, termasuk `Genre`, `Platform`, `Publisher`, `Rating`, serta fitur hasil rekayasa seperti kategori skor dan penjualan. Model ini menunjukkan **performa yang sangat kuat dan akurat**, dibuktikan dengan skor **Precision@5 sebesar 0.7436**. Hasil ini mengindikasikan bahwa 74.36% dari 5 game teratas yang direkomendasikan memiliki kemiripan konten (Genre dan Publisher) yang relevan dengan game input, membuktikan efektivitas pendekatan ini dalam menemukan game yang serupa secara kontekstual.

2.  **Bagaimana memberikan rekomendasi game berdasarkan pola interaksi pengguna dengan game?**
    Sistem **Collaborative Filtering (CF)** berhasil dibangun menggunakan model LightFM. Karena dataset asli tidak memiliki data pengguna, **data interaksi user-item disimulasikan** untuk melatih model. Meskipun menggunakan data sintetis, model ini mampu menunjukkan kemampuan dasar dalam menangkap pola preferensi kolektif antar pengguna, dengan hasil evaluasi **Precision@5 sebesar 0.2384**.

3.  **Sejauh mana performa masing-masing pendekatan (CBF dan CF) dalam menghasilkan rekomendasi yang relevan?**
    Berdasarkan metrik evaluasi (`Precision@k` dan `Recall@k`), **Content-Based Filtering menunjukkan performa yang jauh lebih unggul** dalam proyek ini **(Precision@5 CBF: 0.7436 vs CF: 0.2384)**. Keunggulan signifikan ini disebabkan oleh:
    * **CBF**: Penggunaan set fitur yang kaya dan diberi bobot, sehingga mampu menangkap kemiripan konten dengan presisi tinggi.
    * **CF**: Keterbatasan data interaksi pengguna yang nyata (karena menggunakan data sintetis), yang menyebabkan performanya lebih rendah. Meskipun demikian, model ini tetap menunjukkan potensi jika diimplementasikan pada dataset dengan data interaksi pengguna yang asli.

---

#### **Apakah berhasil mencapai setiap goals yang diharapkan?**
**Iya, sebagian besar goals yang ditetapkan di awal proyek telah berhasil tercapai:**

* **Mengembangkan sistem rekomendasi personal:** **Tercapai.** Kedua model mampu menghasilkan daftar rekomendasi Top-N berdasarkan input pengguna atau preferensi historis (sintetis).
* **Mengeksplorasi dua pendekatan (CBF dan CF):** **Tercapai.** Kedua model berhasil diimplementasikan, dari preprocessing hingga evaluasi.
* **Melatih dan mengevaluasi model:** **Tercapai.** Metrik Precision@5 dan Recall@5 telah dihitung untuk kedua pendekatan, memberikan gambaran kuantitatif tentang kinerja masing-masing model.
* **Memberikan insight dari fitur yang mempengaruhi minat:** **Tercapai sebagian.** Melalui EDA (misalnya, popularitas genre) dan pembobotan fitur dalam model CBF, proyek ini memberikan insight awal tentang fitur apa yang penting untuk kemiripan. Namun, analisis yang lebih mendalam masih bisa dilakukan.
* **Menyediakan laporan dan sistem yang mudah dipahami:** **Tercapai.** Notebook ini berfungsi sebagai laporan analitis yang sistematis, dan adanya fungsi interaktif untuk mencari game dan mendapatkan rekomendasi membuatnya dapat digunakan dan dipahami oleh pengguna akhir.

---

#### **Apakah setiap solusi statement yang direncanakan berdampak? Jelaskan!**
**Iya,** setiap langkah dalam alur pengerjaan memberikan dampak yang terukur terhadap hasil akhir dan tujuan bisnis:

1.  **Dampak Exploratory Data Analysis (EDA):**
    * **Dampak:** EDA tidak hanya sekadar formalitas, tetapi menjadi dasar pengambilan keputusan krusial. Analisis distribusi data menunjukkan adanya *missing values* yang signifikan di kolom rating dan penjualan yang sangat *skewed*.
    * **Kontribusi Bisnis:** Tanpa EDA, model bisa jadi dibangun di atas data yang tidak valid, menghasilkan rekomendasi yang buruk. Pemahaman ini mengarahkan strategi preprocessing yang lebih tepat, seperti penghapusan kolom yang tidak reliabel dan normalisasi fitur, yang pada akhirnya meningkatkan kualitas model.

2.  **Dampak Preprocessing:**
    * **Dampak:** Tahap ini memiliki dampak paling signifikan pada performa model.
        * **Untuk CBF:** Pembuatan `combined_features` dengan pembobotan (misalnya, `Genre` diberi bobot 8x) terbukti sangat efektif, yang tecermin dari skor presisi yang tinggi (74.36%).
        * **Untuk CF:** Simulasi `user_id` adalah langkah krusial yang **memungkinkan implementasi dan evaluasi** pendekatan CF, yang jika tidak, tidak mungkin dilakukan sama sekali.
    * **Kontribusi Bisnis:** Preprocessing yang baik secara langsung meningkatkan akurasi (untuk CBF) dan membuka kemungkinan untuk strategi rekomendasi yang lebih canggih (untuk CF). Ini memastikan bahwa output yang diberikan kepada pengguna lebih relevan dan andal.

3.  **Dampak Modeling (Menerapkan Dua Pendekatan):**
    * **Dampak:** Implementasi CBF dan CF memberikan dua jenis solusi yang berbeda namun saling melengkapi. CBF menjawab kebutuhan "more of the same" (rekomendasi aman), sementara CF (meskipun dengan data sintetis) menjawab kebutuhan "discovery" (rekomendasi baru yang mungkin disukai).
    * **Kontribusi Bisnis:** Ini memberikan fleksibilitas. Untuk pengguna baru, CBF bisa menjadi andalan. Untuk pengguna lama, setelah data interaksi terkumpul, CF bisa diimplementasikan untuk mencegah kebosanan dan meningkatkan *long-term engagement*.

4.  **Dampak Interpretasi dan Evaluasi:**
    * **Dampak:** Evaluasi kuantitatif memberikan bukti nyata tentang keunggulan CBF dalam konteks dataset saat ini.
    * **Kontribusi Bisnis:** Ini adalah justifikasi berbasis data untuk keputusan bisnis. Hasil ini menginformasikan bahwa **investasi dalam pengayaan metadata game (untuk CBF) akan memberikan hasil cepat dan akurat**. Di sisi lain, hasil CF yang lebih rendah **membenarkan pentingnya pengumpulan data interaksi pengguna asli** jika bisnis ingin mengembangkan rekomendasi yang lebih personal di masa depan.

### Kesimpulan Akhir Project
Berdasarkan hasil evaluasi, kedua model menunjukkan kekuatan yang berbeda:
* **Content-Based Filtering sangat unggul dalam akurasi berbasis fitur (Precision@5 74.36%)**. Pendekatan ini ideal untuk merekomendasikan game yang "lebih dari yang sama" kepada pengguna.
* **Collaborative Filtering**, meskipun metriknya lebih rendah **(Precision@5 23.84%)**, menunjukkan potensi untuk rekomendasi yang lebih personal dan beragam. Performanya yang cukup baik dengan data sintetis menandakan potensinya akan jauh lebih besar dengan data interaksi pengguna yang nyata.

Kesimpulannya, tidak ada satu model yang "lebih baik" secara mutlak; keduanya melayani tujuan yang berbeda. CBF lebih presisi untuk kemiripan konten, sementara CF lebih berpotensi untuk penemuan game baru yang sesuai selera.

### **Rekomendasi & Pengembangan Selanjutnya**
1. **Gunakan data interaksi asli**
   Misalnya: rating, klik, atau review dari pengguna asli, bila inign membangun model dengan pendekatan CF.

2. **Perkaya fitur Content-Based Filtering**
   Dengan menambahkan informasi seperti deskripsi game, tag, dan ulasan pengguna untuk meningkatkan kualitas model CBF.

3. **Bangun Hybrid Recommender System**
   Kombinasi antara Content-Based dan Collaborative Filtering bisa memberikan hasil yang lebih seimbang dan akurat.

4. **Eksplorasi Model Lain**
   Seperti Matrix Factorization, Autoencoder, atau Neural Collaborative Filtering.

5. **Peningkatan hasil precision dan juga recall dari model**
   Hasil model yang dikerjakan dalam project ini masih dibilang cukup jauh dari kata optimal tapi masih bisa digunakan untuk merekomendasikan sebuah game secara umum. Tapi bila ingin mendapatkan hasil yang lebih baik metode dan juga dataset yang digunakan harus memiliki kualitas dan juga keragaman data yang lebih tinggi bila dibandingkan dengan project yang ilakukan saat ini.
   - **Implementasi Validasi yang Lebih Kuat**: Gunakan **train-test split** untuk mengevaluasi model pada data yang belum pernah dilihat, sehingga memberikan gambaran performa yang lebih akurat.
   - **Peningkatan Data User**: Gunakan data interaksi pengguna asli (rating, waktu bermain) untuk Collaborative Filtering agar hasil lebih realistis dan personal.
   - **Pengayaan Fitur Content-Based**: Tambahkan fitur lain seperti deskripsi teks game atau tag populer untuk memperkaya representasi metadata.
   - **Hybrid Recommender System**: Gabungkan kekuatan CBF dan CF untuk menciptakan model hybrid yang dapat memberikan rekomendasi yang akurat sekaligus beragam.
   - **Hyperparameter Tuning**: Lakukan tuning pada hyperparameter model LightFM untuk mengoptimalkan performa model CF.

### **Referensi**
* **Dataset**:
  [Video Game Sales Dataset - Kaggle](https://www.kaggle.com/datasets/rush4ratio/video-game-sales-with-ratings)

* **Tools & Libraries**:
  * `Scikit-learn` (TF-IDF, cosine similarity)
  * `LightFM` (Collaborative Filtering)
  * `Pandas`, `Numpy`, `Matplotlib`, `IPyWidgets`

* **Dokumentasi & Literatur**:
  * LightFM: [https://making.lyst.com/lightfm/docs/home.html](https://making.lyst.com/lightfm/docs/home.html)
  * Dokumentasi sklearn dan artikel tutorial sistem rekomendasi

**Note**
Bila terdapat output yang terlihat pada visualisasi hasil pada file ini, maka bisa akses langsung pada pengerjaan project saya langsung di colab menggunakan link berikut ini:

https://colab.research.google.com/drive/11eHsQEC0U-OjWYGsSoQjOV-_MdTRRmlZ?usp=sharing