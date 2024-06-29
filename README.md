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

## Implementasi
1. VM 1
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
2. MongoDB
![image](https://github.com/Daniwahyuaa/FP_TKA_B7/assets/150106905/0f4aab85-cf8a-4538-b878-8a323c967655)
Pindahkan File ke Backend
```
-nope
```
3.. VM 2
![Screenshot 2024-06-29 164531](https://github.com/Daniwahyuaa/FP_TKA_B7/assets/150106905/937c8353-33cc-44d4-9b22-96e7cc062495)

5. MongoDB
![image](https://github.com/Daniwahyuaa/FP_TKA_B7/assets/150106905/fce4f059-ea96-4f85-86b1-ff2e04000e3e)

