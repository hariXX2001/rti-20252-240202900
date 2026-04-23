# WS-01: Distorsi & Paradigma

> **Bab 1 — Research Mindset in IT**

---

## Ringkasan Materi

### Research Trust Model

Pengetahuan ilmiah tidak muncul langsung dari kenyataan. Ia melewati **6 tahap transformasi** yang masing-masing rawan distorsi:

```
Reality → Data → Processing → Analysis → Inference → Knowledge
```

Etika mencegah distorsi yang disengaja (fabrikasi, cherry-picking). Validitas mendeteksi distorsi yang tidak disengaja (confounding variable, sampling bias).

### Tiga Jenis Validitas

| Jenis | Pertanyaan | Contoh Ancaman |
|-------|-----------|----------------|
| **Internal Validity** | Apakah hubungan kausal benar ada? | Confounding variable |
| **External Validity** | Apakah bisa digeneralisasi? | Dataset terlalu homogen |
| **Construct Validity** | Apakah mengukur hal yang benar? | Metrik tidak sesuai klaim |

### Paradigma Riset

Mata kuliah ini menggunakan pendekatan **Positivist** (fenomena TI bisa diukur objektif melalui eksperimen terkontrol) diperkuat **Design Science Research** (DSR). Penting untuk membedakan keduanya:

| Paradigma | Cara Kerja | Contoh di TI |
|-----------|-----------|---------------|
| **Positivis** | Uji hipotesis dengan eksperimen terkontrol | Apakah CNN lebih akurat dari RF pada dataset X? |
| **Design Science Research** | Bangun artefak (sistem/model/framework) untuk menguji proposisi | Dapatkah arsitektur hybrid CNN+LSTM membuktikan peningkatan recall ≥5%? |
| **Interpretivis** | Pahami makna melalui konteks & kualitatif | Bagaimana peneliti manafsirkan anomali data sensor IoT? |

Dalam DSR, artefak **bukan tujuan akhir** — ia adalah instrumen untuk menghasilkan pengetahuan. Pertanyaan riset tetap harus difalsifikasi.

### Mode Berpikir Peneliti

**Curious** (mempertanyakan fenomena) → **Critical** (mengevaluasi klaim berdasarkan bukti) → **Systematic** (merancang investigasi terstruktur dan reproducible).

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan | Membuat sistem yang bekerja | Menghasilkan pengetahuan yang valid |
| Pertanyaan khas | "Bagaimana membuatnya jalan?" | "Apakah klaim ini benar?" |
| Ukuran sukses | Sistem berfungsi, client puas | Hipotesis terjawab, temuan tervalidasi |
| Kegagalan | Harus dihindari | Harus dilaporkan (negative result = kontribusi) |

### Istilah Penting

- **Research Mindset** — Pola pikir yang menuntut bukti dan mempertanyakan asumsi
- **Research Ethics** — Prinsip perilaku: kejujuran, objektivitas, keterbukaan, akuntabilitas
- **HARKing** — Hypothesizing After Results are Known — merumuskan hipotesis setelah melihat data
- **Falsifiability** — Hipotesis harus bisa dibuktikan salah

---

## Template A.1 — Research Mindset Self-Assessment

