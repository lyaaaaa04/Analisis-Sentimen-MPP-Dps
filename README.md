# Analisis Sentimen Ulasan Google Maps Mal Pelayanan Publik Kota Denpasar Menggunakan SVM

## Deskripsi Proyek
Proyek ini merupakan salah satu bentuk implementasi machine learning pada kegiatan **Praktik Kerja Lapangan (PKL)** yang dikerjakan di **Dinas Komunikasi, Informatika, dan Statistik Kota Denpasar**. Proyek ini bertujuan untuk melakukan **analisis sentimen masyarakat terhadap Mal Pelayanan Publik (MPP) Kota Denpasar** berdasarkan **ulasan pengguna di Google Maps**.

Analisis sentimen dilakukan menggunakan pendekatan **machine learning**, dengan algoritma **Support Vector Machine (SVM)** untuk mengklasifikasikan ulasan ke dalam tiga kelas sentimen, yaitu **positif**, **netral**, dan **negatif**. Hasil analisis diharapkan dapat memberikan **insight terkait kualitas pelayanan publik** dari sudut pandang masyarakat.

---

## Dataset
- Sumber data: **Ulasan Google Maps Mal Pelayanan Publik Kota Denpasar**
- Teknik pengumpulan: **Crawling Google Maps**
- Jumlah data awal: **70 ulasan**
- Jumlah kelas sentimen: **3 kelas**
  - Positif
  - Netral
  - Negatif

### Distribusi Data Awal
- Positif: 40
- Negatif: 24
- Netral: 6

---

## Alur Implementasi
Tahapan utama dalam proyek ini meliputi:

1. Load dataset ulasan
2. Preprocessing data teks
3. Augmentasi data teks
4. Penyeimbangan data (resampling)
5. Ekstraksi fitur menggunakan TF-IDF
6. Encoding label sentimen
7. Training model SVM
8. Hyperparameter tuning dengan GridSearchCV
9. Evaluasi model
10. Visualisasi hasil analisis
11. Implementasi inferensi sentimen

---

## Pra-pemrosesan Teks
Tahapan pra-pemrosesan teks dilakukan untuk meningkatkan kualitas data dan meliputi:

- Pembersihan teks (penghapusan URL, mention, hashtag, simbol)
- Case folding (konversi ke huruf kecil)
- Normalisasi kata tidak baku (slang)
- Stopword removal 
- Tokenisasi
- Stemming

---

## Augmentasi Data Teks
Karena jumlah data relatif kecil, dilakukan **augmentasi teks** untuk meningkatkan variasi data menggunakan library `nlpaug`, dengan metode:
- Synonym replacement
- Random word swap
- Random word insertion

Setiap data di-augmentasi sebanyak **2 kali**, sehingga jumlah data meningkat dari **70 menjadi 116 data**.

---

## Penanganan Ketidakseimbangan Kelas
Setelah augmentasi, dataset masih memiliki distribusi kelas yang belum seimbang. Oleh karena itu dilakukan **oversampling** dengan metode resampling hingga setiap kelas memiliki jumlah data yang sama.

### Distribusi Data Setelah Penyeimbangan
- Positif: 64
- Netral: 64
- Negatif: 64

---

## Ekstraksi Fitur
Fitur teks diekstraksi menggunakan metode **TF-IDF (Term Frequency – Inverse Document Frequency)** untuk mengubah teks menjadi representasi numerik.

- Jumlah fitur TF-IDF: **579 fitur**
- N-gram: default (unigram)

---

## Model Machine Learning
Model yang digunakan adalah **Support Vector Machine (SVM)** dengan spesifikasi sebagai berikut:
- Tipe: **Linear SVM (LinearSVC)**
- Library: `scikit-learn`
- Strategi multiclass: One-vs-Rest (default)

---

## Hyperparameter Tuning
Dilakukan pencarian hyperparameter terbaik menggunakan **GridSearchCV** dengan parameter:

- **C**: `[0.01, 0.1, 1, 10, 100]`
- Cross-validation: **5-fold**

### Parameter Terbaik
- **C = 0.01**

