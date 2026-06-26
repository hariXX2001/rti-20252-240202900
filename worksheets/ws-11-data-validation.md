# WS-11: Data Validation & Integrity

> **Bab 11 — Validasi Data & Integritas**

---

## Ringkasan Materi

### Data Trust Model

```
Raw Data → Data Cleaning → Consistency Check → Validation Process → Trusted Data
```

Data mentah belum bisa dipercaya. Harus melewati pipeline validasi sebelum siap untuk analisis statistik.

### Empat Pilar Data Quality

| Pilar | Deskripsi | Contoh Pelanggaran |
|-------|----------|-------------------|
| **Accuracy** | Nilai dalam range masuk akal | Akurasi = 1.5 (di luar [0,1]) |
| **Consistency** | Format seragam di semua run | Run 1: CSV, Run 2: JSON |
| **Completeness** | Tidak ada data hilang dari plan | 97 dari 100 run tercatat |
| **Validity** | Data sesuai desain eksperimen | Parameter baseline tercampur treatment |

### Proses Validasi Progresif

1. **Format validation** — Tipe file, header, kolom
2. **Range validation** — Nilai dalam batas logis
3. **Consistency validation** — Format seragam antar-run
4. **Logic validation** — Data cocok dengan desain eksperimen

Jika gagal di langkah awal → tidak perlu lanjut.

### Anomaly Detection — 3 Jenis

| Jenis | Deskripsi | Deteksi |
|-------|----------|---------|
| **Statistical outlier** | Nilai di luar distribusi normal | IQR: < Q1-1.5×IQR atau > Q3+1.5×IQR |
| **Contextual anomaly** | Normal absolut, abnormal dalam konteks | Run 1-10: ~91%, Run 11-20: ~88% |
| **Pattern anomaly** | Pola sistematis (bukan random) | Performa menurun berurutan |

**Prinsip:** Detect → Investigate → Document → Decide — **JANGAN langsung hapus.**

### Engineering vs Research Validation

| Aspek | Engineering | Research |
|-------|-----------|---------|
| Tujuan | Data sesuai spesifikasi bisnis | Data layak untuk analisis statistik |
| Missing data | Impute / set default | Investigasi penyebab → dokumentasi |
| Outlier | Bug → fix | Mungkin temuan → investigasi |
| Dokumentasi | Minimal (log error) | Komprehensif (anomali + keputusan) |

### Jebakan Kognitif

1. "Logging otomatis ≠ data benar" → bisa ada bug di logger
2. "Outlier = hapus" → bisa jadi temuan penting
3. "Dataset kecil tidak perlu validasi" → justru lebih rentan
4. "Mean normal = data benar" → [94, 95, 93, **44**, 94] → mean 84% terlihat wajar

---

## Template A.11 — Data Validation Checklist

```
DATA VALIDATION CHECKLIST

Completeness:
  [Y] Semua skenario tercakup
  [N] Jumlah run sesuai rencana
  [Y] Tidak ada file output hilang
  Missing: ____ dari ____ data points

Format Consistency:
  [Y] Semua file format sama (CSV/JSON/...)
  [Y] Header konsisten
  [Y] Tipe data konsisten (numerik tetap numerik)

Range & Logic:
  [Y] Nilai dalam range masuk akal
  [Y] Tidak ada waktu negatif
  [N] Metrik 0–100%, tidak di luar range
  Anomali ditemukan: ____________________

Cross-Validation:
  [N] Run identik → hasil mendekati
  [Y] Trend konsisten dengan ekspektasi teori

Keputusan:
  [N] Data siap analisis
  [Y] Perlu cleaning
  [Y] Perlu re-run (skenario: ____)
```

---

## Latihan 1 — Completeness Check

Verifikasi apakah semua data yang direncanakan sudah terkumpul.

| Skenario | Run Direncanakan | Run Tercatat | Missing | Alasan |
|----------|-----------------|-------------|---------|--------|
| A MobileNetV2 Fine-tune| 10  | 10 | 0 | Semua run berhasil |
| B	Baseline CNN 2-Layer | 10  | 10 | 0 | Semua run berhasil |
| C	MobileNetV2 Frozen   | 10  | 9  | 1 | Run dengan seed 606 gagal di epoch 12 karena OOM (VRAM overflow)|


**Total expected:** 30 | **Total actual:** 29 | **Missing:** 1

**Keputusan untuk data missing:**
> Run C-606 (MobileNetV2 Frozen, seed 606) mengalami kegagalan di epoch 12 karena VRAM overflow pada batch size 32.

Tindakan:
1. Dokumentasi anomali di anomaly_log.md
2. Re-run dengan batch size 24 (lebih kecil) menggunakan seed 606 yang sama
3. Catat perubahan parameter di log
4. Jika re-run berhasil, data dari re-run digunakan untuk analisis
5. Jika tetap gagal, dokumentasikan sebagai DNF (Did Not Finish) dan lakukan analisis dengan 9 run (dengan catatan keterbatasan)

