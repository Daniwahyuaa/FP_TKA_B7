# FP_TKA_B7
# Cloud Specifications

|  NRP|Nama Anggota  |
|--|--|
|5027231038|Dani Wahyu A. A.|
|5027221048|Malvin P. R.|
|5027231068|M. Fadhil S.|
|5027231084|Farand F.|

# Permasalahan
Anda adalah seorang lulusan Teknologi Informasi, sebagai ahli IT, salah satu kemampuan yang harus dimiliki adalah Keampuan merancang, membangun, mengelola aplikasi berbasis komputer menggunakan layanan awan untuk memenuhi kebutuhan organisasi.

Pada suatu saat anda mendapatkan project untuk mendeploy sebuah aplikasi Sentiment Analysis dengan komponen Backend menggunakan python: sentiment-analysis.py.

Kemudian juga disediakan sebuah Frontend sederhana menggunakan index.html dan styles.css dengan tampilan antarmuka sebagai berikut. image

Kemudian anda diminta untuk mendesain arsitektur cloud yang sesuai dengan kebutuhan aplikasi tersebut. Apabila dana maksimal yang diberikan adalah 1 juta rupiah per bulan (65 US$) konfigurasi cloud terbaik seperti apa yang bisa dibuat?

## Desain Rancangan Arsitektur Komputasi Awan dan Tabel Harga
Untuk cloud service provider yang kami gunakan untuk final project ini adalah Digital Ocean karena penggunaannya yang mudah dan harganya yang cukup terjangkau bagi kami. Berikut adalah desain rancangan arsitektur komputasi awan kami untuk final project ini. WhatsApp Image 2024-06-25 at 22 55 21_06b9f5f8

Untuk tabel harga dari desain rancangan arsitektur komputasi awan kami adalah sebagai berikut. WhatsApp Image 2024-06-25 at 22 55 25_f8d72088

## Implementasi
1. Setup MongoDB
image

2. Setup 2 Droplet 8$ untuk Back-End
image

image

3. Setup 1 Droplet 8$ untuk Front-End
image

4. Setup Front-End
Install apache2 dengan command sudo apt-get install apache2
Konfigurasi index.html dan styles.css
Masukkan command systemctl restart apache2 untuk restart service apache2 agar konfigurasi bisa terpakai
Akses web melalui IP Droplet atau IP Loadbalancer
image

image

5. Setup Back-End
Install python3 dengan command sudo apt-get install python3
Install dependencies yang diperlukan :
sudo apt-get install python3-pip
pip install flask
pip install flask_cors
pip install textblob
pip install pymongo
pip install gunicorn
Lakukan konfigurasi terhadap sentiment_analysis.py
Nyalakan dengan command gunicorn -b 0.0.0.0:5000 sentiment_analysis:app --daemon agar tetap berjalan ketika device server dimatikan
image

6. Locustfile
Lakukan deploy terhadap locustfile menggunakan locust -f locustfile.py --host http://(IP backend):80

## Pengujian API
1. Analyze text
Akses melalui ip-address-frontend/analyze
image

2. Retrieve History
Akses melalui ip-address-backend/history
image

## Hasil Load Testing
Jumlah RPS maksimum selama 60 detik image Untuk mencari jumlah RPS maksimum, berikut adalah konfigurasi locust kami.
Number of users: 1500

Users per second: 500

RPS maksimum: 188,17

Failures: 0%

Jumlah peak concurrency maksimum yang dapat ditangani oleh server dengan spawn rate 50 dan durasi waktu load testing 60 detik image
Saat pengujian dengan spawn rate 50, didapatkan peak concurrency maksimum 5000 dengan failure 0% serta RPS 143,6 (21,54 poin)

Jumlah peak concurrency maksimum yang dapat ditangani oleh server dengan spawn rate 100 dan durasi waktu load testing 60 detik image
Saat pengujian dengan spawn rate 100, didapatkan peak concurrency maksimum 4000 dengan failure 0% serta RPS 169,17 (25,37 poin)

Jumlah peak concurrency maksimum yang dapat ditangani oleh server dengan spawn rate 200 dan durasi waktu load testing 60 detik image
Saat pengujian dengan spawn rate 200, didapatkan peak concurrency maksimum 2000 dengan failure 0% serta RPS 215,1 (26,025 poin)

Jumlah peak concurrency maksimum yang dapat ditangani oleh server dengan spawn rate 500 dan durasi waktu load testing 60 detik image
Saat pengujian dengan spawn rate 500, didapatkan peak concurrency maksimum 1500 dengan failure 0% serta rata-rata RPS 95,6 dengan peak RPS 251,63.

## Kesimpulan
Berdasarkan pengetesan yang kita lakukan menggunakan locust yang telah disediakan, ada beberapa faktor yang memengaruhi pengetesan tersebut yaitu koneksi internet, peak concurrency yang diinput, spawn rate dalam bentuk users/second yang diinput serta spesifikasi dari vm yang akan diisi dengan backend. Lalu dalam pengetesan ini juga kami menggunakan mongodb Compass yang merupakan GUI dari jenis database yang kita gunakan untuk menghapus data dari pengetesan sebelumnya untuk melakukan pengetesan baru agar optimal.

Berdasarkan pengetesan yang kita dapat juga bisa diambil bahwa semakin banyak spawn rate yang kita gunakan, semakin kecil peak concurrency dari stress test ini lalu RPS atau Request Per Second akan semakin tinggi. Apabila dalam projek kedepannya diperlukan vm serta pengujian yang mementingkan titik kerja dalam backend maka kita memerlukan vm yang diperuntukkan dalam keperluan backend yang lebih banyak dan membuat load balancer untuk vm-vm tersebut serta membuat vm-vm yang diperlukan untuk back-end tersebut dengan spesifikasi yang maksimal serta storage yang cukup untuk keperluan database.

