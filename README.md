📊 Analisis Data & Credit Scoring: Prediksi Risiko Gagal Bayar (Home Credit)

Repositori ini berisi Jupyter Notebook (.ipynb) yang mendokumentasikan proses analisis end-to-end dalam membangun model Machine Learning untuk memprediksi risiko gagal bayar (default risk). Studi kasus ini menggunakan dataset Home Credit, sebuah institusi finansial internasional yang berfokus pada penyediaan pinjaman bagi populasi unbanked (mereka yang memiliki sedikit atau tanpa riwayat kredit tradisional).

Notebook ini tidak hanya berfokus pada pencapaian metrik akurasi algoritma, tetapi juga pada eksplorasi data yang mendalam, penanganan anomali dunia nyata, dan ekstraksi insight bisnis yang dapat ditindaklanjuti oleh manajemen risiko.

📑 Alur Analisis di dalam Notebook

Analisis di dalam notebook ini disusun secara berurutan dan sistematis, mencakup tahapan-tahapan berikut:

1. Exploratory Data Analysis (EDA) & Business Insights

Fokus awal notebook adalah memahami karakteristik dari ratusan ribu nasabah dan mengidentifikasi pola tersembunyi yang berkorelasi dengan gagal bayar.

Analisis Imbalance Class Ekstrem: Memvisualisasikan distribusi target di mana kelas lancar (91.9%) sangat mendominasi dibandingkan kelas gagal bayar (8.1%). Hal ini menjadi landasan mengapa metrik akurasi tradisional tidak bisa digunakan.

Korelasi Demografi & Risiko:

Usia: Nasabah muda (di bawah 25 tahun) memiliki risiko default hampir 2.3x lipat lebih besar dibandingkan nasabah senior (di atas 50 tahun).

Gender & Status Sosial: Nasabah perempuan secara statistik menunjukkan tingkat kepatuhan bayar yang sedikit lebih tinggi. Tingkat pendidikan yang lebih tinggi (seperti Higher Education) juga berkorelasi kuat dengan rendahnya risiko gagal bayar.

Analisis Variabel Eksternal: Meneliti variabel EXT_SOURCE_1, EXT_SOURCE_2, dan EXT_SOURCE_3 (skor terenkripsi dari biro kredit eksternal) yang terbukti menjadi prediktor paling kuat secara global di seluruh dataset.

2. Data Preprocessing & Penanganan Anomali

Data riil selalu kotor. Tahap ini menceritakan bagaimana notebook membersihkan data sebelum dimasukkan ke model:

Penanganan Outlier & Anomali: Mengidentifikasi dan memperbaiki anomali ekstrem, seperti nilai DAYS_EMPLOYED (lama bekerja) yang tercatat sebesar 365243 hari (sekitar 1000 tahun). Anomali ini ditangani dengan menggantinya menjadi NaN (kosong) dan membuat kolom penanda boolean (DAYS_EMPLOYED_ANOM).

Imputasi Missing Values: Banyaknya data kosong pada variabel EXT_SOURCE ditangani secara hati-hati, karena kekosongan data itu sendiri bisa menjadi sinyal risiko (misal: nasabah tidak terdeteksi di biro kredit manapun).

Encoding Variabel Kategorikal: Menggunakan kombinasi One-Hot Encoding untuk variabel kategorikal dengan sedikit nilai unik, dan Label Encoding / Target Encoding untuk variabel dengan kardinalitas tinggi.

3. Feature Engineering (Rekayasa Fitur)

Alih-alih hanya mengandalkan data mentah, notebook ini menyintesis variabel finansial baru yang lebih merepresentasikan kapasitas bayar nasabah secara logis:

Credit_to_Income_Ratio: Mengukur rasio antara total pinjaman dengan pendapatan.

Annuity_to_Income_Ratio: Persentase cicilan bulanan yang harus dibayar dari gaji. Ini adalah indikator utama untuk mendeteksi over-indebtedness (kredit berlebih).

Credit_Term: Rasio antara total kredit dan cicilan (Annuity), yang secara efektif merepresentasikan estimasi tenor/lama waktu pinjaman (dalam bulan).

Days_Employed_Percent: Proporsi lama bekerja relatif terhadap usia nasabah untuk mengukur stabilitas karier dan kematangan finansial.

4. Strategi Pemodelan Machine Learning

Baseline vs. Advanced Model: Eksperimen dimulai dengan model sederhana (seperti Logistic Regression) sebagai baseline, lalu dilanjutkan dengan algoritma ansambel berbasis Tree.

LightGBM (Gradient Boosting): Terpilih sebagai arsitektur final karena efisiensinya dalam memproses jutaan titik data dan penanganan bawaannya terhadap nilai kosong (missing values) secara native.

Cross-Validation: Menggunakan Stratified K-Fold Cross Validation (5-fold) untuk memastikan model tidak overfitting dan dapat menggeneralisasi pola pada data yang belum pernah dilihat sebelumnya, sekaligus menjaga proporsi kelas gagal bayar (8.1%) di setiap lipatan data.

5. Evaluasi & Interpretasi Hasil

ROC-AUC (Receiver Operating Characteristic - Area Under Curve): Model LightGBM mencapai skor ROC-AUC sebesar 0.78. Skor ini sangat solid untuk standar industri pembiayaan konsumen (sebagai perbandingan, tebakan acak adalah 0.50).

Analisis Feature Importance: Notebook membuktikan secara visual bahwa ketiga fitur EXT_SOURCE memegang kendali dominan atas keputusan model, disusul oleh metrik rekayasa fitur seperti Credit_Term dan Days_Employed_Percent.

Trade-off Presisi & Recall: Penjelasan mengenai bagaimana manajemen dapat menggeser threshold model: menaikkan Recall jika sedang fokus menekan NPL (Non-Performing Loan), atau menaikkan Precision jika sedang mengejar pertumbuhan pangsa pasar.

💡 Kesimpulan & Tindak Lanjut Bisnis (Business Actionable)

Dari seluruh rangkaian analisis yang dijalankan, notebook ini menghasilkan tiga rekomendasi strategis utama untuk operasional bisnis:

Optimalisasi Pengadaan Data Eksternal: Karena performa model sangat bergantung pada EXT_SOURCE, perusahaan harus memprioritaskan anggaran IT untuk memastikan uptime API dan keandalan integrasi dengan biro data kredit nasional/pihak ketiga.

Penerapan Risk-Based Pricing: Output probabilitas dari model ini jangan hanya digunakan untuk keputusan "Tolak/Terima". Probabilitas ini bisa dipetakan untuk menentukan besaran suku bunga dinamis (nasabah berisiko menengah tetap diterima, namun dengan bunga/margin yang sedikit lebih tinggi untuk kompensasi risiko).

Kebijakan Khusus Segmen Usia Muda: Lonjakan risiko pada demografi di bawah 25 tahun mengindikasikan perlunya produk khusus, misalnya mewajibkan penjamin (co-signer / orang tua) atau membatasi plafon pinjaman maksimal (LTV - Loan to Value yang lebih rendah) untuk segmen usia ini.

Dokumentasi ini ditulis sebagai representasi alur pemikiran analitis di dalam notebook. Dibuat oleh [Nama Anda] | Portofolio Analisis Data
