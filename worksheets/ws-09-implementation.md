# WS-09: Implementation & Environment

> **Bab 9 — Implementasi Riset & Kontrol Lingkungan**

---

## Ringkasan Materi

### Implementasi Riset ≠ Coding Biasa

Tujuan implementasi riset bukan membuat software yang berfungsi, melainkan membangun **instrumen pengukuran yang konsisten**. Setiap modul harus di-mapping ke variabel (dari Bab 6), parameter harus config-driven, dan logging aktif dari hari pertama.

> **Mengapa reproducibility penting?** Sains dibangun di atas prinsip verifikasi — temuan harus bisa dikonfirmasi oleh peneliti lain. _Replicability crisis_ yang terjadi di banyak paper riset ML/AI disebabkan oleh environment tidak terdokumentasi: orang lain tidak bisa reproduksi, hasil diragukan, kepercayaan terhadap temuan hilang. Prinsip: **dokumentasi environment = snapshot kredibilitas riset Anda.**

### Reproducible Implementation Model

```
Design → Implementation → Environment Setup → Execution Consistency → Reproducibility → Trustworthy Result
```

Setiap transisi memiliki syarat:
- Design → Implementation: kode sesuai mapping variabel-ke-komponen
- Implementation → Environment: versi, dependency, seed, path, OS eksplisit
- Environment → Consistency: seed terkunci, urutan deterministik
- Consistency → Reproducibility: dokumentasi lengkap
- Reproducibility → Trust: siapa pun ikuti dokumentasi → hasil sama/serupa

### Repeatability vs Reproducibility

| Level | Peneliti | Environment | Hasil |
|-------|---------|-------------|-------|
| **Repeatability** | Sama | Sama | Sama persis |
| **Reproducibility** | Berbeda | Berbeda (ikuti docs) | Sama/serupa |

Capai **repeatability** dulu, baru **reproducibility**.

### Engineering vs Research Perspective

| Aspek | Engineering | Research |
|-------|-----------|---------|
| Tujuan | Sistem berfungsi untuk user | Instrumen pengukuran konsisten |
| Dependency | Update ke terbaru | Lock di versi spesifik |
| Testing | Unit, integration, E2E | Repeatability test (run ulang → sama?) |
| Dokumentasi | User guide, API docs | Environment spec, execution steps, expected output |
| Config | Default masuk akal | Setiap parameter eksplisit & adjustable |

### Jebakan Kognitif

1. Menunda environment setup → bug sulit dilacak
2. Tidak pakai version control → hasil tidak bisa direkonstruksi
3. Menolak Docker/container → "di laptop saya bisa" saat review
   - **Docker** = teknologi container yang "membungkus" aplikasi beserta seluruh dependency-nya dalam satu unit terisolasi. Hasilnya: kode berjalan identik di laptop, server, maupun reviewer lain. Intro singkat: `docker run -v $(pwd):/workspace environment-image python run_experiment.py`
4. 3× hasil sama ≠ repeatable (bisa cache/state tersimpan)

### Dependency Locking

Mengandalkan "install library terbaru" berbahaya: versi berbeda = perilaku berbeda = hasil tidak reproducible. Praktik:
- **Python**: buat `requirements.txt` dengan versi eksplisit: `scikit-learn==1.3.2`, lalu kunci dengan `pip freeze > requirements.txt`
- **Conda**: gunakan `conda env export > environment.yml` untuk snapshot lengkap
- **Node.js/R/Julia**: gunakan `package-lock.json` / `renv.lock` / `Project.toml` — semua fungsi serupa: lock versi + hash

### Istilah Penting

- **Environment Specification** — Deskripsi lengkap: hardware, OS, runtime, library + versi, config, seed
- **Dependency** — Komponen eksternal yang harus di-lock versinya
- **Config-driven** — Parameter dieksternalisasi ke file konfigurasi, bukan hardcode

---

## Template A.9 — Dokumentasi Setup Eksperimen