---

## Evaluasi Model
Model terbaik dievaluasi menggunakan data uji (20% dari data) dengan hasil sebagai berikut:

- **Akurasi**: **97%**

### Classification Report
| Sentimen | Precision | Recall | F1-Score |
|--------|-----------|--------|----------|
| Negatif | 0.93 | 1.00 | 0.96 |
| Netral  | 1.00 | 1.00 | 1.00 |
| Positif | 1.00 | 0.92 | 0.96 |
| **Akurasi** |  |  | **0.97** |

Model menunjukkan performa yang sangat baik meskipun jumlah data relatif kecil.

### Confusion Matrix
<img width="530" height="455" alt="image" src="https://github.com/user-attachments/assets/737034df-469f-40e6-8b96-b8be29427920" />

---

## Analisis Kata Dominan (TF-IDF)
Beberapa kata dengan skor TF-IDF tertinggi antara lain:
- pelayanan
- cepat
- ramah
- paspor
- imigrasi
- antre
- petugas

Kata-kata tersebut merepresentasikan topik utama yang sering dibahas oleh pengguna dalam ulasan MPP.

---

## Visualisasi Wordcloud & Insight Sentimen

### Sentimen Positif
**Kata dominan:**
<img width="790" height="427" alt="image" src="https://github.com/user-attachments/assets/46fe9c50-911d-41c3-9e28-0e41c1dad4a8" />

**Insight:**
Sentimen positif didominasi oleh kepuasan terhadap **kualitas pelayanan**, khususnya **kecepatan proses**, **keramahan petugas**, dan **efisiensi pengurusan administrasi** seperti paspor dan layanan imigrasi. Hal tersebut memberi makna bahwa pengguna merasa **puas, terbantu, dan menghargai profesionalisme pelayanan publik** di MPP Kota Denpasar.

---

### Sentimen Netral
**Kata dominan:**
<img width="790" height="427" alt="image" src="https://github.com/user-attachments/assets/22f7a92e-4715-44d8-8fa1-72414809c3a7" />

**Insight:**
Komentar bersifat **informatif dan deskriptif**, banyak membahas penggunaan **layanan digital (PUSPITA)** serta pencarian informasi terkait prosedur layanan.  Hal tersebut memberi makna bahwa pengguna sedang **mencari atau membagikan informasi**, tanpa ekspresi emosi yang kuat.

---

### Sentimen Negatif
**Kata dominan:**
<img width="790" height="427" alt="image" src="https://github.com/user-attachments/assets/3263b67f-bd74-4dac-9114-f174fee64194" />

**Insight:**
Sentimen negatif didominasi oleh **keluhan terkait waktu tunggu**, antrean, dan pengalaman pelayanan langsung di lokasi. Penyebutan petugas dalam konteks negatif mengindikasikan **ketidakpuasan terhadap pelayanan di waktu tertentu**. Hal tersebut memberi makna bahwa masalah utama terletak pada **efisiensi layanan dan manajemen antrean**, bukan pada sistem secara keseluruhan.

---

## Kesimpulan
Berdasarkan hasil analisis, algoritma **Support Vector Machine (SVM)** mampu mengklasifikasikan sentimen ulasan MPP Kota Denpasar dengan sangat baik, mencapai **akurasi 97%**. 

Hasil analisis sentimen dan wordcloud menunjukkan bahwa secara umum **persepsi masyarakat terhadap MPP Kota Denpasar bersifat positif**, terutama terkait kecepatan layanan dan keramahan petugas. Namun demikian, masih terdapat **keluhan terkait waktu tunggu dan antrean**, yang menjadi catatan penting untuk peningkatan kualitas layanan.

Proyek ini membuktikan bahwa **analisis sentimen berbasis ulasan publik** dapat menjadi alat bantu evaluasi pelayanan publik yang efektif dan berbasis data.

---

## Inferensi Analisis Sentimen menggunakan Fungsi Predict
<img width="582" height="182" alt="image" src="https://github.com/user-attachments/assets/db927ff1-1640-495f-bb2e-5c7c750906ae" />