```
Nama Peneliti    : Hari Cahyono
Tanggal          : 23 April 2026

1. Ketika membaca klaim "metode X 95% akurat":
   - Pertanyaan pertama saya: Bagaimana komposisi dataset yang digunakan (apakah seimbang antara uang bagus dan rusak) dan apakah akurasi tersebut diuji pada data yang benar-benar baru (unseen data)?
   - Data yang dibutuhkan untuk verifikasi: Distribusi kelas dataset, detail augmentasi data yang dilakukan, serta matriks evaluasi lengkap seperti Precision, Recall, dan F1-Score untuk memastikan tidak ada bias pada satu kelas.

2. Posisi paradigma:
   - Pendekatan: [ ] Positivis  [ ] Interpretivis  [X] Design Science  [ ] Mixed
   - Alasan: Penelitian ini berfokus pada pengembangan sebuah artefak (sistem deteksi berbasis CNN dengan GUI MATLAB) untuk memecahkan masalah praktis (identifikasi uang layak edar) dan menguji efektivitas artefak tersebut melalui eksperimen terukur.

3. Identifikasi distorsi:
   - Asumsi tersembunyi: Diasumsikan bahwa kondisi pencahayaan dan sudut pengambilan gambar saat implementasi nyata akan selalu seragam seperti saat pengambilan dataset mandiri.
   - Sumber bias potensial: Penggunaan 50 gambar hasil editan dan pengambilan gambar secara mandiri yang mungkin kurang merepresentasikan variasi kerusakan uang di dunia nyata secara luas.
   - Langkah mitigasi: Mengintegrasikan Mekanisme Atensi (CBAM) untuk menangkap fitur kerusakan mikro dan menerapkan Grad-CAM sebagai alat validasi visual guna memastikan model mengklasifikasi berdasarkan indikator kerusakan yang benar, bukan fitur latar belakang (bias).

4. Komitmen etika:
   - Data yang tidak akan dimanipulasi: Hasil akurasi validasi (93%), precision (97%), dan recall (96%) serta hasil survei dari pihak Bank Indonesia.
   - Batasan yang diakui sejak awal: Sistem saat ini masih berbasis desktop (MATLAB GUI) dan belum tersedia dalam platform mobile untuk masyarakat luas.
```

---

## Latihan 1 — Identifikasi Distorsi

Deteksi Kondisi Uang Bagus Dan Rusak Dengan Pengolahan Citra Digital Berbasis Convolutional Neural Network (CNN) (2025)

**Paper yang dipilih:**
> Judul: Deteksi Kondisi Uang Bagus Dan Rusak Dengan Pengolahan Citra Digital Berbasis Convolutional Neural Network (CNN)
> Penulis (Tahun): Arjun Putra Nandika, dkk. (2025)
> Sumber/Link DOI: Jurnal Media Infotama Vol. 21 No. 1 / https://jurnal.unived.ac.id/index.php/jmi/article/view/8121

| Tahap | Apa yang Dilakukan | Potensi Distorsi |
|-------|-------------------|-----------------|
| Reality → Data | Mengumpulkan 500 gambar uang (250 bagus, 250 rusak) menggunakan kamera smartphone dan internet. | Sampling Bias: Data diambil dengan pencahayaan cukup dan sudut seragam, sehingga kurang mewakili realitas uang lusuh di lapangan yang seringkali kotor atau minim cahaya. |
| Data → Processing |Melakukan resizing menjadi 224x224 piksel dan teknik augmentasi data (rotasi, zoom, dll).|Processing Artifacts: Proses kompresi gambar dapat menghilangkan detail tekstur halus yang sebenarnya merupakan indikator utama uang tersebut masuk kategori "rusak".|
| Processing → Analysis |Melatih model CNN menggunakan optimizer Adam selama 50 epoch dengan pembagian data 80:20.|Overfitting: Jumlah 500 data untuk model Deep Learning tergolong kecil, berisiko membuat model hanya "menghafal" dataset yang ada tanpa kemampuan generalisasi yang baik.|
| Analysis → Inference |Menghasilkan akurasi 93%, precision 97%, dan recall 96% berdasarkan data pengujian.|Metric Bias: Fokus pada akurasi tinggi tanpa visualisasi (seperti Grad-CAM) membuat kita tidak tahu apakah model benar-benar melihat "kerusakan" atau hanya melihat "noise" latar belakang.|
| Inference → Knowledge |Menyimpulkan bahwa sistem efektif untuk membantu layanan kas keliling Bank Indonesia.|Generalization Error: Kesimpulan ini mungkin belum tentu valid jika sistem diuji pada perangkat mobile dengan spesifikasi kamera yang jauh berbeda dari saat penelitian.|

**Distorsi paling besar di tahap:** Reality → Data (Karena keterbatasan jumlah dan variasi sampel asli dari lapangan).

**Dua distorsi spesifik yang teridentifikasi:**
1. Environmental Bias: Pengambilan gambar dilakukan dalam kondisi ideal (pencahayaan cukup dan sudut stabil), sehingga model rentan gagal saat menghadapi variasi pencahayaan ekstrem di dunia nyata.
2. Feature Distortion: Penggunaan 50 gambar yang dimanipulasi secara digital untuk kategori "rusak" mungkin menciptakan pola fitur yang berbeda dengan kerusakan fisik asli (seperti uang yang termakan rayap atau luntur).

