# WS-08: Proposal Integration (UTS)

> **Bab 8 — Proposal & Checkpoint**

---

## Ringkasan Materi

### Proposal = Satu Argumen Utuh

Proposal riset bukan kumpulan bab yang independen. Ia adalah **satu argumen** yang mengalir dari masalah ke rencana solusi. Jika satu koneksi putus, seluruh proposal kehilangan koherensi.

### Integration Map — 6 Koneksi Kritis

```
Problem (Bab 2) → Gap (Bab 3) → RQ & H (Bab 4) → Metrik (Bab 5) → Sistem (Bab 6) → Eksperimen (Bab 7)
```

| Koneksi | Pertanyaan Verifikasi |
|---------|----------------------|
| Problem → Gap | Apakah gap muncul dari analisis literatur terhadap masalah? |
| Gap → RQ | Apakah RQ langsung menjawab gap yang teridentifikasi? |
| RQ → Metrik | Apakah setiap variabel di RQ punya metrik terdefinisi? |
| Metrik → Sistem | Apakah setiap metrik bisa diukur oleh komponen sistem? |
| Sistem → Eksperimen | Apakah desain eksperimen menggunakan sistem sebagai instrumen? |

### Koherensi Vertikal + Horizontal

- **Vertikal** — Alur logis atas-ke-bawah (problem → experiment). Setiap section menjawab pertanyaan yang diangkat section sebelumnya dan memunculkan pertanyaan baru.
- **Horizontal** — Konsistensi terminologi (nama variabel di RQ = di hipotesis = di metrik = di desain)

**Operasionalisasi Red Thread** (benang merah):
```
Bab 2 (Problem) → | memperkenalkan masalah X + evidensi |
                          ↓ menimbulkan pertanyaan: "apa akar gap-nya?"
Bab 3 (Gap)     → | menjawab pertanyaan tadi + membuka "lalu apa yang perlu diteliti?" |
                          ↓
Bab 4 (RQ/H)    → | menjawab gap dengan pertanyaan spesifik + prediksi terukur |
                          ↓
Bab 5-7 (Method)→ | menjawab RQ melalui desain eksperimen yang tepat |
```
Jika ada lompatan (section B tidak menjawab pertanyaan section A), red thread putus.

### Jebakan Kognitif

| Jebakan | Deskripsi |
|---------|----------|
| "Selling" Introduction | Menulis promosi, bukan menyajikan data dan gap |
| Copy-paste Methodology | Menyalin deskripsi tekstbook tanpa menyesuaikan ke RQ |
| Optimistic Timeline | Meremehkan waktu implementasi; selalu tambah buffer 30-50% |
| No Possibility of Failure | Mengimplikasikan hasil pasti sukses — proposal jujur mengakui H₀ mungkin tidak ditolak |

### Struktur Proposal

1. **Pendahuluan** — Latar belakang + problem statement (Bab 1-2)
2. **Tinjauan Pustaka** — Literature review + gap + baseline (Bab 3)
3. **RQ / Kontribusi / Hipotesis** — (Bab 4)
4. **Metodologi** — Metrik + sistem + desain eksperimen (Bab 5-7)
5. **Timeline & Output**

### Istilah Penting

- **Integration Map** — Diagram 6 koneksi kritis antar komponen proposal
- **Vertical Coherence** — Alur logis atas-ke-bawah
- **Horizontal Coherence** — Konsistensi terminologi di semua bagian
- **Checkpoint** — Titik self-assessment sebelum transisi dari desain ke eksekusi

---

## Template A.8 — Integration Checklist

