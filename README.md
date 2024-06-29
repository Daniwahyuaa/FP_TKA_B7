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
Untuk cloud service provider yang kami gunakan untuk final project ini adalah Digital Ocean karena penggunaannya yang mudah dan harganya yang cukup terjangkau bagi kami. Berikut adalah desain rancangan arsitektur komputasi awan kami untuk final project ini. -image

Untuk tabel harga dari desain rancangan arsitektur komputasi awan kami adalah sebagai berikut. 
![image](https://github.com/Daniwahyuaa/FP_TKA_B7/assets/150106905/2e3f8afc-2078-43c2-adba-e8119ffaae97)

## Rancangan
Tabel Harga
Total kami menggunakan 2 Virtual Machine. VM1 sebagai Backend dan VM2 sebagai Frontend. Berikut untuk spesifikasi VM yang kami gunakan.

| No. | Nama | Spesifikasi | Fungsi | Harga/Bulan |
|----|------|-------------|--------|-------------|
| 1. | VM1  | Size Standard B1s, 1vCPUs, Ram 1GB | Backend | $10.66 |
| 2. | VM2  | Size Standard B1s, 1vCPUs, Ram 1GB | Frontend | $10.66 |
|--|--|--|Total | $21.32 |


<details>
<summary><h3>Klik Disini</h3></summary>
## Implementasi
1. VM 1
![Screenshot 2024-06-29 164224](https://github.com/Daniwahyuaa/FP_TKA_B7/assets/150106905/f1e56a4f-2638-4bff-b256-7e4ec30ec63d)

2. VM 2
![Screenshot 2024-06-29 164531](https://github.com/Daniwahyuaa/FP_TKA_B7/assets/150106905/937c8353-33cc-44d4-9b22-96e7cc062495)

3. MongoDB
![image](https://github.com/Daniwahyuaa/FP_TKA_B7/assets/150106905/fce4f059-ea96-4f85-86b1-ff2e04000e3e)

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

</details>

## Endpoints:
1. Analyze Text

* Endpoint: POST /analyze
* Description: This endpoint accepts a text input and returns the sentiment score of the text.
* Request:
```
{
   "text": "Your text here"
}
```
* Response:
```
{
  "sentiment": <sentiment_score>
}
```
2. Retrieve History

* Endpoint: GET /history
* Description: This endpoint retrieves the history of previously analyzed texts along with their sentiment scores.
* Response:
```
{
 {
   "text": "Your previous text here",
   "sentiment": <sentiment_score>
 },
 ...
}
```
Kemudian juga disediakan sebuah Frontend sederhana menggunakan index.html dan styles.css dengan tampilan antarmuka sebagai berikut
<img width="1186" alt="image" src="https://github.com/rzkirmdhani/FP-TKA/assets/141987387/6f860aab-2b8e-4a23-acea-6c07e3e6137d">
Kemudian anda diminta untuk mendesain arsitektur cloud yang sesuai dengan kebutuhan aplikasi tersebut. Apabila dana maksimal yang diberikan adalah 1 juta rupiah per bulan (65 US$) konfigurasi cloud terbaik seperti apa yang bisa dibuat?
## Rancangan
Pada final project kami menggunakan Microsoft Azure.


### Tabel Harga
Total kami menggunakan 2 VM. VM1 sebagai Backend dan VM2 sebagai Frontend. Berikut untuk spesifikasi VM yang kami gunakan.
| **No.** | **Nama** | **Spesifikasi** | **Fungsi** | **Harga/Bulan** |
|---------|-------------|-------------|-------------|-------------|
| 1. | VM1 | Size Standard B1s, 1vCPUs, Ram 1GB| Backend    | $7,59|
| 2. | VM2 | Size Standard B1s, 1vCPUs, Ram 1GB| Frontend    | $7,59|
|  |  | Total|     | $15,18|

# Implementasi
## Konfigurasi VM1
![alt text](https://media.discordapp.net/attachments/1191396440845598845/1256581567732776970/020068d1-db73-403c-b174-d30c2eee235b.png?ex=66814a47&is=667ff8c7&hm=4ec417024f70c490524167e7dee171465648626e0dedf17549f0e13988b1adad&=&format=webp&quality=lossless)

### (Backend)
1. Akses vm1 di PowerShell.
```
ssh -i ~/.ssh/id_rsa.pem azureuser@172.190.223.61
```


Buat file sentiment-analysis.py
```
from flask import Flask, request, jsonify
from flask_pymongo import PyMongo
from textblob import TextBlob
from datetime import datetime

app = Flask(__name__)
app.config["MONGO_URI"] = "mongodb://localhost:27017/sentiment_analysis_db"
mongo = PyMongo(app)

@app.route('/analyze', methods=['POST'])
def analyze_sentiment():
    data = request.json
    text = data.get('text')
    if not text:
        return jsonify({'error': 'No text provided'}), 400

    analysis = TextBlob(text)
    sentiment = analysis.sentiment.polarity
    mongo.db.sentiments.insert_one({
        'text': text,
        'sentiment': sentiment,
        'date': datetime.utcnow()
    })
    return jsonify({'sentiment': sentiment}), 200

@app.route('/history', methods=['GET'])
def get_history():
    sentiments = list(mongo.db.sentiments.find().sort("date", -1))
    for sentiment in sentiments:
        sentiment['_id'] = str(sentiment['_id'])
    return jsonify(sentiments), 200

@app.route('/delete-history', methods=['POST'])
def delete_history():
    mongo.db.sentiments.delete_many({})
    return '', 204

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```
### (MongoDB - Database)
1. Unduh MongoDB Community Edition.



2. Transfer file dari Download ke vm1.
```
scp -i ~/.ssh/id_rsa.pem C:/download/mongodb-linux-x86_64-7.0.11.tgz azureuser@172.190.223.61:~
```
3. Edit file konfigurasi /etc/mongod.conf
```
storage:
  dbPath: /var/lib/mongodb

systemLog:
  destination: file
  logAppend: true
  path: /var/log/mongodb/mongod.log

net:
  port: 27017
  bindIp: 127.0.0.1

processManagement:
  timeZoneInfo: /usr/share/zoneinfo
```
4. Jalankan dan Verifikasi mongoDB
```
sudo systemctl start mongod
sudo systemctl status mongod
```

5. Jalankan sentiment-analysis.py
```
python3 sentiment-analysis.py
```


## Konfigurasi VM2
![alt text](https://media.discordapp.net/attachments/1191396440845598845/1256582163219087442/37fc1af1-44a9-4cc0-ac7b-a0850dd55bd1.png?ex=66814ad5&is=667ff955&hm=b5e3ee2742938463c30bc6ffe9a4e632606bc95b3cbab8cbc78697beecde0029&=&format=webp&quality=lossless&width=550&height=248)

### Nginx
1. Akses VM2 di Powershell.
```
ssh -i ~/.ssh/id_rsa.pem azureuser@172.190.221.74
```
2. Edit file nginx pada /etc/nginx/sites-available/default
```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    index index.html;

    server_name _;

    location / {
        try_files $uri $uri/ =404;
    }

    location /analyze {
        proxy_pass http://172.190.223.61:5000/analyze;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /history {
        proxy_pass http://172.190.223.61:5000/history;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /delete-history {
        proxy_pass http://172.190.223.61:5000/delete-history;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
3. Restart nginx
```
sudo systemctl restart nginx
```
4. Ubah file index.html pada /var/www/html/index.html
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sentiment Analysis</title>
    <link href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1 class="text-center">Sentiment Analysis</h1>
        <div class="row">
            <div class="col-md-6">
                <div class="form-container">
                    <form id="sentiment-form">
                        <div class="form-group">
                            <textarea id="text-input" class="form-control" rows="4" placeholder="Enter text here..."></textarea>
                        </div>
                        <button type="submit" class="btn btn-success btn-block">Analyze</button>
                    </form>
                    <p id="result" class="text-center mt-4"></p>
                </div>
            </div>
            <div class="col-md-6">
                <div class="history-container">
                    <h2 class="text-center">History</h2>
                    <ul id="history" class="list-group mt-3"></ul>
                    <button id="clear-history" class="btn btn-danger btn-block mt-3">Clear History</button>
                </div>
            </div>
        </div>
    </div>

    <script>
        document.getElementById('sentiment-form').addEventListener('submit', async function(event) {
            event.preventDefault();
            const text = document.getElementById('text-input').value;
            try {
                const response = await fetch('/analyze', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify({ text }),
                });
                if (!response.ok) throw new Error('Network response was not ok');
                const result = await response.json();
                const resultElement = document.getElementById('result');
                resultElement.textContent = `Sentiment Score: ${result.sentiment}`;
                resultElement.className = result.sentiment === 0 ? 'bg-danger text-white' : 'bg-success text-white';
                fetchHistory();
            } catch (error) {
                console.error('There has been a problem with your fetch operation:', error);
            }
        });

        async function fetchHistory() {
            try {
                const response = await fetch('/history');
                if (!response.ok) throw new Error('Network response was not ok');
                const history = await response.json();
                const historyList = document.getElementById('history');
                historyList.innerHTML = '';
                history.forEach(item => {
                    const listItem = document.createElement('li');
                    listItem.className = `list-group-item history-item ${item.sentiment === 0 ? 'bg-danger text-white' : 'bg-success text-white'}`;
                    listItem.textContent = `Text: ${item.text}, Sentiment: ${item.sentiment}`;
                    historyList.appendChild(listItem);
                });
            } catch (error) {
                console.error('There has been a problem with your fetch operation:', error);
            }
        }

        document.getElementById('clear-history').addEventListener('click', async function() {
            try {
                const response = await fetch('/delete-history', { method: 'POST' });
                if (response.ok) {
                    fetchHistory();
                }
            } catch (error) {
                console.error('Error clearing history:', error);
            }
        });

        // Fetch history on page load
        fetchHistory();
    </script>
</body>
</html>
```

## Load Balancer
Pada final project saat ini kami menggunakan Load Balancer milik Azure.

1. Buat Load Balancer yang dimiliki oleh Azure

2. Buat PublicIP Load Balancer

3. Tambahkan Backend pools yaitu VM1 dan VM2

4. Buat Load Balncing Rules

* Jadi VM1 akan menjalankan Mongodb dan App Flask, mengolah data Sentiment.
* VM2 menjalankan Nginx dan menampilkan frontend, mengarahkan permintaan ke aplikasi Flask di VM1.
* Kunjungi PublicIP vm2 yaitu frontend atau juga bisa pada PublicIP Load balancer dan test lakuakan analisis.
* Frontend VM2 atau Load Balancer.
```
172.190.221.74   X   20.185.234.5
```


* Pada History
```
http://172.190.221.74/history
```


#  Pengujian endpoint API
Menggunakan Postman untuk Pengujian endpoint API.
1. Pada Analyze
```
http://172.190.221.74/analyze
```

2. Pada History
```
http://172.190.221.74/history
```




# Testing 
1. Locust testing dengan waktu 60 detik, maximum RPS 31.3, failure 0%
   
2. Locust testing dengan spawn rate 50, waktu 60 detik, maximum peak concurrency 100, maximum RPS 34.3, failure 0%.



3. Locust testing dengan spawn rate 100, waktu 60 detik, maximum peak concurrency 150, maximum RPS 41.5, failure 0%.



4. Locust testing dengan spawn rate 200, waktu 60 detik, maximum peak concurrency 250, maximum RPS 54.25, failure 0%.



5. Locust testing dengan spawn rate 500, waktu 60 detik, maximum peak concurency 505, maximum RPS 159.75, failure 24.5%.


   
# Kesimpulan dan Saran
Performansi sistem meningkat seiring dengan peningkatan spawn rate dan concurrency, dengan detail sebagai berikut:
   - Pada spawn rate 50, maximum peak concurrency mencapai 100 dan maximum RPS sebesar 34.3.
   - Pada spawn rate 100, maximum peak concurrency mencapai 150 dan maximum RPS sebesar 41.5.
   - Pada spawn rate 200, maximum peak concurrency mencapai 250 dan maximum RPS sebesar 54.25.
   - Pada spawn rate 500, maximum peak concurrency mencapai 500 dan maximum RPS sebesar 120.5.

# Revisi
Melakukan Rezise spesifikasi yang lebih tinggi dari Total $15,18 menjadi $60.74 dengan spesifikasi sebagai berikut.
| **No.** | **Nama** | **Spesifikasi** | **Fungsi** | **Harga/Bulan** |
|---------|-------------|-------------|-------------|-------------|
| 1. | VM1 | Size Standard B2s, 2vCPUs, Ram 4GB| Backend    | $30,37|
| 2. | VM2 | Size Standard B2s, 2vCPUs, Ram 4GB| Frontend    | $30,37|
|  |  | Total|     | $$60.74|

Rezise

VM1

VM2


Dengan melakukan Rezise VM berikut hasil Locust testing.
1. Locust testing dengan waktu 60 detik, maximum RPS 89.5, failure 0%

89.5/200x20= 13.425 Poin.
  
2. Locust testing dengan spawn rate 50, waktu 60 detik, maximum peak concurrency 700, maximum RPS 131.7, failure 0%.

131.7/200x30= 19.755 Poin.

3. Locust testing dengan spawn rate 100, waktu 60 detik, maximum peak concurrency 650, maximum RPS 170, failure 0%.

170/200x30= 25.5 Poin.

4. Locust testing dengan spawn rate 200, waktu 60 detik, maximum peak concurrency 600, maximum RPS 224, failure 0%.

224/200x30= 33.6 Poin.

5. Locust testing dengan spawn rate 500, waktu 60 detik, maximum peak concurency 650, maximum RPS 226.4, failure 0%.

226.4/200x30= 33.96 Poin.


# Kesimpulan dan Saran
Gunakan spesifikasi VM dengan maksimal asalkan tidak melebihi budget yang di tentukan.

# Video Revisi
Klik [Video]()

## Kesimpulan
Berdasarkan pengetesan yang kita lakukan menggunakan locust yang telah disediakan, ada beberapa faktor yang memengaruhi pengetesan tersebut yaitu koneksi internet, peak concurrency yang diinput, spawn rate dalam bentuk users/second yang diinput serta spesifikasi dari vm yang akan diisi dengan backend. Lalu dalam pengetesan ini juga kami menggunakan mongodb Compass yang merupakan GUI dari jenis database yang kita gunakan untuk menghapus data dari pengetesan sebelumnya untuk melakukan pengetesan baru agar optimal.

Berdasarkan pengetesan yang kita dapat juga bisa diambil bahwa semakin banyak spawn rate yang kita gunakan, semakin kecil peak concurrency dari stress test ini lalu RPS atau Request Per Second akan semakin tinggi. Apabila dalam projek kedepannya diperlukan vm serta pengujian yang mementingkan titik kerja dalam backend maka kita memerlukan vm yang diperuntukkan dalam keperluan backend yang lebih banyak dan membuat load balancer untuk vm-vm tersebut serta membuat vm-vm yang diperlukan untuk back-end tersebut dengan spesifikasi yang maksimal serta storage yang cukup untuk keperluan database.

