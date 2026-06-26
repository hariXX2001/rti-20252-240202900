# WS-12: Result Presentation & Visualization

> **Bab 12 — Penyajian Hasil & Visualisasi**

---

## Ringkasan Materi

### Data → Insight Model

```
Validated Data → Structured Presentation → Visualization → Pattern Recognition → Insight
```

Penyajian **mendahului** analisis. Tabel dan grafik membantu peneliti "melihat" data sebelum menghitung. Langsung ke uji statistik tanpa visualisasi berisiko kesimpulan yang secara teknis benar tapi kontekstual salah (Anscombe's Quartet, 1973).

### Tabel = Presisi, Grafik = Pola

Keduanya **saling melengkapi**:
- Tabel: angka presisi, self-contained (dipahami tanpa teks), sortable
- Grafik: pola visual, tren, perbandingan cepat

### Jenis Grafik Berdasarkan Tujuan

| Tujuan | Jenis Grafik |
|--------|-------------|
| Perbandingan antar-skenario | Bar chart (grouped/stacked) |
| Distribusi per-skenario | Box plot / violin plot |
| Tren temporal | Line chart |
| Korelasi dua variabel | Scatter plot |
| Proporsi (total = 100%) | Pie chart (hati-hati!) |

### Contoh Tabel Hasil yang Baik

| Model | Accuracy (%) | F1-Score (%) | Training Time (min) |
|-------|-------------|-------------|---------------------|
| BERT | 88.4 ± 1.2 | 87.1 ± 1.4 | 45.2 ± 3.1 |
| LSTM | 86.1 ± 1.8 | 84.5 ± 2.0 | 12.8 ± 1.2 |
| SVM | 82.3 ± 0.9 | 80.7 ± 1.1 | 0.3 ± 0.1 |

*N=10 per model. Mean ± std. Diurutkan berdasarkan Accuracy.*

### Visualization Bias — Yang Harus Dihindari

| Bias | Deskripsi | Dampak |
|------|----------|--------|
| Truncated axis | Y tidak dari 0 | Memperbesar perbedaan kecil |
| Inconsistent scale | Dua grafik skala beda | Perbandingan menyesatkan |
| Cherry-picked data | Hanya tampilkan yang "menang" | Selektif, tidak jujur |
| 3D effects | Efek 3D tanpa dimensi data ke-3 | Distorsi tanpa informasi |
| Missing error bar | Tidak ada variabilitas | Menyembunyikan ketidakpastian |

### Engineering vs Research Presentation

| Aspek | Engineering | Research |
|-------|-----------|---------|
| Tujuan grafik | Dashboard monitoring | Mendukung argumen ilmiah |
| Informasi wajib | KPI, threshold | Mean, std, CI, N, p-value |
| Bias handling | Less critical | Wajib dihindari (peer-review) |

---

## Template A.12 — Result Presentation Plan

```
RESULT PRESENTATION PLAN

Research Question : Apakah terdapat perbedaan performa yang signifikan antara MobileNetV2 dan Baseline CNN 2-Layer dalam mengklasifikasikan tingkat kerusakan uang Rupiah (ringan, sedang, berat) berdasarkan Macro F1-Score dan Accuracy pada dataset citra berlabel ahli BI dengan α = 0,05?
Metrik Utama      : Macro F1-Score

Tabel Hasil:
| Skenario                 | Metrik 1 (Accuracy%)  | Metrik 2 (Macro F1-Score (%)) |  n |
|--------------------------|-----------------------|-------------------------------|----|
| A	MobileNetV2 Fine-tune  |     86.89 ± 1.23      |         85.12 ± 1.45          | 10 |
| C	MobileNetV2 Frozen     |     75.62 ± 0.87      |         74.31 ± 0.92          | 9* |
| B	Baseline CNN 2-Layer   |     71.84 ± 0.92      |         70.67 ± 0.98          | 10 |



Visualisasi yang Direncanakan:
| # | Jenis Grafik | Pesan Utama | Metrik |
|---|-------------|-------------|--------|
| 1 |Bar Chart + Error Bar|MobileNetV2 Fine-tune (A) secara signifikan lebih unggul dari Baseline CNN (B) pada Macro F1-Score dengan selisih 14.45% (p=0.0012, d=2.34)|Mean Macro F1-Score ± std|
| 2 |Box Plot|Distribusi Macro F1-Score per model; A-5 (71.23%) adalah outlier yang terlihat jelas; A tetap unggul meskipun ada outlier|Semua run Macro F1-Score (n=10, 10, 9)|
| 3 |Learning Curve (Line Chart)|MobileNetV2 Fine-tune (A) konvergen lebih cepat (epoch 15-20) dan loss lebih rendah dibanding Baseline CNN (B)|Training & validation loss per epoch|
| 4 |Confusion Matrix Heatmap	|Model A memiliki error terendah di semua kelas; performa terbaik pada kelas "Ringan"|Confusion matrix rata-rata (10 run)|

Bias Check:
  [Y] Y-axis mulai dari 0 (atau dijustifikasi)
  [Y] Error bar/CI ditampilkan
  [Y] Semua data disertakan (tidak cherry-picked)
  [Y] Tidak menggunakan 3D tanpa alasan
```

---

## Latihan 1 — Tabel Hasil

Buat tabel hasil eksperimen Anda (boleh dengan data simulasi jika belum punya data riil).

| Skenario | Metrik 1 (mean ± std) | Metrik 2 (mean ± std) | n |
|----------|----------------------|----------------------|---|
| *Contoh: BERT-base* | *88.4 ± 1.2%* | *45.2 ± 3.1 min* | *10* |
| | | | |
| | | | |

**Checklist tabel:**
- [ ] Self-contained (judul jelas, satuan ada, N tercantum)
- [ ] Mean ± std (bukan single number)
- [ ] Diurutkan berdasarkan metrik utama
- [ ] Format konsisten di semua baris

---

## Latihan 2 — Rencana Visualisasi

Rencanakan 2-3 grafik untuk menyajikan data dari Latihan 1. Setiap grafik = satu pesan.

| # | Jenis Grafik | Pesan | Data yang Digunakan |
|---|-------------|-------|---------------------|
| 1 | *Contoh: Bar chart + error bar* | *Perbandingan accuracy antar 3 model* | *Mean accuracy ± std* |
| 2 | *Box plot* | *Distribusi F1 per model* | *Semua run F1* |
| 3 | *Scatter plot* | *Trade-off accuracy vs training time* | *Mean accuracy vs mean time* |

---

## Latihan 3 — Bias Detection

Evaluasi visualisasi berikut untuk bias (skenario dari contoh):

**Skenario:** Metode A = 91.2%, Metode B = 90.8%. Bar chart dengan Y-axis mulai dari 90%.

| Pertanyaan | Jawaban |
|-----------|---------|
| Apakah Y-axis menyesatkan? | *Contoh: Ya — A terlihat 2× B padahal beda 0.4%* |
| Apakah error bar ditampilkan? | |
| Apakah semua kondisi ditampilkan? | |
| Apa solusinya? | |

**Evaluasi grafik Anda sendiri dari Latihan 2:**
- [ ] Semua bias check lulus
- [ ] Ada yang perlu diperbaiki: ____

---

## Refleksi

> Mengapa tabel dan grafik keduanya diperlukan — tidak cukup salah satu saja? Pernahkah Anda membuat grafik yang (tanpa sengaja) menyesatkan?

> "Tabel dan grafik bukan alat yang bersaing, tapi alat yang saling melengkapi. Satu memberikan presisi, yang lain memberikan pola. Peneliti yang baik menggunakan keduanya untuk menceritakan kisah data yang lengkap dan jujur."
> Belum pernah
