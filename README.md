# pretest-data-analyst
A. KNOWLEDGE BASE
SQL Aggregation Understanding Sebuah tabel transaksi memiliki struktur sebagai berikut:
transaction_id | user_id | amount | transaction_date

1. Pertanyaan: Jelaskan perbedaan hasil antara:
SUM(amount),
SUM(DISTINCT amount).
- SUM(amount): Menjumlahkan seluruh nilai dalam kolom amount tanpa mempedulikan apakah ada nilai yang sama (duplikat). 

- SUM(DISTINCT amount): Hanya menjumlahkan nilai unik.

2.Data Architecture Understanding Pertanyaan:
Apa perbedaan antara data source dan data warehouse?
- Data Source: Sumber data mentah/operasional, Gudang data yang sudah dibersihkan dan terintegrasi dari berbagai sumber, dioptimalkan untuk analisis dan pelaporan.

Mengapa dashboard atau laporan sebaiknya tidak langsung membaca data dari data source?
- Risiko Mengganggu Sistem Operasional,Data Tidak Konsisten & Belum Siap Analisis

3.Error Analysis & Debugging Pertanyaan:
Apa perbedaan antara error yang disebabkan oleh perubahan schema dan error karena query yang salah? 
-Error karena Perubahan Schema,Terjadi karena struktur tabel berubah, tetapi query masih memakai struktur lama.
-Error karena Query yang SalahTerjadi karena penulisan atau logika query memang keliru.

Bagaimana cara membedakan kedua jenis error tersebut dalam praktik sehari-hari?
-Cek riwayat query,Lihat pesan error,Cek perubahan database

4.ETL Concept Pertanyaan:
Jelaskan definisi dari ETL (Extract, Transform, Load) process.

-ETL adalah proses untuk mengambil data dari sumber, mengolah/membersihkan data, lalu menyimpannya ke data warehouse agar siap digunakan untuk analisis dan laporan.

Sebutkan secara singkat fungsi dari masing-masing tahap ETL.
-Extract,Mengambil data dari berbagai sumber (database, API, file, dll).
-Transform,Membersihkan dan mengubah data agar rapi dan sesuai kebutuhan analisis
-Load,Menyimpan data yang sudah siap ke data warehouse

BI Tool & Performance Awareness Kondisi: Sebuah dashboard Business Intelligence (BI) menjadi sangat lambat ketika dibuka oleh user. Pertanyaan:
Sebutkan minimal tiga penyebab utama dashboard menjadi lambat.
- Query terlalu berat,Terlalu banyak JOIN, data besar, dan agregasi kompleks.
- Membaca langsung dari database operasional
- Tidak ada caching,Dashboard selalu menghitung ulang data setiap dibuka.
B. USE CASE (STUDI KASUS)
Use Case 1: Data Belum Tersedia

Kondisi: Anda diminta membuat visualisasi dashboard, namun data yang dibutuhkan belum tersedia atau belum dimigrasikan ke data warehouse.

Pertanyaan:
B. USE CASE (STUDI KASUS)
Use Case 1: Data Belum Tersedia

Kondisi: Anda diminta membuat visualisasi dashboard, namun data yang dibutuhkan belum tersedia atau belum dimigrasikan ke data warehouse.

Pertanyaan:

Apa langkah pertama yang harus dilakukan?
- lakukan koordinasi dengan tim Data Engineer atau Backend Developer,Memastikan apakah data tersebut memang sudah ada di Data Source (database produksi) atau benar-benar belum dikumpulkan sama sekali dan melakukan request sample data ke tim terkait
  
Use Case 2: Perbedaan Nilai Database dan Dashboard

Kondisi: Terdapat data transaksi di database dan di dashboard, namun nilai yang ditampilkan di dashboard berbeda dengan nilai di database.

Pertanyaan:

Apa kemungkinan penyebab terjadinya perbedaan tersebut?
-kemungkinan paling besar ada pada masalah data duplikat ataupun data di dalam database itu adalah data raw dan Perbedaan logika query.

Langkah apa yang harus dilakukan untuk memverifikasi dan menyelesaikan masalah ini?
- Verifikasi Adanya Data Duplikat (Data Raw)
- Periksa apakah dashboar ,Menggunakan data yang sudah dibersihkan Atau masih membaca data raw
- Bandingkan query,Query yang dipakai dashboard dan Query saat mengecek database
- Samakan Definisi dan Perbaiki

Use Case 3: Filter Tidak Sesuai Ekspektasi User

Kondisi: User ingin memfilter data berdasarkan kondisi tertentu, namun hasil yang ditampilkan tidak sesuai atau data tidak muncul seperti yang diharapkan.

Pertanyaan:

Apa langkah-langkah yang perlu dilakukan untuk menganalisis masalah ini?
- Klarifikasi Ekspektasi User,pastikan Filter apa yang dipilih user dan Data apa yang diharapkan muncul.
- Cek Data di Database,pastikan data dengan kondisi filter tersebut memang ada.
- Periksa Logika Filter di Query Bandingkan Filter di dashboard dan Kondisi WHERE di query.
  
Bagaimana cara memastikan filter bekerja sesuai dengan logika data?
- Samakan definisi filter,Definisi bisnis dan logika query harus sama.
- Gunakan data yang sudah dibersihkan,hindari filter langsung di data raw.
- Tes setiap filter satu per satu,pastikan tiap filter bekerja sebelum digabung
  
Use Case 4: Tipe Data Tidak Mendukung Aggregasi
Kondisi: Tipe data pada database tidak sesuai untuk dilakukan agregasi di BI tools (misalnya data numerik tersimpan sebagai string).

Pertanyaan:

Apa yang harus dilakukan untuk mengatasi masalah ini?
- Masalah diatasi dengan mengubah tipe data agar sesuai untuk agregasi melalui proses data transformation pada ETL, sehingga data siap digunakan oleh BI tools.
  
Apa nama proses yang dilakukan untuk memperbaiki kondisi tersebut?
- Data Transformation
  
Use Case 5: Nilai Terlalu Besar atau Terlalu Kecil
Kondisi: Query tidak menghasilkan error, namun hasil perhitungan menunjukkan angka yang terlalu besar atau terlalu kecil.

Pertanyaan:

Apa langkah-langkah validasi yang perlu dilakukan?
- Cek Data Duplikat
- Periksa Logika Query
- Validasi dengan Data Sampel
Bagaimana cara memastikan bahwa hasil perhitungan sudah benar?
-Gunakan satu sumber data yang sama,Dashboard dan query manual harus dari tabel yang sama
- Samakan definisi metric,Pastikan rumus bisnis jelas
- Lakukan cross-check
