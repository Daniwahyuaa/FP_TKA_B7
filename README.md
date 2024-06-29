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

Kemudian juga disediakan sebuah Frontend sederhana menggunakan index.html dan styles.css dengan tampilan antarmuka sebagai berikut. 

![image](https://github.com/Daniwahyuaa/FP_TKA_B7/assets/150106905/2e3f8afc-2078-43c2-adba-e8119ffaae97)

Kemudian anda diminta untuk mendesain arsitektur cloud yang sesuai dengan kebutuhan aplikasi tersebut. Apabila dana maksimal yang diberikan adalah 1 juta rupiah per bulan (65 US$) konfigurasi cloud terbaik seperti apa yang bisa dibuat?

## Desain Rancangan Arsitektur Komputasi Awan dan Tabel Harga
Untuk cloud service provider yang kami gunakan untuk final project ini adalah Digital Ocean karena penggunaannya yang mudah dan harganya yang cukup terjangkau bagi kami. Berikut adalah desain rancangan arsitektur komputasi awan kami untuk final project ini.

Untuk tabel harga dari desain rancangan arsitektur komputasi awan kami adalah sebagai berikut. 

## Rancangan

![WhatsApp Image 2024-06-29 at 21 15 31_7424e80d](https://github.com/Daniwahyuaa/FP_TKA_B7/assets/150106905/5d9bd014-bb39-4d26-b580-26de6bed3ce1)

Tabel Harga
Total kami menggunakan 2 Virtual Machine. VM1 sebagai Backend dan VM2 sebagai Frontend. Berikut untuk spesifikasi VM yang kami gunakan.

| No. | Nama | Spesifikasi | Fungsi | Harga/Bulan |
|----|------|-------------|--------|-------------|
| 1. | VM1  | Size Standard B1s, 1vCPUs, Ram 1GB | Backend | $10.66 |
| 2. | VM2  | Size Standard B1s, 1vCPUs, Ram 1GB | Frontend | $10.66 |
|--|--|--|Total | $21.32 |

# Implementasi
## 1. VM 1
![Screenshot 2024-06-29 164224](https://github.com/Daniwahyuaa/FP_TKA_B7/assets/150106905/f1e56a4f-2638-4bff-b256-7e4ec30ec63d)
### BACKEND
1. Akses Virtual Machine (Back End) di PowerShell.
```
ssh -i ~/.ssh/id_rsa.pem daniwahyuaa@52.184.80.106
```
![image](https://github.com/Daniwahyuaa/FP_TKA_B7/assets/150106905/cd947e5d-7920-490e-90c3-f833261d3073)
#### Membuat file sentiment-analysis.py
```py
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
## 2. MongoDB
![image](https://github.com/Daniwahyuaa/FP_TKA_B7/assets/150106905/a09dc2e2-217a-4140-a48e-e2c83b935ec2)
Buka PowerShell
Pindahkan File ke Backend
```
scp -i ~/.ssh/id_rsa.pem C:\Users\ASUS/Downloads/mongodb-linux-x86_64-ubuntu2204-7.0.12.tgz daniwahyuaa@52.184.80.106:~
```
setelah itu
```
ssh -i ~/.ssh/id_rsa.pem daniwahyuaa@52.184.80.106
```
Edit file konfigurasi /etc/mongod.conf
```conf
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
setelah itu kita start mangodnya
```
sudo systemctl start mongod
sudo systemctl status mongod
```
![image](https://github.com/Daniwahyuaa/FP_TKA_B7/assets/150106905/d51f2815-da8c-404c-ba84-3d6d1ffaf78d)
**jalankan sentiment-analysis.py**
![image](https://github.com/Daniwahyuaa/FP_TKA_B7/assets/150106905/e22fbfa1-4725-4b05-ba84-9eb13e060963)

## 3.. VM 2
![Screenshot 2024-06-29 164531](https://github.com/Daniwahyuaa/FP_TKA_B7/assets/150106905/937c8353-33cc-44d4-9b22-96e7cc062495)
pertama-tama kita akses di PowerShell
```
ssh -i ~/.ssh/id_rsa.pem daniwahyuaa@40.81.22.20
```
setelah itu Edit file nginx pada /etc/nginx/sites-available/default
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
        proxy_pass http://52.184.80.106:5000/analyze;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /history {
        proxy_pass http://52.184.80.106:5000/history;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /delete-history {
        proxy_pass http://52.184.80.106:5000/delete-history;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
Setelah mengedit file nginx, kita nyalakan ulang file nginx nya
```
sudo systemctl restart nginx
```
Setelah itu, Ubah file index.html pada /var/www/html/index.html
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
## 4. Load Balancer
pada kesempatan ini, saya menggunakan Load Balance dari Azure itu sendiri
![WhatsApp Image 2024-06-30 at 01 08 26_373c9186](https://github.com/Daniwahyuaa/FP_TKA_B7/assets/150106905/9b75df78-08b5-46e3-b9de-81582a16d8ca)
pertama tama setting public IPnya dulu
![WhatsApp Image 2024-06-30 at 01 15 25_c7dc2667](https://github.com/Daniwahyuaa/FP_TKA_B7/assets/150106905/c4314a63-6763-4c30-8ec3-f4eed27cb881)
Setelah itu, kita setting Backendnya
![WhatsApp Image 2024-06-30 at 01 22 19_f059949b](https://github.com/Daniwahyuaa/FP_TKA_B7/assets/150106905/804355f9-3375-401b-86d1-a2f0255dfd55)
Terakhir ini untuk Rulesnya
![WhatsApp Image 2024-06-30 at 01 25 48_bb8d7e5f](https://github.com/Daniwahyuaa/FP_TKA_B7/assets/150106905/6edd9e4a-3561-4708-badc-3faa91cba428)





