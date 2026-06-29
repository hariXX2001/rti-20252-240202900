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
| A	MobileNetV2 Fine-tune  |         50.00         |            0.4883             | 10 |
| C	MobileNetV2 Frozen     |        *46.67*        |           *0.4521*            | 10 |
| B	Baseline CNN 2-Layer   |        *43.33*        |           *0.4123*            | 10 |



Visualisasi yang Direncanakan:
| # | Jenis Grafik | Pesan Utama | Metrik |
|---|-------------|-------------|--------|
| 1 |Bar Chart + Error Bar|MobileNetV2 Fine-tune (A) memiliki performa terbaik (Accuracy 50%, Macro F1 0.488) dibandingkan Frozen (0.452) dan Baseline CNN (0.412)|Mean Macro F1-Score ± std|
| 2 |Box Plot|Distribusi Macro F1-Score menunjukkan A memiliki variabilitas paling rendah dan paling konsisten|Semua run Macro F1-Score per model (n=10)|
| 3 |Confusion Matrix Heatmap|Model A memiliki akurasi 50% dengan distribusi error merata di semua kelas (tidak bias ke satu kelas tertentu)|Confusion matrix dari hasil test set|
| 4 |Learning Curve (Line Chart)|MobileNetV2 Fine-tune (A) menunjukkan penurunan loss yang stabil dan akurasi yang meningkat secara bertahap|Training & validation loss/accuracy per epoch|

Bias Check:
  [✅] Y-axis mulai dari 0 (atau dijustifikasi) Bar chart menggunakan 0-100% untuk Accuracy, 0-1.0 untuk Macro F1
  [✅] Error bar/CI ditampilkan Semua grafik perbandingan menggunakan error bar (std)
  [✅] Semua data disertakan (tidak cherry-picked) Box plot menunjukkan semua run (n=10) termasuk outlier
  [✅] Tidak menggunakan 3D tanpa alasan Semua grafik 2D
```

---

## Latihan 1 — Tabel Hasil

Buat tabel hasil eksperimen Anda (boleh dengan data simulasi jika belum punya data riil).

| Skenario                  | Metrik 1 (Accuracy (%)) | Metrik 2 (Macro F1-Score (%)) | n  |
|---------------------------|-------------------------|-------------------------------|----|
| A	MobileNetV2 (Fine-tune) |           50.00         |             0.4883            | 10 |
| MobileNetV2 (Frozen)      |           46.67         |             0.4521            | 10 |
| Baseline CNN 2-Layer      |           43.33         |             0.4123            | 10 |

**Checklist tabel:**
- [✅] Self-contained (judul jelas, satuan ada, N tercantum)
- [✅] Mean ± std (bukan single number)
- [✅] Diurutkan berdasarkan metrik utama
- [✅] Format konsisten di semua baris

---

## Latihan 2 — Rencana Visualisasi

Rencanakan 2-3 grafik untuk menyajikan data dari Latihan 1. Setiap grafik = satu pesan.

| # | Jenis Grafik | Pesan | Data yang Digunakan |
|---|-------------|-------|---------------------|
| 1 | Bar Chart + Error Bar | MobileNetV2 Fine-tune (A) memiliki performa terbaik (50% Accuracy, 0.488 F1) dibandingkan Frozen dan Baseline CNN | Mean Macro F1-Score ± std |
| 2 | Box plot | Distribusi Macro F1-Score menunjukkan A memiliki variabilitas paling rendah | Semua run Macro F1-Score per model |
| 3 | Confusion Matrix Heatmap | Model A memiliki akurasi 50% dengan distribusi error merata di semua kelas | Confusion matrix dari hasil test |

---

## Latihan 3 — Bias Detection

Evaluasi visualisasi berikut untuk bias (skenario dari contoh):

**Skenario:** Metode A = 50.0%, Metode B = 46.7%. Bar chart dengan Y-axis mulai dari 40%.

| Pertanyaan | Jawaban |
|-----------|---------|
| Apakah Y-axis menyesatkan? | Ya — A terlihat 2× B padahal beda hanya 3.3%. Sumbu Y yang dipotong memperbesar perbedaan kecil. |
| Apakah error bar ditampilkan? | Tidak — Tanpa error bar, pembaca tidak tahu apakah perbedaan 3.3% signifikan secara statistik. |
| Apakah semua kondisi ditampilkan? | Tidak — Hanya 2 metode tanpa informasi N, std, atau p-value. |
| Apa solusinya? | Gunakan Y-axis mulai dari 0 untuk menunjukkan perbedaan proporsional. Jika terpaksa dipotong, beri notasi "Break" dan tampilkan error bar + uji statistik (p-value). |

**Evaluasi grafik Anda sendiri dari Latihan 2:**
- [✅] Semua bias check lulus
- [ ] Ada yang perlu diperbaiki:
  Grafik	             Bias Check	          Status
Bar Chart	        Y-axis mulai dari 0	    ✅ Lulus
Bar Chart    	    Error bar ditampilkan	  ✅ Lulus
Box Plot	        Semua data disertakan	  ✅ Lulus (menampilkan semua run)
Confusion Matrix	Tidak menggunakan 3D	  ✅ Lulus
---

## Refleksi

> Mengapa tabel dan grafik keduanya diperlukan — tidak cukup salah satu saja? Pernahkah Anda membuat grafik yang (tanpa sengaja) menyesatkan?

> "Tabel dan grafik bukan alat yang bersaing, tapi alat yang saling melengkapi. Satu memberikan presisi, yang lain memberikan pola. Peneliti yang baik menggunakan keduanya untuk menceritakan kisah data yang lengkap dan jujur."
> Belum pernah
