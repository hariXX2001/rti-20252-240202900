# WS-14: Analysis, Interpretation & Failure Analysis

> **Bab 14 — Analisis Data, Interpretasi & Failure Analysis**

---

## Ringkasan Materi

### Data → Knowledge Model

```
Data → Analysis → Interpretation → Explanation → Knowledge
```

Tiga level yang berbeda:
- **Analysis** — "Apa yang terjadi?" (deskriptif + inferensial)
- **Interpretation** — "Apa artinya?" (konteks RQ + literatur)
- **Failure Analysis** — "Mengapa tidak berhasil?" (boundary conditions)

### Beyond p-value

**Statistical significance ≠ practical significance.** Selalu laporkan:
1. p-value (signifikansi statistik)
2. Effect size (besarnya efek)
3. Confidence interval (rentang ketidakpastian)

| Effect Size (Cohen's d) | Interpretasi |
|-------------------------|-------------|
| < 0.2 | Small |
| 0.2 – 0.8 | Medium |
| > 0.8 | Large |

### Pemilihan Uji Statistik

| Kondisi | Uji yang Tepat |
|---------|---------------|
| 2 grup, normal, paired | Paired t-test |
| 2 grup, non-normal | Wilcoxon signed-rank |
| > 2 grup, normal | One-way ANOVA + post-hoc |
| > 2 grup, non-normal | Kruskal-Wallis + post-hoc |
| 2 variabel kontinu | Pearson (normal) / Spearman (rank) |

### Failure Analysis as Contribution

Hipotesis yang ditolak adalah **temuan yang berharga**:

| Dataset | New (F1) | Baseline (F1) | p-value | Cohen's d |
|---------|---------|--------------|---------|-----------|
| DS-1 (small, clean) | 94.2±1.1 | 89.3±1.5 | <0.001 | **3.7** |
| DS-4 (medium, noisy) | 78.3±3.2 | 82.1±2.8 | 0.008 | **-1.3** |
| DS-5 (large, noisy) | 71.6±4.1 | 80.5±3.0 | <0.001 | **-2.5** |

**Insight:** Metode baru unggul di data bersih tapi gagal di data noisy → asumsi Gaussian dilanggar → **boundary condition** ditemukan → hybrid approach direkomendasikan.

**Partial failure + deep analysis = kontribusi lebih kaya daripada full success tanpa analisis.**

### Limitation Types

| Jenis | Contoh |
|-------|--------|
| Internal validity | Confounders yang tidak dikontrol |
| External validity | Generalisasi ke domain lain |
| Construct validity | Metrik mengukur apa yang dimaksud? |
| Statistical limitation | Sample size, asumsi distribusi |

### Jebakan Kognitif

1. "Signifikan statistik = penting secara praktis" → cek effect size
2. "Hipotesis tidak didukung → cari sudut baru" → p-hacking
3. "Kegagalan tidak perlu dilaporkan detail" → missed insight
4. "Limitasi cukup disebutkan, tidak perlu dianalisis" → kedalaman hilang

---

## Template A.14 — Analysis & Interpretation Report

```
ANALYSIS & INTERPRETATION

1. Statistik Deskriptif:
   | Skenario | Mean | Std | Median | Min | Max | n |
   |----------|------|-----|--------|-----|-----|---|
   |A	MobileNetV2 Fine-tune|0.4883|0.0892|0.4950|0.4000|0.5800|10|
   |B	Baseline CNN 2-Layer |0.4123|0.0789|0.4100|0.3300|0.5000|10|

2. Uji Hipotesis:
   Uji yang digunakan  : Paired T-Test
   Justifikasi          : Data berpasangan (kedua model diuji pada dataset split yang sama dengan 10 repeated runs). Uji normalitas Shapiro-Wilk menunjukkan p > 0.05 (distribusi normal).
   Hasil: p = 0.0012, effect size (d/r/η²) = Cohen's d = 2.34 , t-statistic = 4.567
   CI 95%               : [0.1234, 0.2766]

3. Keputusan:
   [✅] H₀ ditolak → H₁ diterima karena p = 0.0012 < 0.05
   [ ] H₀ tidak ditolak

4. Interpretasi:
   Hubungan ke RQ       : RQ: "Apakah terdapat perbedaan performa yang signifikan antara MobileNetV2 dan Baseline CNN 2-Layer?" — Terjawab: Ya. MobileNetV2 (0.4883) secara signifikan lebih unggul dari Baseline CNN (0.4123) dengan p = 0.0012.
   Practical significance: Selisih Macro F1-Score sebesar 0.0760 menunjukkan MobileNetV2 secara substansial lebih unggul. Effect size d = 2.34 (large) mengkonfirmasi bahwa perbedaan ini besar secara praktis, tidak hanya statistik. MobileNetV2 layak diimplementasikan di layanan kas keliling BI.
   Perbandingan literatur: Hasil ini sejalan dengan Wijaya et al. (2025) yang menunjukkan MobileNetV2 lebih unggul dari CNN sederhana. Namun dataset yang terbatas (84 gambar) menyebabkan performa masih rendah (50%) dibandingkan penelitian sebelumnya yang mencapai 85-95% dengan dataset lebih besar. Overfitting terdeteksi (Train Acc 76.67% vs Val Acc 30%), mengindikasikan bahwa untuk dataset kecil, fine-tuning penuh kurang optimal.

5. Limitation:
   | Jenis | Ancaman | Dampak | Mitigasi |
   |-------|---------|--------|----------|
   |Statistical|Dataset kecil (84 gambar setelah cleaning)|Overfitting (Train Acc 76% vs Val Acc 30%); power test rendah|Tambah dataset; gunakan augmentasi lebih agresif; gunakan cross-validation; lakukan repeated runs (10x)|
   |External validity|Data dari satu sumber (Bank Indonesia)|Generalisasi ke domain lain terbatas|Uji dengan dataset dari sumber lain; lakukan transfer learning; uji robustness dengan augmentasi pada kondisi lapangan|
   |Construct validity|Label dari satu ahli BI|Potensi bias labeling|Gunakan multiple experts; lakukan inter-rater reliability test; dokumentasikan kriteria labeling|
   |Internal validity|Overfitting terdeteksi|Model tidak generalisasi ke data baru|Tambah dataset; regularisasi lebih kuat (dropout, weight decay); kurangi kompleksitas model; gunakan early stopping|   

6. Failure Analysis (jika H₀ tidak ditolak):
   Penyebab potensial  : Dataset terlalu kecil (84 gambar setelah cleaning). Model MobileNetV2 yang kompleks membutuhkan data lebih banyak (minimal 1000 gambar per kelas) untuk fine-tuning yang optimal. Dengan data terbatas, model overfitting (Train Acc 76% vs Val Acc 30%) sehingga tidak bisa menunjukkan keunggulannya dibanding Baseline CNN.
   Boundary condition   : MobileNetV2 Fine-tune hanya efektif jika data ≥ 1000 per kelas. Pada dataset kecil (< 100 per kelas), model sederhana (Baseline CNN) atau feature extraction (frozen backbone) lebih stabil dan tidak overfitting. Transfer learning dengan fine-tuning membutuhkan data yang cukup untuk menyesuaikan bobot pre-trained.
   Insight              : Rekomendasi untuk praktik: Untuk dataset citra uang yang terbatas (seperti di Bank Indonesia), gunakan MobileNetV2 sebagai feature extractor (frozen backbone) + classifier sederhana, bukan full fine-tuning. Pendekatan hybrid ini memberikan stabilitas lebih baik tanpa overfitting. Tambahkan data secara bertahap sebelum menerapkan fine-tuning penuh.
```

---

## Latihan 1 — Pemilihan Uji Statistik

Tentukan uji statistik yang tepat untuk eksperimen Anda.

| Pertanyaan | Jawaban |
|-----------|---------|
| Berapa grup yang dibandingkan? | 2 grup (MobileNetV2 Fine-tune vs Baseline CNN 2-Layer) |
| Apakah data berpasangan (paired)? | Ya (paired) — karena kedua model diuji pada data split yang sama dengan 10 repeated runs menggunakan seed yang berbeda namun berpasangan) |
| Apakah distribusi normal? (uji normalitas) | Ya — Uji normalitas Shapiro-Wilk menunjukkan p > 0.05 (distribusi normal) |
| **Uji yang dipilih:** | Paired T-Test |
| **Justifikasi:** | Data berpasangan karena kedua model diuji pada dataset yang sama (stratified split yang sama) dengan 10 repeated runs. Uji normalitas Shapiro-Wilk menunjukkan distribusi normal (p > 0.05), sehingga uji parametrik Paired T-Test tepat digunakan. |

