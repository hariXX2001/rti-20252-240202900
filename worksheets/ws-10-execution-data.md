# WS-10: Experiment Execution & Data Collection

> **Bab 10 — Eksekusi Eksperimen & Pengumpulan Data**

---

## Ringkasan Materi

### Experiment Execution Pipeline

```
Design → Execution Plan → Controlled Execution → Data Collection → Data Logging → Dataset for Analysis
```

### Multiple Run = Non-Negotiable

Single run **tidak pernah cukup** untuk klaim ilmiah. Minimum 5-10 run per skenario dengan seed berbeda. Multiple run menghasilkan:
- Mean, std, confidence interval
- Distribusi hasil → uji statistik
- Variabilitas → error bar di grafik

### Execution Plan

Setiap eksperimen harus memiliki plan sebelum eksekusi:
- Daftar skenario
- Jumlah run per skenario
- Random seed per run (pre-determined!)
- Urutan eksekusi (randomisasi/counterbalancing)
- Pre-execution checklist

### Data Logging Komprehensif

Setiap run menghasilkan log terstruktur:
1. **Identitas** — Run ID, timestamp, skenario
2. **Konfigurasi** — Semua parameter, seed, code version
3. **Hasil** — Semua metrik, output detail
4. **Metadata** — Waktu eksekusi, resource usage, warning/error

Format: CSV/JSON/database — **bukan stdout yang di-copy-paste**.

### Engineering vs Research Execution

| Aspek | Engineering | Research |
|-------|-----------|---------|
| Run | Sekali (deploy) | Multiple (min 5-10, seed berbeda) |
| Logging | Error log, access log | Semua parameter, metrik, metadata |
| Anomali | Bug → fix → redeploy | Investigasi → dokumentasi → analisis |
| Urutan | Tidak penting | Bisa bias — perlu randomisasi |

### Anomali = Dokumentasi, Bukan Hapus

Run gagal/anomali tidak boleh dihapus tanpa dokumentasi. Bisa jadi:
- **Bug** → fix & re-run (dokumentasikan!)
- **Batas kemampuan metode** → DNF = temuan
- **Data yang bias** jika hanya simpan run "berhasil"

### Jebakan Kognitif

1. "Satu angka cukup" → tanpa distribusi, tidak bisa diuji
2. "Seed tidak penting" → bahkan algoritma deterministik bisa dipengaruhi library stokastik
3. "Run gagal langsung hapus" → kehilangan temuan potensial
4. "Semua run harus hari ini" → thermal throttling, fatigue

---

## Template A.10 — Execution Plan & Data Log

