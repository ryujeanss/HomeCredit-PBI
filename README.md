# 📊 Home Credit Default Risk Prediction

**End-to-End Data Analysis & Machine Learning | Credit Scoring**

## 📌 Project Overview

Proyek ini bertujuan untuk menganalisis karakteristik nasabah dan membangun model *Machine Learning* untuk memprediksi risiko gagal bayar (*default risk*) pada pengajuan pinjaman menggunakan dataset **Home Credit Default Risk**.

Home Credit merupakan perusahaan pembiayaan yang berfokus pada perluasan akses kredit bagi masyarakat yang memiliki riwayat kredit terbatas atau belum memiliki riwayat kredit formal.

Proyek ini mencakup proses analisis data secara end-to-end, mulai dari *Exploratory Data Analysis* (EDA), *data preprocessing*, *feature engineering*, pemodelan Machine Learning, hingga evaluasi model dan penyusunan rekomendasi bisnis.

Fokus utama proyek ini adalah memahami faktor-faktor yang berkaitan dengan risiko gagal bayar serta mengevaluasi potensi penggunaan model prediktif untuk mendukung pengambilan keputusan dalam manajemen risiko kredit.

## 🎯 Project Objectives

- Mengidentifikasi pola dan karakteristik nasabah yang berkaitan dengan risiko gagal bayar.
- Membersihkan data dan menangani anomali serta *missing values*.
- Membuat fitur baru untuk merepresentasikan kemampuan finansial nasabah.
- Mengembangkan dan mengevaluasi model klasifikasi untuk prediksi risiko kredit.
- Mengidentifikasi fitur yang paling berpengaruh terhadap prediksi model.
- Menerjemahkan hasil analisis menjadi rekomendasi yang relevan bagi bisnis.

## 📂 Project Workflow

### 1. Exploratory Data Analysis (EDA)

Tahap ini dilakukan untuk memahami distribusi data, karakteristik nasabah, dan pola yang berkaitan dengan status pembayaran pinjaman.

Analisis yang dilakukan meliputi:

- **Target Distribution:** Mengidentifikasi ketidakseimbangan kelas (*class imbalance*) antara nasabah yang melakukan pembayaran tepat waktu dan nasabah yang mengalami gagal bayar.
- **Demographic Analysis:** Mengeksplorasi hubungan antara usia, gender, tingkat pendidikan, dan karakteristik demografi lainnya dengan tingkat gagal bayar.
- **External Credit Scores:** Menganalisis variabel `EXT_SOURCE_1`, `EXT_SOURCE_2`, dan `EXT_SOURCE_3` untuk memahami hubungannya dengan risiko kredit.
- **Financial Characteristics:** Mengeksplorasi karakteristik pinjaman, pendapatan, cicilan, dan riwayat pekerjaan nasabah.

**Key Findings:**

- Distribusi target menunjukkan bahwa sekitar 91,9% nasabah termasuk kelas pembayaran lancar dan 8,1% termasuk kelas gagal bayar.
- Terdapat perbedaan tingkat gagal bayar pada beberapa kelompok demografi.
- Variabel `EXT_SOURCE_1`, `EXT_SOURCE_2`, dan `EXT_SOURCE_3` menunjukkan hubungan yang kuat dengan target prediksi.

Temuan tersebut digunakan sebagai dasar untuk menentukan strategi preprocessing, pemilihan fitur, dan evaluasi model.

### 2. Data Preprocessing

Tahap preprocessing dilakukan untuk meningkatkan kualitas data sebelum digunakan dalam pemodelan.

Langkah-langkah yang dilakukan:

- **Anomaly Detection:** Mengidentifikasi nilai tidak wajar pada variabel `DAYS_EMPLOYED`, termasuk nilai `365243` yang tidak merepresentasikan durasi bekerja secara normal.
- **Missing Value Handling:** Memeriksa distribusi nilai kosong dan menentukan strategi penanganan yang sesuai.
- **Categorical Encoding:** Mengubah variabel kategorikal menjadi format yang dapat digunakan oleh algoritma Machine Learning.
- **Data Quality Checks:** Memeriksa tipe data, distribusi variabel, dan konsistensi data.