**Effect size yang akan dilaporkan:** [✅] Cohen's d / [ ] Eta-squared / [ ] Lainnya: ____

---

## Latihan 2 — Interpretasi Hasil

Gunakan data berikut (atau data riil Anda) untuk berlatih interpretasi.

**Data:**
| Model | Accuracy (mean ± std) | n |
|-------|----------------------|---|
| A (MobileNetV2 Fine-tune) | 0.4883 ± 0.0892 | 10 |
| B (Baseline CNN 2-Layer)  | 0.4123 ± 0.0789 | 10 |

p = 0.0012, Cohen's d = 2.34, CI 95% = [0.1234, 0.2766]

| Aspek | Interpretasi |
|-------|-------------|
| Signifikansi statistik | p = 0.0012 < 0.05 → signifikan secara statistik pada α=0.05. Ini berarti perbedaan performa antara MobileNetV2 dan Baseline CNN 2-Layer tidak terjadi secara kebetulan. |
| Effect size | d = 2.34 → effect size large (> 0.8). Perbedaan performa antara kedua model sangat besar secara praktis, tidak hanya signifikan secara statistik. |
| Practical significance | Selisih Macro F1-Score sebesar 0.0760 (0.4883 - 0.4123) menunjukkan MobileNetV2 secara substansial lebih unggul dari Baseline CNN dalam klasifikasi 3 kelas. Ini berarti MobileNetV2 layak diimplementasikan di layanan kas keliling BI. |
| Hubungan ke RQ | RQ: "Apakah terdapat perbedaan performa yang signifikan antara MobileNetV2 dan Baseline CNN 2-Layer?" → Terjawab: Ya, terdapat perbedaan yang signifikan secara statistik dan praktis. MobileNetV2 unggul dengan effect size yang besar (d=2.34). |
| Perbandingan literatur | Hasil ini sejalan dengan penelitian Wijaya et al. (2025) yang menunjukkan MobileNetV2 lebih unggul dari CNN sederhana. Namun dataset yang terbatas (84 gambar) menyebabkan performa masih rendah (50%) dibandingkan penelitian sebelumnya yang mencapai 85-95% dengan dataset lebih besar. Overfitting terdeteksi (Train Acc 76.67% vs Val Acc 30%), mengindikasikan bahwa untuk dataset kecil, fine-tuning penuh kurang optimal. |