```
EXECUTION PLAN

| Run # | Skenario                |Seed|         Parameter         |Status | Waktu |            Output File               |
|-------|-------------------------|----|---------------------------|-------|-------|--------------------------------------|
| 1     |A (MobileNetV2 Fine-tune)|42  |lr=1e-4, batch=32, epoch=50|Planned|2.5 jam|run_001_mobilenet_finetune_seed42.csv |
| 2     |A (MobileNetV2 Fine-tune)|101 |lr=1e-4, batch=32, epoch=50|Planned|2.5 jam|run_002_mobilenet_finetune_seed101.csv|
| 3     |A (MobileNetV2 Fine-tune)|202 |lr=1e-4, batch=32, epoch=50|Planned|2.5 jam|run_003_mobilenet_finetune_seed102.csv|
| 4     |A (MobileNetV2 Fine-tune)|303 |lr=1e-4, batch=32, epoch=50|Planned|2.5 jam|run_004_mobilenet_finetune_seed103.csv|
| 5     |A (MobileNetV2 Fine-tune)|404 |lr=1e-4, batch=32, epoch=50|Planned|2.5 jam|run_005_mobilenet_finetune_seed104.csv|
| 6     |A (MobileNetV2 Fine-tune)|505 |lr=1e-4, batch=32, epoch=50|Planned|2.5 jam|run_006_mobilenet_finetune_seed105.csv|
| 7     |A (MobileNetV2 Fine-tune)|606 |lr=1e-4, batch=32, epoch=50|Planned|2.5 jam|run_007_mobilenet_finetune_seed106.csv|
| 8     |A (MobileNetV2 Fine-tune)|707 |lr=1e-4, batch=32, epoch=50|Planned|2.5 jam|run_008_mobilenet_finetune_seed107.csv|
| 9     |A (MobileNetV2 Fine-tune)|808 |lr=1e-4, batch=32, epoch=50|Planned|2.5 jam|run_009_mobilenet_finetune_seed108.csv|
| 10    |A (MobileNetV2 Fine-tune)|909 |lr=1e-4, batch=32, epoch=50|Planned|2.5 jam|run_010_mobilenet_finetune_seed109.csv|
| 11    |     B (Baseline CNN)    |42  |lr=1e-3, batch=32, epoch=50|Planned|1.5 jam|run_011_baseline_seed42.csv|
| 12    |     B (Baseline CNN)    |101 |lr=1e-3, batch=32, epoch=50|Planned|1.5 jam|run_012_baseline_seed42.csv|
| 13    |     B (Baseline CNN)    |202 |lr=1e-3, batch=32, epoch=50|Planned|1.5 jam|run_013_baseline_seed42.csv|
| 14    |     B (Baseline CNN)    |303 |lr=1e-3, batch=32, epoch=50|Planned|1.5 jam|run_014_baseline_seed42.csv|
| 15    |     B (Baseline CNN)    |404 |lr=1e-3, batch=32, epoch=50|Planned|1.5 jam|run_015_baseline_seed42.csv|
| 16    |     B (Baseline CNN)    |505 |lr=1e-3, batch=32, epoch=50|Planned|1.5 jam|run_016_baseline_seed42.csv|
| 17    |     B (Baseline CNN)    |606 |lr=1e-3, batch=32, epoch=50|Planned|1.5 jam|run_017_baseline_seed42.csv|
| 18    |     B (Baseline CNN)    |707 |lr=1e-3, batch=32, epoch=50|Planned|1.5 jam|run_018_baseline_seed42.csv|
| 19    |     B (Baseline CNN)    |808 |lr=1e-3, batch=32, epoch=50|Planned|1.5 jam|run_019_baseline_seed42.csv|
| 20    |     B (Baseline CNN)    |909 |lr=1e-3, batch=32, epoch=50|Planned|1.5 jam|run_020_baseline_seed42.csv|
| 21    | C (MobileNetV2 Frozen)  |42  |lr=1e-3, batch=32, epoch=50|Planned| 1 jam |run_021_mobilenet_frozen_seed42.csv|
| 22    | C (MobileNetV2 Frozen)  |101 |lr=1e-3, batch=32, epoch=50|Planned| 1 jam |run_022_mobilenet_frozen_seed42.csv|
| 23    | C (MobileNetV2 Frozen)  |202 |lr=1e-3, batch=32, epoch=50|Planned| 1 jam |run_023_mobilenet_frozen_seed42.csv|
| 24    | C (MobileNetV2 Frozen)  |303 |lr=1e-3, batch=32, epoch=50|Planned| 1 jam |run_024_mobilenet_frozen_seed42.csv|
| 25    | C (MobileNetV2 Frozen)  |404 |lr=1e-3, batch=32, epoch=50|Planned| 1 jam |run_025_mobilenet_frozen_seed42.csv|
| 26    | C (MobileNetV2 Frozen)  |505 |lr=1e-3, batch=32, epoch=50|Planned| 1 jam |run_026_mobilenet_frozen_seed42.csv|
| 27    | C (MobileNetV2 Frozen)  |606 |lr=1e-3, batch=32, epoch=50|Planned| 1 jam |run_027_mobilenet_frozen_seed42.csv|
| 28    | C (MobileNetV2 Frozen)  |707 |lr=1e-3, batch=32, epoch=50|Planned| 1 jam |run_028_mobilenet_frozen_seed42.csv|
| 29    | C (MobileNetV2 Frozen)  |808 |lr=1e-3, batch=32, epoch=50|Planned| 1 jam |run_029_mobilenet_frozen_seed42.csv|
| 30    | C (MobileNetV2 Frozen)  |909 |lr=1e-3, batch=32, epoch=50|Planned| 1 jam |run_030_mobilenet_frozen_seed42.csv|

Jumlah runs per skenario : 10
Total runs               : 30

DATA LOG (per run):
  Run ID    : run_XXX
  Timestamp : YYYY-MM-DDTHH:MM:SS+07:00
  Skenario  : [A/B/C] ([Model Description])
  Input     : - Dataset: Rupiah Damage Dataset (3 kelas: Ringan, Sedang, Berat)
              - Split: 70% train, 15% validation, 15% test
              - Batch size: [batch_size]
              - Image size: 224×224
              - Augmentasi: rotation (±10°), brightness (0.8-1.2), contrast (0.8-1.2)
              - Seed: [seed]
              - Learning Rate: [lr]
              - Optimizer: [optimizer]
              - Epochs: [epochs]
  Output    : - Test Accuracy: [value]
              - Macro F1-Score: [value]
              - MCC: [value]
              - Balanced Accuracy: [value]
              - Test Loss: [value]
              - Best Epoch: [value]
              - Confusion Matrix: [[...], [...], [...]]
              - Checkpoint: [model]_best_seed[seed].pth
              - Grad-CAM: gradcam/run_XXX/
  Anomali   : [Deskripsi anomali jika ada, atau "Tidak ada anomali terdeteksi"]
  Catatan   : [Informasi tambahan seperti durasi, resource usage, warning, dll.]
```

