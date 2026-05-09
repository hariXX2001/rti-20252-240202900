# WS-03: Literature Mapping & Gap

> **Bab 3 — Literature Review, Research Gap & Baseline**

---

## Ringkasan Materi

### Literature Review = Positioning, Bukan Ringkasan

Literature review bukan merangkum paper satu per satu. Pendekatan yang benar adalah **concept-centric** — organisasi berdasarkan tema, metode, atau variabel. Tujuan: menemukan **pola, kontradiksi, dan gap**.

**Perbandingan pendekatan Author-centric vs Concept-centric:**

| Aspek | Author-centric (Hindari) | Concept-centric (Gunakan) |
|-------|--------------------------|---------------------------|
| Struktur | Per penulis/paper ("Rahman et al. menyatakan...") | Per konsep/metode ("Pendekatan berbasis transformer") |
| Tujuan | Ringkasan isi paper | Perbandingan metode & identifikasi gap |
| Contoh paragraph | "Rahman (2023) pakai CNN. Lee (2022) pakai LSTM. Zhang (2021) pakai RF." | "Tiga pendekatan dominan: CNN digunakan oleh 4 paper untuk representasi fitur visual; LSTM untuk data sekuensial; RF sebagai baseline klasik." |
| Hasil akhir | Daftar paper | Peta pengetahuan + gap yang teridentifikasi |

### Empat Jenis Research Gap

| Jenis Gap | Deskripsi | Contoh |
|-----------|----------|--------|
| **Performance Gap** | Performa belum memadai | Akurasi deteksi hanya 78% pada kasus tertentu |
| **Method Gap** | Pendekatan belum diterapkan | Belum ada yang pakai transformer untuk task ini |
| **Data Gap** | Dataset terbatas/tidak representatif | Semua studi pakai dataset sintetis |
| **Context Gap** | Belum diuji pada konteks berbeda | Belum ada evaluasi di negara berkembang |

Gap terkuat = kombinasi 2+ jenis.

### Systematic Search Strategy