---

## Latihan 2 — Anomaly Investigation

Periksa data Anda untuk anomali. Gunakan metode IQR atau z-score.

Dataset Sampel (Macro F1-Score untuk MobileNetV2 Fine-tune)

Run  	Seed	Macro F1-Score (%)
A-1	  42	    87.12
A-2	  101	    86.89
A-3  	202	    87.34
A-4	  303	    86.77
A-5	  404	    71.23
A-6	  505	    87.01
A-7	  606	    86.45
A-8	  707	    87.21
A-9	  808	    86.92
A-10	909	    87.45

**Deteksi outlier:**
Langkah 1: Urutkan data (ascending)
71.23, 86.45, 86.77, 86.89, 86.92, 87.01, 87.12, 87.21, 87.34, 87.45

Langkah 2: Tentukan kuartil
Q1 (persentil ke-25) = 86.77 (data ke-3)
Q3 (persentil ke-75) = 87.21 (data ke-8)
IQR = Q3 - Q1 = 87.21 - 86.77 = 0.44

Langkah 3: Tentukan batas outlier
Batas bawah = Q1 - 1.5 × IQR = 86.77 - 0.66 = 86.11
Batas atas = Q3 + 1.5 × IQR = 87.21 + 0.66 = 87.87

Langkah 4: Identifikasi outlier
Nilai di bawah 86.11: 71.23 ✅ OUTLIER
Nilai di atas 87.87: Tidak ada

**Investigasi (untuk setiap outlier):**

| Outlier | Nilai | Kemungkinan Penyebab  |                                                        Keputusan              |
|---------|-------|-----------------------|-------------------------------------------------------------------------------|
| Run A-5 | 71.23 | 1. Thermal throttling pada run ke-5 setelah 4 run berturut-turut
                    2. Ada background process (Windows Update) yang berjalan saat training
                    3. Data augmentation menghasilkan sample yang tidak representatif
                    4. Model tidak konvergen (loss NaN?)                                           TIDAK DIHAPUS
                                                  1. Investigasi log: cek loss curve, GPU temperature, background process
                                                  2. Dokumentasi di anomaly_log.md
                                                  3. Jika ditemukan bug → fix dan re-run
                                                  4. Jika hanya random outlier → tetap sertakan dalam analisis (menunjukkan variabilitas model)
                                                  5. Sertakan di boxplot sebagai outlier point (beri tanda berbeda) |

Anomali Lain yang Terdeteksi
Skenario	      Seed	      Metrik	    Nilai	                    Penyebab	                            Keputusan
B (Baseline)	  202	       Accuracy	    98.21%	  Diduga data leakage: training set tercampur test set	INVESTIGASI
Cek stratified split; jika benar leakage → re-run
Jika tidak → dokumentasikan sebagai outlier

C (Frozen)	    707	        Waktu	      15.3 jam	15× lebih lama dari rata-rata (1 jam)                 INVESTIGASI
Cek resource usage saat run; kemungkinan ada background process
---

## Latihan 3 — Validation Report

Buat laporan validasi ringkas untuk dataset eksperimen Anda.

**1. Completeness:** 96,67% data terkumpul
**2. Format:** [Y] Konsisten / [ ] Ada inkonsistensi: ____
**3. Range check (anomali):** Run A-5 (seed 404): Macro F1-Score = 71.23% (outlier)
                              Run B-202 (seed 202): Accuracy = 98.21% (potensi data leakage)
                              Run C-606 (seed 606): Missing (OOM)
**4. Logic check:** [Y] Parameter sesuai plan / [ ] Ada ketidaksesuaian: ____

**Kesimpulan:** [Y] Data siap analisis / [ ] Perlu tindakan: 
1. Re-run C-606 (MobileNetV2 Frozen, seed 606) dengan batch size 24
2. Investigasi B-202 (Baseline CNN, seed 202) untuk potensi data leakage
3. Investigasi A-5 (MobileNetV2 Fine-tune, seed 404) untuk outlier
4. Dokumentasikan semua temuan di anomaly_log.md

---

## Refleksi

> Apa perbedaan antara "data yang benar" dan "data yang dipercaya"? Mengapa proses validasi formal diperlukan meskipun data dikumpulkan secara otomatis?

> Data yang benar adalah data yang secara teknis memiliki format, tipe, dan nilai yang sesuai dengan spesifikasi. Misalnya, metrik Accuracy berada di range [0,1], header CSV lengkap, dan semua file dapat dibuka. Data yang benar bersifat teknis dan sintaksis.
> Data yang dipercaya adalah data yang telah melalui proses verifikasi menyeluruh — tidak hanya formatnya benar, tetapi juga proses pengumpulannya terkontrol, tidak ada missing data yang tidak terjelaskan, outlier telah diinvestigasi, dan tidak ada bias sistematis. Data yang dipercaya bersifat substantif dan semantik.
