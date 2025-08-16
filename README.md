Tentu, saya sudah memperbarui file `README.md` dengan memasukkan tautan dataset yang Anda berikan pada bagian "Cara Menjalankan".

Berikut adalah konten `README.md` yang sudah diperbarui.

-----

# PROYEK TUGAS AKHIR DEEP LEARNING: SPEECH RECOGNITION

**Mata Kuliah:** Deep Learning  
**Judul:** Speech Recognition (Suara ke Teks) menggunakan Mozilla Common Voice  
**Model:** CNN-RNN dengan CTC Loss  
**Kelompok:** 7 - Studi Kasus 2

| Nama                            | NIM          |
| ------------------------------- | ------------ |
| Muhammad Jovi Syawal Difa       | 227006516003 |
| Joevan Pramana Achmad           | 227006516015 |
| Rifki Eko Pratomo               | 227006516024 |
| Tongam Deni Gamaliel Situmoran  | 227006516061 |

-----

## 📜 Abstrak

Proyek ini bertujuan untuk membangun dan melatih model *speech-to-text* menggunakan dataset **Mozilla Common Voice** berbahasa Inggris. Pendekatan yang digunakan adalah membangun arsitektur model *Deep Learning* dari awal yang terdiri dari **Convolutional Neural Network (CNN)** untuk ekstraksi fitur dari spektrogram audio dan **Recurrent Neural Network (RNN)** dengan arsitektur GRU untuk memproses sekuens temporal. Model ini dioptimalkan menggunakan *Connectionist Temporal Classification (CTC) Loss*, yang cocok untuk tugas sekuens-ke-sekuens di mana penyelarasan antara input dan output tidak eksplisit.

Eksperimen dilakukan dalam dua tahap yang didokumentasikan dalam dua notebook terpisah:

1.  **Implementasi Awal:** Sebuah model dasar untuk menguji alur kerja.
2.  **Fine-Tuning & Perbaikan:** Peningkatan signifikan pada arsitektur model dan parameter pelatihan untuk mencoba meningkatkan performa.

## 📁 Struktur Proyek

```
.
├── cv-corpus-19.0-delta-2024-09-13/  # Direktori dataset (di Google Drive)
│   └── en/
│       ├── clips/
│       │   └── *.mp3
│       └── validated.tsv
├── Deep_Learning_UAS_Kelompok_7_Studi_Kasus_2.ipynb             # Notebook Eksperimen Awal
└── Deep_Learning_UAS_Kelompok_7_Studi_Kasus_2_Finetuned.ipynb    # Notebook Eksperimen Perbaikan
```

## 🧠 Pendekatan dan Arsitektur

### 1\. Preprocessing Data

  - **Input Audio**: Audio dari dataset di-resample ke **16,000 Hz** dan diubah menjadi format mono.
  - **Ekstraksi Fitur**: Setiap file audio diubah menjadi **Mel Spectrogram** dengan 128 *mel-bands*. Spektrogram ini berfungsi sebagai representasi visual dari audio yang dapat diproses oleh lapisan CNN.
  - **Teks Target**: Kalimat transkripsi diubah menjadi sekuens integer berdasarkan vocabulary karakter yang dibuat dari dataset.

### 2\. Arsitektur Model

Model ini merupakan gabungan dari CNN dan RNN:

#### a. Model Awal (`...Studi_Kasus_2.ipynb`)

  - **CNN**: Satu lapisan `Conv2d` sederhana untuk mengekstraksi fitur dasar dari spektrogram.
  - **RNN**: Satu lapisan `GRU` *bidirectional* dengan 256 unit *hidden*.
  - **Tujuan**: Membangun *baseline* dan memastikan seluruh alur data, dari preprocessing hingga training, berjalan dengan benar.

#### b. Model Perbaikan (`...Finetuned.ipynb`)

  - **CNN**: Jaringan yang lebih dalam dengan **dua lapisan `Conv2d`**. Setiap lapisan diikuti oleh `BatchNorm2d` (untuk stabilisasi), `ReLU` (untuk non-linearitas), dan `Dropout` (untuk mencegah overfitting).
  - **RNN**: `GRU` *bidirectional* yang lebih dalam (**3 lapisan**) dan lebih lebar (**512 unit *hidden***), juga dilengkapi dengan `Dropout`.
  - **Tujuan**: Meningkatkan kapasitas model untuk mempelajari fitur yang lebih kompleks dan pola temporal yang lebih panjang.

## 📊 Eksperimen dan Hasil

### Eksperimen 1: Implementasi Awal

  - **Dataset**: \~109 sampel training, \~28 sampel testing.
  - **Pelatihan**: Dilatih selama 20 *epoch*.
  - **Hasil**: Model **gagal total** untuk belajar. *Training loss* tidak menunjukkan penurunan yang signifikan.
      - **Word Error Rate (WER) Final**: **100%**.
      - **Prediksi**: Model menghasilkan teks kosong, menandakan bahwa ia tidak mampu mempelajari pola apa pun dari data.
  - **Analisis**: Kegagalan ini kemungkinan besar disebabkan oleh **dataset yang sangat kecil** dan **arsitektur model yang terlalu sederhana** untuk tugas yang kompleks ini.