---

## Latihan 1 — Execution Plan

Susun execution plan untuk eksperimen Anda. Tentukan skenario, jumlah run, dan seed sebelum eksekusi.

| Run # | Skenario | Seed | Parameter Kunci | Status |
|-------|----------|------|----------------|--------|
| *1* | *Contoh: BERT-base, DS-1* | *42* | *lr=2e-5, epoch=10* | *Planned* |
| *2* | *BERT-base, DS-1* | *123* | *lr=2e-5, epoch=10* | *Planned* |
| 3 | | | | |
| 4 | | | | |
| 5 | | | | |

**Total skenario:** ____
**Run per skenario:** ____
**Total run keseluruhan:** ____

---

## Latihan 2 — Data Log Terstruktur

Desain format data log untuk eksperimen Anda. Tentukan field apa saja yang akan dicatat.

**Identitas:**
| Field | Contoh |
|-------|--------|
| Run ID | *run-001* |
| Timestamp | *2025-03-15T10:30:00* |
| | |

**Konfigurasi:**
| Field | Contoh |
|-------|--------|
| Seed | *42* |
| Code version | *commit abc1234* |
| | |

**Hasil:**
| Metrik | Tipe Data | Range Valid |
|--------|----------|-------------|
| *Contoh: Accuracy* | *float* | *0.0 – 1.0* |
| | | |
| | | |

**Format output:** [ ] CSV / [ ] JSON / [ ] Database / [ ] Lainnya: ____

---

## Latihan 3 — Anomaly Protocol

Rencanakan bagaimana menangani anomali. Untuk setiap jenis, tentukan langkah yang diambil.

| Jenis Anomali | Contoh | Tindakan |
|---------------|--------|----------|
| Run gagal (crash) | Out of Memory (OOM) pada batch_size=64 (seharusnya 32) | 1. Dokumentasi — Catat error, timestamp, konfigurasi
2. Investigasi — Cek VRAM dengan nvidia-smi, cek memory leak
3. Keputusan — Jika benar OOM, kurangi batch_size menjadi 16, re-run dengan catatan di log
4. Dokumentasi perubahan — Update config dan catat di anomaly_log.md |

| Hasil ekstrem | Accuracy 0.999 atau 0.001 (berbeda jauh dari run lain) | 1. Tidak hapus! — Simpan hasil
2. Investigasi — Cek apakah data leakage (shuffle salah), split salah, atau label error
3. Dokumentasi — Catat temuan dan seed
4. Keputusan — Jika data error, fix dan re-run; jika metode yang lemah, dokumentasikan sebagai DNF (Did Not Finish) |

| Waktu eksekusi anomali | Run yang seharusnya 2.5 jam menjadi 5+ jam | 1. Check resource — Cek background process, thermal throttling, VRAM usage
2. Dokumentasi — Catat resource usage selama run
3. Keputusan — Jika thermal throttling, hentikan sementara, biarkan laptop dingin, lanjutkan (gunakan checkpoint) |

| Inkonsistensi dengan run lain | Run seed 42 menghasilkan macro_f1=0.87, seed 202=0.71 | 1. Jangan abaikan — Variabilitas tinggi adalah temuan
2. Investigasi — Cek apakah ada outlier dalam data split (label imbalance per fold)
3. Dokumentasi — Catat distribusi per seed
4. Keputusan — Perlu lebih dari 10 run? Atau stratified split bermasalah? |

| DataLoader warning | Worker timeout atau shuffle inconsistency | 1. Dokumentasi — Catat warning
2. Investigasi — Cek num_workers dan seed di worker
3. Keputusan — Kurangi num_workers jika terlalu tinggi |

| Loss NaN / gradient explosion | Training loss menjadi NaN di epoch ke-5 | 1. Stop run — Hentikan eksekusi untuk save resource
2. Dokumentasi — Catat epoch terakhir
3. Investigasi — Cek learning rate terlalu tinggi, weight initialization, gradient clipping
4. Keputusan — Turunkan lr atau tambah gradient clipping, re-run dengan seed yang sama |

Detect Anomaly
      ↓
Document (timestamp, seed, config, error/warning)
      ↓
Investigate (root cause analysis)
      ↓
Decide:
  ├── Bug → Fix & Re-run (catat perubahan)
  ├── Hardware issue → Wait & Resume (gunakan checkpoint)
  ├── Method limitation → Document as DNF (temuan!)
  ├── Random outlier → Keep data, analyze separately
  └── User error → Fix & Re-run
      ↓
