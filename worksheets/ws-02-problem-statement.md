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
  Konteks  : Sistem deteksi kondisi dan nominal uang kertas berbasis citra digital untuk layanan keuangan

System Context
  Input       : Citra uang kertas dari kamera (smartphone/webcam)
  Process     : Preprocessing → Feature extraction → Klasifikasi menggunakan CNN/Hybrid CNN-Attention
  Output      : Label kondisi (bagus/rusak) + nominal uang
  Outcome     : Meningkatkan akurasi dan efisiensi deteksi uang secara otomatis
  Constraints : Dataset terbatas, variasi pencahayaan, keterbatasan perangkat (mobile)
  Stakeholders: Bank Indonesia, lembaga keuangan, masyarakat umum

Fenomena → Problem
  Fenomena yang diamati             : Identifikasi kondisi uang masih dilakukan secara manual dan bergantung pada manusia
  Gejala (symptom) yang terukur     : Proses lambat, subjektif, dan rawan kesalahan (human error)
  Masalah yang didiagnosis          : Model otomatis yang ada masih terbatas (hanya klasifikasi biner dan kurang robust)
  Masalah riset (researchable)      : Belum ada model yang mampu mengklasifikasikan kondisi dan nominal uang secara akurat dengan generalisasi tinggi pada berbagai kondisi lingkungan
  Variabel yang terukur             : Akurasi, precision, recall, F1-score, waktu inferensi, robustness terhadap noise & pencahayaan


Problem Quality Check
  [✓] Clarity — Apakah satu orang membaca akan paham?
  [✓] Measurability — Apakah ada metrik kuantitatif?
  [✓] Relevance — Apakah penting untuk domain?
  [✓] Testability — Apakah bisa gagal?
  [✓] Impact — Apakah ada kontribusi jika terjawab?

Problem Statement (1 paragraf):
  Proses identifikasi kondisi uang kertas saat ini masih dilakukan secara manual sehingga tidak efisien dan rentan terhadap kesalahan manusia. Penelitian sebelumnya telah menunjukkan bahwa metode Convolutional Neural Network (CNN) mampu mengklasifikasikan kondisi uang dengan akurasi yang tinggi, namun masih terbatas pada klasifikasi biner (uang bagus dan rusak) serta belum mampu mengidentifikasi nominal uang secara bersamaan. Selain itu, keterbatasan dataset dan kurangnya pengujian pada kondisi lingkungan yang bervariasi menyebabkan model memiliki kemampuan generalisasi yang rendah. Oleh karena itu, diperlukan pengembangan model yang lebih komprehensif dengan mengintegrasikan klasifikasi kondisi dan nominal uang, serta meningkatkan robustness model melalui pendekatan arsitektur yang lebih modern dan evaluasi yang lebih menyeluruh agar sistem dapat digunakan secara efektif dalam kondisi nyata.
```

---

## Latihan 1 — Dari Topik ke Masalah Riset

**Topik awal:** Deteksi uang kertas menggunakan CNN

| Tahap | Hasil |
|-------|-------|
| Reality | Identifikasi uang masih dilakukan secara manual |
| Observed Issue (Symptom) | Proses lambat dan sering terjadi kesalahan |
| Diagnosed Problem (Root Cause) | Tidak adanya sistem otomatis yang akurat dan robust |
| Researchable Problem | Model CNN belum mampu mengklasifikasikan kondisi + nominal uang secara optimal |
| Measurable Variable | Akurasi, precision, recall, F1-score, waktu inferensi |

**Apakah terjebak solution-first thinking?** [ ] Ya / [✓] Tidak
> Jika ya, kembali ke tahap mana? ________________________

---

## Latihan 2 — System Context Decomposition

Gambarkan konteks sistem dari masalah riset di Latihan 1.

| Komponen | Deskripsi |
|----------|----------|
| Input | Gambar uang dari kamera |
| Process | Preprocessing + CNN / Hybrid CNN-Attention |
| Output | Klasifikasi kondisi dan nominal uang |
| Outcome | Efisiensi meningkat, mengurangi human error |
| Constraints | Dataset terbatas, variasi lingkungan |
| Stakeholders | Bank, masyarakat, peneliti |

**Komponen mana yang paling relevan dengan masalah riset?** Process (model dan metode klasifikasi)

---

## Latihan 3 — Problem Quality Check

Evaluasi problem statement yang sudah dibuat menggunakan 5 kriteria.

| Kriteria | Skor (1-5) | Justifikasi |
|----------|-----------|-------------|
| Clarity | 5 | Masalah jelas dan spesifik |
| Measurability | 5 | Menggunakan metrik kuantitatif |
| Relevance | 5 | Penting untuk sektor keuangan dan AI |
| Testability | 5 | Bisa diuji melalui eksperimen |
| Impact | 5 | Berpotensi diterapkan di dunia nyata |

**Skor total:** 25 / 25

**Problem statement versi final (1 paragraf):**
> Proses identifikasi kondisi dan nominal uang kertas masih dilakukan secara manual sehingga tidak efisien dan rentan terhadap kesalahan manusia. Penelitian sebelumnya telah menggunakan CNN untuk klasifikasi citra, namun umumnya hanya terbatas pada deteksi kondisi uang tanpa integrasi identifikasi nominal serta memiliki keterbatasan dalam generalisasi akibat dataset yang terbatas. Oleh karena itu, diperlukan pengembangan model berbasis deep learning yang mampu mengklasifikasikan kondisi dan nominal uang secara simultan dengan performa yang terukur menggunakan akurasi, precision, recall, dan F1-score, serta memiliki ketahanan terhadap variasi kondisi lingkungan agar dapat digunakan secara efektif dalam aplikasi nyata.

---

## Refleksi

> Bandingkan "masalah" yang biasa ditemui saat coding (bug, error) dengan masalah riset. Apa perbedaan fundamental dalam cara mendefinisikan dan mendekati keduanya?

**Jawaban:**
> Masalah dalam coding biasanya berupa error atau bug yang harus segera diperbaiki agar sistem dapat berjalan dengan benar. Fokusnya adalah menemukan solusi praktis secepat mungkin. Sedangkan masalah riset berfokus pada kesenjangan pengetahuan yang harus dianalisis dan dibuktikan secara ilmiah. Dalam riset, masalah harus dirumuskan secara jelas, terukur, dan dapat diuji, serta tidak selalu harus “diselesaikan” tetapi harus dipahami dan divalidasi melalui metode yang sistematis.
