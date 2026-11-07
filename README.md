Network Intrusion Detection and Attack Classification with Random Forest
Implementasi Random Forest untuk mengklasifikasikan trafik jaringan normal dan trafik serangan menggunakan dataset UNSW-NB15. Eksperimen dilakukan melalui dua skenario, yaitu klasifikasi biner dan klasifikasi multikelas.
Ruang lingkup: Repository ini berisi notebook untuk persiapan data, pelatihan, evaluasi, analisis fitur, dan penyimpanan model. Notebook ini belum merupakan sistem pemantauan jaringan real-time dan tidak melakukan pemblokiran serangan secara otomatis.

Ringkasan Proyek
Komponen	Keterangan
Algoritma	Random Forest Classifier
Dataset	UNSW-NB15
Bahasa	Python
Framework utama	pandas, NumPy, scikit-learn
Lingkungan yang disarankan	Google Colab atau Jupyter Notebook
Skenario 1	Klasifikasi biner: normal atau serangan
Skenario 2	Klasifikasi multikelas berdasarkan kategori serangan


Tujuan
- Membangun model Random Forest untuk klasifikasi trafik jaringan.
- Membedakan trafik normal dan trafik serangan melalui klasifikasi biner.
- Mengelompokkan trafik berdasarkan kategori serangan melalui klasifikasi multikelas.
- Mengevaluasi performa model menggunakan metrik klasifikasi.
- Mengidentifikasi fitur yang berpengaruh terhadap hasil prediksi.
- Menyimpan model dan metadata untuk mendukung pengembangan aplikasi pada tahap berikutnya.
Dataset
Dataset yang digunakan adalah UNSW-NB15. Notebook memerlukan dua file berikut:
Nama file	Fungsi
UNSW_NB15_training-set.csv	Data untuk pelatihan model
UNSW_NB15_testing-set.csv	Data untuk pengujian model


Sumber dataset: UNSW-NB15 Dataset di Kaggle.

Notebook akan mencari kedua file pada folder data/. Jika tidak ditemukan, notebook mencoba mengunduh dataset melalui kagglehub. Apabila proses unduh otomatis gagal, file CSV dapat diunggah secara manual ketika notebook meminta data.

Penting: gunakan nama file yang sama persis seperti pada tabel agar proses pembacaan data dapat berjalan.
Metodologi
Alur eksperimen pada notebook:
1. Persiapan lingkungan — mengimpor library dan mengatur parameter eksperimen.
2. Pemuatan dataset — membaca file training dan testing UNSW-NB15.
3. Eksplorasi data — memeriksa dimensi data, distribusi label, kategori serangan, dan nilai kosong.
4. Preprocessing — memisahkan fitur dan target serta memproses fitur kategorikal.
5. Pelatihan model — membangun model Random Forest untuk klasifikasi biner dan multikelas.
6. Evaluasi model — menghitung metrik dan menampilkan visualisasi hasil prediksi.
7. Analisis fitur — menampilkan fitur dengan nilai feature importance tertinggi pada model biner.
8. Penyimpanan model — mengekspor pipeline model, metadata, sampel data, dan kebutuhan aplikasi.
Preprocessing
- Kolom id dikeluarkan dari fitur karena berfungsi sebagai identitas baris.
- Kolom label digunakan sebagai target klasifikasi biner.
- Kolom attack_cat digunakan sebagai target klasifikasi multikelas.
- Kolom proto, service, dan state diproses menggunakan OneHotEncoder.
- Kategori dengan frekuensi kurang dari 100 ditangani sebagai kategori infrequent.
- Kategori yang tidak ditemukan saat pelatihan ditangani melalui handle_unknown="infrequent_if_exist".
- Fitur numerik diteruskan tanpa scaling. Random Forest berbasis pohon keputusan sehingga scaling bukan persyaratan umum untuk algoritma ini.
- Preprocessing dan model digabungkan dalam satu pipeline scikit-learn.
Konfigurasi Model
Konfigurasi Random Forest yang digunakan pada kedua skenario:
Parameter	Nilai
n_estimators	150
max_depth	18
min_samples_leaf	3
random_state	21
n_jobs	-1
class_weight	None


