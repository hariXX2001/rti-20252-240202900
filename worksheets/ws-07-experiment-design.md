# WS-07: Experimental Design & Validity

> **Bab 7 — Experimental Design & Validity**

---

## Ringkasan Materi

### Correlation ≠ Causality

Kausalitas membutuhkan 3 syarat:
1. **Covariance** — X dan Y bergerak bersama
2. **Temporal precedence** — X berubah sebelum Y
3. **Elimination of alternatives** — Tidak ada faktor lain yang menjelaskan Y

Controlled experiment adalah satu-satunya metode yang bisa membuktikan kausalitas.

### Empat Jenis Validitas

| Jenis | Pertanyaan | Ancaman Umum |
|-------|-----------|-------------|
| **Internal** | Apakah hubungan IV→DV nyata? | Confounding variable, selection bias |
| **External** | Apakah bisa digeneralisasi? | Dataset terlalu spesifik |
| **Construct** | Apakah mengukur konsep yang benar? | Metrik tidak sesuai |
| **Conclusion** | Apakah kesimpulan statistik valid? | Sample size kecil, uji salah |

Internal dan external validity sering berkonflik: semakin terkontrol (internal kuat) → semakin artificial (external lemah).

### Tiga Tipe Eksperimen dalam Riset TI

| Tipe | Deskripsi | Kapan Digunakan |
|------|----------|----------------|
| **Comparison Study** | Metode A vs B pada kondisi identik | Membandingkan pendekatan berbeda |
| **Ablation Study** | Full system → lepas komponen satu per satu | Mengukur kontribusi tiap komponen |
| **Parameter Study** | Variasikan satu parameter, amati dampak | Uji sensitifitas/robustness |

### Fairness dalam Perbandingan

Perbandingan yang adil = **kondisi identik** untuk semua metode: dataset sama, preprocessing sama, tuning effort sebanding, environment sama, metrik sama.

Contoh tidak adil: Transformer (30 fitur tambahan + Bayesian optimization) vs RF (default params) → hasilnya misleading.

### Threats to Validity = Diidentifikasi Sebelum Eksperimen

Ancaman validitas harus diidentifikasi **sebelum** eksperimen dan mitigasinya dirancang sebagai bagian dari desain — bukan ditulis sebagai boilerplate setelah selesai.

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan testing | Memastikan sistem memenuhi requirement | Membuktikan hubungan kausal antar variabel |
| Baseline | Versi sebelumnya (last release) | Metode tervalidasi dari literatur |
| Kegagalan | Bug → fix → release | H₀ tidak ditolak → tetap kontribusi ilmiah |
| Sukses | 100% test pass | Evidence valid — mendukung atau menolak hipotesis |

### Istilah Penting

- **Causality** — Hubungan sebab-akibat (covariance + temporal + elimination)
- **Controlled Experiment** — Ubah satu variabel, kontrol sisanya, amati efek
- **Fairness** — Semua metode diuji pada kondisi yang benar-benar identik
- **Threats to Validity** — Faktor yang bisa melemahkan kesimpulan jika tidak dimitigasi
- **Conclusion Validity** — Validitas statistik: power, sample size, uji yang tepat

---

## Template A.7 — Desain Eksperimen Lengkap

```
EXPERIMENT DESIGN

Research Question : Apakah MobileNetV2 memberikan performa klasifikasi yang lebih baik dibandingkan baseline CNN untuk mendeteksi rupiah kertas yang rusak?
Hypothesis        : MobileNetV2 akan menghasilkan Macro F1-Score yang secara signifikan lebih tinggi dibandingkan Baseline CNN pada dataset gambar rupiah kertas rusak.
Tipe Eksperimen   : [✅] Comparison  [ ] Ablation  [ ] Parameter

Kondisi Eksperimen:
| Kondisi | Deskripsi | IV Value | CV Settings |
|---------|-----------|----------|-------------|
| Control |Baseline CNN (custom CNN)|Baseline CNN|Dataset sama, stratified split (70/15/15), random seed 42, preprocessing & augmentasi identik, optimizer Adam, lr=0.001, batch_size=32, epochs=50|
| Treatment |MobileNetV2|MobileNetV2|Sama persis dengan Control|

Fairness Checklist:
  [✅] Train, validation, dan test set yang sama persis (stratified split
  [✅] Resize ke 224×224, normalization, augmentasi (rotation, brightness, contrast) sama untuk kedua model
  [✅] Hyperparameter yang sama (lr, batch_size, epochs). MobileNetV2 pakai pretrained weights, Baseline CNN di-train from scratch
  [✅] Python, PyTorch version, GPU device, dan random seed yang sama
  [✅] Macro F1-Score (primary), MCC, Balanced Accuracy, Precision, Recall

Threat Analysis:
| Threat Type | Ancaman Spesifik | Mitigasi |
|-------------|-----------------|----------|
| Internal    |        Data leakage antar train-test split         |     Stratified split + fixed random seed + tidak ada overlap     |
| External    |        Dataset sangat spesifik (hanya rupiah Indonesia)        |     Dokumentasikan karakteristik dataset dengan jelas dan sarankan pengujian di domain lain     |
| Construct   |        Metrik tidak mencerminkan kegunaan di dunia nyata         |     Tambahkan Grad-CAM visualization dan evaluasi visual oleh manusia     |
| Conclusion  |          Hasil tidak stabil karena single run       |     Jalankan multiple runs (minimal 5–10 run) dengan random seed berbeda     |

Statistical Plan:
  Uji statistik   : Independent Samples T-Test (atau Wilcoxon Rank-Sum Test jika distribusi tidak normal)
  Justifikasi      : Eksperimen akan dijalankan sebanyak 10 runs dengan random seed berbeda untuk mendapatkan distribusi performa. Uji ini digunakan untuk membandingkan rata-rata performa (terutama Macro F1-Score) antara kedua model secara statistik.
  Alpha            : 0.05
  Effect size min  : Cohen’s d ≥ 0.5 (medium effect)
```