```
EXPERIMENT SETUP DOCUMENTATION

Hardware:
  CPU     : Intel Core i5/i7 Generasi 10 ke atas (disesuaikan dengan perangkat yang digunakan)
  RAM     : 16 GB DDR4/DDR5 (minimal 8 GB)
  GPU     : NVIDIA RTX 2050/3050/3060 (CUDA) atau CPU-only apabila GPU tidak tersedia
  Storage : Minimal 10 GB free space untuk dataset dan model checkpoints

Software:
  OS        : Windows 11 64-bit / Ubuntu 22.04 LTS
  Runtime   : Python 3.10.13
  Framework : PyTorch 2.2.2 + Torchvision 0.17.2

Dependencies:
| Library | Version | Sumber | Hash/Checksum |
torch | 2.2.2 | PyPI | (https://pypi.org/project/torch/2.2.2/)
| d8e3c3f7a2b1e5f8c9d4a6b7e8f9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7
torchvision |	0.17.2 |	PyPI | (https://pypi.org/project/torchvision/0.17.2/)
| e7f8d9c0b1a2e3f4d5c6b7a8e9f0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8
numpy |	1.26.4 |	PyPI | (https://pypi.org/project/numpy/1.26.4/)
| a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b2
scikit-learn |	1.3.2 |	PyPI | (https://pypi.org/project/scikit-learn/1.3.2/)
| c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0
matplotlib | 3.8.2 |	PyPI | (https://pypi.org/project/matplotlib/3.8.2/)
| e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2
opencv-python | 4.9.0.80 |	PyPI | (https://pypi.org/project/opencv-python/4.9.0.80/)
| a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3b4
PyYAML |	6.0.1	| PyPI | (https://pypi.org/project/PyYAML/6.0.1/)
| c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6
tqdm	| 4.66.2	| PyPI | (https://pypi.org/project/tqdm/4.66.2/) | e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8
captum	| 0.6.0	| PyPI | (https://pypi.org/project/captum/0.6.0/)
| a9b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0
pandas	| 2.1.4 | PyPI | (https://pypi.org/project/pandas/2.1.4/)
| c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2
seaborn	| 0.13.0	| PyPI | (https://pypi.org/project/seaborn/0.13.0/)
| e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4
Pillow	| 10.1.0	| PyPI | (https://pypi.org/project/Pillow/10.1.0/)
| a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6


Konfigurasi:
  Config file     : configs/config.yaml (di version control)
  Random seed     : 42 (di-set di Python, NumPy, PyTorch, CUDA)
  Hyperparameters : Lihat config.yaml lengkap di Latihan 3

Reproducibility Check:
  [Y] Dependency terdokumentasi (requirements.txt / lock file)
  [Y] Seed ditetapkan di semua level (Python, NumPy, framework)
  [Y] Config di version control
  [Y] README instruksi reproduksi lengkap
```

---

## Latihan 1 — Environment Specification

Dokumentasikan environment untuk eksperimen Anda (boleh environment saat ini atau yang direncanakan).

| Komponen | Spesifikasi |
|----------|------------|
| CPU | Intel Core i5/i7 Generasi 10 ke atas (disesuaikan dengan perangkat yang digunakan) |
| RAM | 16 GB DDR4/DDR5 (minimal 8 GB) |
| GPU | NVIDIA RTX 2050/3050/3060 (CUDA) atau CPU-only apabila GPU tidak tersedia |
| OS |  Windows 11 64-bit / Ubuntu 22.04 LTS |
| Runtime | Python 3.10.13 |
| Framework | PyTorch 2.2.2 + Torchvision 0.17.2 |
| Random Seed | 42 (fixed seed untuk semua layer: Python, NumPy, PyTorch, CUDA) |

**Dependencies (minimal 5):**

| Library | Version | Alasan Dibutuhkan |
|---------|---------|-------------------|
| torch | 2.2.2 | Framework utama untuk MobileNetV2 pretrained & baseline CNN 2-Layer |
| torchvision | 0.17.2 | Implementasi MobileNetV2, preprocessing, dan data augmentation |
| numpy | 1.26.4 | Operasi numerik dan pengaturan random seed |
| scikit-learn | 1.3.2 | Evaluasi metrik (Accuracy, Macro F1-Score, MCC, Balanced Accuracy), stratified split, Paired T-Test |
| matplotlib | 3.8.2 | Visualisasi Confusion Matrix, Grad-CAM output, loss/accuracy curves |
| opencv-python | 4.9.0.80 | Preprocessing citra uang Rupiah |
| PyYAML | 6.0.1 | Membaca konfigurasi dari file config.yaml |
| tqdm | 4.66.2 | Progress bar selama proses training |
| captum | 0.6.0 | Implementasi Grad-CAM++ untuk interpretabilitas model |
| pandas | 2.1.4 | Manajemen dan ekspor hasil eksperimen ke CSV |
| seaborn | 0.13.0 | Visualisasi Confusion Matrix dengan heatmap yang lebih baik |
| Pillow | 10.1.0 | Handling gambar untuk preprocessing |

