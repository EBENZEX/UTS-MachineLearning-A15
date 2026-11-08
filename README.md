# Klasifikasi spam SMS · UTS Milestone 1

Ekstrak seluruh folder tugas sebelum membuka `Spam_SMS_UTS.ipynb`. Folder boleh dipindahkan atau diganti namanya. Biarkan notebook, `requirements.txt`, dan `data/` berdampingan.

## Menjalankan notebook

1. Siapkan environment **Python 3.12**. Buka terminal di folder yg berisi notebook dan gunakan Python dari environment tersebut.
2. Pasang pustaka sekali dengan perintah berikut.

   ```sh
   python -m pip install -r requirements.txt
   ```

3. Buka `Spam_SMS_UTS.ipynb` di VS Code (ekstensi Jupyter) atau JupyterLab. Pilih environment tadi sebagai kernel, lalu pilih **Restart Kernel → Run All**.

Jika ingin memakai JupyterLab dan belum memasangnya, jalankan dua perintah ini dari terminal yg sama.

```sh
python -m pip install jupyterlab
python -m jupyterlab Spam_SMS_UTS.ipynb
```

## Data

- `data/super_sms_dataset.csv` berisi SMS dan label dari [Super SMS](https://github.com/smspamresearch/spstudy/blob/a0889f36fb59e23a7f853a6932485d93d18b8afc/Data/super_sms_dataset.csv). Pertahankan file aslinya karena notebook memeriksa SHA-256 sebelum membaca data.
- `data/pembagian_awal.csv` menyimpan penempatan awal ID pesan. File ini selalu dibaca sebelum penanganan pola lintas bagian di bagian 3.

## Isi folder hasil

Setiap tahap menyimpan berkas di lokasi tetap. **Run All** memperbarui berkas di bawah ini. Kalau hanya menjalankan ulang sel prediksi SMS sendiri, yg diperbarui hanya tabel di `prediksi_sendiri/`.

| Folder di dalam `hasil/` | Isinya |
|---|---|
| `preprocessing/` | ID pembagian akhir, baris yg dikeluarkan, dan pola SMS yg melintasi pembagian. |
| `eda/` | Grafik distribusi kelas dan panjang SMS pada data train. |
| `model/` | `logistic_regression_kata.joblib` (baseline), `svm_kata_karakter.joblib` (kandidat), dan `konfigurasi.json`. Kedua file model sudah memuat TF-IDF masing-masing. |
| `evaluasi/` | [Tabel perbandingan model](hasil/evaluasi/hasil_validasi.csv), skor per fold, confusion matrix, classification report, dan prediksi per SMS validasi. |
| `learning_curve/` | Grafik dan tabel perubahan macro-F1 saat jumlah SMS latih ditambah. |
| `prediksi_sendiri/` | Hasil percobaan untuk SMS yg dimasukkan di bagian 8. |

Setelah training, isi `pesan_sendiri` di bagian 8 bisa diubah untuk mencoba SMS lain dengan model yg baru dilatih.
