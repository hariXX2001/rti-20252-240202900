# WS-05: Variabel & Metrik

> **Bab 5 — Metric, Measurement & Data**

---

## Ringkasan Materi

### Measurement Alignment Model

Setiap pengukuran yang valid harus bisa ditelusuri melalui rantai ini tanpa lompatan logis:

```
Problem → Concept → Variable → Metric → Data → Result
```

### Operationalization = Keputusan Desain

Menerjemahkan konsep abstrak menjadi variabel terukur bukan proses mekanis. "Code quality" yang diukur via SonarQube code smells membawa asumsi implisit. Setiap operasionalisasi harus didokumentasikan dan dijustifikasi.

### Empat Tipe Data (NOIR)

| Tipe | Ciri | Contoh | Operasi Valid |
|------|------|--------|---------------|
| **Nominal** | Kategori, tanpa urutan | Jenis algoritma (RF, SVM, CNN) | Modus, chi-square |
| **Ordinal** | Urutan, interval tidak sama | Skala Likert (1-5) | Median, Spearman |
| **Interval** | Jarak bermakna, tanpa nol absolut | Suhu Celsius | Mean, Pearson, t-test |
| **Ratio** | Jarak bermakna + nol absolut | Waktu eksekusi (ms) | Semua operasi |

Tipe data menentukan uji statistik yang valid. Kebanyakan metrik performa TI = ratio; persepsi pengguna = ordinal.

### Kriteria Pemilihan Metrik

- **Representative** — Mewakili konsep yang diteliti
- **Sensitive** — Cukup peka menangkap perbedaan bermakna (hindari ceiling effect)
- **Feasible** — Bisa dikumpulkan dalam batasan waktu dan biaya

### Pre-registration

Metrik harus ditentukan **sebelum** eksperimen. Memilih metrik setelah melihat data = **p-hacking**. Metrik tambahan yang ditemukan kemudian dilaporkan sebagai *exploratory*, bukan *confirmatory*.

### Primary vs Secondary Metric

- **Primary Metric** — Langsung terikat ke hipotesis, menentukan kesimpulan
- **Secondary Metric** — Pendukung, dilaporkan di samping primary; statusnya suplementer

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Pemilihan metrik | Berdasarkan kebiasaan/tool yang ada | Berdasarkan construct validity |
| Anomali | Dihapus untuk laporan bersih | Diinvestigasi — bisa jadi temuan |
| Kapan dipilih | Setelah sistem jadi (monitoring) | Sebelum eksperimen (by design) |

### Istilah Penting

- **Operationalization** — Transformasi konsep abstrak menjadi variabel terukur
- **Construct Validity** — Sejauh mana pengukuran benar-benar mengukur konsep yang dimaksud
- **Measurement Scale** — Klasifikasi data (NOIR) yang menentukan analisis valid
- **Multi-metric Evaluation** — Menggunakan beberapa metrik untuk menangkap konsep kompleks

---

## Template A.5 — Definisi Variabel, Metrik & Justifikasi

```
VARIABLE & METRIC DEFINITION

Research Question: Seberapa baik MobileNetV2 dibandingkan dengan baseline CNN 2-Layer dalam mengklasifikasikan tingkat kerusakan uang Rupiah (ringan, sedang, berat) berdasarkan Accuracy dan Macro F1-Score pada dataset 1.000 citra berlabel ahli BI?

| Variabel | Tipe | Konsep | Metrik | Skala | Satuan | Cara Mengukur | Justifikasi |
|----------|------|--------|--------|-------|--------|---------------|-------------|
|    Arsitektur Model      | IV   |    Kemampuan representasi fitur citra    |    MobileNetV2 vs CNN 2-Layer    |   Nominal    |     -  |       Implementasi dan training model        |       Variabel utama perbandingan sesuai RQ      |
|     Performa Klasifikasi     | DV   |    Kemampuan membedakan 3 tingkat kerusakan    |     Macro F1-Score (Primary)
Matthews Correlation Coefficient (MCC) (Secondary)
Balanced Accuracy   |   Ratio    |     - / %   |        Perhitungan dari Confusion Matrix       |       Macro F1-Score dipilih karena imbalance antar kelas. MCC sebagai secondary karena sangat robust pada data tidak seimbang      |
|     Akurasi Keseluruhan     | CV   |   Performa global model    |    Accuracy    |    Ratio   |    %   |         Perhitungan dari Confusion Matrix      |       Secondary metric untuk interpretasi mudah      |
|  Performa per Kelas  |  DV  |  Kemampuan model pada masing-masing kelas  |  Precision, Recall, F1-Score per kelas  |  Ratio  |  %  |  Classification Report  |  Penting untuk menganalisis kelas minoritas (kerusakan ringan)  |
|  Jumlah Data per Kelas  |  CV  |  Keseimbangan dataset  |  Jumlah citra per kelas  |  Ratio  |  Gambar  |  Dokumentasi dataset  |  Mengontrol bias imbalance  |
|  Variasi Citra  |  CV  |  Realisme kondisi lapangan  |  Tingkat augmentasi (brightness, rotation, noise)  |  Ordinal  |  -  |  Parameter augmentasi  |  Mengontrol faktor eksternal  |

Alignment Check:
  RQ → Concept → Variable → Metric → Data → Result
  [ V ] Setiap langkah terdokumentasi
  [ v ] Tidak ada "lompatan logis"
  [ v ] Metrik mengukur apa yang dimaksud (construct validity)
```