```
PROPOSAL INTEGRATION CHECKLIST

Koneksi Vertikal (Flow Atas-Bawah):
  [✅] Problem → Gap: masalah terdokumentasi di literatur
  [✅] Gap → RQ: pertanyaan menjawab gap spesifik
  [✅] RQ → Hypothesis: hipotesis memprediksi jawaban
  [✅] Hypothesis → Metric: metrik mengukur variabel dalam hipotesis
  [✅] Metric → System: komponen sistem menghasilkan/mengukur metrik
  [✅] System → Experiment: desain eksperimen menggunakan sistem

Koneksi Horizontal (Konsistensi):
  [✅] Istilah sama di semua bagian
  [✅] Variabel di RQ = variabel di hipotesis = metrik di desain
  [✅] Scope tidak berubah dari masalah ke eksperimen

Cognitive Trap Checklist:
  [✅] Tidak ada paragraf "promosi" di pendahuluan (hanya data & gap)
  [✅] Metodologi disesuaikan ke RQ, bukan copy-paste textbook
  [✅] Timeline sudah ditambah buffer 30-50% dari estimasi awal
  [✅] Proposal mengakui kemungkinan H0 tidak ditolak (honest uncertainty)
  [✅] Tidak ada klaim "pasti berhasil" atau "meningkatkan signifikan"

Rubrik Self-Assessment:
| Kriteria     | 1 (Lemah)                                        | 2 (Cukup)                                     | 3 (Baik)                                           | Skor |
|------------- |--------------------------------------------------|-----------------------------------------------|----------------------------------------------------|------|
| Koherensi    | >2 koneksi vertikal terputus                     | 1-2 koneksi lemah, argumen masih bisa diikuti | Semua 6 koneksi terhubung, red thread jelas        | 3 |
| Specificity  | Variabel/metrik masih abstrak, tidak ada angka   | Sebagian metrik terdefinisi numerik           | Semua metrik + threshold + unit pengukuran jelas   | 3 |
| Feasibility  | Timeline >6 bulan tanpa memperhitungkan sumber   | Timeline 3-6 bulan dengan asumsi tertentu     | Timeline 1-3 bulan realistis dengan rencana detail  | 2 |
| Rigor        | Baseline tidak jelas atau straw man              | 1-2 baseline dengan justifikasi partial       | 2+ baseline SOTA + justifikasi pemilihan lengkap   | 3 |
```

---

## Latihan 1 — Kompilasi Proposal Mini

Kumpulkan hasil dari WS-02 sampai WS-07 menjadi satu ringkasan proposal.

| Komponen | Sumber | Isi (1-2 kalimat) |
|----------|--------|-------------------|
| Problem Statement | WS-02 | Proses identifikasi kondisi uang kertas Rupiah di layanan kas keliling Bank Indonesia masih dilakukan secara manual oleh petugas. Hal ini menyebabkan proses menjadi lambat, subjektif, dan rentan terhadap human error, terutama pada volume uang yang tinggi. |
| Gap | WS-03 | Mayoritas penelitian hanya melakukan klasifikasi biner (baik/rusak) menggunakan dataset kecil dengan kondisi ideal. Sangat sedikit yang melakukan klasifikasi granular (ringan, sedang, berat) pada uang Rupiah menggunakan model efisien mobile seperti MobileNetV2 serta pengujian robustness di kondisi lapangan. |
| RQ | WS-04 | Seberapa baik MobileNetV2 dibandingkan dengan baseline CNN 2-Layer dalam mengklasifikasikan tingkat kerusakan uang Rupiah (ringan, sedang, berat) berdasarkan Accuracy dan Macro F1-Score pada dataset citra berlabel ahli BI? |
| Hipotesis | WS-04 | H₁: MobileNetV2 menghasilkan Accuracy dan Macro F1-Score yang secara signifikan lebih tinggi daripada baseline CNN 2-Layer (α = 0.05). |
| Variabel & Metrik | WS-05 | Independent Variable (IV) = Arsitektur model (MobileNetV2 vs Baseline CNN 2-Layer). Dependent Variable (DV) = Performa klasifikasi dengan metrik utama Macro F1-Score, serta secondary metrics: Accuracy, MCC, dan Balanced Accuracy. |
| Sistem | WS-06 | Sistem klasifikasi berbasis PyTorch dengan arsitektur modular (model factory pattern + config.yaml). Mendukung swapping antara MobileNetV2 dan Baseline CNN 2-Layer, preprocessing (resize & normalization), data augmentation (rotation, brightness, contrast), training loop dengan early stopping, metrics logger, serta Grad-CAM++ untuk interpretabilitas. |
| Desain Eksperimen | WS-07 | Comparative study dengan 10 repeated runs (random seed berbeda). Menggunakan stratified split 70/15/15, hyperparameter dan augmentasi yang sama untuk kedua model, evaluasi menggunakan Confusion Matrix, Grad-CAM, dan uji statistik (Paired T-Test / Wilcoxon). |

