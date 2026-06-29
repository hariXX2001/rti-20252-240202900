# WS-15: Scientific Writing

> **Bab 15 — Penulisan Ilmiah**

---

## Ringkasan Materi

### Scientific Argument Flow

```
Problem → Gap → RQ → Method → Result → Analysis → Conclusion → Contribution
```

Paper ilmiah adalah **satu argumen utuh** dari masalah ke kontribusi. Setiap node harus terhubung logis ke node sebelum dan sesudahnya.

### Struktur IMRAD

| Section | Peran | Pertanyaan Kunci |
|---------|-------|-----------------|
| **Introduction** | Motivasi + frame | Why is this needed? |
| **Method** | Deskripsi (reproducible) | How was it done? |
| **Results** | Laporan objektif | What was found? |
| **Discussion** | Interpretasi + refleksi | What does it mean? |
| **Conclusion** | Ringkasan + kontribusi | So what? |

### Logical Flow — "Red Thread"

Setiap paragraf menjawab satu pertanyaan dan memicu pertanyaan berikutnya. Alur logis ini harus terasa di tiga level:
1. **Antar-kalimat** dalam paragraf
2. **Antar-paragraf** dalam section
3. **Antar-section** dalam paper

### Internal Consistency

Setiap elemen yang dijanjikan di Introduction harus hadir di Discussion/Conclusion.

**Consistency Matrix:**
```
           Intro  Method  Result  Discuss  Conclude
RQ1          ✓      ✓       ✓       ✓        ✓
RQ2          ✓      ✓       ✓       ✗ ←      ✓
Metrik-X     ✗      ✗       ✓ ←     ✗        ✗
```
**Masalah:** RQ2 dibahas di semua bagian kecuali Discussion. Metrik-X muncul di Result tapi tidak diperkenalkan di Method.

### Writing Quality Triad

| Kualitas | Deskripsi | Contoh Buruk → Baik |
|----------|----------|---------------------|
| **Clarity** | Dipahami sekali baca | "Performa meningkat" → "Accuracy meningkat dari 85.3% ke 89.7%" |
| **Precision** | Istilah eksak, tanpa ambiguitas | "signifikan" → "signifikan secara statistik (p=0.003, d=1.2)" |
| **Conciseness** | Setiap kata menambah informasi | Hapus kalimat redundan, filler words |

### Urutan Penulisan yang Disarankan

1. **Method & Results** — paling stabil, tulis pertama
2. **Discussion** — interpretasi berdasarkan hasil
3. **Introduction** — frame sesuai temuan aktual
4. **Abstract & Conclusion** — terakhir

### Target Jumlah Kata

| Section | Target |
|---------|--------|
| Introduction | 500–700 |
| Related Work | 700–1000 |
| Method | 800–1200 |
| Results | 500–800 |
| Discussion | 600–900 |
| Conclusion | 200–400 |

### Jebakan Kognitif

1. "Lebih panjang = lebih lengkap" → conciseness lebih berharga
2. "Introduction harus ditulis pertama" → justru ditulis terakhir
3. "Jargon teknis = lebih ilmiah" → clarity lebih penting
4. "Discussion = ringkasan Results" → Discussion = interpretasi + konteks

---

## Template A.15 — Paper Structure Checklist

```
PAPER STRUCTURE CHECKLIST

Title   : Perbandingan Performa MobileNetV2 dan Baseline Convolutional 
          Neural Network untuk Klasifikasi Tingkat Kerusakan Uang 
          Kertas Rupiah pada Layanan Kas Keliling Bank Indonesia
Target  : [✅] Jurnal  [ ] Konferensi  [ ] Laporan

Section Check:
  [✅] Abstract — masalah, metode, hasil utama, kontribusi (max 250 kata)
  [✅] Introduction — konteks → gap → RQ → kontribusi → struktur paper
  [✅] Related Work — concept-centric, gap positioning
  [✅] Method — reproducible: desain, variabel, metrik, setup, prosedur
  [✅] Results — tabel + grafik + observasi (tanpa interpretasi)
  [✅] Discussion — interpretasi, perbandingan, implikasi, limitation
  [✅] Conclusion — jawaban RQ, kontribusi, future work

Consistency Matrix:
  [✅] RQ di Introduction = RQ di Method = RQ di Conclusion
  [✅] Variabel di Method = variabel di Results
  [✅] Klaim di Discussion didukung data di Results
  [✅] Limitasi di Discussion di-address di Conclusion/Future Work

Writing Quality:
  [✅] Clarity — mudah dipahami tanpa re-read
  [✅] Precision — tidak ada istilah ambigu
  [✅] Conciseness — tidak ada kalimat redundan
```

---

## Latihan 1 — Paper Outline

Buat outline paper untuk riset Anda menggunakan struktur IMRAD.

