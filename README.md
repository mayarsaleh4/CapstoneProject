# Capstone Project . Analysis of Authorship in Arabic Sources

This repository contains the implementation of our capstone project for Arabic authorship verification using a Deep Impostors inspired framework. The system reformulates authorship verification as a **signal comparison** and **unsupervised anomaly detection** problem, targeting **classical Arabic** with a case study on **Al-Jahiz**. 

---

## Project Goals

- Build a reproducible pipeline for **open set authorship verification** in Arabic.
- Avoid direct supervised classification on the true author by using **impostor author pairs**.
- Produce interpretable outputs: stylometric signals, DTW distance matrices, anomaly scores, and clustering-based decisions. 

---

## Methodology Overview

The pipeline consists of:

1. **Preprocessing**
   - Normalization, diacritics removal, stopword removal, tokenization, light stemming, noise filtering.

2. **Contextual Embedding**
   - AraBERT embeddings generated from segmented text chunks under token-length constraints. 

3. **Deep Impostor Training (Siamese CNN + BiLSTM)**
   - Train a Siamese network per impostor pair using contrastive learning.
   - Apply each trained pair-model to test books to produce a 1D stylometric signal per book. 

4. **Signal Comparison (DTW)**
   - Dynamic Time Warping aligns signals and generates per-pair DTW matrices.

5. **Anomaly Detection (Isolation Forest)**
   - Detect stylistic outliers using DTW-derived profiles and produce anomaly scores.

6. **Clustering and Final Decision (K-Means, k=2)**
   - Cluster mean anomaly scores into two stylistic groups.
   - Interpret clusters as authentic vs potentially disputed, anchored by well-established works.

---

## Case Study and Dataset Summary

- **Target author**: Al-Jahiz (selected due to a stable core corpus plus disputed attributions). 
- **Impostors**: 25 classical Arabic books from comparable eras and genres.
- **Test set**: 55 books attributed to Al-Jahiz, segmented into 90 test samples with size alignment to reduce length bias. 

---

## Repository Structure

- `Phase A/`
  - Background, conceptual model, and methodology design.

- `Phase B/`
  - Full implementation and experiment pipeline.
  - Includes preprocessing, embedding, training, DTW, Isolation Forest, clustering, and evaluation.

---

## Quick Start (Recommended . Google Colab)

1. Clone the repo or download it:
   - https://github.com/mayarsaleh4/CapstoneProject

2. Open the main notebook in `Phase B/` (pipeline notebook location is documented in Phase B). 

3. Mount Google Drive and follow the notebook cells in order:
   - Preprocess
   - Fine-tune AraBERT 
   - Generate test embeddings
   - Train Siamese models per impostor pair
   - Generate signals
   - DTW
   - Isolation Forest
   - K-Means clustering and evaluation outputs 

---

## Compute Notes

- Development was tested on Google Colab Pro+.
- Large-scale training was run on Lambda Cloud to reduce runtime across many impostor pairs. 
- Long stages (training and DTW generation) were implemented as **resumable loops** to handle runtime interruptions safely.

---

## Outputs (What You Get)

The pipeline saves outputs into structured folders (Drive-based), including:

- `results/signals/` . 1D stylometric signals per (book, impostor pair)
- `results/dtw/` . DTW matrices and heatmaps
- `results/iforest/` . per-pair anomaly scores + aggregated score tables
- `results/kmeans/` . final cluster assignments and plots

---

## Team

- Mayar Salih
- Bolos Khoury 

Supervisors:
- Dr. Renata Avros
- Prof. Zeev Volkovich