### Eksperimen 2: Fine-Tuning dan Perbaikan

  - **Dataset**: Upaya dilakukan untuk menggunakan sampel data yang lebih besar (target 7500), namun karena keterbatasan file yang valid di dataset yang tersedia, jumlah sampel efektif tetap sama (\~123 training, \~14 testing).
  - **Pelatihan**:
      - Arsitektur model yang jauh lebih solid dan dalam digunakan.
      - Parameter pelatihan dioptimalkan: *epoch* ditingkatkan menjadi **50**, *gradient accumulation* digunakan untuk mensimulasikan *batch size* yang lebih besar, dan *gradient clipping* diterapkan untuk menstabilkan pelatihan.
  - **Hasil**:
      - Proses pelatihan menunjukkan tanda-tanda pembelajaran; ***training dan validation loss* menurun secara konsisten** dari epoch ke epoch.
      - Namun, metrik evaluasi akhir **WER tetap 100%**.
      - **Prediksi**: Model menghasilkan teks acak (*gibberish*), bukan teks kosong. Ini menunjukkan model *mencoba* memprediksi tetapi belum dapat membentuk kata atau kalimat yang benar.
  - **Analisis Akhir**: Meskipun arsitektur dan proses pelatihan telah ditingkatkan secara signifikan, performa model masih sangat buruk. **Penyebab utamanya hampir pasti adalah ukuran dataset yang sangat tidak memadai**. Model *Deep Learning* untuk *speech recognition* membutuhkan ribuan hingga puluhan ribu sampel audio untuk dapat belajar secara efektif. Dengan hanya \~123 sampel, model tidak memiliki cukup variasi untuk generalisasi.

## 🚀 Cara Menjalankan

1.  **Prasyarat**:

      - Akun Google dengan Google Drive.
      - Dataset Mozilla Common Voice (versi `cv-corpus-19.0-delta-2024-09-13` atau yang kompatibel).

2.  **Setup**:

      - Unduh dataset Mozilla Common Voice dari [**Tautan Dataset Ini**](https://drive.google.com/drive/folders/1YXHwGj04-BuaxOeNwxLBeHq6fpT73TIQ?usp=drive_link) dan letakkan di Google Drive Anda pada path berikut: `MyDrive/cv-corpus-19.0-delta-2024-09-13/en/`. Pastikan folder `clips` dan file `validated.tsv` ada di dalamnya.
      - Buka salah satu file `.ipynb` menggunakan Google Colaboratory.

3.  **Eksekusi**:

      - Jalankan sel pertama untuk menghubungkan Google Drive dan menginstal *library* yang diperlukan.
      - Jalankan semua sel secara berurutan. Proses pelatihan (terutama pada notebook *fine-tuned*) akan memakan waktu cukup lama.

4.  **Output**:

      - Model yang telah dilatih dan *checkpoint*-nya akan disimpan di Google Drive pada direktori yang ditentukan di dalam kode (misalnya, `MyDrive/cnn-rnn-common-voice-en-finetuned`).

## 💡 Kesimpulan dan Saran Pengembangan

**Kesimpulan:**
Proyek ini berhasil mengimplementasikan arsitektur CNN-RNN untuk *speech recognition* dan mendemonstrasikan proses iteratif dalam perbaikan model. Meskipun hasil akhir belum memuaskan (WER 100%), eksperimen ini memberikan wawasan berharga tentang tantangan utama dalam melatih model dari awal, terutama pentingnya **jumlah data**.

**Saran Pengembangan Lanjutan:**

1.  **Gunakan Dataset yang Jauh Lebih Besar**: Ini adalah langkah paling krusial. Gunakan seluruh dataset Common Voice atau dataset publik lainnya seperti LibriSpeech.
2.  **Data Augmentation**: Terapkan teknik augmentasi pada data audio (misalnya, menambahkan noise, mengubah kecepatan, *pitch shifting*) untuk meningkatkan variasi data secara artifisial.
3.  **Hyperparameter Tuning**: Lakukan pencarian *hyperparameter* yang lebih sistematis (misalnya, *learning rate*, *dropout rate*, jumlah lapisan RNN) menggunakan *tools* seperti Optuna atau Weights & Biases.
4.  **Transfer Learning**: Daripada melatih dari awal, gunakan **model *pre-trained*** seperti **Wav2Vec2** atau **Whisper**. *Fine-tuning* model-model ini pada dataset yang lebih kecil sekalipun kemungkinan besar akan memberikan hasil yang jauh lebih baik dan lebih cepat.