---

## Latihan 1 — Operationalization Chain

Gunakan RQ dari WS-04. Definisikan variabel dan metriknya.

**RQ:**Seberapa baik MobileNetV2 dibandingkan dengan baseline CNN 2-Layer dalam mengklasifikasikan tingkat kerusakan uang Rupiah (ringan, sedang, berat) berdasarkan Accuracy dan Macro F1-Score pada dataset 1.000 citra berlabel ahli BI?

| Variabel | Tipe | Konsep Abstrak | Metrik Konkret | Skala (NOIR) | Satuan |
|----------|------|---------------|----------------|-------------|--------|
| Arsitektur Model | IV | Pendekatan ekstraksi fitur citra | MobileNetV2 vs CNN 2-Layer | Nominal | - |
| Performa Klasifikasi | DV | Keberhasilan deteksi tingkat kerusakan | Macro F1-Score, MCC, Balanced Accuracy | Ratio | - / % |
| Variasi Citra | CV | Kondisi gambar dunia nyata | Tingkat augmentasi & pencahayaan | Ordinal | - |

**Apakah ada lompatan logis dalam rantai?** [ ] Ya / [ x ] Tidak
> Jika ya, di mana? ____________________________________

---

## Latihan 2 — Evaluasi Metrik

Evaluasi metrik DV yang dipilih di Latihan 1 menggunakan 3 kriteria.

| Kriteria | Skor (1-5) | Justifikasi |
|----------|-----------|-------------|
| Representative | 5 | Memberikan bobot yang sama untuk setiap kelas (ringan, sedang, berat) |
| Sensitive | 4 | Sensitif terhadap performa kelas minoritas |
| Feasible | 5 | Mudah dihitung dengan library scikit-learn |

**Apakah perlu secondary metric?** [ X ] Ya / [ ] Tidak
> Jika ya, apa dan mengapa?Ya, Matthews Correlation Coefficient (MCC) dan Balanced Accuracy.
1. MCC sangat robust terhadap imbalance dan memberikan gambaran keseluruhan yang lebih baik.
2. Balanced Accuracy memberikan rata-rata recall per kelas.

**Contoh kasus ceiling effect untuk metrik ini:**
> Jika kedua model mencapai Macro F1-Score > 97%, maka sulit membedakan mana yang lebih unggul secara bermakna.

---

## Latihan 3 — Data Quality Check

Bayangkan data yang akan dikumpulkan dari eksperimen. Evaluasi 4 dimensi kualitas data.

| Dimensi | Pertanyaan | Jawaban | Strategi Mitigasi |
|---------|-----------|---------|------------------|
| Completeness | Apakah semua data point terkumpul? | Ya (setelah augmentasi) | Script validasi otomatis |
| Consistency | *Apakah ada kontradiksi internal?* | Rendah | Double labeling oleh ahli BI + Kappa test |
| Validity | *Apakah benar-benar mengukur yang dimaksud?* | Tinggi | Validasi visual dengan Grad-CAM |
| Representativeness | *Apakah sampel mewakili populasi target?* | Sedang-Tinggi | Augmentasi realistis + sampling dari sumber BI |

---

## Refleksi

> Mengapa memilih metrik setelah melihat data dianggap p-hacking? Apa bedanya dengan eksplorasi data yang sah?

**Jawaban:**
> Memilih metrik setelah melihat data dianggap p-hacking karena peneliti dapat memilih metrik yang secara tidak sadar membuat hasil eksperimen terlihat lebih baik atau signifikan, sehingga mengurangi objektivitas dan reliabilitas penelitian.
> Eksplorasi data yang sah dilakukan secara transparan, dilabeli sebagai exploratory, dan tidak digunakan untuk membuktikan hipotesis utama. Semua metrik yang digunakan untuk menguji hipotesis (confirmatory) harus ditentukan sebelum eksperimen dimulai.
