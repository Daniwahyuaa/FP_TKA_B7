# FP_TKA_B7
# Cloud Specifications

|  NRP|Nama Anggota  |
|--|--|
|5027231038|Dani Wahyu A. A.|
|5027221048|Malvin P. R.|
|5027231068|M. Fadhil S.|
|5027231084|Farand F.|

## Rancangan Arsitektur Cloud 1
![image](https://github.com/Daniwahyuaa/FP_TKA_B7/assets/150106905/86c268df-c573-4360-bd25-7c363f06bf7b)

## Configuration 1
| No | Name             | Specifications          | Function       | Price/Month |
|----|------------------|-------------------------|----------------|-------------|
| 1  | VM3-LoadBalancer | Regular 1vCPU 2GB Memory| Load Balancer  | $12         |
| 2  | VM2-Worker1      | Regular 1vCPU 1GB Memory| App Worker 1   | $8          |
| 3  | VM2-Worker2      | Regular 1vCPU 1GB Memory| App Worker 2   | $8          |
| 4  | VM2-Worker3      | Regular 1vCPU 1GB Memory| App Worker 3   | $8          |
| 5  | VM3-Database     | Regular 1vCPU 2GB Memory| Database Server| $12         |
|    | **TOTAL**        |                         |                | **$40**     |

**Masalah yang Mungkin:**

1. **Beban Server Database:**
   - Database Server hanya memiliki 1vCPU dan 2GB memori, yang mungkin tidak cukup jika beban database meningkat.
   
2. **Reliabilitas dan Ketersediaan:**
   - Tidak ada redundansi untuk Database Server. Jika server ini gagal, seluruh sistem akan terhenti.

3. **Skalabilitas:**
   - Menggunakan tiga App Worker dengan 1vCPU dan 1GB memori masing-masing mungkin kurang efisien dibandingkan dengan menggunakan VM yang lebih kuat atau menambahkan mekanisme auto-scaling.


## Rancangan Arsitektur Cloud 1
![image](https://github.com/Daniwahyuaa/FP_TKA_B7/assets/150106905/854c6f34-dfd2-4a02-9ee4-fd86f5dbd11b)

## Configuration 2
| No | Name             | Specifications          | Function       | Price/Month |
|----|------------------|-------------------------|----------------|-------------|
| 1  | VM3-LoadBalancer | Regular 1vCPU 2GB Memory| Load Balancer  | $12         |
| 2  | VM2-Worker       | Regular 1vCPU 2GB Memory| App Worker 1   | $12         |
| 3  | VM2-Worker1      | Regular 1vCPU 2GB Memory| App Worker 2   | $12         |
| 4  | VM3-Database     | Regular 1vCPU 2GB Memory| Database Server| $12         |
|    | **TOTAL**        |                         |                | **$48**     |

**Masalah yang Mungkin:**

1. **Redundansi App Worker:**
   - Dalam konfigurasi ini, hanya ada dua App Worker. Jika salah satu dari mereka gagal, beban akan menjadi sangat berat untuk satu worker yang tersisa.

2. **Beban Server Database:**
   - Sama seperti pada Konfigurasi 1, Database Server hanya memiliki 1vCPU dan 2GB memori yang mungkin tidak cukup jika beban database meningkat.

3. **Ketersediaan:**
   - Tidak ada redundansi atau failover untuk Database Server. Jika server ini gagal, aplikasi tidak bisa mengakses database.

4. **Load Balancer:**
   - Load Balancer hanya memiliki 1vCPU dan 2GB memori, yang mungkin tidak cukup jika beban lalu lintas sangat tinggi.