1. **Database utama**: IEEE Xplore, ACM DL, Scopus
   - Akses IEEE/ACM melalui jaringan kampus atau VPN institusi
   - Alternatif bebas biaya: Google Scholar, ResearchGate ([researchgate.net](https://www.researchgate.net)), arXiv ([arxiv.org](https://arxiv.org))
2. **Boolean query** yang terdokumentasi eksplisit
   - Contoh: `("anomaly detection" OR "intrusion detection") AND ("deep learning" OR "neural network") NOT ("medical imaging")`
   - Gunakan tanda kutip untuk frasa eksak; AND/OR/NOT mengontrol scope
3. **Snowballing** — dua arah:
   - **Backward snowballing**: buka daftar referensi di paper kunci → telusuri paper yang dikutip
   - **Forward snowballing**: di Google Scholar, klik "Cited by" di bawah paper kunci → temukan paper yang mengutipnya
   - Ulangi 1–2 tingkat untuk membangun cakupan komprehensif
4. Klaim "belum ada penelitian" harus didukung **bukti pencarian**

### Baseline Selection — 3 Kriteria

| Kriteria | Pertanyaan |
|----------|-----------|
| **Relevan** | Apakah menyelesaikan masalah yang sama? |
| **Representatif** | Apakah mewakili common practice? |
| **State-of-the-Art** | Apakah terbaru/terbaik? |

Membandingkan deep learning 2024 dengan decision tree sederhana tanpa justifikasi = **straw man comparison** (perbandingan tidak jujur).

### Research vs Engineering

| Aspek | Engineering | Research |
|-------|------------|----------|
| Tujuan baca literatur | Mencari solusi yang sudah ada | Memahami apa yang belum terjawab |
| Cara membaca paper | Tutorial, how-to | Metode, limitasi, gap |
| Baseline | Framework terpopuler | State-of-the-art yang rigorous |
| Dokumentasi pencarian | Tidak diperlukan | Wajib (reproducible) |

### Istilah Penting

- **Concept-centric** — Organisasi literatur berdasarkan konsep/metode, bukan per penulis
- **Snowballing** — Backward (telusuri referensi) + Forward (cari yang mengutip paper kunci)
- **Research Position** — Pernyataan eksplisit posisi riset terhadap studi sebelumnya
- **Straw man comparison** — Memilih baseline lemah agar metode sendiri terlihat lebih baik

---

## Template A.3 — Literature Mapping & Gap Identification

```
LITERATURE MAPPING

Topik      : Klasifikasi Tingkat Kerusakan Uang Rupiah (Ringan, Sedang, Berat) berbasis Deep Learning
Database   : Google Scholar, IEEE Xplore, Garuda, ResearchGate, arXiv
Query      : ("uang rupiah" OR "rupiah banknote" OR "currency damage" OR "banknote fitness" OR "kerusakan uang") AND ("CNN" OR "Convolutional Neural Network" OR "deep learning" OR "MobileNet")
Tahun      : 2021 sampai dengan 2025
Hasil awal : 35 paper → Screening → 7 paper final

Literature Matrix (concept-centric):

#,Study,Tahun,Method,Dataset,Result,Limitasi
1,Nandika et al.,2025,CNN standar,500 gambar (bagus vs rusak),Akurasi 93%,"Hanya klasifikasi biner, dataset kecil, tidak ada tingkat kerusakan"
2,Roshan et al.,2025,VGG16,Gambar uang Rupiah,Akurasi tinggi,"Klasifikasi biner (bagus/rusak), belum granular"
3,Jaman et al.,2025,CNN + Mobile App,Bangladesh banknotes,Akurasi ~95%+,"Negara berbeda, fokus recognition + counterfeit, minim damage level"
4,Kerdsri & Treeratpitu (Bank of Thailand),2021,ResNet-101,Banknote defect (multi-denomination),Baik untuk defect classification,"Bukan Rupiah, fokus defect spesifik, tidak granular ringan-sedang-berat"
5,Neto et al.,2023,CNN,Brazilian Real banknotes,Akurasi baik untuk damaged notes,"Konteks Brasil, lebih ke recognition untuk tunanetra"
6,Swain et al.,2023,RFE-CNN,Indian currency,Akurasi tinggi,"Fokus recognition & fake, bukan tingkat kerusakan fisik"
7,Veeramsetty et al.,2022,CNN-based,Indian banknotes,Baik untuk feature extraction,"Dataset India, tidak spesifik tingkat kerusakan"

Pola yang ditemukan:
  Metode dominan     : CNN dan variannya (VGG16, ResNet, MobileNet).
  Dataset umum       : Dataset mandiri berukuran kecil hingga sedang (500–2000 gambar), sering diambil dalam kondisi terkontrol.
  Limitasi berulang  : Mayoritas hanya klasifikasi biner (bagus/rusak atau asli/palsu), sedikit yang melakukan klasifikasi granular tingkat kerusakan, minim pengujian pada kondisi lapangan (pencahayaan variatif, kamera smartphone).

GAP IDENTIFICATION

Gap 1: [Data Gap + Method Gap]
  Deskripsi    : Hampir semua studi hanya melakukan klasifikasi biner (bagus/rusak). Sangat sedikit yang mengklasifikasikan tingkat kerusakan secara granular (ringan, sedang, berat) khusus untuk uang Rupiah. Belum ada yang mengoptimalkan model ringan seperti MobileNetV2 untuk kasus ini.
  Bukti        : Dari 7 paper (2021–2025) yang direview, semuanya masih dominan biner atau recognition saja.
  Signifikansi : Membantu Bank Indonesia dalam proses sortir uang tidak layak edar yang lebih presisi, terutama mendeteksi kerusakan ringan (sobek kecil, luntur pinggiran) yang sering lolos.

Gap 2: [Context Gap + Performance Gap]
  Deskripsi    : Model existing jarang diuji pada variasi kerusakan ringan di kondisi nyata Indonesia (pencahayaan tidak ideal, kamera ponsel).
  Bukti        : Dari 7 paper (2021–2025) yang direview, semuanya masih dominan biner atau recognition saja.
  Signifikansi : Membantu Bank Indonesia dalam proses sortir uang tidak layak edar yang lebih presisi, terutama mendeteksi kerusakan ringan (sobek kecil, luntur pinggiran) yang sering lolos.

Baseline Selection:
Baseline,Relevansi,Representatif,Sumber
CNN 2-Layer,Tinggi,Pendekatan dasar paling umum,Nandika et al. (2025)
VGG16,Tinggi,Arsitektur klasik yang kuat,Roshan et al. (2025)
---

## Latihan 1 — Concept-Centric Literature Table

Gunakan topik riset dari WS-02. Cari minimal 5 paper relevan menggunakan database akademik.

> **Panduan pencarian:**
> - Database: IEEE Xplore, ACM DL, Google Scholar, atau ResearchGate
> - Tulis query Boolean yang digunakan: contoh `("object detection" OR "image classification") AND ("edge computing") NOT ("medical")`. Dokumentasikan query secara eksplisit.
> - Akses gratis: buka Google Scholar → cari judul paper → klik [PDF] jika tersedia, atau akses lewat campus VPN

**Topik riset:** Klasifikasi Tingkat Kerusakan Uang Rupiah (Ringan, Sedang, Berat) berbasis Deep Learning
**Query pencarian:** ("uang rupiah" OR "rupiah banknote" OR "kerusakan uang" OR "banknote damage") AND ("CNN" OR "Convolutional Neural Network" OR "deep learning" OR "VGG16" OR "MobileNet")
**Database:** Google Scholar, Garuda, IEEE Xplore, ResearchGate

| # | Study | Tahun | Method | Dataset | Result | Limitasi |
|---|-------|-------|--------|---------|--------|----------|
| 1 | Nandika et al. | 2025 | CNN Standar | 500 gambar (bagus vs rusak)| Akurasi 93% | Hanya klasifikasi biner, dataset kecil |
| 2 | Roshan et al. | 2025 | VGG16 |Gambar Rupiah (3 tingkat kerusakan: >20%, >40%, >50%)| Akurasi 93.33%| Masih terbatas variasi kerusakan ringan & kondisi lapangan |
| 3 | Albani et al.| 2024 |Xception (Transfer Learning) | Uang Rupiah tidak layak edar| Akurasi tinggi| Fokus tidak layak edar (bukan granular ringan-sedang-berat)|
| 4 |Kurniadi et al. |2025|CNN + KAN |Uang Rupiah denomination |Baik untuk klasifikasi |Lebih ke denomination daripada tingkat kerusakan|
| 5 | Veeramsetty et al. / Indian study| 2022-2023|CNN-based |Indian banknotes |Akurasi baik|Konteks negara lain, tidak spesifik Rupiah |

**Pola yang terlihat — Metode dominan:** Metode dominan: Convolutional Neural Network (CNN) dan Transfer Learning (VGG16, Xception).
**Limitasi yang berulang:** Mayoritas penelitian masih menggunakan klasifikasi biner (bagus/rusak), dataset relatif kecil, variasi kerusakan ringan kurang dieksplorasi, dan minim pengujian pada kondisi lapangan (pencahayaan variatif, kamera smartphone).
---

## Latihan 2 — Gap Identification

Berdasarkan tabel di Latihan 1, identifikasi gap.

| Jenis Gap | Ditemukan? | Gap Statement |
|-----------|-----------|---------------|
| Performance Gap | [v] Ya / [ ] Tidak | Akurasi model masih rendah pada kelas kerusakan ringan (sobek kecil, luntur pinggiran) |
| Method Gap | [v] Ya / [ ] Tidak | Belum ada penerapan MobileNetV2 (model ringan untuk mobile) dikombinasikan dengan attention mechanism untuk klasifikasi 3 kelas kerusakan Rupiah |
| Data Gap | [ v] Ya / [ ] Tidak | Dataset yang ada jarang memiliki label granular (ringan, sedang, berat) dan kurang representatif kondisi nyata |
| Context Gap | [v ] Ya / [ ] Tidak | Minim pengujian pada konteks Indonesia (layanan kas keliling BI dengan kamera ponsel) |

**Gap utama yang dipilih:**Data Gap + Method Gap
**Mengapa gap ini penting (bukan sekadar "belum ada yang meneliti")?**
> Karena klasifikasi biner yang ada saat ini kurang membantu Bank Indonesia dalam mengambil keputusan akurat mengenai uang yang masih layak edar versus yang harus ditarik dari peredaran. Deteksi dini kerusakan ringan dapat meningkatkan efisiensi proses sortir dan menjaga kualitas uang Rupiah yang beredar.
---

## Latihan 3 — Baseline Selection

Pilih 2 baseline dari literatur yang sudah dibaca.

| # | Baseline | Mengapa Relevan | Mengapa Representatif | Apakah SOTA? | Sumber |
|---|----------|----------------|----------------------|-------------|--------|
| 1 | CNN 2-Layer | Task klasifikasi citra kerusakan uang sama | Pendekatan dasar yang paling umum digunakan | Tidak | Nandika et al. (2025) |
| 2 | VGG16 | Telah digunakan untuk klasifikasi tingkat kerusakan Rupiah | Arsitektur klasik yang kuat dan sering dijadikan pembanding | Tidak (tapi kuat) | Roshan et al. (2025) |

**Apakah pemilihan baseline ini bisa dianggap straw man?** [ ] Ya / [v] Tidak
> Justifikasi: Baseline dipilih karena merupakan metode yang paling umum dan representatif di literatur terkini (common practice), bukan baseline yang sengaja dibuat lemah untuk membuat metode baru terlihat superior.
---

## Refleksi

> Apa perbedaan antara "belum ada yang meneliti ini" (klaim tanpa bukti) dengan research gap yang valid? Bagaimana cara membuktikan bahwa sebuah gap benar-benar ada?

**Jawaban:**
> Perbedaan antara “belum ada yang meneliti ini” (klaim tanpa bukti) dengan research gap yang valid adalah: klaim “belum ada” bersifat absolut dan sering tidak didukung proses pencarian yang sistematis, sedangkan research gap yang valid didasarkan pada literature mapping yang jelas (query Boolean, database, tahun, screening) serta menunjukkan pola limitasi yang berulang di studi-studi existing.
> Cara membuktikan bahwa sebuah gap benar-benar ada adalah dengan mendokumentasikan proses pencarian literatur secara eksplisit, membuat tabel literature matrix concept-centric, dan menunjukkan kekurangan yang konsisten dari beberapa paper terkini (2021–2025)