Untuk klasifikasi biner, prediksi kelas serangan ditentukan menggunakan ambang probabilitas 0,45. Klasifikasi multikelas menggunakan hasil prediksi kelas dari predict().
Perubahan dari Template
Lima perubahan parameter diterapkan pada template awal:
No.	Parameter	Nilai awal	Nilai setelah perubahan
1	RANDOM_STATE	42	21
2	n_estimators	100	150
3	max_depth	20	18
4	min_samples_leaf	2	3
5	Ambang prediksi biner	0,50	0,45


Perubahan tersebut merupakan konfigurasi eksperimen. Dampaknya perlu dinilai dengan menjalankan notebook dan membandingkan metrik serta waktu pelatihan dengan hasil template awal.
Evaluasi
Notebook mengevaluasi dua skenario berikut:
Skenario	Metrik yang dihitung
Klasifikasi biner	Accuracy, Precision, Recall, F1-score, ROC-AUC, dan waktu pelatihan
Klasifikasi multikelas	Accuracy, Macro F1-score, Weighted F1-score, dan waktu pelatihan


Visualisasi yang dihasilkan mencakup distribusi label, confusion matrix, kurva ROC untuk klasifikasi biner, confusion matrix ternormalisasi untuk klasifikasi multikelas, serta 15 fitur terpenting pada model biner.

Nilai metrik tidak dicantumkan secara tetap di README karena hasil aktual diperoleh setelah seluruh sel notebook dijalankan.
Menjalankan Notebook
Lingkungan yang disarankan adalah Google Colab. Notebook juga dapat dijalankan pada Jupyter Notebook dengan dependensi yang sesuai.
1. Unduh atau clone repository.
2. Buka file random-forest-unsw-nb15.ipynb di Google Colab atau Jupyter Notebook.
3. Siapkan file UNSW_NB15_training-set.csv dan UNSW_NB15_testing-set.csv.
4. Letakkan file CSV di folder data/ atau unggah ketika diminta oleh notebook.
5. Jalankan seluruh sel secara berurutan.
6. Setelah proses selesai, periksa folder artifacts/ dan file artifacts.zip yang dihasilkan.
Library yang Digunakan
- Python
- NumPy
- pandas
- Matplotlib
- Seaborn
- scikit-learn
- joblib
- kagglehub (untuk opsi pengunduhan dataset)
Artefak Keluaran
Setelah seluruh sel notebook berhasil dijalankan, notebook menyimpan hasil pada folder artifacts/.
artifacts/
├── model/
│   ├── rf_binary.joblib
│   ├── rf_multiclass.joblib
│   └── metadata.json
├── data/
│   └── sample_flows.csv
└── requirements.txt
File	Keterangan
rf_binary.joblib	Pipeline model untuk klasifikasi normal dan serangan
rf_multiclass.joblib	Pipeline model untuk klasifikasi kategori serangan
metadata.json	Metadata fitur, kelas, metrik, feature importance, dan versi library
sample_flows.csv	Sampel data pengujian untuk percobaan penggunaan model
requirements.txt	Informasi dependensi yang disiapkan notebook


Model disimpan bersama tahap preprocessing di dalam pipeline. Oleh karena itu, data yang diberikan saat penggunaan ulang model harus mengikuti struktur fitur yang digunakan ketika model dilatih.
Struktur Repository
random-forest-unsw-nb15/
├── README.md
└── random-forest-unsw-nb15.ipynb
Folder artifacts/ dan file artifacts.zip akan dibuat setelah notebook dijalankan.
Batasan
- Eksperimen menggunakan file dataset UNSW-NB15, bukan paket jaringan yang ditangkap secara langsung.
- Model melakukan klasifikasi terawasi berdasarkan label yang tersedia pada dataset.
- Repository ini belum menyediakan pemantauan trafik jaringan real-time.
- Repository ini belum menyediakan dashboard operasional atau mekanisme otomatis untuk memblokir trafik serangan.
- Evaluasi perlu mempertimbangkan distribusi kelas serta metrik setiap kelas, bukan hanya accuracy.
- Hasil dapat dipengaruhi oleh versi library dan lingkungan eksekusi.
Referensi
- UNSW-NB15 Dataset — Kaggle
