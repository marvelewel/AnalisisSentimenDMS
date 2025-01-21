# 📊 Analisis Sentimen DMS

## 🚀 Deskripsi Proyek
Proyek ini bertujuan untuk **menganalisis sentimen** dalam komentar Youtube DMS menggunakan teknik **machine learning dan deep learning**. Model yang digunakan meliputi **SVM, Random Forest, LSTM, dan CNN**, dengan pendekatan **TF-IDF dan Word2Vec** untuk ekstraksi fitur dari teks.

## 📚 Dataset
Dataset yang digunakan terdiri dari ulasan atau komentar terkait DMS yang dikategorikan ke dalam **tiga kelas utama**:
- ✅ **Positif**
- ❌ **Negatif**
- ⚖️ **Netral**

## 🔍 Preprocessing Data
Agar model dapat memahami teks dengan lebih baik, dilakukan serangkaian preprocessing:
1. **Pembersihan teks** 🧹 (menghapus karakter khusus, angka, dan URL)
2. **Tokenisasi** ✂️ menggunakan `nltk.word_tokenize()`
3. **Stopword removal** 🚫 menggunakan daftar stopwords bahasa Indonesia
4. **Lemmatization/stemming** 🔄 untuk menyederhanakan kata

## 🔎 Data Scraping
**YouTube**: Menggunakan **Google API** untuk mengumpulkan komentar terkait DMS dari video YouTube.

### 📝 Implementasi Data Scraping
Saya menggunakan **Google API** untuk mengumpulkan komentar dari video YouTube terkait DMS. Berikut implementasi kode yang digunakan:

```python
from googleapiclient.discovery import build
import pandas as pd

# Mendefinisikan API key
youtube = build("youtube", "v3", developerKey="YOUR_API_KEY")

def get_video_comments(video_id):
    comments = []
    request = youtube.commentThreads().list(part="snippet", videoId=video_id, maxResults=100)
    while request:
        response = request.execute()
        for item in response["items"]:
            comment = item["snippet"]["topLevelComment"]["snippet"]
            comments.append({
                "author": comment["authorDisplayName"],
                "comment": comment["textDisplay"],
                "likes": comment["likeCount"]
            })
        request = youtube.commentThreads().list_next(request, response)
    return comments

def save_comments_to_csv(comments, filename):
    data = pd.DataFrame(comments)
    data.to_csv(filename, index=False)

video_ids = ["xOg20MgYZTY", "8Assb2RwAJA", "eFDgYSKk3KU"]
all_comments = []
for video_id in video_ids:
    all_comments.extend(get_video_comments(video_id))

save_comments_to_csv(all_comments, "komentar_youtube_dms.csv")
```

## 🏰️ Ekstraksi Fitur
Saya menggunakan dua metode utama untuk mengubah teks menjadi bentuk numerik:
- 📌 **TF-IDF (Term Frequency - Inverse Document Frequency)**
- 📌 **Word2Vec** untuk representasi kata berbasis vektor

## 🧠 Model yang Digunakan
Beberapa algoritma machine learning dan deep learning diterapkan untuk klasifikasi sentimen:
1. 🤖 **Support Vector Machine (SVM)**
2. 🌲 **Random Forest**
3. 🔁 **Long Short-Term Memory (LSTM)**
4. 🧠 **Convolutional Neural Network (CNN)**

### 📈 Evaluasi Model
Model dievaluasi berdasarkan:
- 🎯 **Akurasi**

## ⚙️ Cara Menjalankan Proyek
1️⃣ Clone repository ini:
   ```bash
   git clone https://github.com/marvelewel/analisis-sentimen-dms.git
   cd analisis-sentimen-dms
   ```
2️⃣ Install dependensi yang diperlukan:
   ```bash
   pip install -r requirements.txt
   ```
3️⃣ Jalankan preprocessing dan latih model:
   ```bash
   python train_model.py
   ```
4️⃣ Untuk melakukan prediksi sentimen pada teks baru:
   ```bash
   python predict.py --text "DMS ini sangat membantu pekerjaan saya!"
   ```

## 🏆 Hasil Analisis
- Karena keterbatasan spek laptop, saya menggunakan epoch = 1 untuk model deep learningnya, sehingga saya rasa kurang optimal.
- Model deep learning (LSTM dan CNN) menunjukkan akurasi yang lebih rendah dibandingkan model tradisional seperti **SVM**. Oleh karena itu, saya memilih **SVM** untuk prediksi sentimen karena hasilnya lebih akurat dalam kasus ini.

### 🔮 Contoh Prediksi Sentimen
- **Teks yang dimasukkan:** "Video penelusuran DMS kali ini sangat seru dan mengerikan"
- **Predicted Sentiment:** ⚖️ *Netral*
- **Penjelasan:** Model mengklasifikasikan teks ini sebagai *Netral* karena kata "seru" memiliki konotasi positif, sedangkan "mengerikan" memiliki konotasi negatif, sehingga secara keseluruhan sentimen menjadi seimbang.


## 📝 Lisensi
Proyek ini dilisensikan di bawah [MIT License](LICENSE).

💡 **Terima kasih!** 🔍🚀

