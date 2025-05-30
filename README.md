# Laporan Proyek Machine Learning - Ahmad Fuad Fauzi

## Project Overview

Seiring dengan pertumbuhan industri hiburan digital, anime telah menjadi salah satu bentuk hiburan paling populer di seluruh dunia. Menurut Parrot Analytics, anime menghasilkan total pendapatan global sebesar $19,8 miliar, termasuk $5,5 miliar dari streaming dan $14,3 miliar dari penjualan merchandise, menggaris bawahi dampak ekonomi dan budayanya yang signifikan di seluruh dunia. Platform seperti MyAnimeList, Crunchyroll, dan Netflix telah mengintegrasikan berbagai judul anime ke dalam katalog mereka untuk memenuhi permintaan global.

Namun, banyaknya jumlah anime yang tersedia seringkali menyulitkan pengguna untuk menemukan anime yang sesuai dengan preferensi mereka. Pengguna digital saat ini memiliki rentang atensi pendek dan menginginkan pengalaman yang tidak hanya relevan tetapi juga personal dalam waktu singkat. Oleh karena itu, personalisasi menjadi salah satu komponen krusial dalam platform distribusi konten untuk meningkatkan engagement dan kepuasan pengguna. Seperti yang ditekankan oleh Spicerack, personalisasi bukan lagi sekadar tren, melainkan sebuah ekspektasi dasar bagi konsumen digital. Dengan menyesuaikan pengalaman digital berdasarkan preferensi, perilaku, dan kebutuhan individu, platform dapat menciptakan koneksi yang lebih kuat dengan pengguna, meningkatkan kepuasan, dan mendorong loyalitas jangka panjang.

Proyek ini bertujuan untuk membangun sebuah sistem rekomendasi anime yang dapat memberikan saran yang dipersonalisasi kepada pengguna berdasarkan data karakteristik anime dan interaksi pengguna sebelumnya. Dengan menerapkan teknik Content-Based Filtering dan Collaborative Filtering, sistem ini diharapkan dapat memberikan rekomendasi yang lebih akurat dan relevan, sehingga meningkatkan retensi pengguna dan nilai bisnis platform yang menerapkannya.

References
[1] Dive into Average Human Attention Span Statistics & Facts. (2024, October 7). Alis Behavioral Health. Retrieved May 28, 2025, from https://www.alisbh.com/blog/average-human-attention-span-statistics-and-facts/
[2] Japanese Anime Captured $19.8 Billion in 2023 Global Revenue, Cementing Japan's Role as a Global Entertainment Leader. (2024, December 19). Parrot Analytics. Retrieved May 28, 2025, from https://www.parrotanalytics.com/announcements/japanese-anime-captured-dollar198-billion-in-2023-global-revenue-cementing-japans-role-as-a-global-entertainment-leader/
[3] Spicerack. (2023). The importance of personalization in digital customer experiences. Retrieved May 28, 2025, from https://spicerack.co.uk/article/the-importance-of-personalization-in-digital-customer-experiences

## Business Understanding
Memahami lanskap bisnis di balik platform distribusi anime sangat penting untuk mengembangkan solusi yang efektif. Dengan semakin besarnya popularitas anime secara global, platform seperti MyAnimeList, Crunchyroll, dan Netflix menghadapi tantangan sekaligus peluang besar. Mereka berupaya untuk menarik dan mempertahankan basis pengguna yang besar dengan menawarkan katalog anime yang luas. Namun, luasnya katalog ini justru menjadi pedang bermata dua.

### Problem Statement
Pengguna platform distribusi anime seperti MyAnimeList atau Netflix sering kali dihadapkan dengan banyaknya pilihan anime yang tersedia — mencapai ribuan judul dari berbagai genre, studio, dan demografi. Tanpa bantuan sistem rekomendasi yang efektif, pengguna:

* Menghabiskan waktu lebih lama untuk menemukan anime yang sesuai dengan preferensinya.
* Berisiko melewatkan judul-judul yang sebenarnya relevan dengan minat mereka.
* Merasa kewalahan karena kurangnya personalisasi dalam pencarian konten.

Di sisi lain, platform penyedia anime juga kehilangan peluang untuk meningkatkan keterlibatan dan retensi pengguna jika sistem rekomendasi tidak dapat bekerja secara optimal.

### Business Goals
Proyek ini bertujuan untuk:

1. Membangun sistem rekomendasi anime yang akurat dan personal, meskipun tanpa data interaksi pengguna.
2. Meningkatkan pengalaman pengguna dalam menemukan anime yang sesuai dengan minat mereka.
3. Mendukung pengambilan keputusan berbasis konten, baik untuk pengguna baru maupun yang sudah berpengalaman.


**Solution statements**
Untuk mencapai tujuan tersebut, akan digunakan dua pendekatan utama:

A. Content-Based Filtering (utama)
Sistem merekomendasikan anime berdasarkan kemiripan kontennya.Fitur-fitur yang dapat dimanfaatkan: Genres, Themes, Demographics, Synopsis (bisa diolah dengan TF-IDF + cosine similarity), Studios, Source, Rating, Type, Duration_Minutes. Contoh metode:
- TF-IDF + Cosine Similarity untuk teks (sinopsis, genre).
- One-hot encoding + similarity untuk fitur kategorikal.

Output: "Karena kamu menyukai anime A, kamu mungkin akan menyukai anime B yang mirip dari sisi genre dan cerita."

B. Popularity-Based Recommendation
Sistem menyarankan anime berdasarkan popularitas secara umum.Fitur yang digunakan:Score, Scored_Users, Ranked, Popularity, Members, Favorites. Pendekatan ini berguna sebagai baseline atau fallback, terutama saat pengguna belum memilih anime sebelumnya.Output: "Anime-anime terpopuler yang sedang digemari oleh komunitas."

