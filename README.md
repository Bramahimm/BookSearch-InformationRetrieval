# Goodreads Book Search Engine
**Mata Kuliah:** Temu Kembali Informasi (COM620321) | **Universitas Lampung** | **Periode:** 2025/2026 Genap  
**Developer:** [Nama Anda] | **NIM:** [NIM Anda]

## 📖 Deskripsi Proyek
Proyek ini mengimplementasikan sistem temu kembali informasi (STKI) berbasis *command line interface* (CLI) untuk pencarian buku menggunakan dataset Goodreads. Sistem dirancang untuk mendemonstrasikan alur kerja STKi secara lengkap: mulai dari preprocessing teks, indexing dokumen berbasis model probabilistik, perhitungan skor relevansi, hingga evaluasi performa menggunakan metrik standar IR.

## ⚙️ Fitur Utama
- **Preprocessing Pipeline:** Case folding, penghapusan karakter non-alfanumerik/URL/HTML, tokenisasi NLTK, dan *stopword removal* (bahasa Inggris).
- **Model IR:** BM25 (Best Matching 25) untuk menghitung skor relevansi probabilistik dan melakukan ranking dokumen.
- **Evaluasi Sistem:** Perhitungan manual metrik MAP, Precision@K, dan NDCG@K berdasarkan perbandingan hasil pencarian dengan *ground truth*.
- **Antarmuka Interaktif:** CLI berbasis Python yang menerima input query pengguna dan menampilkan hasil terurut beserta skor relevansi.

## 📊 Dataset & Validasi
- **Sumber:** [Goodreads Best Books Ever (Kaggle)](https://www.kaggle.com/datasets/meetnaren/goodreads-best-books)
- **Jumlah Dokumen Awal:** ~54.301 entri
- **Filtering:** Hanya dokumen dengan deskripsi ≥100 kata yang diproses sesuai ketentuan STKI.
- **Dokumen Valid:** 35.894 buku
- **Korpus Pencarian:** Penggabungan kolom `book_title`, `book_authors`, `genres`, dan `book_desc` untuk memperkaya sinyal relevansi multi-field.

## 📂 Struktur Direktori
BooksSearch/
├── data/
│ ├── raw/ # Dataset mentah (book_data.csv)
│ ├── processed/ # Hasil preprocessing (cleaned_corpus.csv)
│ └── ground_truth.json # 10 query uji beserta label relevansi manual
├── main.ipynb # Notebook utama (pipeline lengkap)
└── README.md # Dokumentasi proyek


## 🚀 Cara Menjalankan Sistem
Proyek ini dioptimalkan untuk dijalankan di **Google Colab**. Ikuti langkah berikut:
1. Pastikan file dataset `book_data.csv` tersimpan di folder `data/raw/`.
2. Buka `main.ipynb` di Google Colab.
3. Hubungkan notebook ke Google Drive (`drive.mount('/content/drive')`).
4. Sesuaikan variabel `BASE_DIR` pada cell pertama dengan path proyek Anda.
5. Jalankan seluruh sel secara berurutan (`Runtime > Run All`).
6. Sistem akan otomatis melakukan preprocessing, membangun indeks BM25, menjalankan evaluasi 10 query, dan membuka sesi pencarian interaktif.

**Dependensi Utama:** `pandas`, `numpy`, `nltk`, `rank_bm25`, `tqdm`

## 📈 Hasil Evaluasi
Evaluasi dilakukan menggunakan 10 query dengan konteks berbeda dan dibandingkan terhadap *ground truth* yang dianotasi secara manual. Metrik dihitung secara transparan tanpa bergantung pada library evaluasi *black-box*.

| Metrik | Nilai |
|--------|-------|
| Mean Average Precision (MAP) | 0.3081 |
| Precision@3 | 0.2667 |
| Precision@5 | 0.2200 |
| NDCG@5 | 0.3331 |

**Catatan Teknis:** Nilai metrik berada dalam kisaran realistis untuk model BM25 pada dataset teks multi-field. Sistem bersifat *unsupervised* (tidak memerlukan pembagian data latih/uji). Evaluasi murni mengukur konsistensi ranking hasil terhadap label relevansi yang telah ditetapkan. Query dengan kata kunci spesifik (mis. *thriller psychological*) cenderung menghasilkan AP lebih tinggi dibandingkan query umum.

## 🔍 Implementasi Teknis
- **Konsistensi Preprocessing:** Pipeline pembersihan teks dan tokenisasi diterapkan secara identik pada corpus dan input query untuk memastikan pencocokan token yang akurat.
- **Parameter BM25:** `k1 = 1.2` (mengontrol saturasi term frequency), `b = 0.75` (normalisasi panjang dokumen).
- **Penanganan Ground Truth:** Pencocokan judul dilakukan setelah normalisasi string (lowercase, penghapusan spasi berlebih) untuk mengurangi *false negative* akibat perbedaan penulisan atau subtitle.
- **Arsitektur:** Single-notebook (`main.ipynb`) dipilih untuk memudahkan reproduktibilitas, pelacakan alur data, dan presentasi akademis.



