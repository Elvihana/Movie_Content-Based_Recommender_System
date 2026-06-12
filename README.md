# Movie Content-Based Recommender System 

## Latar Belakang
Dalam industri hiburan digital, pengguna sering kali menghadapi kesulitan untuk menemukan film baru yang sesuai dengan preferensi mereka di antara banyaknya pilihan yang tersedia. Sistem rekomendasi berbasis konten (*Content-Based Recommender System*) hadir sebagai solusi untuk menyarankan film baru yang memiliki kemiripan karakteristik spesifik dengan film yang disukai oleh pengguna.

## Tujuan Proyek
* Membangun sistem rekomendasi film berbasis fitur teks (*Content-Based Recommender System*).
* Menyediakan fungsi yang mampu mengembalikan 10 rekomendasi film teratas berdasarkan kesamaan metadata intrinsik film.

## Dataset
Proyek ini mengintegrasikan tiga jenis dataset publik dari DQLab CDN:
1. `movie_rating_df.csv`: Berisi informasi dasar film seperti ID film (`tconst`), judul, jenis, tahun rilis, durasi, genre, nilai rata-rata rating, dan jumlah *votes*.
2. `actor_name.csv`: Berisi data nama aktor/aktris (`primaryName`) beserta ID film terkait yang mereka bintangi (`knownForTitles`).
3. `directors_writers.csv`: Berisi data nama sutradara (`director_name`) dan penulis skenario (`writer_name`) untuk setiap ID film.

## Tahapan Analisis
1. **Load Data & Eksplorasi Awal**: Membaca ketiga dataset menggunakan Pandas serta menampilkan 5 baris teratas dan informasi tipe datanya.
2. **Transformasi Menjadi List**: Mengubah data string pada nama sutradara dan penulis skenario yang dipisahkan koma menjadi bentuk objek *list*.
3. **Unnesting & Grouping Data Aktor**: Memecah (*unnest*) relasi jamak film pada data aktor agar berkorespondensi 1-1, kemudian mengelompokkannya kembali (*grouping*) berdasarkan ID film untuk mendapatkan daftar *cast* lengkap per film.
4. **Integrasi Data (Merging)**: Menggabungkan (*inner join*) tabel dasar film dengan tabel daftar pemain (*cast*), serta tabel sutradara dan penulis.
5. **Pembersihan Data & Reformat Tabel**: Menghapus kolom yang tidak diperlukan, mengisi nilai kosong (*missing values*) dengan label `'Unknown'`, memformat ulang kolom genre ke dalam bentuk *list*, serta melakukan *rename* pada beberapa nama kolom utama.
6. **Sanitasi Teks (Sanitize)**: Membuat fungsi untuk menghapus spasi (`replace(' ', '')`) dan mengubah semua huruf menjadi kecil (`lower()`) pada kolom *cast, genres, writer*, dan *director* agar proses tokenisasi lebih akurat.
7. **Pembuatan Metadata Soup**: Menggabungkan seluruh fitur teks yang telah disanitasi dari setiap film menjadi satu kesatuan kalimat string panjang (*soup*).

## Model yang Digunakan
* **CountVectorizer (scikit-learn)**: Digunakan untuk mengonversi teks gabungan (*soup*) menjadi matriks frekuensi kemunculan kata (*count matrix*) dengan mengeliminasi kata-kata umum menggunakan parameter `stop_words='english'`.
* **Cosine Similarity (scikit-learn)**: Digunakan untuk menghitung skor derajat kemiripan antar-vektor film dari *count matrix*. Output yang dihasilkan berada pada rentang skor -1 sampai 1, di mana nilai yang mendekati 1 menandakan tingkat kemiripan yang sangat tinggi.

## Hasil
Proyek ini menghasilkan sebuah sistem pemetaan kemiripan antar-film. Melalui implementasi pemetaan terbalik (*reverse mapping*) indeks menggunakan fungsi `content_recommender(title)`, sistem berhasil mengurutkan tingkat kemiripan film dari yang tertinggi dan menampilkan daftar 10 film teratas. Sebagai contoh, ketika fungsi dijalankan dengan input judul film `'The Lion King'`, sistem secara otomatis mengembalikan 10 film rekomendasi terbaik yang relevan berdasarkan kesamaan kru produksi, pemain, dan genre.

## Kesimpulan
Sistem rekomendasi berbasis konten diimplementasikan dengan memanfaatkan kombinasi fungsi teks terstruktur, `CountVectorizer`, dan perhitungan matriks `Cosine Similarity`. Pendekatan ini terbukti efektif dalam menyaring dan merekomendasikan film-film yang serupa berdasarkan karakteristik intrinsiknya (kru, pemain, genre) tanpa bergantung pada data riwayat rating atau perilaku pengguna lain.