---

## Latihan 1 — Desain Eksperimen

Susun desain eksperimen berdasarkan RQ, variabel, dan sistem dari WS-04 sampai WS-06.

**RQ:** Apakah MobileNetV2 memberikan performa klasifikasi yang lebih baik dibandingkan baseline CNN untuk mendeteksi rupiah kertas yang rusak?
**Tipe eksperimen:** [✅] Comparison / [ ] Ablation / [ ] Parameter

| Kondisi | Deskripsi | IV Value | CV Settings |
|---------|-----------|----------|-------------|
| Control | Baseline CNN | Baseline CNN | Dataset sama, stratified split (70/15/15), seed 42, preprocessing & augmentasi identik, Adam optimizer, lr=0.001, batch_size=32, epochs=50 |
| Treatment | MobileNetV2 | MobileNetV2 | Sama persis dengan Control |

---

## Latihan 2 — Fairness Checklist

Evaluasi apakah desain eksperimen di Latihan 1 sudah fair.

| Kriteria | Status | Detail |
|----------|--------|--------|
| Dataset identik | ✅ | Train/val/test split yang sama persis (stratified) |
| Preprocessing setara | ✅ | Resize 224×224, normalization, augmentasi sama |
| Tuning effort setara | ✅ | Hyperparameter sama untuk kedua model |
| Environment identik | ✅ | Python, PyTorch, GPU, dan random seed yang sama |
| Metrik evaluasi sama | ✅ | Macro F1-Score (primary), MCC, Balanced Accuracy |

**Ada yang tidak fair?** [ ] Ya / [✅] Tidak
> Jika ya, bagaimana cara memperbaikinya? ________________

---

## Latihan 3 — Threat Analysis

Identifikasi ancaman validitas untuk desain eksperimen ini.

| Threat Type | Ancaman Spesifik | Mitigasi |
|-------------|-----------------|----------|
| Internal | Ketidakseimbangan kelas | Macro F1-Score & MCC sebagai metrik utama |
| External | Dataset terlalu spesifik (rupiah Indonesia) | Dokumentasi dataset lengkap + saran generalisasi |
| Construct | Metrik tidak mencerminkan real-world usage | Tambahkan Grad-CAM visualization |
| Conclusion | Hasil tidak stabil (single run) | 10× repeated runs dengan random seed berbeda |

**Ancaman mana yang paling sulit dimitigasi?** External Validity
**Mengapa?**
> Dataset gambar rupiah kertas rusak sangat spesifik terhadap domain mata uang Indonesia, sehingga sulit digeneralisasi ke mata uang lain atau kondisi kerusakan yang berbeda.
---

## Refleksi

> Sebuah paper melaporkan "metode kami mengalahkan semua baseline." Apa 3 pertanyaan pertama yang harus diajukan untuk mengevaluasi klaim ini?

**Jawaban:**
1. Apakah perbandingan dilakukan secara fair? (Apakah baseline menggunakan dataset, preprocessing, tuning effort, environment, dan jumlah run yang sama persis?)
2. Seberapa kuat validitas internal eksperimennya? (Apakah confounding variables sudah dikontrol dengan baik?)
3. Apakah klaim didukung oleh uji statistik yang tepat beserta effect size dan multiple runs?