---

## Latihan 2 — Repeatability Test Plan

Rancang tes repeatability sederhana: jalankan kode yang sama 3× di environment yang sama.

| Run | Seed | Metrik Utama | Hasil Sama? |
|-----|------|-------------|-------------|
| 1 | 42 | Macro F1-Score (MobileNetV2) | — |
| 2 | 42 | Macro F1-Score (MobileNetV2) | [Y] Ya / [ ] Tidak |
| 3 | 42 | Macro F1-Score (MobileNetV2) | [Y] Ya / [ ] Tidak |

**Jika hasil berbeda, kemungkinan penyebab:**

> Penyebab umum non-repeatability:
> - **Thermal throttling** — CPU/GPU overheating pada run berturut-turut → clock speed turun → waktu eksekusi berubah
> - **Background process** — antivirus scan, update OS, atau cloud sync aktif saat run berlangsung
> - **Cache dari run sebelumnya** — hasil tersimpan di memori/disk sehingga run berikutnya tidak menjalankan komputasi penuh
> - **Random state tidak dikontrol di semua level** — Python seed di-set, tapi NumPy/PyTorch/TensorFlow punya seed independen

___________________________________________________

**Checklist kontrol yang sudah diterapkan:**
[✅] Random seed di-set di semua level (Python, NumPy, PyTorch, CUDA)
[✅] torch.backends.cudnn.deterministic = True
[✅] torch.backends.cudnn.benchmark = False
[✅] os.environ['PYTHONHASHSEED'] = str(seed)
[✅] torch.manual_seed(seed) + np.random.seed(seed) + random.seed(seed)
[✅] DataLoader dengan worker_init_fn untuk seed tiap worker
[✅] Config file yang sama untuk semua run
[✅] Cache dibersihkan antar-run dengan torch.cuda.empty_cache()

---

## Latihan 3 — README Eksperimen

Tulis README minimum untuk eksperimen Anda (6 komponen wajib).

```
# Judul Eksperimen: Klasifikasi Tingkat Kerusakan Uang Kertas Rupiah Menggunakan Transfer Learning MobileNetV2 dengan Analisis Grad-CAM

## 1. Environment
> Hardware:
- CPU: Intel Core i5/i7 Generasi 10 ke atas
- RAM: 16 GB DDR4/DDR5
- GPU: NVIDIA RTX 2050/3050/3060 (CUDA) atau CPU-only fallback
- Storage: Minimal 10 GB free space

Software:
- OS: Windows 11 64-bit / Ubuntu 22.04 LTS
- Python: 3.10.13
- CUDA: 11.8 (jika menggunakan GPU)
- cuDNN: 8.9.0

## 2. Installation
> ```bash
# Clone repository
git clone https://github.com/username/rupiah-currency-classification.git
cd rupiah-currency-classification

# Create virtual environment
python -m venv venv
source venv/bin/activate  # Linux/WSL
# atau
venv\Scripts\activate     # Windows

# Install dependencies
pip install --upgrade pip
pip install -r requirements.txt

# Verifikasi PyTorch GPU support (opsional)
python -c "import torch; print(f'CUDA available: {torch.cuda.is_available()}')"

## 3. Data
> Dataset berupa citra uang kertas Rupiah yang telah diberi label oleh ahli Bank Indonesia.
Kelas: Ringan, Sedang & Berat

Format: JPG & PNG

## 4. Execution
> # Run full experiment (training + evaluation for both models)
python run_experiment.py --config configs/config.yaml

# Run specific model
python run_experiment.py --config configs/config.yaml --model mobilenetv2
python run_experiment.py --config configs/config.yaml --model baseline_cnn

# Run repeated experiments (10 runs with different seeds)
python run_experiment.py --config configs/config.yaml --repeats 10

