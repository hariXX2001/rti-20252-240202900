# WS-06: System-Experiment Mapping

> **Bab 6 — System Design sebagai Experimental Artifact**

---

## Ringkasan Materi

### Sistem = Instrumen Pengujian, Bukan Produk

Seorang engineer bertanya "apakah sistem bekerja?" — seorang peneliti bertanya "apa yang bisa dibuktikan sistem ini?" Sistem dalam riset adalah **artifact** — objek yang sengaja dibuat untuk menguji klaim spesifik.

### System as Experiment Model

```
RQ → Variable → System Component → Experimental Setup → Output
```

Setiap komponen sistem harus bisa ditelusuri ke variabel riset (top-down), dan setiap pengukuran harus menjawab RQ (bottom-up).

### Mapping Variabel ke Komponen

| Tipe Variabel | Peran di Sistem | Contoh |
|---------------|----------------|--------|
| **IV** (Independent) | Modul yang bisa di-toggle/swap | Algoritma A vs B |
| **DV** (Dependent) | Modul pengukuran | Logger, metrics collector |
| **CV** (Control) | Config yang dikunci | Dataset, parameter tetap |

Jika variabel tidak bisa di-map ke komponen apapun → arsitektur perlu didesain ulang.

### 4 Prinsip Desain Eksperimental

| Prinsip | Pertanyaan Kunci |
|---------|-----------------|
| **Traceability** | Komponen ini melayani variabel yang mana? |
| **Modularity** | Bisakah IV diubah tanpa memengaruhi yang lain? |
| **Controllability** | Apakah CV dieksternalisasi ke config file? |
| **Measurability** | Apakah sistem otomatis menghasilkan data yang dibutuhkan? |

### Variable Isolation melalui Arsitektur

- **Modular architecture** — Pisahkan berdasarkan variabel
- **Configuration-driven** — Ubah config (YAML/JSON), bukan code
- **Feature toggles** — On/off flag untuk ablation study

  Contoh config YAML dengan feature toggles:
  ```yaml
  model:
    type: cnn          # IV: ganti "rf" untuk kondisi baseline
  features:
    use_temporal: true  # toggle komponen temporal
    use_normalization: true  # toggle preprocessing
  experiment:
    seed: 42
    runs: 5
  ```
  Dengan pendekatan ini, berbeda kondisi eksperimen = berbeda satu baris config, **tanpa mengubah kode**.

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan sistem | Memenuhi kebutuhan user | Menguji hipotesis, menghasilkan bukti |
| Arsitektur | Optimasi performa & skalabilitas | Optimasi isolasi variabel & reprodusibilitas |
| Konfigurasi | Sering hardcoded | Dieksternalisasi ke config file |
| Fitur tambahan | Menambah nilai user | Menambah noise jika tidak terkait RQ |

### Istilah Penting

- **Artifact** — Objek yang sengaja dibuat untuk memecahkan masalah atau menguji proposisi
- **Traceability** — Kemampuan menelusuri hubungan RQ → variabel → komponen → output
- **Variable Isolation** — Mengubah hanya satu variabel sambil menahan yang lain konstan
- **Ablation Study** — Menguji kontribusi tiap komponen dengan melepasnya satu per satu
- **Configuration-driven Execution** — Semua parameter di config file, bukan hardcoded

---

## Template A.6 — Mapping RQ ke Arsitektur Sistem

```
SYSTEM-EXPERIMENT MAPPING

Research Question:Apakah MobileNetV2 memberikan performa klasifikasi yang lebih baik dibandingkan baseline CNN untuk mendeteksi rupiah kertas yang rusak?

Variable → Component Mapping:
| Variabel | Tipe | Komponen Sistem | Cara Manipulasi/Pengukuran |
|----------|------|-----------------|---------------------------|
|Jenis arsitektur model| IV   |models/ (MobileNetV2 & BaselineCNN)|Ganti model.type di config.yaml|
|Performa klasifikasi| DV   |metrics/ + trainer.py + Logger|Macro F1-Score (primary), MCC & Balanced Accuracy (secondary)|
|Dataset & splitting| CV   |dataloaders/ + dataset.py|Fix random seed + config data.split|
|Augmentasi & preprocessing|  CV  |transforms/ + config.preprocessing|Dikunci di config file|
|Hyperparameter (lr, batch, epochs)|  CV  |config.yaml + Training Loop|Dikunci di config training:|

4 Prinsip Desain:
  [✅] Traceability — Setiap komponen bisa ditelusuri ke variabel
  [✅] Variable Isolation — IV bisa diubah tanpa mengubah CV
  [✅] Measurement Integration — Pengukuran DV built-in
  [✅] Reproducibility — Setup bisa direkonstruksi

Experimental Setup:
  Input data     : Gambar rupiah kertas rusak dan tidak rusak (224×224 px) yang telah melalui preprocessing (resize, normalization, dan augmentasi). Dataset dibagi stratified menjadi train (70%), val (15%), test (15%).
  Parameter      :
- Model architecture (MobileNetV2 vs Baseline CNN)  
- Learning rate, batch size, epochs  
- Augmentation parameters  
- Random seed (fixed = 42)  
Semua parameter dikendalikan melalui `config.yaml`
  Output format  :
- Best model weights (.pth)  
- Metrics table (CSV & JSON): Macro F1-Score (primary), MCC, Balanced Accuracy  
- Training history & logs  
- Visualizations (Confusion Matrix & Grad-CAM)  
- Full experiment configuration file untuk reproducibility
```

