# WS-04: Research Question & Hypothesis

> **Bab 4 — Research Question, Contribution & Hypothesis**

---

## Ringkasan Materi

### RQ Bukan Pertanyaan Biasa

Research Question yang baik secara implisit mengandung cetak biru eksperimen: subjek, baseline, metrik, domain, dataset.

| Kualitas | Contoh |
|----------|--------|
| **Buruk** | "Bagaimana pengaruh deep learning terhadap deteksi malware?" |
| **Baik** | "Apakah CNN menghasilkan F1-Score lebih tinggi dari RF pada CIC-MalMem-2022?" |

Perbedaan: RQ yang baik menyebutkan **metode spesifik**, **metrik terukur**, **baseline**, dan **dataset**.

### Tiga Jenis RQ

| Jenis | Pola | Kebutuhan |
|-------|------|-----------|
| **Comparison** | A vs B → mana lebih baik? | ≥ 2 metode, metrik sama |
| **Improvement** | A' vs A → modifikasi lebih baik? | Pre/post, bukti perbaikan |
| **Exploratory** | Faktor X₁...Xₙ → pengaruh terhadap Y? | Multi-variabel, korelasi/regresi |

### Contribution Statement

Tiga jenis kontribusi: **Improvement** (metode terbukti lebih baik), **Comparison** (perbandingan sistematis yang belum ada), **Novel Approach** (pendekatan baru). Kontribusi harus terhubung langsung dengan gap — kontribusi tanpa gap = klaim tanpa justifikasi.

### Hypothesis H₀ / H₁

- **H₀** (Null) = Tidak ada perbedaan signifikan — asumsi default, harus dibuktikan salah
- **H₁** (Alternative) = Ada perbedaan signifikan — diterima hanya jika H₀ ditolak
- Harus **falsifiable**, mengandung **metrik terukur**, dirumuskan **SEBELUM eksperimen**

### Rantai Operasionalisasi

```
RQ → Variable → Metric → Data → Analysis
```

Jika rantai ini tidak lengkap, RQ belum mature. Bi-directional: RQ yang tidak bisa jadi hipotesis testable harus direvisi mundur.

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan pertanyaan | Apa yang harus dibangun? | Apa yang harus dibuktikan? |
| Bentuk jawaban | Sistem yang berfungsi | Bukti empiris terukur |
| Sukses diukur oleh | User satisfaction, uptime | Signifikansi statistik, effect size |
| Jika gagal | Debug dan perbaiki | Laporkan, analisis mengapa |

### Istilah Penting

- **Research Question (RQ)** — Pertanyaan spesifik: variabel terukur + metrik + konteks
- **Contribution Statement** — Apa yang diketahui setelah riset selesai yang sebelumnya belum ada
- **H₀ / H₁** — Null vs Alternative Hypothesis
- **Falsifiability** — Kondisi hipotesis ditolak harus bisa didefinisikan sebelum eksperimen
- **Operationalization** — Proses mewujudkan konsep abstrak menjadi variabel terukur

---

## Template A.4 — RQ-Contribution-Hypothesis

```
RQ-CONTRIBUTION-HYPOTHESIS

Gap Statement  : Mayoritas literatur hanya melakukan klasifikasi biner (bagus/rusak) pada uang Rupiah. Sangat sedikit yang melakukan klasifikasi granular tingkat kerusakan (ringan, sedang, berat) menggunakan model efisien yang cocok untuk aplikasi mobile.

Research Question:
  Tipe         : [ X ] Comparison  [ ] Improvement  [ ] Exploratory
  Formulasi    : Seberapa baik MobileNetV2 dibandingkan dengan baseline CNN 2-Layer dalam mengklasifikasikan tingkat kerusakan uang Rupiah (ringan, sedang, berat) berdasarkan Accuracy dan Macro F1-Score pada dataset 1.000 citra berlabel ahli BI?
  Variabel IV  : Arsitektur model (MobileNetV2 vs CNN 2-Layer)
  Variabel DV  : Performa klasifikasi tingkat kerusakan
  Metrik       : Accuracy dan Macro F1-Score
  Dataset      : 1.000 citra uang Rupiah berlabel tingkat kerusakan oleh ahli Bank Indonesia
  Baseline     : CNN 2-Layer

Quality Check RQ:
  [ v ] Variabel spesifik
  [ v ] Metrik jelas
  [ v ] Baseline ada
  [ v ] Konteks disebutkan
  [ v ] Memerlukan eksperimen (bukan hanya survei literatur)

Contribution Statement:
  Apa yang baru diketahui : Perbandingan performa MobileNetV2 versus baseline CNN 2-Layer untuk klasifikasi 3 tingkat kerusakan uang Rupiah beserta analisis visual menggunakan Grad-CAM.
  Jenis kontribusi        : [ x ] Improvement  [ x ] Comparison  [ ] Novel approach
  Gap yang diisi          : Data Gap + Method Gap

Hypothesis Pair:
  H₀ : Tidak ada perbedaan signifikan dalam Accuracy dan Macro F1-Score antara MobileNetV2 dengan baseline CNN 2-Layer pada klasifikasi tingkat kerusakan uang Rupiah.
  H₁ : MobileNetV2 menghasilkan Accuracy dan Macro F1-Score yang secara signifikan lebih tinggi daripada baseline CNN 2-Layer.
  Threshold              : α = 0.05 (p-value < 0.05)
  Justifikasi threshold  : Merupakan standar signifikansi statistik yang umum digunakan dalam penelitian machine learning dan computer vision.
```

