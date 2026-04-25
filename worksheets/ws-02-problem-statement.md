# WS-02: Problem Statement

> **Bab 2 — Problem Formulation & System Context**

---

## Ringkasan Materi

### Problem Formation Model

Masalah riset melewati 5 tahap transformasi. Melompat langsung dari Reality ke Variable adalah kesalahan paling umum.

```
Reality → Observed Issue (Symptom) → Diagnosed Problem (Root Cause)
→ Researchable Problem (Scoped) → Measurable Variable (Operationalized)
```

### Topic ≠ Problem ≠ Research Problem

| Level | Contoh | Status |
|-------|--------|--------|
| **Topik** | Keamanan IoT | Terlalu luas, tidak bisa diuji |
| **Problem** | MQTT tidak terenkripsi | Spesifik tapi belum riset |
| **Research Problem** | Belum ada studi membandingkan overhead TLS 1.3 vs DTLS pada MQTT di IoT RAM < 64KB | Bisa dirancang eksperimennya |

### Symptom vs Root Cause

Apa yang diamati (gejala) ≠ mengapa terjadi (akar masalah). Gunakan **5 Whys** atau **Fishbone Diagram** untuk menggali.

Contoh: "User meninggalkan checkout" (symptom) → "Waktu loading > 8 detik karena API call sequential" (root cause).

### System Thinking

Setiap masalah riset TI harus terikat pada komponen sistem: **Input → Process → Output → Outcome → Constraints → Stakeholders**.

### Problem Quality Check

Masalah riset yang layak harus memenuhi 5 kriteria:
- **Clarity** — Satu orang membaca akan paham
- **Measurability** — Ada metrik kuantitatif
- **Relevance** — Penting untuk domain
- **Testability** — Bisa gagal (falsifiable)
- **Impact** — Ada kontribusi jika terjawab

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan | Menyelesaikan masalah (*solve*) | Memahami dan membuktikan (*understand & prove*) |
| Masalah | Bug, error, fitur belum ada | Gap dalam pengetahuan |
| Scope | Selesaikan semua yang perlu | Batasi agar bisa dibuktikan |
| Output | Working system | Evidence, paper, replicable findings |

### Istilah Penting

- **Problem Statement** — Formulasi tertulis: konteks sistem + gap + dampak + justifikasi
- **System Context** — Deskripsi lengkap: input, proses, output, outcome, constraints, stakeholders
- **Problem Drift** — Masalah "bermutasi" dari pendahuluan ke metodologi karena statement awal tidak presisi
- **Solution-First Thinking** — Memulai dari solusi tanpa masalah yang jelas — berbahaya dalam riset
- **Operational Definition** — Definisi variabel yang cukup jelas agar peneliti lain bisa mengukur hal yang sama

---

## Template A.2 — Problem Statement Builder

```
PROBLEM STATEMENT BUILDER

Domain & Konteks
  Domain   : Computer Vision / Deep Learning
  Konteks  : Sistem deteksi kondisi uang kertas berbasis citra digital untuk mendukung layanan keuangan

System Context
  Input       : Citra uang kertas dari kamera (smartphone/webcam)
  Process     : Preprocessing → Feature extraction → Klasifikasi menggunakan CNN / CNN + Attention
  Output      : Label kondisi (bagus/rusak)
  Outcome     : Meningkatkan akurasi dan efisiensi deteksi uang secara otomatis
  Constraints : Dataset terbatas (hanya 500 gambar), variasi pencahayaan terbatas, 
                sudut pengambilan gambar seragam (kondisi ideal)
  Stakeholders: Bank Indonesia, lembaga keuangan, masyarakat, peneliti

Fenomena → Problem
  Fenomena yang diamati             : Identifikasi kondisi uang kertas masih dilakukan 
                                      secara manual di lapangan (layanan kas keliling Bank Indonesia).
  Gejala (symptom) yang terukur     : Proses lambat, subjektif, dan rentan human error.
  Masalah yang didiagnosis          : Model CNN yang dikembangkan Nandika (2025) mencapai akurasi 
                                      93%, namun: (a) menggunakan dataset kecil (500 gambar) 
                                      dengan kondisi ideal, (b) tidak ada uji robustness terhadap 
                                      variasi pencahayaan, (c) tidak ada evaluasi interpretabilitas.
  Masalah riset (researchable)      : Belum ada evaluasi eksperimental yang secara kuantitatif 
                                      mengukur sejauh mana variasi kondisi lingkungan (pencahayaan, 
                                      sudut, dan noise) mempengaruhi performa model CNN, serta 
                                      apakah model benar-benar memfokuskan pada fitur kerusakan 
                                      yang relevan berdasarkan analisis interpretabilitas.
  Variabel yang terukur             : Akurasi, precision, recall, F1-score, serta:
                                      (1) perubahan akurasi pada variasi pencahayaan (terang, redup, gelap).
                                      (2) perubahan akurasi pada variasi sudut (0°, 45°, 90°).
                                      (3) perubahan akurasi pada noise (Gaussian/blur).
                                      (4) analisis fokus fitur menggunakan Grad-CAM.


Problem Quality Check
  [✓] Clarity — Masalah jelas dan spesifik (dataset kecil, kondisi ideal, tidak ada interpretabilitas)
  [✓] Measurability — Menggunakan metrik kuantitatif + evaluasi interpretabilitas
  [✓] Relevance — PPenting untuk domain computer vision dan sektor keuangan (kas keliling BI)
  [✓] Testability — Bisa diuji melalui eksperimen dengan variasi kondisi lingkungan
  [✓] Impact — Berpotensi meningkatkan validitas dan keandalan model di dunia nyata

Problem Statement (1 paragraf):
   Proses identifikasi kondisi uang kertas di layanan kas keliling Bank Indonesia saat ini masih dilakukan secara manual sehingga tidak efisien dan rentan terhadap kesalahan manusia. Penelitian Nandika (2025) telah mengembangkan model CNN untuk klasifikasi kondisi uang dengan akurasi 93%, namun model tersebut dilatih pada dataset terbatas (500 gambar) dengan kondisi pengambilan yang ideal sehingga kemampuan generalisasinya terhadap kondisi dunia nyata belum teruji. Selain itu, penelitian tersebut belum mengevaluasi aspek interpretabilitas model, sehingga tidak diketahui apakah model benar-benar memfokuskan pada fitur kerusakan yang relevan seperti sobekan atau noda, atau justru pada artefak latar belakang yang tidak relevan. Oleh karena itu, diperlukan evaluasi eksperimental yang mengukur pengaruh variasi kondisi lingkungan terhadap performa model CNN serta analisis interpretabilitas menggunakan Grad-CAM guna memastikan validitas dan keandalan model dalam kondisi operasional nyata.
```

