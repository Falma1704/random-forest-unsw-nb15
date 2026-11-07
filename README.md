Deteksi Anomali dan Klasifikasi Serangan Jaringan Menggunakan Random Forest pada Dataset UNSW-NB15
Proyek ini menggunakan algoritma Random Forest untuk mengklasifikasikan trafik jaringan pada dataset UNSW-NB15. Notebook berisi tahap persiapan data, pelatihan model, evaluasi, analisis fitur penting, serta penyimpanan model dan file pendukung.
Tujuan
Membangun model machine learning untuk membantu mengenali trafik jaringan normal dan trafik serangan melalui dua pendekatan:
1. Klasifikasi biner — membedakan trafik normal (0) dan serangan (1) menggunakan kolom label.
2. Klasifikasi multikelas — memprediksi kategori trafik/serangan berdasarkan kolom attack_cat yang terdiri dari 10 kelas (Normal dan 9 kategori serangan).
Dataset
Dataset yang digunakan adalah UNSW-NB15, dengan file:
- UNSW_NB15_training-set.csv
- UNSW_NB15_testing-set.csv
Notebook akan mencari file CSV di folder data/ terlebih dahulu. Jika file belum tersedia, notebook mencoba mengunduh dataset melalui kagglehub menggunakan dataset mrwellsdavid/unsw-nb15. Jika proses tersebut gagal, kedua file CSV dapat diunggah secara manual melalui Google Colab.
Metode dan proses
- Memuat serta mengeksplorasi data training dan testing.
- Menghapus kolom id karena hanya berfungsi sebagai nomor urut.
- Melakukan One-Hot Encoding pada fitur kategori proto, service, dan state.
- Menggunakan fitur numerik tanpa scaling.
- Menggabungkan preprocessing dan Random Forest dalam Pipeline.
- Melatih model klasifikasi biner dan multikelas.
- Mengevaluasi model dan menampilkan feature importance.
- Menyimpan model serta metadata untuk penggunaan pada tahap aplikasi berikutnya.
Perubahan dari template
Terdapat lima perubahan parameter dibandingkan template awal:
No.	Parameter	Nilai awal	Nilai perubahan
1	RANDOM_STATE	42	21
2	n_estimators	100	150
3	max_depth	20	18
4	min_samples_leaf	2	3
5	Ambang prediksi biner	0.50	0.45


Perubahan tersebut digunakan sebagai eksperimen. Dampaknya terhadap hasil klasifikasi perlu dilihat setelah notebook dijalankan dan metrik evaluasi dibandingkan dengan hasil template.
Metrik evaluasi
Klasifikasi biner:
- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC
- Confusion matrix dan ROC curve
Klasifikasi multikelas:
- Accuracy
- Macro F1-score
- Weighted F1-score
- Classification report dan confusion matrix
Notebook juga menampilkan 15 fitur dengan nilai feature importance tertinggi pada model biner.
Cara menjalankan
Notebook disiapkan untuk dijalankan menggunakan Google Colab.
1. Buka Google Colab.
2. Pilih File → Upload notebook, lalu unggah random-forest-unsw-nb15.ipynb.
3. Jalankan seluruh sel melalui Runtime → Run all.
4. Pastikan file dataset tersedia atau ikuti proses unduh/unggah dataset yang ditampilkan notebook.
5. Tunggu proses preprocessing, training, dan evaluasi sampai selesai.
6. Pada bagian akhir, notebook membuat artifacts.zip yang dapat diunduh.
Hasil keluaran
Setelah notebook dijalankan, folder artifacts/ dibuat dengan struktur berikut:
artifacts/
├── model/
│   ├── rf_binary.joblib
│   ├── rf_multiclass.joblib
│   └── metadata.json
├── data/
│   └── sample_flows.csv
└── requirements.txt
File artifacts.zip berisi keluaran tersebut. Model biner digunakan untuk prediksi normal atau serangan, sedangkan model multikelas digunakan untuk memprediksi kategori pada attack_cat.
Struktur repository
.
├── README.md
└── random-forest-unsw-nb15.ipynb
Folder artifacts/ dan file artifacts.zip dihasilkan setelah notebook dijalankan, sehingga belum tersedia pada repository awal sebelum proses training.
Catatan
Notebook ini merupakan tahap pemodelan (modeling). Angka hasil evaluasi tidak dicantumkan di README karena nilainya dihasilkan saat notebook dijalankan pada lingkungan dan versi library yang digunakan. Hasil aktual dapat dilihat pada output sel evaluasi di notebook
