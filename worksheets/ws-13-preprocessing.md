# WS-13: Data Preprocessing

> **Bab 13 — Preprocessing & Persiapan Data untuk Analisis**

---

## Ringkasan Materi

### Data Refinement Pipeline

```
Raw Data → Cleaning → Transformation → Normalization → Processed Data → Analysis Ready
```

Setiap tahap memiliki tujuan berbeda. **Preprocessing bukan langkah teknis biasa** — setiap keputusan preprocessing adalah keputusan riset yang bisa mengubah kesimpulan.

### Empat Prinsip Preprocessing

| Prinsip | Deskripsi |
|---------|----------|
| **Consistency** | Metode sama untuk data yang sama |
| **Transparency** | Setiap langkah terdokumentasi |
| **Reproducibility** | Orang lain bisa mengulang dengan hasil sama |
| **Minimal Distortion** | Ubah sesedikit mungkin; jika normalisasi tidak perlu, jangan lakukan |

### Cleaning Triad

| Masalah | Strategi | Risiko |
|---------|---------|--------|
| **Missing values** | | |
| — Listwise deletion | Missing < 5%, random | Data loss |
| — Mean/median imputation | Sedikit missing, dist. normal | Mengurangi variabilitas |
| — Model-based imputation | Banyak missing, pola sistematis | Introduces dependency |
| — Flag & separate | Missing karena alasan substantif | Kompleksitas analisis |
| **Duplikat** | Identifikasi → verifikasi → hapus | False positive (data mirip ≠ duplikat) |
| **Error format** | Standardisasi tipe, encoding | Kehilangan informasi saat konversi |

### Normalisasi — Kapan & Metode Mana

| Metode | Formula | Output | Sensitif Outlier? |
|--------|---------|--------|-------------------|
| Min-max | (x-min)/(max-min) | [0, 1] | Ya |
| Z-score | (x-mean)/std | Unbounded | Lebih robust |
| Robust scaling | (x-median)/IQR | Unbounded | Paling robust |

**Kunci:** Parameter normalisasi harus dihitung dari **training set saja** — bukan seluruh data. Pelanggaran = **data leakage**.

### Data Leakage Prevention

Data leakage terjadi ketika informasi dari test set "bocor" ke preprocessing:
- Normalisasi parameter dari seluruh dataset ← **SALAH**
- Cross-validation dilakukan sebelum split ← **SALAH**
- Feature selection menggunakan label test set ← **SALAH**

### Jebakan Kognitif

1. "Preprocessing cuma teknis — tidak perlu detail" → bisa ubah kesimpulan
2. "Lebih banyak preprocessing = lebih bersih = lebih baik" → over-processing distorsi data
3. "Normalisasi selalu diperlukan" → belum tentu, tergantung metode analisis
4. "Imputation sama untuk semua situasi" → strategi harus sesuai konteks

---

## Template A.13 — Preprocessing Documentation Log

