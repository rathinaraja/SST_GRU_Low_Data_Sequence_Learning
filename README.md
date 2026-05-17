<h1 align="center">
  Contrast-Enhanced Gating in GRUs <br>
  for Robust Low-Data Sequence Learning
</h1>

<p align="center">
  ✨ <b>CVPR 2026 Workshop</b> ✨
</p>

<p align="center">
  <b>🧠 GRU</b> &nbsp;|&nbsp;
  <b>⚡ Squared Sigmoid-Tanh</b> &nbsp;|&nbsp;
  <b>📉 Low-Data Sequence Learning</b> &nbsp;|&nbsp;
  <b>🎥 Human Motion</b> &nbsp;|&nbsp;
  <b>📈 Time-Series Forecasting</b>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/CVPR-2026-blueviolet?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Model-SST--GRU-teal?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Task-Low--Data%20Sequence%20Learning-orange?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Status-Code%20Coming%20Soon-red?style=for-the-badge" />
</p>

<p align="center">
  Official project page for <b>Contrast-Enhanced Gating in GRUs for Robust Low-Data Sequence Learning</b>.
</p>

<p align="center">
  <img src="assets/sst_gru_cell.jpg" width="760">
</p>

---

## 📚 Table of Contents

- [📌 Overview](#-overview)
- [🧠 Motivation](#-motivation)
- [⚡ Squared Sigmoid-Tanh](#-squared-sigmoid-tanh)
- [🧬 SST-GRU](#-sst-gru)
- [🧪 Experiments](#-experiments)
- [📊 Results](#-results)
- [🖼️ Figures](#️-figures)
- [📁 Repository Contents](#-repository-contents)
- [🚧 Code Release Status](#-code-release-status)
- [📄 Paper](#-paper)
- [📚 Citation](#-citation)
- [⚠️ Notes](#️-notes)
- [📬 Contact](#-contact)

---

## 📌 Overview

**SST-GRU** introduces **Squared Sigmoid-Tanh (SST)**, a simple parameter-free modification to GRU gating for robust low-data sequence learning.

Standard GRUs rely on sigmoid and tanh nonlinearities for temporal gating. While effective, these activations can produce weak gate separation and unstable learning when training data are limited or sparse. SST addresses this by squaring the outputs of sigmoid and tanh activations to increase contrast between weak and strong activation regimes.

The core idea is:

> Make weak activations weaker and strong activations more distinguishable, so GRU gates can perform sharper information filtering.

SST is designed as a **drop-in modification** for GRU-based models. It does not introduce additional trainable parameters and adds negligible computational overhead.

---

## 🧠 Motivation

GRUs are compact, efficient, and widely used for sequence learning. They remain useful in low-resource settings such as:

- sign language recognition
- human activity recognition
- gait classification
- sensor-based time-series modeling
- financial forecasting

However, in low-data or sparse-signal settings, standard sigmoid/tanh gating may be overly smooth. This can reduce the model’s ability to distinguish between informative and non-informative temporal signals.

To visualize sparse representations in the Indian Sign Language dataset, PCA was applied to three representative classes: **friend**, **phone call**, and **location**.

<p align="center">
  <img src="assets/pca_friend.jpg" width="30%">
  <img src="assets/pca_phone_call.jpg" width="30%">
  <img src="assets/pca_location.jpg" width="30%">
</p>

SST improves gate selectivity by increasing activation contrast.

```text
sigmoid output: 0.9 → squared value: 0.81
sigmoid output: 0.1 → squared value: 0.01
```

This suppresses weak gate responses while preserving stronger responses, encouraging sharper temporal filtering.

---

## ⚡ Squared Sigmoid-Tanh

SST consists of two activation modifications:

### 1. Squared Sigmoid

The standard sigmoid function is:

```text
SF(x) = 1 / (1 + exp(-x))
```

The squared sigmoid function is:

```text
SS(x) = SF(x)^2
```

Squared sigmoid preserves the output range `[0, 1]` while increasing the contrast between low and high activation values.

### 2. Squared Tanh

The standard tanh function is:

```text
TF(x) = (exp(x) - exp(-x)) / (exp(x) + exp(-x))
```

The squared tanh transformation is defined as:

```text
ST(x) =  TF(x)^2   if x >= 0
ST(x) = -TF(x)^2   if x < 0
```

This preserves the sign of the original tanh output while increasing nonlinearity away from the origin.

<p align="center">
  <img src="assets/ss_st_activation.jpg" width="760">
</p>

---

## 🧬 SST-GRU

SST is integrated into the GRU gating pathway.

A standard GRU uses:

- reset gate
- update gate
- candidate hidden state
- hidden state update

SST-GRU applies squared sigmoid to GRU gates to increase gate polarization and improve information filtering.

The goal is not to increase model size, but to improve the behavior of the existing GRU gates.

### Key Properties

SST is:

- **parameter-free**
- **drop-in compatible**
- **computationally lightweight**
- **designed for low-data sequence learning**
- **useful for sparse temporal patterns**
- **compatible with existing GRU pipelines**

<p align="center">
  <img src="assets/sst_gru_cell.jpg" width="760">
</p>

---

## 🧪 Experiments

SST-GRU was evaluated across multiple low-data and sparse-sequence tasks.

### 1. Indian Sign Language Recognition

The sign language experiment uses the ISL setup from the earlier MOPGRU work. The dataset contains:

- 13 Indian sign gestures
- 30 videos per gesture
- 30 frames per video
- 640 × 480 video resolution

SST was incorporated into the MOPGRU configuration, producing **MOPGRU-SST**.

### 2. Gait Classification

The gait classification experiment uses accelerometer sequences collected from 244 subjects during walking tasks.

To simulate sparse sensor data, 20% of the values in 1024-sample input sequences were randomly zeroed out.

### 3. Human Activity Recognition

SST-GRU was evaluated on two HAR datasets:

- **WISDM**
- **UCI-HAR**

Both datasets were tested under sparsity conditions to simulate real-world sensor-feed challenges.

### 4. Gold Price Time-Series Forecasting

SST-GRU was also evaluated on historical gold price forecasting using Yahoo Finance data.

The model was trained to predict temporal price trends using financial time-series features such as:

- open price
- high price
- low price
- close price
- adjusted close price

---

## 📊 Results

### Indian Sign Language Recognition

| Model | Test Accuracy |
|---|---:|
| MOPGRU | 95% |
| **MOPGRU-SST** | **100%** |

SST improved separability in hidden and dense representations, as shown through t-SNE analysis.

<p align="center">
  <img src="assets/tsne_hidden_mopgru.jpg" width="45%">
  <img src="assets/tsne_hidden_mopgru_sst.jpg" width="45%">
</p>

<p align="center">
  <img src="assets/tsne_dense_mopgru.jpg" width="45%">
  <img src="assets/tsne_dense_mopgru_sst.jpg" width="45%">
</p>

---

### Gait Classification

| Model | Test Accuracy | Precision | Recall | F1-score | AUC |
|---|---:|---:|---:|---:|---:|
| GRU | 79.8% | 78.9% | 77.8% | 78.3% | 0.86 |
| **GRU-SST** | **84.3%** | **87.7%** | 77.4% | **82.2%** | **0.92** |

SST improved test accuracy and AUC, indicating stronger discrimination under sparse temporal inputs.

<p align="center">
  <img src="assets/sparsity_distribution.jpg" width="720">
</p>

<p align="center">
  <img src="assets/roc_gru.jpg" width="45%">
  <img src="assets/roc_gru_sst.jpg" width="45%">
</p>

---

### Human Activity Recognition

| Model | Dataset | Training Accuracy | Testing Accuracy |
|---|---|---:|---:|
| GRU | WISDM | 99.5% | 97.08% |
| **GRU-SST** | WISDM | **100%** | **99.28%** |
| GRU | UCI-HAR | 99% | 93.08% |
| **GRU-SST** | UCI-HAR | **100%** | **98.30%** |

SST consistently improved test accuracy on both HAR datasets.

---

### Gold Price Forecasting

| Model | Training Loss | Validation Loss | MSE |
|---|---:|---:|---:|
| GRU | 0.70 | 0.08 | 0.28 |
| **GRU-SST** | **0.54** | **0.06** | **0.08** |

SST substantially reduced test-set MSE in the forecasting experiment.

---

## 🖼️ Figures

The repository includes the following manuscript figures:

| Figure | File |
|---|---|
| SST-GRU cell | `assets/sst_gru_cell.jpg` |
| Squared sigmoid and squared tanh behavior | `assets/ss_st_activation.jpg` |
| ISL PCA visualization | `assets/pca_friend.jpg`, `assets/pca_phone_call.jpg`, `assets/pca_location.jpg` |
| Hidden-layer t-SNE visualization | `assets/tsne_hidden_mopgru.jpg`, `assets/tsne_hidden_mopgru_sst.jpg` |
| Dense-layer t-SNE visualization | `assets/tsne_dense_mopgru.jpg`, `assets/tsne_dense_mopgru_sst.jpg` |
| Gait ROC curves | `assets/roc_gru.jpg`, `assets/roc_gru_sst.jpg` |

---

## 📁 Repository Contents

```text
SST-GRU/
├── README.md
├── LICENSE
│
├── assets/
│   ├── sst_gru_cell.jpg
│   ├── ss_st_activation.jpg
│   ├── pca_friend.jpg
│   ├── pca_phone_call.jpg
│   ├── pca_location.jpg
│   ├── tsne_hidden_mopgru.jpg
│   ├── tsne_hidden_mopgru_sst.jpg
│   ├── tsne_dense_mopgru.jpg
│   ├── tsne_dense_mopgru_sst.jpg
│   ├── sparsity_distribution.jpg
│   ├── roc_gru.jpg
│   └── roc_gru_sst.jpg
│
└── paper/
    └── CVPR_2026_SST_Camera_ready_version.pdf
```

---

## 🚧 Code Release Status

The implementation code is being prepared for public release.

Current status:

```text
Paper: Available
Figures: Available
Code: Coming soon
Experiments: Coming soon
Pretrained models: Coming soon
```

---

## 📄 Paper

The camera-ready paper is available here:

[Download the CVPR 2026 Camera-Ready Paper](paper/CVPR_2026_SST_Camera_ready_version.pdf)

---

## 📚 Citation

If you find this work useful, please cite:

```bibtex
@inproceedings{subramanian2026sstgru,
  title     = {Contrast-Enhanced Gating in GRUs for Robust Low-Data Sequence Learning},
  author    = {Subramanian, Barathi and Jeyaraj, Rathinaraja and Paul, Anand},
  booktitle = {CVPR Workshop},
  year      = {2026}
}
```

The official citation will be updated after publication.

---

## ⚠️ Notes

This repository is intended for research and educational use.

The current README is based on the CVPR 2026 workshop camera-ready manuscript. Code and reproducibility materials will be added after repository cleanup.

Reported results should be interpreted in the context of the experimental setup described in the paper, especially because some datasets are small or controlled low-data benchmarks.

---

## 📬 Contact

For questions, please open an issue in this repository.

---

<p align="center">
  ⭐ If this project is useful, please consider starring the repository. ⭐
</p>