---

## Latihan 1 — Dari Gap ke RQ

Gunakan gap yang ditemukan di WS-03. Transformasikan menjadi Research Question.

**Gap dari WS-03:**Data Gap + Method Gap (klasifikasi hanya biner dan belum ada penerapan model ringan seperti MobileNetV2 untuk klasifikasi tingkat kerusakan granular pada uang Rupiah).

**RQ versi pertama (tulis bebas):**
> Bagaimana cara mendeteksi tingkat kerusakan uang Rupiah secara lebih akurat?

**Evaluasi RQ:**

| Komponen | Ada? | Isi |
|----------|------|-----|
| Metode spesifik | Tidak | - |
| Metrik terukur | Tidak | - |
| Baseline | Tidak | - |
| Dataset/konteks | Sebagian | Uang Rupiah |

**Tipe RQ:** [X] Comparison / [ ] Improvement / [ ] Exploratory

**RQ versi revisi (setelah evaluasi):**
> Seberapa baik MobileNetV2 dibandingkan dengan baseline CNN 2-Layer dalam mengklasifikasikan tingkat kerusakan uang Rupiah (ringan, sedang, berat) berdasarkan Accuracy dan Macro F1-Score pada dataset 1.000 citra berlabel ahli BI?

---

## Latihan 2 — Hypothesis Pair

Rumuskan pasangan hipotesis dari RQ di Latihan 1.

| Komponen | Isi |
|----------|-----|
| H₀ | Tidak ada perbedaan signifikan dalam Accuracy dan Macro F1-Score antara MobileNetV2 dengan baseline CNN 2-Layer pada klasifikasi tingkat kerusakan uang Rupiah |
| H₁ | MobileNetV2 menghasilkan Accuracy dan Macro F1-Score yang secara signifikan lebih tinggi daripada baseline CNN 2-Layer |
| Metrik |Accuracy dan Macro F1-Score |
| Threshold | p-value < 0.05 |
| Justifikasi threshold | Standar konvensional dalam penelitian ML/Computer Vision |

**Apakah hipotesis ini falsifiable?** [ X ] Ya / [ ] Tidak
> Bagaimana cara membuktikannya salah? Dengan melakukan eksperimen menggunakan 5-fold cross validation, menghitung metrik evaluasi, kemudian melakukan uji statistik (Independent t-test atau Wilcoxon Signed-Rank Test). Jika p-value > 0.05, maka H₀ diterima dan H₁ ditolak.

---

## Latihan 3 — Rantai Operasionalisasi

Lengkapi rantai dari RQ hingga metode analisis.

| Tahap | Isi |
|-------|-----|
| RQ | Seberapa baik MobileNetV2 dibandingkan CNN 2-Layer dalam klasifikasi tingkat kerusakan uang Rupiah… |
| Variable (IV) | Arsitektur model (MobileNetV2 vs CNN 2-Layer) |
| Variable (DV) | Performa klasifikasi tingkat kerusakan |
| Metric | Accuracy dan Macro F1-Score |
| Data source | Dataset 1.000 citra uang Rupiah berlabel ahli BI |
| Analysis method | Training dengan 5-fold cross validation, evaluasi metrik, uji statistik, Confusion Matrix, dan Grad-CAM |

**Apakah rantai lengkap?** [ X ] Ya / [ ] Tidak
> Jika tidak, tahap mana yang perlu direvisi? ______________

---

## Refleksi

> Ambil satu judul skripsi/paper yang pernah dibaca. Coba ekstrak RQ-nya. Apakah RQ tersebut memenuhi semua komponen (metode, metrik, baseline, konteks)? Jika tidak, apa yang hilang?

**Judul:**Deteksi Kondisi Uang Bagus Dan Rusak Dengan Pengolahan Citra Digital Berbasis Convolutional Neural Network (CNN) – Nandika et al. (2025)
**RQ yang diekstrak:** Apakah CNN dapat mendeteksi kondisi uang rusak dengan baik?
**Komponen yang hilang:** 
1. Tidak ada baseline perbandingan
2. Metrik hanya akurasi (tidak menggunakan Macro F1-Score)
3. Tidak menyebutkan jumlah dan karakteristik dataset
4. Tidak ada granularitas tingkat kerusakan (ringan/sedang/berat)
5. Tidak spesifik konteks eksperimen