```
PREPROCESSING LOG

Dataset           : Citra Uang Kertas Rupiah (3 kelas: Ringan, Sedang, Berat)
Jumlah data awal  : 90 records (30 per kelas: 10 train, 10 val, 10 test)

Cleaning:
| Masalah | Jumlah Kasus | Penanganan | Justifikasi |
|---------|-------------|------------|-------------|
| Missing | 2 dari 90 (2.2%) | Hapus (listwise deletion) |Jumlah missing < 5% dan terjadi secara acak (tidak bias pada kelas tertentu). Menghapus sejumlah kecil data tidak akan mengganggu distribusi kelas atau performa model secara signifikan.|
| Duplikat| 3 dari 90 (3.3%) | Hapus duplikat, pertahankan satu |Data duplikat dapat menyebabkan overfitting karena model melihat gambar yang sama berulang kali. Hal ini juga dapat memberikan bobot berlebih pada gambar tertentu dan menghasilkan evaluasi yang bias.|
| Error   | 1 dari 90 (1.1%) | verifikasi ulang dengan ahli | Label yang salah (mislabel) dapat merusak performa model karena model belajar dari data yang salah. Verifikasi ulang memastikan label sesuai dengan kondisi gambar sebelum digunakan untuk training.|

Transformation:
| Transformasi | Variabel | Detail | Alasan |
|-------------|----------|--------|--------|
|Resize|Citra (semua gambar)|224 × 224 piksel|MobileNetV2 membutuhkan input ukuran 224×224. Resize dilakukan untuk menstandarisasi dimensi input agar kompatibel dengan arsitektur model.|
|Augmentasi (training set)|Citra (hanya train)|RandomRotation(10°), RandomHorizontalFlip(p=0.3), ColorJitter(brightness=0.1, contrast=0.1)|Augmentasi membantu model lebih generalisasi dengan menambah variasi data secara artifisial. Hanya diterapkan pada training set untuk menghindari data leakage|
|Konversi ke Tensor|Citra (semua data)|PIL Image → PyTorch Tensor (ToTensor)|Model PyTorch memproses data dalam format tensor. Konversi ini mengubah gambar dari format PIL ke tensor dan menskalakan nilai pixel dari [0,255] ke [0,1].|
|Normalisasi|Citra (semua data)|Mean=[0.485, 0.456, 0.406], Std=[0.229, 0.224, 0.225]|Menggunakan statistik normalisasi dari ImageNet karena MobileNetV2 menggunakan pre-trained weights. Ini memastikan input model memiliki distribusi yang sama dengan data training asli.|


Normalization:
  Metode    : Normalize dengan Mean & Std ImageNet
  Alasan    : MobileNetV2 menggunakan pre-trained weights dari ImageNet, sehingga input harus memiliki distribusi yang sama dengan data training ImageNet agar transfer learning bekerja optimal. Normalisasi juga membantu mempercepat konvergensi selama training.
  Parameter : mean = [0.485, 0.456, 0.406]
std = [0.229, 0.224, 0.225]
(dihitung dari: ImageNet, BUKAN dari dataset)

Leakage Check:
  [Y] Parameter normalisasi dari training set saja
  [Y] Tidak ada informasi test set dalam preprocessing
  [Y] Cross-validation dilakukan setelah split

Jumlah data akhir :
Split	  Sebelum Cleaning	Setelah Cleaning
Train	          30	            28
Val	            30	            28
Test	          30	            28
Total	          90	            84

Script tersedia   : [Y] Ya → path:C:\rupiah-classification\train.py| [ ] Belum
Script tersedia   : [Y] Ya → path:C:\rupiah-classification\create_dummy.py (opsional)| [ ] Belum
```

---

## Latihan 1 — Cleaning Plan

Periksa dataset Anda (atau dataset contoh) dan dokumentasikan masalah yang ditemukan.

| Masalah | Jumlah Kasus | Penanganan | Justifikasi |
|---------|-------------|------------|-------------|
| Gambar korup / tidak terbaca | 2 dari 90 (2.2%) | Listwise deletion (hapus) | < 5%, terjadi secara acak (tidak bias pada kelas tertentu), tidak mengganggu distribusi kelas |
| Gambar duplikat (identik) | 3 dari 90 (3.3%) | Hapus duplikat, pertahankan satu | Data duplikat menyebabkan overfitting dan memberikan bobot berlebih pada gambar tertentu |
| Label tidak sesuai (mislabel) | 1 dari 90 (1.1%) | Verifikasi ulang dengan ahli | Label yang salah dapat merusak performa model secara signifikan |
| Resolusi terlalu rendah | 0 dari 90 (0%) | — | Semua gambar memiliki resolusi ≥ 224×224 |
| Format file tidak didukung | 0 dari 90 (0%) | — | Semua gambar dalam format JPG/JPEG |

**Jumlah data sebelum cleaning:** 90 gambar
**Jumlah data setelah cleaning:** 84 gambar
**Persentase data yang hilang/berubah:** 6.7%

---

## Latihan 2 — Normalisasi Decision

Tentukan apakah data Anda perlu normalisasi, dan jika ya, metode apa yang tepat.