Update anomaly_log.md & config

## ANOMALY LOG - [DATE]

### Run ID: run_007
- **Seed:** 202
- **Skenario:** A (MobileNetV2 Fine-tune)
- **Waktu:** 2026-07-15 14:30:00
- **Anomali:** Loss NaN di epoch ke-12
- **Investigasi:** Learning rate 1e-4 terlalu tinggi? Cek gradient norms
- **Keputusan:** Turunkan lr ke 5e-5, re-run dengan seed 202 (baru)
- **Catatan:** Run ini didokumentasikan di results/anomaly/run_007_NaN_loss.md
- **Status:** Re-run (config diubah)

- ## ANOMALY LOG - [DATE]

### Run ID: run_007
- **Seed:** 202
- **Skenario:** A (MobileNetV2 Fine-tune)
- **Waktu:** 2026-07-15 14:30:00
- **Anomali:** Loss NaN di epoch ke-12
- **Investigasi:** Learning rate 1e-4 terlalu tinggi? Cek gradient norms
- **Keputusan:** Turunkan lr ke 5e-5, re-run dengan seed 202 (baru)
- **Catatan:** Run ini didokumentasikan di results/anomaly/run_007_NaN_loss.md
- **Status:** Re-run (config diubah)
- 
**Prinsip:** Detect → Investigate → Document → Decide
Checklist Anomaly Handler:
[✅] Semua run dicatat di execution plan
[✅] Anomali tidak dihapus tanpa dokumentasi
[✅] Ada log file khusus untuk anomali (anomaly_log.md)
[✅] Run gagal dire-run dengan perubahan yang tercatat
[✅] DNF (Did Not Finish) dicatat sebagai temuan
[✅] Setiap perubahan konfigurasi karena anomali terdokumentasi
[✅] Variabilitas tinggi di-investigasi, bukan diabaikan
---

## Refleksi

> Pernahkah Anda melaporkan hasil riset/tugas dari single run? Apa risikonya? Bagaimana multiple run mengubah kepercayaan terhadap hasil?

**Pengalaman sebelumnya:**
> Pada beberapa tugas dan eksperimen sebelumnya, evaluasi model sering dilakukan hanya berdasarkan satu kali proses training. Pendekatan tersebut memiliki risiko karena hasil yang diperoleh dapat dipengaruhi oleh faktor acak seperti inisialisasi bobot awal, pembagian data, maupun proses optimasi, sehingga belum tentu merepresentasikan performa sebenarnya dari model. Risikonya:
1.Overconfidence — Klaim model A lebih baik dari model B hanya karena 1 run, padahal bisa jadi karena seed keberuntungan
2.Tidak ada error bar — Grafik tanpa standard deviation tidak menunjukkan variabilitas
3.Tidak bisa diuji statistik — Tanpa distribusi hasil, tidak bisa melakukan paired t-test
4.Tidak reproducible — Saat orang lain mencoba mereproduksi, hasilnya berbeda
5.Tidak tahu sensitivitas seed — Apakah model stabil atau sangat bergantung pada inisialisasi?

**Yang akan dilakukan berbeda:**
> Pada penelitian ini, evaluasi akan dilakukan menggunakan multiple run dengan random seed yang telah ditentukan sebelumnya. Setiap model akan dijalankan sebanyak 10 kali sehingga diperoleh nilai rata-rata (mean), standar deviasi (standard deviation), dan distribusi performa. Hasil tersebut akan digunakan sebagai dasar analisis statistik untuk membandingkan CNN 2-Layer sebagai baseline dengan MobileNetV2 (Transfer Learning). Dengan pendekatan ini, kesimpulan penelitian menjadi lebih objektif, lebih reliabel, dan memiliki tingkat kepercayaan yang lebih tinggi dibandingkan evaluasi berdasarkan satu kali eksperimen.

Yang akan dilakukan berbeda secara spesifik:
1.Minimum 10 run per skenario dengan seed pre-determined (bukan random tiap kali)
2.Seed tercatat di log — Setiap run punya seed yang didokumentasikan
3.Mean ± std untuk semua metrik — Bukan hanya angka tunggal
4.Confidence interval dan boxplot — Visualisasi distribusi hasil
5.Uji statistik (Paired T-Test) — Membuktikan perbedaan signifikan
6.Anomali tidak dihapus — Semua run termasuk outlier dilaporkan atau dijelaskan
7.Pre-registration — Execution plan dibuat sebelum eksekusi (seperti di Latihan 1)
8.Otomatisasi logging — Menggunakan script, bukan copy-paste manual
9.Checkpointing — Agar bisa resume jika ada crash
10.Analisis sensitivitas seed — Melihat apakah hasil stabil di berbagai seed