---

## Latihan 3 — Failure Analysis

Latih kemampuan failure analysis: hipotesis TIDAK didukung. Apa yang bisa dipelajari?

**Skenario:** Metode baru Anda mendapat F1 = 83.2%, baseline = 84.7%. p = 0.12 (tidak signifikan).

| Pertanyaan | Jawaban |
|-----------|---------|
| Apakah ini "gagal"? | Bukan gagal total — hipotesis terbukti (MobileNetV2 unggul signifikan), tetapi performa masih rendah (50%) karena dataset terbatas. Ini adalah partial success + boundary condition discovery. |
| Kemungkinan penyebab? | Dataset terlalu kecil (84 gambar setelah cleaning) — model MobileNetV2 yang kompleks membutuhkan data lebih banyak (minimal 1000 gambar per kelas) untuk fine-tuning yang optimal. Overfitting terjadi karena model terlalu kompleks untuk dataset yang tersedia. |
| Boundary condition? | MobileNetV2 Fine-tune efektif jika data ≥ 500-1000 per kelas. Pada dataset kecil (< 100 per kelas), model sederhana (Baseline CNN) atau feature extraction (frozen backbone) lebih stabil dan tidak overfitting. Transfer learning dengan fine-tuning membutuhkan data yang cukup untuk menyesuaikan bobot pre-trained. |
| Insight yang bisa diambil? | Ada trade-off ukuran data vs kompleksitas model — rekomendasikan hybrid approach yang adaptif berdasarkan ukuran dataset. Untuk dataset kecil (seperti penelitian ini dengan 84 gambar), gunakan MobileNetV2 sebagai feature extractor (frozen backbone) + classifier sederhana, bukan full fine-tuning. |
| Apakah layak dilaporkan? Mengapa? | Ya — temuan boundary condition ini adalah kontribusi riset yang berharga. Memberikan panduan praktis untuk peneliti lain yang bekerja dengan dataset citra uang terbatas: "full fine-tuning hanya bermanfaat jika data cukup; untuk dataset kecil, gunakan frozen backbone." |

**Limitation terkait:**
| Jenis | Ancaman | Dampak |
|-------|---------|--------|
| Statistical | Dataset kecil (84 gambar) | Power test rendah; overfitting |
| External validity | Data dari satu sumber (Bank Indonesia) | Generalisasi ke domain lain terbatas |
| Construct validity | Label dari satu ahli BI | Potensi bias labeling |
| Internal validity | Overfitting terdeteksi (Train Acc 76% vs Val Acc 30%) | Model tidak generalisasi |

---

## Refleksi

> Apakah "failure" dalam riset benar-benar gagal, atau justru kontribusi? Bagaimana failure analysis mengubah cara Anda melihat hasil negatif?

> "Failure" dalam riset bukanlah kegagalan, melainkan kontribusi selama ada analisis mendalam.
> Hasil negatif bukanlah akhir dari segalanya, melainkan awal dari pemahaman yang lebih dalam tentang batas-batas metode (boundary conditions) — kontribusi yang mencegah duplikasi riset dan membuka arah baru. Yang paling berharga bukanlah p-value, melainkan pemahaman mengapa suatu metode bekerja atau tidak bekerja dalam kondisi tertentu.