| Variabel | Range Asli | Distribusi | Outlier? | Metode Normalisasi | Alasan |
|----------|-----------|-----------|----------|-------------------|--------|
| Pixel values (RGB) | 0 – 255 | Mendekati normal (natural image) | Tidak ada outlier signifikan | Min-max scaling + Z-score (ImageNet stats) | Model menggunakan pre-trained ImageNet yang membutuhkan input terstandarisasi; normalisasi juga mempercepat konvergensi |
| Image size | 224×224 (setelah resize) | Seragam | Tidak ada | Tidak perlu | Semua gambar sudah di-resize ke ukuran yang sama |
| Macro F1-Score | 0.0 – 1.0 | Mendekati normal | Ada (71.23% outlier) | Tidak perlu | Sudah dalam [0,1], uji statistik tidak memerlukan normalisasi |
| Accuracy | 0.0 – 1.0 | Mendekati normal | Tidak ada outlier signifikan | Tidak perlu | Sudah dalam [0,1] |
| Training Loss | 0.0 – 1.5 | Asimetris | Tidak ada | Tidak perlu | Hanya untuk visualisasi, tidak untuk analisis statistik |

**Apakah normalisasi diperlukan?** [Y] Ya / [ ] Tidak
**Justifikasi:**
> Normalisasi diperlukan untuk data citra karena model menggunakan pre-trained weights dari ImageNet yang membutuhkan input dengan distribusi yang sama (mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225]). Normalisasi juga mempercepat konvergensi selama training. Untuk data metrik hasil eksperimen, normalisasi tidak diperlukan karena semua metrik sudah dalam skala [0,1] dan uji statistik yang digunakan (Paired T-Test) tidak memerlukan normalisasi data.

**Leakage check:**
- [Y] Parameter dihitung dari training set saja
- [Y] Normalisasi diterapkan setelah train-test split

---

## Latihan 3 — Preprocessing Report

Buat ringkasan preprocessing lengkap — dokumentasi yang cukup bagi orang lain untuk mereplikasi.

```
PREPROCESSING SUMMARY

1. Dataset: Citra Uang Kertas Rupiah (3 kelas: Ringan, Sedang, Berat)
2. Data awal: 90 records, 3 channel RGB, ukuran bervariasi (di-resize ke 224x224) features
3. Cleaning:
   - Missing values: 2 kasus, metode: Listwise deletion (hapus)
   - Duplikat: 3 kasus, tindakan: Hapus duplikat, pertahankan satu
   - Error: 1 kasus, tindakan: Verifikasi ulang dengan ahli
4. Transformation:
   - Resize: 224×224 piksel (untuk MobileNetV2 input)
   - Augmentasi (hanya training): RandomRotation(10°), RandomHorizontalFlip(p=0.3), ColorJitter(0.1)
   - Konversi: PIL Image → PyTorch Tensor (ToTensor)
5. Normalisasi: Normalize dengan mean & std ImageNet (metode), parameter dari ImageNet (bukan dari dataset)
   - mean = [0.485, 0.456, 0.406]
   - std = [0.229, 0.224, 0.225]
6. Data akhir: 84 records, 3 channel RGB, 224×224 piksel features
7. Leakage check: [Y] Lulus / [ ] Ada masalah
```

---

## Refleksi

> Apakah Anda pernah melakukan normalisasi "karena biasa dilakukan" tanpa mempertimbangkan apakah benar-benar diperlukan? Apa risiko over-preprocessing?

> Ya, saya pernah melakukannya. Saya dulu otomatis menormalisasi semua variabel numerik dengan Min-Max Scaling tanpa bertanya apakah metode analisis saya benar-benar membutuhkannya.
Hal ini terjadi karena kebiasaan dari tutorial — banyak contoh di internet selalu memulai dengan normalisasi, sehingga saya ikuti tanpa pikir panjang. Selain itu, ada anggapan keliru bahwa normalisasi selalu membuat model lebih baik, padahal tidak semua metode membutuhkannya. Saya juga tidak memeriksa skala variabel terlebih dahulu — padahal beberapa variabel seperti akurasi dan F1-Score sudah berada dalam rentang [0,1], sehingga normalisasi tidak memberikan manfaat tambahan dan hanya mempersulit interpretasi hasil.
> Kehilangan interpretasi	Setelah normalisasi, nilai tidak lagi dalam satuan asli → sulit dimaknai
  Distorsi hubungan	Normalisasi mengubah jarak antar data, bisa mengubah pola asli
  Data leakage	Jika parameter normalisasi dihitung dari seluruh data (bukan training set)
  Kompleksitas berlebihan	Menambah langkah yang sebenarnya tidak dibutuhkan