---

## Latihan 2 — Analisis Kasus Etika

Skenario: Seorang peneliti menemukan bahwa jika 3 data point outlier dihapus, hasil eksperimennya menjadi signifikan. Dengan outlier, hasilnya tidak signifikan.

| Perspektif | Analisis |
|------------|---------|
| Kejujuran ilmiah |Melaporkan temuan apa adanya. Menghapus outlier hanya agar hasil terlihat bagus tanpa alasan teknis yang kuat (seperti kerusakan sensor atau kesalahan input) dianggap sebagai malpraktik ilmiah.|
| Transparansi |Menjelaskan kriteria pembersihan data secara eksplisit. Jika outlier dihapus, peneliti harus mendokumentasikan mengapa data tersebut dianggap tidak valid dalam bagian metodologi.|
| Peer review |Memberikan kesempatan bagi penelaah untuk menguji integritas hasil. Kejujuran mengenai kegagalan prediksi pada data tertentu justru membantu komunitas memahami batasan model.|

**Keputusan akhir dan justifikasi:**
> Keputusan: Melaporkan kedua hasil (dengan dan tanpa outlier) atau tetap menyertakan outlier dengan memberikan penjelasan logis mengenai kegagalan deteksi pada titik tersebut.
> Justifikasi: Dalam deteksi uang rusak, outlier bisa jadi merupakan kondisi uang yang sangat ekstrem yang tidak dikenali model. Melaporkan kegagalan ini jauh lebih bernilai untuk pengembangan sistem yang robust daripada memaksakan signifikansi yang semu.

---

## Latihan 3 — Posisi Paradigma

**Topik riset:** Pengembangan Sistem Deteksi Kelayakan Uang Rupiah berbasis Hybrid CNN-Attention Network dengan Visualisasi Grad-CAM.

> **Skala 1–5:** 1 = tidak sesuai sama sekali dengan topik ini, 5 = sangat sesuai dan dominan digunakan pada riset bertopik serupa.

| Kriteria | Positivis | Interpretivis | Design Science |
|----------|-----------|---------------|----------------|
| Kesesuaian dengan topik (1–5) | 4 — Karena melibatkan uji hipotesis kuantitatif pada akurasi model. | *1 — Tidak sesuai karena tidak mengkaji makna sosial atau perilaku subjek secara kualitatif. | *5 — Sangat sesuai karena fokus utamanya adalah membangun artefak (sistem/model) untuk solusi praktis.|
| Jenis data yang dikumpulkan | Metrik performa (Akurasi, F1-Score, Loss) | - | Evaluasi artefak, kecepatan komputasi, dan peta panas (heatmap) Grad-CAM |
| Limitasi paradigma |Seringkali mengabaikan konteks kegunaan praktis bagi pengguna akhir|Terlalu subjektif dan sulit direplikasi dalam skala teknis |Fokus pada fungsionalitas artefak terkadang mengabaikan pengembangan teori dasar|

**Paradigma yang dipilih:** Design Science Research (DSR) diperkuat dengan pendekatan Positivis.
**Alasan:** Riset ini bertujuan menciptakan solusi teknologi (artefak) berupa model CNN yang ditingkatkan. Validitasnya diukur secara objektif melalui eksperimen terkontrol (Positivis), namun tujuan akhirnya adalah kegunaan artefak tersebut dalam menyelesaikan masalah di dunia nyata (DSR).

---

## Refleksi

> Sebelum membaca materi ini, apakah pernah mempertanyakan klaim "95% akurat"? Setelah memahami rantai distorsi, pertanyaan apa yang sekarang akan diajukan saat membaca paper?

**Jawaban:**
> "Apakah 95% itu dihasilkan dari dataset yang seimbang, atau jangan-jangan hanya akurat di satu kelas saja?"
> "Bagaimana model mengambil keputusan tersebut? Apakah model benar-benar mendeteksi kerusakan uang atau hanya terdistorsi oleh noise pada latar belakang foto?"
> "Apakah hasil ini tetap valid jika diuji pada kondisi pencahayaan yang berbeda dari saat pengambilan data awal?"