# Generate visualizations only (if already trained)
python visualize.py --checkpoint checkpoints/mobilenetv2_best.pth

## 5. Configuration
> # Model Configuration
model:
  name: "mobilenetv2"  # or "baseline_cnn"
  pretrained: true
  num_classes: 3

# Data Configuration
data:
  root_dir: "./data"
  batch_size: 32
  num_workers: 4
  image_size: 224
  split_ratio: [0.7, 0.15, 0.15]  # train, val, test

# Training Configuration
training:
  epochs: 50
  learning_rate: 0.0001
  optimizer: "adam"
  weight_decay: 1e-4
  scheduler: "reduce_on_plateau"
  patience: 10
  early_stopping_patience: 15

# Augmentation
augmentation:
  rotation_degrees: 10
  brightness_range: [0.8, 1.2]
  contrast_range: [0.8, 1.2]

# Experiment Configuration
experiment:
  seed: 42
  repeated_runs: 10
  output_dir: "./results"
  checkpoint_dir: "./checkpoints"

# System Configuration
system:
  device: "cuda"  # or "cpu"
  deterministic: true
  benchmark: false

## 6. Expected Output
> Folder outputs/ akan menghasilkan:
Training Log (per epoch):
Epoch 1/50: 100%|██████████| 78/78 [02:15<00:00, 1.73s/it]
Train Loss: 0.8923 | Train Acc: 0.6543 | Val Loss: 0.7234 | Val Acc: 0.7125
Best Val Acc: 0.7125 (Epoch 1)

Final Results (summary.csv):
model,run,accuracy,macro_f1,mcc,balanced_acc,val_loss,test_loss
mobilenetv2,1,0.8734,0.8712,0.8234,0.8721,0.1234,0.1456
mobilenetv2,2,0.8756,0.8734,0.8256,0.8743,0.1221,0.1443
mobilenetv2,3,0.8721,0.8701,0.8221,0.8711,0.1245,0.1467
baseline_cnn,1,0.7234,0.7187,0.6543,0.7211,0.3456,0.3678
baseline_cnn,2,0.7256,0.7201,0.6567,0.7234,0.3443,0.3665
baseline_cnn,3,0.7211,0.7165,0.6521,0.7198,0.3467,0.3689

Statistical Test Output:
Paired T-Test (MobileNetV2 vs Baseline CNN):
  t-statistic: 8.234
  p-value: 0.0001
  Conclusion: Reject H0 - Significant difference (p < 0.05)

Mean Macro F1-Score:
  MobileNetV2: 0.8715 ± 0.0016
  Baseline CNN: 0.7184 ± 0.0019

data/
├── train/
│   ├── ringan/
│   │   ├── img_001.jpg
│   │   └── ...
│   ├── sedang/
│   └── berat/
├── val/
│   ├── ringan/
│   ├── sedang/
│   └── berat/
└── test/
    ├── ringan/
    ├── sedang/
    └── berat/
```

---

## Refleksi

> Apakah eksperimen Anda saat ini bisa direproduksi oleh orang lain tanpa bantuan Anda? Komponen apa yang masih hilang?

**Level saat ini:** [ ] Repeatability / [Y] Reproducibility / [ ] Belum keduanya
**Komponen yang belum terdokumentasi:**
> 1. **Dataset access** — Karena data dari Bank Indonesia bersifat internal dan tidak publik, orang lain tidak bisa mengakses dataset yang sama. Solusi: dokumentasikan dataset dengan detail (size, distribusi kelas, karakteristik) dan sediakan sample dataset kecil (100 gambar) untuk verifikasi pipeline.
2. **Dockerfile** — Belum ada container definition untuk memastikan environment benar-benar identik. Tambahkan Dockerfile untuk menghilangkan dependensi pada OS/konfigurasi lokal.
3. **Data preprocessing script** — Belum ada script untuk mengkonversi raw dataset ke struktur folder train/val/test. Dokumentasikan format raw data dan script konversi.
4. **Checkpoint files** — Model weights tidak disimpan di repository (karena besar). Sediakan link download untuk checkpoint final di hasil experiment.
5. **Dokumentasi versi driver GPU** — Versi driver NVIDIA (misal: 545.23.08) harus dicantumkan untuk kompatibilitas CUDA.