Anomali pada `DAYS_EMPLOYED` ditangani dengan mengganti nilai khusus tersebut menjadi `NaN` dan membuat fitur indikator `DAYS_EMPLOYED_ANOM` untuk mempertahankan informasi mengenai keberadaan anomali.

### 3. Feature Engineering

*Feature engineering* dilakukan untuk menghasilkan variabel baru yang dapat membantu model mengenali pola risiko kredit.

Fitur yang dikembangkan meliputi:

| Feature | Description |
|---|---|
| `Credit_to_Income_Ratio` | Rasio jumlah pinjaman terhadap pendapatan nasabah. |
| `Annuity_to_Income_Ratio` | Rasio cicilan tahunan terhadap pendapatan nasabah. |
| `Credit_Term` | Estimasi durasi pinjaman berdasarkan jumlah kredit dan cicilan. |
| `Days_Employed_Percent` | Perbandingan durasi bekerja dengan usia nasabah. |

Fitur-fitur tersebut dirancang untuk merepresentasikan beban pinjaman, kemampuan finansial, dan stabilitas pekerjaan nasabah.

### 4. Machine Learning Modeling

Tahap pemodelan bertujuan untuk mengembangkan model klasifikasi yang mampu membedakan nasabah berdasarkan tingkat risiko gagal bayar.

Pendekatan yang digunakan meliputi:

- **Baseline Model:** Menggunakan Logistic Regression sebagai model pembanding awal.
- **Tree-Based Models:** Mengevaluasi pendekatan berbasis pohon keputusan untuk menangkap hubungan nonlinier dalam data.
- **LightGBM:** Menggunakan algoritma *gradient boosting* sebagai model utama karena efisiensi komputasi dan kemampuannya menangani data tabular.
- **Cross-Validation:** Menggunakan Stratified K-Fold Cross-Validation sebanyak lima fold untuk mengevaluasi konsistensi performa model dengan mempertahankan proporsi kelas pada setiap fold.

Penanganan *class imbalance* juga menjadi pertimbangan penting karena jumlah nasabah yang mengalami gagal bayar jauh lebih sedikit dibandingkan nasabah yang tidak mengalami gagal bayar.

### 5. Model Evaluation

Evaluasi model menggunakan metrik yang relevan untuk permasalahan klasifikasi dengan distribusi kelas yang tidak seimbang.

**Primary Metric: ROC-AUC**

Model LightGBM memperoleh skor ROC-AUC sebesar **0.78** berdasarkan hasil evaluasi yang didokumentasikan dalam notebook.

ROC-AUC digunakan untuk mengukur kemampuan model dalam membedakan nasabah yang mengalami gagal bayar dan nasabah yang tidak mengalami gagal bayar pada berbagai nilai *classification threshold*.

Metrik evaluasi lainnya yang relevan meliputi:

- **Precision:** Mengukur proporsi prediksi gagal bayar yang benar.
- **Recall:** Mengukur proporsi kasus gagal bayar aktual yang berhasil diidentifikasi.
- **F1-Score:** Menggabungkan precision dan recall ke dalam satu metrik.
- **Confusion Matrix:** Menunjukkan jumlah prediksi benar dan salah pada setiap kelas.

Pemilihan metrik dan *classification threshold* perlu mempertimbangkan konsekuensi bisnis dari kesalahan prediksi, baik berupa risiko menyetujui pinjaman berisiko tinggi maupun risiko menolak nasabah yang sebenarnya layak memperoleh kredit.

### 6. Feature Importance & Interpretation

Analisis *feature importance* dilakukan untuk memahami variabel yang paling banyak berkontribusi terhadap prediksi model.

Berdasarkan hasil analisis yang didokumentasikan, variabel `EXT_SOURCE_1`, `EXT_SOURCE_2`, dan `EXT_SOURCE_3` termasuk fitur penting dalam model, diikuti oleh sejumlah fitur hasil rekayasa seperti `Credit_Term` dan `Days_Employed_Percent`.