---

## Latihan 2 — Integration Checklist

Verifikasi 6 koneksi kritis. Isi dengan merujuk tabel di Latihan 1.

| Koneksi | Status | Bukti |
|---------|--------|-------|
| Problem → Gap | ✅ | Gap muncul langsung dari limitasi proses manual di Problem Statement dan pola literatur yang mayoritas hanya klasifikasi biner serta dataset kecil. |
| Gap → RQ | ✅ | RQ langsung menjawab gap dengan membandingkan MobileNetV2 (model efisien) terhadap baseline CNN untuk klasifikasi 3 kelas granular pada uang Rupiah. |
| RQ → Hypothesis | ✅ | H₁ memberikan prediksi terukur (Accuracy & Macro F1 lebih tinggi) yang langsung menjawab pertanyaan RQ. |
| Hypothesis → Metric | ✅ | Macro F1-Score dan Accuracy sebagai primary metric langsung mengukur klaim hipotesis. |
| Metric → System | ✅ | Sistem menyediakan metrics logger, model factory, dan Grad-CAM++ yang mendukung semua metrik yang dibutuhkan. |
| System → Experiment | ✅ | Desain eksperimen menggunakan sistem sebagai instrumen utama (config-driven, repeated runs, fairness). |

**Koneksi mana yang paling lemah?** Metric → System (masih perlu dokumentasi kode yang lebih jelas).
**Bagaimana cara memperkuatnya?**
> Menambahkan diagram arsitektur sistem dan contoh file config.yaml di bagian metodologi proposal.

**Konsistensi horizontal — apakah istilah dan scope konsisten?** [✅] Ya / [ ] Tidak
> Jika tidak, di bagian mana terjadi inkonsistensi? 

---

## Latihan 3 — Rubrik Self-Assessment

Evaluasi proposal mini menggunakan rubrik.

| Kriteria | Skor (1-3) | Justifikasi |
|----------|-----------|-------------|
| Koherensi | Semua 6 koneksi vertikal terhubung dengan baik. Red thread dari masalah manual BI sampai eksperimen perbandingan MobileNetV2 vs Baseline CNN sangat jelas. | 3 |
| Specificity | Semua metrik (Macro F1-Score, Accuracy, MCC, Balanced Accuracy) sudah terdefinisi dengan jelas beserta skala dan penggunaannya. | 3 |
| Feasibility | Timeline 2-3 bulan cukup realistis untuk pengembangan model dan eksperimen, namun pengumpulan serta labeling dataset oleh ahli BI berpotensi memakan waktu lebih lama. | 2 |
| Rigor | Baseline jelas (Nandika et al. + CNN 2-Layer), repeated runs (10x), stratified split, fairness antar model, Grad-CAM, dan uji statistik. | 3 |

**Skor total:** 11 / 12

**Apakah proposal siap untuk fase eksekusi?** [✅] Ya / [ ] Belum
> Jika belum, apa yang perlu diperbaiki?
> 1. Mempercepat proses kerjasama dengan Bank Indonesia untuk dataset.
> 2. Menambahkan diagram arsitektur sistem dan contoh config.yaml.

---

## Refleksi

> Dari seluruh proses WS-01 sampai WS-08, bagian mana yang paling mudah dan paling sulit? Mengapa? Apa yang akan dilakukan berbeda jika mengulang dari awal?

**Bagian termudah:** Problem Statement dan Gap Analysis (WS-02 & WS-03). Karena isu di layanan kas keliling BI sangat nyata dan mudah ditemukan literaturnya.
**Bagian tersulit:** Merumuskan desain eksperimen yang fair dan rigorous (WS-07), terutama memastikan perbandingan antar model benar-benar apples-to-apples (hyperparameter, augmentasi, splitting, repeated runs).
**Yang akan dilakukan berbeda:**
> Mulai lebih awal mencari kerjasama dengan Bank Indonesia untuk dataset, karena labeling tingkat kerusakan sangat bergantung pada ahli.
> Lebih intensif mempelajari PyTorch modular design sejak awal agar bagian Sistem (WS-06) lebih kuat.