## Latihan 1 — Dari Topik ke Masalah Riset

**Topik awal:** Deteksi uang kertas menggunakan CNN

| Tahap | Hasil |
|-------|-------|
| Reality | Identifikasi uang di layanan kas keliling BI masih manual |
| Observed Issue (Symptom) | Proses lambat, subjektif, dan rentan human error |
| Diagnosed Problem (Root Cause) | Model CNN memiliki akurasi tinggi namun belum diuji pada variasi kondisi lingkungan dan tidak memiliki evaluasi interpretabilitas
| Researchable Problem | Belum ada evaluasi eksperimental terhadap pengaruh variasi lingkungan terhadap performa CNN serta analisis interpretabilitas fokus model |
| Measurable Variable | Akurasi, precision, recall, F1-score, perubahan akurasi pada variasi kondisi, dan Grad-CAM |

**Apakah terjebak solution-first thinking?** [ ] Ya / [✓] Tidak
> Jika ya, kembali ke tahap mana? ________________________
---

## Latihan 2 — System Context Decomposition

Gambarkan konteks sistem dari masalah riset di Latihan 1.

| Komponen | Deskripsi |
|----------|----------|
| Input | Gambar uang dari kamera smartphone |
| Process | Preprocessing → CNN classification |
| Output | Klasifikasi kondisi (bagus/rusak) |
| Outcome | Efisiensi meningkat, mengurangi human error |
| Constraints | Dataset kecil, kondisi ideal |
| Stakeholders | Bank Indonesia, lembaga keuangan, masyarakat, peneliti |

**Komponen mana yang paling relevan dengan masalah riset?** Process (model CNN dan keterbatasannya)

Constraints (dataset kecil, kondisi ideal)
---

## Latihan 3 — Problem Quality Check

Evaluasi problem statement yang sudah dibuat menggunakan 5 kriteria.

| Kriteria | Skor (1-5) | Justifikasi |
|----------|-----------|-------------|
| Clarity | 5 | Masalah spesifik dan berbasis evidence |
| Measurability | 5 | Variabel jelas dan operasional |
| Relevance | 5 | Relevan untuk AI dan sektor keuangan |
| Testability | 5 | Bisa diuji melalui eksperimen |
| Impact | 5 | Meningkatkan keandalan model di kondisi nyata |

**Skor total:** 25 / 25

**Problem statement versi final (1 paragraf):**
> Proses identifikasi kondisi uang kertas di layanan kas keliling Bank Indonesia saat ini masih dilakukan secara manual sehingga tidak efisien dan rentan terhadap kesalahan manusia. Penelitian Nandika (2025) telah mengembangkan model CNN dengan akurasi 93%, namun masih memiliki keterbatasan dalam hal generalisasi karena dilatih pada dataset terbatas dengan kondisi ideal serta belum dievaluasi dari sisi interpretabilitas. Oleh karena itu, penelitian ini berfokus pada evaluasi eksperimental terhadap pengaruh variasi kondisi lingkungan terhadap performa model CNN serta analisis interpretabilitas menggunakan Grad-CAM untuk memastikan validitas dan keandalan model dalam kondisi operasional nyata.
---

## Refleksi

> Bandingkan "masalah" yang biasa ditemui saat coding (bug, error) dengan masalah riset. Apa perbedaan fundamental dalam cara mendefinisikan dan mendekati keduanya?

**Jawaban:**
> Masalah dalam coding biasanya berupa error atau bug yang harus segera diperbaiki agar sistem dapat berjalan dengan benar. Fokusnya adalah menemukan solusi praktis secepat mungkin. Sedangkan masalah riset berfokus pada kesenjangan pengetahuan yang harus dianalisis dan dibuktikan secara ilmiah. Dalam riset, masalah harus dirumuskan secara jelas, terukur, dan dapat diuji, serta tidak selalu harus "diselesaikan" tetapi harus dipahami dan divalidasi melalui metode yang sistematis.
