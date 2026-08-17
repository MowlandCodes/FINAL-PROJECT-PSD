# Evaluasi Kinerja Filter Spasial Median dan Gaussian dalam Mereduksi Salt-and-Pepper serta Additive White Gaussian Noise pada Citra Digital

[![Python](https://img.shields.io/badge/Python-3.14-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

Final project mata kuliah **Pengolahan Sinyal Digital (PSD)** yang mengevaluasi dan membandingkan performa **filter spasial non-linear (Median Filter)** dan **linear (Gaussian Filter)** dalam mereduksi dua jenis noise umum pada citra digital: **Salt-and-Pepper (S&P) Noise** dan **Additive White Gaussian Noise (AWGN)**.

## Fitur

- Injeksi noise **Salt-and-Pepper** (`p = 0.05`) dan **AWGN** (`σ = 25.0`) pada citra uji grayscale.
- Evaluasi **Median Filter** dan **Gaussian Filter** (σₛ = 1.5) dengan variasi ukuran kernel `3×3`, `5×5`, dan `7×7`.
- **Stress-test mismatch**: menerapkan Gaussian Filter (linear) pada Salt-and-Pepper noise untuk membuktikan ketidakcocokan filter linear terhadap noise impulsif.
- Evaluasi kuantitatif menggunakan **MSE**, **PSNR**, dan **SSIM**.
- Dukungan *fallback*: bila citra lokal tidak ditemukan, sistem otomatis menghasilkan citra sintetis berukuran `256×256`.

## Struktur Direktori

```
FINAL_PROJECT/
├── notebook/
│   └── final_project.ipynb     # Eksperimen utama (Jupyter Notebook)
├── paper/
│   └── 452024611065 - M. Faridh Maulana.tex   # Paper IEEE (LaTeX + PDF)
├── images/
│   ├── golden-gate.jpg         # Citra uji / ground truth
│   └── visual_comparison.png   # Output perbandingan visual
├── pyproject.toml              # Dependensi project (uv)
└── README.md
```

## Metodologi Eksperimen

Tiga skenario pengujian dirancang terhadap citra `golden-gate.jpg`:

| Skenario | Noise | Filter | Ukuran Kernel |
|----------|-------|--------|---------------|
| **Exp-1** | Salt-and-Pepper (`p = 0.05`) | Median | `3×3`, `5×5`, `7×7` |
| **Exp-2** | AWGN (`σ = 25.0`) | Gaussian (σₛ = 1.5) | `3×3`, `5×5`, `7×7` |
| **Exp-3** | Salt-and-Pepper (`p = 0.05`) — *Mismatch Test* | Gaussian (linear) | `3×3`, `5×5`, `7×7` |

## Hasil Eksperimen

| Skenario | Metode | Kernel | MSE | PSNR (dB) | SSIM |
|----------|--------|--------|------|-----------|------|
| Exp-1: S&P (p=0.05) | Median | 3×3 | 38.87 | **32.23** | **0.9417** |
| Exp-1: S&P (p=0.05) | Median | 5×5 | 85.02 | 28.84 | 0.8707 |
| Exp-1: S&P (p=0.05) | Median | 7×7 | 117.35 | 27.44 | 0.8274 |
| Exp-2: AWGN (σ=25) | Gaussian | 3×3 | 113.31 | 27.59 | 0.6138 |
| Exp-2: AWGN (σ=25) | Gaussian | 5×5 | **107.37** | **27.82** | 0.7229 |
| Exp-2: AWGN (σ=25) | Gaussian | 7×7 | 109.94 | 27.72 | **0.7480** |
| Exp-3: Mismatch | Gaussian | 3×3 | 158.23 | 26.14 | 0.5692 |
| Exp-3: Mismatch | Gaussian | 5×5 | 130.74 | 26.97 | 0.6807 |
| Exp-3: Mismatch | Gaussian | 7×7 | 129.59 | 27.01 | 0.7115 |

### Insight

- **Median Filter 3×3** paling efektif untuk **Salt-and-Pepper noise** (PSNR 32.23 dB, SSIM 0.9417) karena mekanisme *order-statistic* mengeliminasi bit ekstrem (0 dan 255) tanpa merusak piksel sekitar.
- **Gaussian Filter 5×5** optimal untuk **AWGN** (MSE terendah 107.37, PSNR 27.82 dB), sedangkan kernel **7×7** mempertahankan kemiripan struktur terbaik (SSIM 0.7480).
- **Stress-test mismatch** membuktikan filter linear gagal pada noise impulsif: Gaussian Filter 3×3 menghasilkan performa terburuk di seluruh pengujian (MSE 158.23, SSIM 0.5692) akibat artefak *smearing*.

## Prasyarat

- Python `>= 3.14`
- [uv](https://docs.astral.sh/uv/) sebagai *package manager*

## Cara Menjalankan

```bash
# 1. Sinkronkan dependensi dari pyproject.toml
uv sync

# 2. Jalankan Jupyter Notebook
uv run jupyter notebook notebook/final_project.ipynb
```

Eksekusi seluruh *cell* notebook akan:
1. Memuat/men-generate citra uji.
2. Menginjeksi noise S&P dan AWGN.
3. Menerapkan Median & Gaussian Filter untuk tiap ukuran kernel.
4. Menampilkan tabel hasil (MSE, PSNR, SSIM) dan mengekspor tabel LaTeX.
5. Menghasilkan perbandingan visual `images/visual_comparison.png`.

## Output

| Output | Deskripsi |
|--------|-----------|
| `images/visual_comparison.png` | Perbandingan visual *ground truth*, citra ber-noise, dan hasil filtering |
| `paper/…pdf` | Paper IEEE lengkap berisi teori, metodologi, hasil, dan analisis |

## Author

- **M. Faridh Maulana** — Teknik Informatika, Universitas Darussalam Gontor
- Email: `mfaridhmaulana47@student.cs.unida.gontor.ac.id`

## Repositori

[github.com/MowlandCodes/FINAL-PROJECT-PSD](https://github.com/MowlandCodes/FINAL-PROJECT-PSD)
