# analisis-sentimen_gojek

# 🎵 Analisis Sentimen Ulasan Aplikasi Gojek (NLP)

A _Natural Language Processing_ (NLP) project developed for a Dicoding course submission to classify text into **positive**, **neutral**, and **negative** sentiment categories.

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-1.x-yellow)

---

## 📊 Dataset & Sumber Data

- **Sumber Data:** Scraping otomatis dari Google Play Store menggunakan library `google_play_scraper`.
- **Jumlah Data Mentah:** 35.000 ulasan (7.000 ulasan untuk masing-masing rating 1 hingga 5).
- **Pelabelan Sentimen (Berdasarkan Rating):**
  - Rating 1 & 2 = **Negatif** (14.000 data)
  - Rating 3 = **Netral** (7.000 data)
  - Rating 4 & 5 = **Positif** (14.000 data)
- **Data Bersih:** 16.679 data (setelah melalui tahap _preprocessing_ ketat dan _noise filtering_).

---

## ⚙️ Tahapan Preprocessing

Untuk menghasilkan model yang akurat, data mentah melalui beberapa tahapan pembersihan:

1. **Case Folding & Cleaning:** Mengubah teks ke huruf kecil, menghapus URL, simbol, karakter khusus, dan menormalkan huruf yang berulang (typo).
2. **Slang Normalization:** Mengubah kata gaul/singkatan menjadi kata baku menggunakan kamus _custom_ (contoh: `ga`/`gak` -> `tidak`, `bgt` -> `banget`, `apk` -> `aplikasi`).
3. **Custom Stopwords:** Menghapus _stopwords_ bahasa Indonesia bawaan NLTK, **tetapi mempertahankan kata negasi dan konteks** (seperti _tidak, bukan, kurang, jangan, belum, tapi, namun, cukup, lumayan, biasa_) agar makna sentimen tidak berubah.
4. **Data Cleansing:** Menghapus data kosong, duplikat, dan data kontradiktif (teks yang sama namun memiliki label berbeda).
5. **Noise Filtering (Confidence Score):** Menyaring label yang berpotensi _noise_ menggunakan _Stratified K-Fold_ dan _Logistic Regression_. Hanya 60% data terbaik (berdasarkan skor probabilitas keyakinan) dari masing-masing kelas yang dipertahankan.

---

## 🧠 Skema Percobaan & Hasil Evaluasi

Proyek ini membandingkan 3 skema arsitektur model yang berbeda untuk menemukan pendekatan terbaik:

| Skema       | Algoritma   | Ekstraksi Fitur | Pembagian Data | Akurasi Training | Akurasi Testing |
| :---------- | :---------- | :-------------- | :------------- | :--------------- | :-------------- |
| **Skema 1** | Dense (MLP) | TF-IDF          | 80/20          | 94.15%           | **87.44%**      |
| **Skema 2** | BiLSTM      | Embedding       | 80/20          | 93.70%           | 86.30%          |
| **Skema 3** | Conv1D      | Embedding       | 70/30          | 91.40%           | 81.29%          |

_Catatan: Berdasarkan hasil evaluasi, **Skema 1 (Dense MLP + TF-IDF)** memiliki akurasi testing tertinggi. Namun, untuk keperluan deployment dan pengujian teks baru, arsitektur **BiLSTM (Skema 2)** disimpan sebagai artefak model karena kemampuannya yang sangat baik dalam memahami konteks sekuensial pada teks._

---

## 📁 Struktur File Project

- `googlePlay_Scraping.ipynb` : Notebook untuk proses pengambilan data (scraping) dari Play Store.
- `notebook_Training.ipynb` : Notebook utama untuk _preprocessing_, pelatihan model, dan evaluasi.
- `dataset_gojek_raw.csv` : Dataset mentah hasil _scraping_ (35.000 baris).
- `dataset_gojek_clean.csv` : Dataset yang sudah dibersihkan dan siap dilatih.
- `model_bilstm.keras` : File model Deep Learning BiLSTM yang sudah dilatih.
- `tokenizer.pkl` : File Tokenizer yang disimpan untuk keperluan prediksi teks baru.
- `requirements.txt` : Daftar _library_ Python yang dibutuhkan.

---

## 🛠️ Cara Menjalankan Project

Kamu tinggal menjalankan googlePlay_Scraping.ipnyb jika ingin menggunakan data baru dan notebook_Traingin.ipnyb jika ingin menjalankan keseluruhan proses analisis sentimen.
Kamu bisa menggunakan Jupyter Notebook atau Google Colab untuk menggunakan project ini.

### 1. Prasyarat

Pastikan kamu sudah menginstal Python (versi 3.8 ke atas). Instal semua _library_ yang dibutuhkan dengan perintah:

```bash
pip install -r requirements.txt
```