Hasil ini menunjukkan bahwa informasi dari sumber penilaian kredit eksternal memiliki keterkaitan yang kuat dengan prediksi risiko gagal bayar dalam dataset.

Namun, *feature importance* tidak secara langsung membuktikan hubungan sebab-akibat. Interpretasi hasil juga perlu mempertimbangkan kualitas data, potensi bias, dan karakteristik populasi yang digunakan.

## 💡 Business Insights & Recommendations

Berdasarkan hasil analisis, berikut beberapa rekomendasi yang dapat dipertimbangkan oleh tim manajemen risiko kredit.

### 1. Evaluasi Kualitas Data Kredit Eksternal

Variabel `EXT_SOURCE` memiliki peran penting dalam prediksi model. Oleh karena itu, perusahaan dapat mengevaluasi kelengkapan, kualitas, dan konsistensi data dari sumber eksternal untuk mendukung penilaian risiko kredit yang lebih baik.

### 2. Risk-Based Credit Assessment

Probabilitas gagal bayar dapat dipertimbangkan sebagai salah satu masukan dalam proses penilaian risiko kredit. Hasil prediksi dapat digunakan bersama informasi finansial lainnya untuk membantu menentukan tingkat risiko, kebutuhan pemeriksaan tambahan, atau persyaratan kredit yang sesuai.

### 3. Evaluasi Risiko pada Segmen Nasabah

Perbedaan tingkat gagal bayar antarsegmen demografi dapat menjadi dasar untuk melakukan analisis lanjutan. Namun, kebijakan kredit tidak sebaiknya ditentukan hanya berdasarkan usia, gender, atau karakteristik demografi lainnya.

Penilaian harus mempertimbangkan kemampuan membayar, relevansi variabel terhadap risiko, keadilan keputusan, serta ketentuan regulasi yang berlaku.

### 4. Optimasi Classification Threshold

Tim manajemen risiko dapat mengevaluasi berbagai nilai *classification threshold* untuk memahami trade-off antara precision dan recall.

Pemilihan threshold sebaiknya mempertimbangkan biaya kesalahan prediksi, potensi kerugian kredit, dan tujuan bisnis, bukan hanya mengejar satu metrik evaluasi.

## 🛠️ Tools & Technologies

- **Python** — Data analysis and model development
- **Pandas & NumPy** — Data manipulation and numerical computation
- **Matplotlib & Seaborn** — Exploratory data analysis and visualization
- **Scikit-learn** — Preprocessing, baseline modeling, and model evaluation
- **LightGBM** — Gradient boosting classification
- **Jupyter Notebook** — Interactive analysis and documentation

## 📁 Repository Structure

```text
home-credit-default-risk/
│
├── notebooks/
│   └── home_credit_analysis.ipynb
│
├── README.md
│
└── requirements.txt
```

*Catatan: Struktur direktori di atas merupakan contoh. Sesuaikan nama folder dan file dengan struktur repositori yang sebenarnya.*

## 📊 Key Results

| Evaluation Component | Result |
|---|---|
| Problem Type | Binary Classification |
| Primary Metric | ROC-AUC |
| Main Model | LightGBM |
| ROC-AUC Score | 0.78 |
| Cross-Validation | Stratified 5-Fold |

*Hasil evaluasi di atas mengikuti skor yang dicatat dalam notebook. Pastikan nilai tersebut berasal dari evaluasi pada data validasi atau pengujian yang sesuai sebelum dipublikasikan.*

## ⚠️ Limitations

- Hasil model bergantung pada kualitas, kelengkapan, dan representativitas dataset.
- Ketidakseimbangan kelas dapat memengaruhi evaluasi dan keputusan klasifikasi.
- Feature importance tidak menunjukkan hubungan sebab-akibat.
- Hasil eksperimen pada dataset historis belum menjamin performa yang sama pada data nasabah baru.
- Model ini merupakan proyek analisis dan eksperimen, bukan sistem penilaian kredit siap produksi.