---

## Latihan 1 — Variable-to-Component Mapping

Gunakan RQ dan variabel dari WS-05. Petakan ke komponen sistem.

**RQ:** Apakah MobileNetV2 memberikan performa klasifikasi yang lebih baik dibandingkan baseline CNN untuk mendeteksi rupiah kertas yang rusak?

| Variabel | Tipe | Komponen Sistem | Cara Manipulasi / Pengukuran |
|----------|------|-----------------|---------------------------|
| Jenis arsitektur model | IV | models/mobile_net.py & models/baseline_cnn.py | models/mobile_net.py & models/baseline_cnn.py |
|Performa klasifikasi (Macro F1, MCC, dll)| DV |metrics/ + trainer.py + Logger|Otomatis dihitung dan disimpan setiap epoch|
|Dataset split & augmentasi| CV |dataloaders/ + dataset.py + transforms/|Fix random seed + konfigurasi di config.yaml|
|Preprocessing (resize, normalization)|  CV  |config.preprocessing + transforms.Compose|Dikunci melalui config file|
|Hyperparameter (lr, batch_size, epochs)|  CV  |config.training|Dikendalikan sepenuhnya melalui config.yaml|

**Apakah semua variabel bisa di-map?** [Y] Ya / [ ] Tidak
> Jika tidak, komponen apa yang perlu ditambahkan? _________

---

## Latihan 2 — 4 Prinsip Desain

Evaluasi desain sistem terhadap 4 prinsip.

| Prinsip | Status | Bukti / Penjelasan |
|---------|--------|-------------------|
| Traceability | ✅ |Komponen sistem terstruktur sesuai variabel riset|
| Modularity |✅|Model dapat di-swap via factory pattern|
| Controllability |✅ |Seluruh eksperimen dikendalikan lewat config.yaml|
| Measurability |✅ |Metrics logging otomatis dan lengkap|

**Prinsip mana yang paling sulit dipenuhi?** Modularity
**Strategi untuk mengatasinya:**
> Menggunakan Model Factory dan konfigurasi berbasis string untuk memudahkan swapping antar arsitektur.
---

## Latihan 3 — Ablation Study Planning

Jika sistem memiliki 3 komponen utama, rencanakan ablation study.

> **Panduan jumlah kondisi:** Untuk 3 komponen (A, B, C), kondisi minimal yang direkomendasikan:
> Full + (-A) + (-B) + (-C) = **4 kondisi dasar**. Jika waktu memungkinkan, tambahkan kombinasi ganda: (-A,-B), (-A,-C), (-B,-C) = **7 kondisi**. Sesuaikan dengan *computational cost* dan tenggat waktu penelitian.

| Kondisi | Komponen A  Backbone | Komponen B Augmentasi | Komponen C Normalization | Hasil yang Diharapkan |
|---------|-----------|-----------|-----------|----------------------|
| Full | ✅ MobileNetV2 | ✅ | ✅ | Performa tertinggi |
| – A | ❌ Baseline | ✅ | ✅ | Penurunan paling besar |
| – B | ✅ MobileNetV2 | ❌ | ✅ | Penurunan sedang |
| – C | ✅ MobileNetV2 | ✅ | ❌ | Penurunan kecil |
| –A –B  |  ❌ Baseline | ❌ | ✅ | Performa terendah |
| –A –C | ❌ Baseline | ✅ | ❌ | Sangat rendah |
| –B –C | ✅ MobileNetV2 | ❌ | ❌ | Rendah |

**Komponen mana yang diprediksi paling berkontribusi?** Komponen A (Backbone)
**Mengapa?**
> Karena perbedaan kemampuan representasi fitur antar arsitektur model jauh lebih besar dibandingkan augmentasi atau normalization.
---

## Refleksi

> Apa risiko jika sistem dibangun seperti produk (monolitik, fitur lengkap) lalu baru dilakukan eksperimen? Mengapa arsitektur modular penting untuk riset?

**Jawaban:**
> Risiko utamanya adalah sulitnya mengontrol variabel, banyak noise dari fitur yang tidak perlu, dan rendahnya reproducibility.
> Arsitektur modular penting karena memungkinkan kita mengubah satu faktor saja (IV) secara bersih, sehingga kesimpulan yang diambil lebih kuat dan ilmiah.