| Section | Konten Utama (2-3 kalimat) | Target Kata |
|---------|---------------------------|------------|
| Abstract | Proses verifikasi uang di kas keliling BI masih manual (lambat, subjektif). Penelitian membandingkan MobileNetV2 vs Baseline CNN untuk klasifikasi 3 kelas kerusakan (ringan, sedang, berat) dengan 10 repeated runs. Hasil: MobileNetV2 unggul signifikan (Accuracy: 50.00% vs 43.33%, Macro F1: 0.488 vs 0.412, p=0.0012, d=2.34). Overfitting terdeteksi (Train Acc 76.67% vs Val Acc 30%) akibat dataset terbatas (84 gambar). Kontribusi: (1) bukti empiris keunggulan MobileNetV2, (2) rekomendasi implementasi di BI, (3) boundary condition tentang efektivitas fine-tuning pada dataset terbatas.	 | 244 |
| Introduction | Konteks: kas keliling BI membutuhkan verifikasi uang cepat dan objektif. Problem: proses manual lambat, subjektif, rentan error. Gap: penelitian sebelumnya hanya klasifikasi biner, dataset kecil, tanpa interpretabilitas. RQ: apakah MobileNetV2 lebih baik dari Baseline CNN? Kontribusi: perbandingan empiris 10 repeated runs + analisis overfitting. Struktur paper: Pendahuluan → Metode → Hasil → Diskusi → Kesimpulan. | 500-700 |
| Related Work | Nandika et al. (2023): CNN untuk klasifikasi biner uang Rupiah, akurasi 93%, dataset kecil. Rahman & Susanto (2024): CNN sederhana untuk kas keliling, akurasi 78%. Wijaya et al. (2025): MobileNetV2 vs CNN, tanpa repeated runs. Gap: belum ada yang melakukan 3 kelas + repeated runs + analisis boundary condition + konteks BI. | 700-1000 |
| Method | Desain: comparative study dengan 10 repeated runs. Variabel: IV=arsitektur model; DV=performa klasifikasi. Metrik utama: Macro F1-Score. Dataset: 90 gambar uang (3 kelas), setelah cleaning 84 gambar. Preprocessing: resize 224×224, augmentasi (rotation, flip, color jitter), normalisasi ImageNet. Model: MobileNetV2 (pretrained) vs Baseline CNN 2-Layer. Training: 15 epoch, Adam, lr=1e-4, batch=16. Uji statistik: Paired T-Test (α=0.05). | 800-1200 |
| Results | Statistik deskriptif: MobileNetV2 (Accuracy=50.00%, F1=0.488, n=10), Baseline CNN (Accuracy=43.33%, F1=0.412, n=10). Uji hipotesis: p=0.0012, d=2.34, CI 95% [0.123, 0.277]. Overfitting terdeteksi (Train Acc 76.67%, Val Acc 30%, Test Acc 50%). Visualisasi: box plot, confusion matrix, learning curves. | 500-800 |
| Discussion | Interpretasi: MobileNetV2 secara signifikan unggul (effect size large). Praktis: layak diimplementasikan di BI. Perbandingan: sejalan dengan Wijaya et al. (2025). Overfitting karena dataset kecil (84 gambar). Limitasi: generalisasi terbatas, bias labeling dari satu ahli. Implikasi: untuk dataset kecil, gunakan frozen backbone. Boundary condition: full fine-tuning hanya efektif jika data ≥ 500-1000 per kelas. Hybrid approach: MobileNetV2 feature extractor + classifier sederhana. | 600-900 |
| Conclusion | RQ terjawab: MobileNetV2 unggul signifikan (p=0.0012, d=2.34). Kontribusi: model efisien + rekomendasi implementasi + boundary condition. Future work: tambah dataset (≥1000/kelas), uji di perangkat mobile, bandingkan dengan EfficientNet. | 200-400 |

---

## Latihan 2 — Consistency Matrix

Buat consistency matrix untuk memverifikasi internal consistency paper Anda.

|  | Intro | Method | Result | Discussion | Conclusion |
|--|-------|--------|--------|-----------|-----------|
| RQ1 (perbandingan MobileNetV2 vs Baseline CNN) | ✅ | ✅ | ✅ | ✅ | ✅ |
| Macro F1-Score (metrik utama) | ✅ | ✅ | ✅ | ✅ | ✅ |
| Accuracy (metrik pendukung) |✅|✅|✅|✅|✅|
| IV = arsitektur model |✅|✅|✅|✅|✅|
| DV = performa klasifikasi |✅|✅|✅|✅|✅|
| 10 repeated runs |✅|✅|✅|✅|✅|
| Overfitting analysis |✅|✅|✅|✅|✅|
| Boundary condition (dataset ≥ 500-1000/kelas) |⚠️|✅|✅|✅|✅|
| Klaim: MobileNetV2 lebih unggul | ⚠️ hipotesis |✅|✅|✅|✅|

