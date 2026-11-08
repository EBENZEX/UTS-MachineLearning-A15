# Klasifikasi Spam SMS Berbasis TF-IDF: Perbandingan Logistic Regression dan Linear SVM pada Dataset Super SMS

[[https://colab.research.google.com/assets/colab-badge.svg]](https://colab.research.google.com/drive/1KjFqYSlPPyB6gYbXdABLdGQa_FwONIzH?usp=sharing)
[https://img.shields.io/badge/Python-3.12.14-3776AB?logo=python&logoColor=white]
[https://img.shields.io/badge/scikit--learn-1.9.1-F7931E?logo=scikitlearn&logoColor=white]
[https://img.shields.io/badge/Milestone_1-UTS_Selesai-2EA043]

Repositori ini memuat kode eksperimen, dataset, hasil evaluasi lengkap, dan laporan **UTS Milestone 1** mata kuliah Machine Learning untuk tugas klasifikasi biner SMS (*ham* vs *spam*) menggunakan representasi fitur **TF-IDF** serta perbandingan model **Logistic Regression** (baseline) dan **Linear SVM** (kandidat) pada [Super SMS Dataset](https://github.com/smspamresearch/spstudy/blob/a0889f36fb59e23a7f853a6932485d93d18b8afc/Data/super_sms_dataset.csv) ([Salman, Ikram, & Kaafar, 2024](https://doi.org/10.1109/ACCESS.2024.3364671)).

---

## Identitas Kelompok (A15 · IF-A Pagi)

**Program Studi S-1 Teknik Informatika · Fakultas Informatika · Universitas Mikroskil (2026)**

| NIM | Nama Anggota | Tanggung Jawab & Kontribusi |
| :--- | :--- | :--- |
| `231111575` | **Julian Caesar Eben Ezer Simorangkir** | Perumusan masalah, pembersihan data, EDA, pemodelan, evaluasi, dan penyusunan laporan |
| `231111644` | **Howard Farrel Hura** | Perumusan masalah, pembersihan data, EDA, pemodelan, evaluasi, dan penyusunan laporan |
| `231111131` | **Kevin Luther Kiyosaki Gultom** | Perumusan masalah, pembersihan data, EDA, pemodelan, evaluasi, dan penyusunan laporan |

---

## Tautan Cepat & Berkas Utama

| Komponen | Lokasi / Tautan | Keterangan |
| :--- | :--- | :--- |
| **Google Colab** | [Buka di Google Colab](https://colab.research.google.com/drive/1KjFqYSlPPyB6gYbXdABLdGQa_FwONIzH?usp=sharing) | Eksekusi interaktif di cloud |
| **Notebook Utama** | [`Spam_SMS_UTS.ipynb`](Spam_SMS_UTS.ipynb) | 9 sel kode & 11 sel penjelasan (`SEED = 42`) |
| **Laporan UTS (PDF)** | [`A15_SpamPesan_IF_A_Pagi.pdf`](A15_SpamPesan_IF_A_Pagi.pdf) | Dokumen laporan lengkap Bab 1–4 beserta lampiran |
| **Tabel Evaluasi** | [`hasil/evaluasi/hasil_validasi.csv`](hasil/evaluasi/hasil_validasi.csv) | Ringkasan metrik CV 3-fold dan data validation |
| **Konfigurasi Lingkungan** | [`hasil/model/konfigurasi.json`](hasil/model/konfigurasi.json) | Detail parameter pipeline, versi pustaka, dan SHA-256 |

---

## Ringkasan Alur & Pencegahan Data Leakage

1. **Integritas Sumber Data**: Berkas `data/super_sms_dataset.csv` (67.010 baris mentah) diverifikasi menggunakan SHA-256 (`ba825b46...894d3`). Sebanyak 21 baris tidak valid/korup disisihkan, dinormalisasi menggunakan Unicode NFKC, dan 95 baris duplikat teks (*casefold*) dihapus hingga tersisa 66.894 baris unik.
2. **Karantina Pola Lintas Split (*Zero Group Overlap*)**: Pola huruf `a-z` tanpa angka/tanda baca dikelompokkan ke kolom `kelompok`. Sebanyak 606 pesan bertemplate serupa yang beririsan antar-split dikarantina demi mencegah kebocoran data (*data leakage*). Pembagian akhir menghasilkan **46.301 train** (69,85%), **9.955 validation** (15,02%), dan **10.032 test** (15,13% — disisihkan tanpa disentuh untuk evaluasi akhir UAS).
3. **Cross-Validation Berkelompok**: Validasi silang menggunakan `StratifiedGroupKFold` (3-fold, `SEED = 42`) berdasarkan kolom `kelompok` di dalam `Pipeline` scikit-learn agar kamus kosa kata dan bobot IDF dihitung murni hanya dari lipatan latih (*train fold*).

[hasil/eda/eda.png]

---

## Ringkasan Hasil Eksperimen (Milestone 1)

Sesuai ketentuan evaluasi yang dirangkum pada laporan UTS, berikut perbandingan performa antara model baseline dan kandidat pada data validasi terpisah (9.955 SMS):

| Metrik Evaluasi | Baseline: Logistic Regression (TF-IDF Kata) | Kandidat: Linear SVM (TF-IDF Kata + Karakter) | Selisih (SVM − LR) |
| :--- | :---: | :---: | :---: |
| **CV 3-Fold Macro-F1 (Train)** | 0,9820 ± 0,0013 | **0,9909 ± 0,0014** | +0,0089 |
| **Macro-F1 (Validation)** | 0,9798 | **0,9897** | +0,0099 |
| **Accuracy (Validation)** | 0,9808 | **0,9902** | +0,0093 |
| **Precision Spam (Validation)** | 0,9861 | **0,9925** | +0,0064 |
| **Recall Spam (Validation)** | 0,9647 | **0,9823** | +0,0177 |
| **F1 Spam (Validation)** | 0,9753 | **0,9874** | +0,0121 |
| **FP Rate Ham (Validation)** | 0,0088 (53 pesan) | **0,0048 (29 pesan)** | -0,0040 (-24 pesan) |
| **Jumlah FN / Spam Lolos** | 138 pesan | **69 pesan** | -69 pesan |
| **Jumlah Fitur Terbentuk** | 30.000 kolom | 80.000 kolom | +50.000 kolom |
| **Waktu Pelatihan (Full Train)** | 1,88 detik | 9,82 detik | +7,94 detik |

### Visualisasi Evaluasi Model

| Matriks Kebingungan (Confusion Matrix) | Kurva Belajar (Learning Curve) |
| :---: | :---: |
| [hasil/evaluasi/confusion_matrix.png] | [hasil/learning_curve/learning_curve.png] |

---

## Struktur Folder Repositori

```text
├── .gitattributes                         # Menjaga byte akhir baris CSV lintas OS agar hash SHA-256 konsisten
├── A15_SpamPesan_IF_A_Pagi.pdf            # Laporan resmi UTS Milestone 1 (PDF)
├── README.md                              # Dokumentasi utama repositori
├── requirements.txt                       # Dependensi pustaka Python 3.12
├── Spam_SMS_UTS.ipynb                     # Notebook eksperimen (Bagian 1–9)
├── data/
│   ├── super_sms_dataset.csv              # Dataset mentah Super SMS (verifikasi SHA-256)
│   └── pembagian_awal.csv                 # Penempatan awal 66.894 ID pesan
└── hasil/
    ├── preprocessing/                     # Berkas baris dikarantina & pembagian akhir data
    ├── eda/                               # Grafik distribusi kelas & panjang pesan train
    ├── model/                             # Pipeline terlatih (.joblib) & konfigurasi JSON
    ├── evaluasi/                          # Tabel metrik, skor per fold, confusion matrix, & prediksi validasi
    ├── learning_curve/                    # Grafik dan tabel kurva belajar kandidat SVM
    └── prediksi_sendiri/                  # Hasil uji coba 7 SMS manual (normal vs samaran)