## Data Understanding
Dataset ini diambil dari kaggle [Anime Database 2022](https://www.kaggle.com/datasets/harits/anime-database-2022) berisi 21.460 entri anime unik dengan 28 kolom berbeda, memberikan informasi yang kaya tentang setiap judul anime.Dataset Memiliki Total Baris: 21.460, Total Kolom: 28 dan Baris Duplikat: 0 (menunjukkan integritas data yang baik)

Missing Values
- Score dan Scored Users: Keduanya memiliki 32.14% nilai yang hilang. Ini adalah tantangan terbesar karena Score adalah metrik kualitas utama dan Scored_Users menunjukkan seberapa banyak interaksi rating yang ada.
- Ranked: Memiliki 8.97% nilai yang hilang.
- Episodes, Duration_Minutes, dan Rating: Memiliki missing values yang lebih sedikit, berkisar antara 2.54% hingga 2.79%.

Variabel-variabel pada anime dataset adalah sebagai berikut:
1. Identifikasi & Judul Anime:
    - `ID`: ID unik anime di [MyAnimeList.net](https://myanimelist.net/). Ini akan menjadi kunci penghubung untuk setiap entri anime.
    - `Title`: Judul asli anime.
    - `Synonyms`: Nama sinonim atau alternatif dari anime. Berguna untuk pencarian dan pemahaman konteks.
    - `Japanese`: Judul anime dalam bahasa Jepang.
    - `English`: Judul anime dalam bahasa Inggris.
2. Metadata Dasar & Produksi:
    - `Synopsis`: Ringkasan atau gambaran umum cerita anime. Ini adalah fitur teks yang sangat kaya dan penting untuk - Content-Based Filtering.
    - `Type`: Tipe format anime (misalnya, TV, Movie, OVA, Special).
    - `Episodes`: Jumlah episode. Kolom ini memiliki missing values yang perlu ditangani.
    - `Status`: Status penayangan anime (misalnya, Finished Airing, Currently Airing, Not yet aired).
    - `Start_Aired & End_Aired`: Tanggal atau tahun mulai dan berakhirnya penayangan. Berguna untuk analisis tren temporal.
    - `Premiered`: Musim penayangan perdana (misalnya, Spring 2022).
    - `Broadcast`: Jadwal siaran anime.
    - `Producers`: Daftar studio atau perusahaan produser. Fitur kategorikal yang bisa dipecah.
    - `Licensors`: Daftar perusahaan pemberi lisensi.
    - `Studios`: Daftar studio animasi yang bertanggung jawab atas produksi. Penting untuk Content-Based Filtering.
    - `Source`: Sumber asli adaptasi anime (misalnya, Manga, Original, Light novel).
3. Karakteristik Konten:
    - `Genres`: Daftar genre anime (misalnya, Action, Comedy, Fantasy, Slice of Life). Fitur multi-label yang sangat penting untuk Content-Based Filtering.
    - `Themes`: Daftar tema atau elemen cerita umum (misalnya, School, Mecha, Super Power, Military). Juga fitur multi-label yang penting.
    - `Demographics`: Demografi target audiens (misalnya, Shounen, Seinen, Shoujo).
4. Metrik Kualitas & Popularitas:
    - `Duration_Minutes`: Total durasi per menit setiap episode. Memiliki missing values kecil.
    - `Rating`: Peringkat usia (misalnya, PG-13, R+). Memiliki missing values kecil.
    - `Score`: Skor rata-rata anime dari pengguna MyAnimeList.net. Memiliki banyak missing values (32.14%). Ini adalah metrik kualitas eksplisit dari komunitas.
    - `Scored_Users`: Jumlah pengguna yang memberikan skor. Terkait langsung dengan Score dan memiliki persentase missing values yang sama.
    - `Ranked`: Peringkat anime berdasarkan skor. Memiliki missing values.
    - `Popularity`: Peringkat berdasarkan popularitas (jumlah pengguna yang menambahkan anime ke daftar mereka). Nilai yang lebih kecil berarti lebih populer. Distribusi nilai sangat luas.
    - `Members`: Jumlah pengguna yang telah menambahkan anime ke daftar mereka. Ini adalah metrik implicit feedback yang sangat kuat dan menunjukkan popularitas. Distribusi nilai juga sangat luas.
    - `Favorites`: Jumlah pengguna yang telah menandai anime sebagai favorit mereka. Indikator implicit feedback lainnya untuk preferensi yang kuat. Distribusi nilai juga sangat luas.

### Exploratory Data Analytics:
#### Insight Distribusi Data Numerik

Berikut adalah analisis distribusi dan implikasi dari fitur-fitur numerik utama:

1.  Episodes
    * Sebesar ~96% anime memiliki jumlah episode antara 1 hingga 102 (sebagian besar anime 1–100 episode).
    * Jumlah episode di atas 100 sangat jarang (hanya sekitar 4%), dan semakin tinggi jumlah episodenya, proporsinya makin kecil.
    * Implikasi: Anime panjang (lebih dari 100 episode) sangat jarang dalam dataset ini. Data cenderung didominasi oleh anime TV musiman, OVA, atau film.

2.  **Duration_Minutes**
    * Distribusi positif *skewed* (condong ke kanan, bukan kiri, karena nilai rendah lebih banyak).
    * Sekitar 27% anime berdurasi kurang dari 7 menit (kemungkinan anime pendek atau spesial).
    * Lebih dari 20% memiliki durasi antara 23–29 menit, yang merupakan durasi standar anime per episode.
    * Jumlah dengan durasi di atas 60 menit (film penuh) relatif kecil.
    * Implikasi: Mayoritas data adalah episode pendek atau standar TV (sekitar 24 menit), bukan film panjang.

3.  **Score**
    * Distribusi mendekati normal dengan puncak di rentang skor 6.2–7.4.
    * Hanya sebagian kecil yang memiliki skor di bawah 5 atau di atas 8.
    * Implikasi: Penilaian pengguna condong ke arah netral-positif. Anime jarang diberi nilai ekstrem rendah atau sangat tinggi.

4.  **Scored_Users**
    * Sekitar 58% anime memiliki kurang dari 33 ribu pengguna yang memberikan skor.
    * Proporsinya turun drastis dan semakin sedikit untuk anime dengan lebih dari 100 ribu pengguna yang memberikan skor.
    * Implikasi: Mayoritas anime dalam dataset ini kurang populer atau belum banyak ditonton/dinilai oleh banyak pengguna.

5.  **Ranked**
    * Rentang skor *ranked* tersebar sangat merata, tidak ada dominasi dari satu *bin*.
    * Proporsi dari tiap rentang ranking relatif konsisten di sekitar 3%.
    * Implikasi: Ranking anime tersebar luas secara merata, dan banyak anime memiliki ranking resmi (tidak *missing*).

#### Analisis Korelasi Antar Fitur Numerik

Analisis korelasi ini membantu kita memahami hubungan linier antara berbagai fitur numerik dalam dataset.

1. Korelasi Kuat Positif

    * `Scored_Users` dengan `Ranked`: 0.72
        Terdapat korelasi positif yang kuat antara jumlah pengguna yang memberikan skor dan peringkat anime. Semakin banyak pengguna yang memberi skor pada suatu anime (`Scored_Users`), kemungkinan besar peringkat anime tersebut (`Ranked`) juga semakin baik (angka rank lebih kecil). Dapat disimpulkan Anime yang populer dan banyak diulas cenderung memiliki posisi peringkat yang lebih tinggi.
    
    * `Episodes` dengan `Duration_Minutes`: 0.70
        Ada korelasi positif yang kuat antara jumlah episode dan durasi total per menit. Anime dengan lebih banyak episode cenderung memiliki durasi keseluruhan yang lebih panjang. Temuan ini adalah validasi yang diharapkan, menunjukkan bahwa total durasi sebuah serial memang berkorelasi kuat dengan jumlah episodenya.

2. Korelasi Lemah atau Hampir Nol

    * `Score` dengan `Episodes`: 0.03
        Korelasi yang sangat lemah antara panjangnya sebuah anime (jumlah episode) dan skor yang diberikan oleh pengguna. Dapat disimpulkan jumlah episode bukan indikator kualitas suatu anime menurut penilaian penonton. Anime pendek bisa sangat bagus, dan anime panjang bisa jadi biasa saja, atau sebaliknya.
    
    * `Score` dengan `Duration_Minutes`: -0.01
        Durasi total anime hampir tidak memiliki pengaruh terhadap skor yang diberikan. Mirip dengan jumlah episode, durasi total anime (baik pendek maupun panjang) tidak berkorelasi dengan tingkat kesukaan atau tidak kesukaan penonton.

3. Korelasi Negatif Sedang

    * `Ranked` dengan `Score`: -0.56
        Terdapat korelasi negatif sedang antara peringkat (`Ranked`) dan skor (`Score`). Semakin tinggi skor suatu anime (semakin bagus), maka peringkatnya akan semakin kecil (semakin baik posisinya). Korelasi ini sesuai harapan, karena sistem peringkat (ranking) di MyAnimeList.net secara umum ditentukan berdasarkan skor rata-rata pengguna.

4. Korelasi Lemah Positif

    * `Score` dengan `Scored_Users`: 0.11
        Ada kecenderungan positif yang sangat lemah bahwa anime yang mendapat skor lebih tinggi juga memiliki jumlah pengguna yang lebih banyak yang memberikan rating. Popularitas (dalam hal jumlah penilai) mungkin sedikit terkait dengan persepsi kualitas, namun hubungannya tidak signifikan secara kuat. Anime populer belum tentu selalu memiliki skor yang sangat tinggi, dan sebaliknya. 

#### Insight distribusi dan kelengkapan data kategorikal

1.  **Title**
    * Setiap judul bersifat unik (jumlah count = 1), menunjukkan bahwa data ini tidak memiliki duplikasi pada kolom Title.

2.  **Synonyms**
    * Sebagian besar nilai di kolom ini adalah 'Unknown' (9280 baris atau 43.24%), menunjukkan kurangnya informasi alternatif judul dari sebagian besar entri.
    * *Minna no Uta* muncul cukup sering (345 kali), mungkin karena merupakan seri lagu anak yang memiliki banyak episode.

3.  **Japanese & English Titles**
    * *English Title* memiliki tingkat ketidaktahuan yang sangat tinggi (57.68% Unknown), menunjukkan banyak anime tidak memiliki judul dalam bahasa Inggris.
    * Sebaliknya, *Japanese Title* relatif lengkap (hanya 0.29% Unknown), menandakan bahwa mayoritas data memiliki nama asli berbahasa Jepang.

4.  **Synopsis**
    * 12.67% entri tidak memiliki sinopsis, yang dapat menghambat proses pemahaman isi anime.
    * Beberapa deskripsi tampak pendek dan generik (misalnya "Film by Takashi Ito."), perlu validasi apakah ini sinopsis atau hanya metadata.

5.  **Type & Status**
    * Mayoritas anime berjenis *TV* (6280), *OVA* (3982), dan *Movie* (3900).
    * Sebagian besar anime berstatus *Finished Airing* (20,693), memberikan konteks kuat untuk analisis historis.

6.  **Airing Information (Start/End/Premiered/Broadcast)**
    * Tingkat 'Unknown' tinggi pada kolom:
        * *Premiered* (75.66%)
        * *Broadcast* (85.35%)
        * *End\_Aired* (5.00%)
        * *Start\_Aired* (1.74%)
    * Artinya, banyak entri tidak memiliki informasi waktu tayang yang lengkap, yang membatasi analisis tren waktu.

7.  **Production Credits**
    * Banyak data tidak memiliki informasi *Producers* (49.34%), *Licensors* (78.68%), dan *Studios* (37.70%), menunjukkan metadata produksi cukup tidak lengkap.
    * Namun, Toei Animation, NHK, dan Funimation adalah pihak yang paling sering muncul.

8.  **Source Material**
    * Sebagian besar anime berasal dari:
        * *Original* (7324)
        * *Manga* (4408)
    * Tetapi 16.78% dari data tidak memiliki informasi sumber (Unknown).

9.  **Genres, Themes, Demographics**
    * Kolom-kolom ini memiliki persentase 'Unknown' yang cukup besar:
        * *Genres* (17.24%)
        * *Themes* (44.96%)
        * *Demographics* (62.56%)
    * Ini menjadi kendala dalam analisis segmentasi konten berdasarkan genre atau target audiens.

10. **Rating**
    * Mayoritas anime ditujukan untuk remaja dan semua umur:
        * *PG-13*: 7655 entri
        * *G - All Ages*: 6840 entri
    * Menunjukkan dominasi konten yang cocok untuk penonton umum.

## Data Preparation
Untuk memastikan kualitas data sebelum membangun sistem rekomendasi, dilakukan beberapa tahapan pembersihan dan transformasi data sebagai berikut:

* **Menghapus baris dengan nilai kosong (missing values)**
  Dari total 21.460 baris, setelah menghapus seluruh baris yang memiliki nilai kosong, tersisa 12.937 baris data yang valid dan siap diproses.

* **Menghapus kolom dengan dominasi nilai 'Unknown' > 40%**
  Kolom-kolom seperti `'Synonyms'`, `'English'`, `'Premiered'`, `'Broadcast'`, `'Licensors'`, `'Producers'`, `'Themes'`, dan `'Demographics'` dihapus karena nilai 'Unknown'-nya melebihi ambang batas tersebut.

* **Transformasi logaritmik pada fitur yang memiliki distribusi sangat miring (skewed)**
  Kolom berikut dilakukan transformasi log untuk mengurangi skewness dan meningkatkan kinerja model:

  * `'Episodes'` → `'Episodes_log'`
  * `'Duration_Minutes'` → `'Duration_Minutes_log'`
  * `'Scored_Users'` → `'Scored_Users_log'`
  * `'Members'` → `'Members_log'`
  * `'Favorites'` → `'Favorites_log'`

* **Ekstraksi dan encoding fitur `Genres`**
  Kolom `Genres` yang awalnya berupa string dengan beberapa genre dipisahkan menggunakan koma. Data dikonversi menjadi format multihot encoding menggunakan `MultiLabelBinarizer`. Hasil encoding ini digabungkan kembali ke dalam dataframe utama.

* **Menghapus kolom non-relevan untuk pemodelan**
  Kolom seperti `'ID'`, `'Synopsis'`, `'Title'`, `'Japanese'`, `'Studios'`, dan `'End_Aired'` dihapus. `'End_Aired'` dihapus karena sebagian besar anime masih berstatus ongoing (hanya 6.568 dari 12.937 baris yang memiliki nilai).

* **Ekstraksi tahun dari kolom `Start_Aired`**
  Tahun diambil dari data tanggal pada kolom `Start_Aired`. Nilai yang kosong diisi dengan nilai median, kemudian kolom aslinya dihapus dan hasil ekstraksi disimpan di kolom baru `Start_Year`.

* **One-Hot Encoding pada kolom kategorikal**
  Kolom kategorikal berikut dilakukan encoding menjadi format numerik:

  * `'Type'`
  * `'Status'`
  * `'Source'`
  * `'Rating'`

* **Normalisasi (Min-Max Scaling)**
  Semua fitur numerik (selain kolom hasil encoding `Genres`) dinormalisasi ke rentang 0–1 menggunakan `MinMaxScaler` untuk mempersiapkan perhitungan *cosine similarity* pada model berbasis konten.

* **Menyimpan hasil akhir**
  Seluruh data yang telah dibersihkan dan ditransformasikan disimpan ke dalam sebuah dataframe final dengan nama `df_final`.

## Modeling
Dalam tahap ini, dibangun dua pendekatan sistem rekomendasi yaitu **Popularity-Based Filtering** dan **Content-Based Filtering**. Keduanya menggunakan data hasil preprocessing pada `df_final`.

### Popularity-Based Filtering

Pendekatan ini merekomendasikan anime berdasarkan tingkat popularitasnya. Ukuran popularitas yang digunakan adalah jumlah anggota (`Members_log`) yang menandakan seberapa banyak pengguna yang menambahkan anime tersebut ke daftar mereka. Anime dengan nilai `Members_log` tertinggi akan dianggap sebagai yang paling populer dan direkomendasikan secara umum kepada semua pengguna.

### Content-Based Filtering

Sistem ini merekomendasikan anime yang memiliki kemiripan fitur dengan anime tertentu. Langkah-langkahnya sebagai berikut:

1. **Penyusunan Matriks Fitur**
   Menggunakan seluruh kolom numerik dan kategorikal (hasil One-Hot Encoding) kecuali kolom judul, sebuah matriks fitur disusun untuk mewakili setiap anime secara numerik.

2. **Normalisasi Data**
   Setiap fitur dinormalisasi menggunakan *MinMaxScaler* agar setiap kolom berada dalam skala yang sama. Ini penting agar fitur dengan rentang besar tidak mendominasi perhitungan kemiripan.

3. **Perhitungan Cosine Similarity**
   Digunakan cosine similarity untuk mengukur tingkat kemiripan antar anime berdasarkan vektor fiturnya. Semakin tinggi nilai similarity, semakin mirip dua anime tersebut.

4. **Fungsi Rekomendasi**
   Dibuat fungsi yang menerima input berupa judul anime dan mengembalikan daftar anime lain yang paling mirip berdasarkan skor kemiripan.

Pendekatan **popularity-based** cocok digunakan untuk pengguna baru atau saat tidak ada input preferensi, sementara **content-based** dapat memberikan rekomendasi yang lebih personal berdasarkan anime yang telah disukai pengguna.

## Evaluation

Sistem rekomendasi yang dibangun pada proyek ini tidak berbasis interaksi pengguna (seperti rating, klik, atau histori penayangan), sehingga metrik evaluasi kuantitatif seperti RMSE, MAE, Precision\@K, atau Recall\@K tidak dapat diterapkan secara langsung.

Sebagai gantinya, evaluasi dilakukan dengan pendekatan **analisis distribusi skor kemiripan (cosine similarity)** antar item pada sistem content-based filtering.

### Metrik yang Digunakan

* **Cosine Similarity**
  Cosine similarity mengukur kemiripan antara dua vektor dalam ruang fitur berdasarkan sudut di antara keduanya. Formula umumnya adalah:

```
Cosine Similarity = (A · B) / (||A|| * ||B||)
```

  Nilainya berkisar antara:

  * `1`: sangat mirip,
  * `0`: tidak mirip,
  * `-1`: sangat berlawanan (tidak berlaku pada data non-negatif seperti ini).

###  Hasil Evaluasi

* Distribusi skor cosine similarity cenderung berada di rentang menengah hingga tinggi.
* Ini menunjukkan bahwa fitur-fitur yang digunakan (genre, tipe, status, sumber cerita, rating, dan statistik lainnya) cukup informatif dalam membedakan dan mengelompokkan anime.
* Fungsi rekomendasi mampu mengembalikan daftar anime yang relevan secara konten dengan anime input pengguna.

###  Keterbatasan

* Tidak adanya ground truth (data interaksi pengguna) membuat kita tidak bisa mengukur secara objektif seberapa relevan hasil rekomendasi untuk masing-masing individu.
* Evaluasi ini masih bersifat kualitatif dan eksploratif.

### Rekomendasi Evaluasi Lanjutan

Jika ke depannya tersedia data interaksi pengguna, maka metrik seperti berikut dapat digunakan:

* **Precision\@K**: Persentase item relevan dalam K hasil rekomendasi.
* **Recall\@K**: Persentase item relevan yang berhasil ditemukan dari seluruh item relevan.
* **Mean Average Precision (MAP)**: Mengukur rata-rata presisi dari posisi item-item relevan dalam ranking.


#  Penutup

Proyek sistem rekomendasi anime ini dibuat sebagai bagian dari submission *Bootcamp CodingCamp powered by DBS* pada alur belajar **Machine Learning Engineer**, sub bab **Machine Learning Terapan**.