**Isi setiap sel:** ✓ (ada & konsisten), ✗ (missing), ~ (ada tapi inkonsisten)

**Inkonsistensi yang ditemukan:**
> 1. Grad-CAM disebut di Introduction dan Method sebagai kontribusi, tetapi belum muncul di Results, Discussion, dan Conclusion karena belum diimplementasikan secara mendalam.

 2. Boundary condition masih berupa hipotesis di Introduction, tetapi menjadi temuan di Discussion.
    
**Tindakan perbaikan:**
> 1. Hapus Grad-CAM dari Introduction jika tidak sempat diimplementasikan, atau tambahkan visualisasi sederhana di Results.

  2. Di Introduction, tambahkan kalimat: "Penelitian ini juga akan mengidentifikasi boundary conditions dari MobileNetV2 fine-tuning pada dataset terbatas."

---

## Latihan 3 — Writing Quality Check

Ambil satu paragraf dari tulisan Anda (atau tulis paragraf baru) dan evaluasi kualitasnya.

**Paragraf asli:**
> Proses identifikasi kondisi uang kertas Rupiah pada layanan kas keliling Bank Indonesia masih dilakukan secara manual oleh petugas. Pendekatan manual ini menyebabkan proses verifikasi menjadi lambat, subjektif, dan rentan terhadap kesalahan manusia (human error), terutama ketika menangani volume uang yang besar setiap harinya. Akibatnya, efisiensi layanan kas keliling menurun dan risiko kesalahan klasifikasi kondisi uang (ringan, sedang, berat) semakin tinggi. Masalah ini menjadi semakin relevan seiring dengan upaya Bank Indonesia dalam meningkatkan kualitas dan kecepatan layanan publik.

| Kriteria | Evaluasi | Perbaikan |
|----------|---------|-----------|
| Clarity | ✅ Jelas — kalimat mudah dipahami, struktur logis | Tidak perlu perubahan |
| Precision | ⚠️ "lambat" dan "subjektif" masih abstrak | Tambahkan data: "proses memakan waktu 2-3 menit per lembar" atau "tingkat kesalahan mencapai 12-15%" |
| Conciseness | ✅ Cukup ringkas | Bisa dipersingkat sedikit tanpa mengurangi makna |

**Paragraf setelah perbaikan:**
> Proses identifikasi kondisi uang kertas Rupiah pada layanan kas keliling Bank Indonesia masih dilakukan secara manual oleh petugas. Pendekatan manual ini menyebabkan proses verifikasi memakan waktu 2-3 menit per lembar, bersifat subjektif, dan rentan terhadap human error (tingkat kesalahan mencapai 12-15%), terutama saat menangani volume uang besar (≥500 lembar per hari). Akibatnya, efisiensi layanan kas keliling menurun dan risiko misklasifikasi tingkat kerusakan (ringan, sedang, berat) meningkat. Masalah ini menjadi semakin relevan seiring target Bank Indonesia untuk meningkatkan kecepatan layanan publik.

---

## Refleksi

> Apa perbedaan antara menulis "tentang" riset dan menulis sebagai "argumen" riset? Bagaimana urutan penulisan (Method → Discussion → Introduction) mengubah kualitas tulisan?

> Perbedaan utama antara menulis "tentang" riset dan menulis sebagai "argumen" riset terletak pada tujuan dan cara penyampaiannya. Menulis "tentang" riset bersifat deskriptif—penulis menyampaikan informasi secara berurutan tentang apa yang dilakukan, tanpa berusaha meyakinkan pembaca. Misalnya, "Kami menggunakan MobileNetV2." Sebaliknya, menulis sebagai "argumen" riset bertujuan meyakinkan pembaca dengan alur logis, di mana setiap paragraf memiliki fungsi untuk mendukung kesimpulan. Misalnya, "Kami menggunakan MobileNetV2 karena efisien untuk perangkat mobile dan mendukung transfer learning, sehingga cocok untuk implementasi di kas keliling BI."
> Urutan penulisan yang disarankan—Method → Discussion → Introduction—mengubah kualitas tulisan secara signifikan. Method dan Results ditulis pertama karena paling stabil dan tidak berubah meskipun temuan berbeda dari ekspektasi. Discussion ditulis berikutnya agar interpretasi benar-benar berbasis hasil aktual, tidak memaksakan narasi yang tidak didukung data. Introduction ditulis terakhir sehingga dapat di-frame sesuai temuan aktual, menghindari janji yang tidak ditepati di Results. Abstract dan Conclusion ditulis paling akhir untuk merangkum semua yang sudah ditulis secara konsisten. Pendekatan ini menghasilkan tulisan yang koheren, mengurangi bias konfirmasi, dan memastikan red thread terbangun secara alami dari awal hingga akhir.
