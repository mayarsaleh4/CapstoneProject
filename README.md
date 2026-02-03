# Capstone Project . Analysis of Authorship in Arabic Sources

This repository contains the implementation of our capstone project for Arabic authorship verification using a Deep Impostors inspired framework. The system reformulates authorship verification as a **signal comparison** and **unsupervised anomaly detection** problem, targeting **classical Arabic** with a case study on **Al-Jahiz**. :contentReference[oaicite:1]{index=1}

---

## Project Goals

- Build a reproducible pipeline for **open set authorship verification** in Arabic.
- Avoid direct supervised classification on the true author by using **impostor author pairs**.
- Produce interpretable outputs: stylometric signals, DTW distance matrices, anomaly scores, and clustering-based decisions. :contentReference[oaicite:2]{index=2}

---

## Methodology Overview

The pipeline consists of:

1. **Preprocessing**
   - Normalization, diacritics removal, stopword removal, tokenization, light stemming, noise filtering. :contentReference[oaicite:3]{index=3}

2. **Contextual Embedding**
   - AraBERT embeddings generated from segmented text chunks under token-length constraints. :contentReference[oaicite:4]{index=4}

3. **Deep Impostor Training (Siamese CNN + BiLSTM)**
   - Train a Siamese network per impostor pair using contrastive learning.
   - Apply each trained pair-model to test books to produce a 1D stylometric signal per book. :contentReference[oaicite:5]{index=5}

4. **Signal Comparison (DTW)**
   - Dynamic Time Warping aligns signals and generates per-pair DTW matrices. :contentReference[oaicite:6]{index=6}

5. **Anomaly Detection (Isolation Forest)**
   - Detect stylistic outliers using DTW-derived profiles and produce anomaly scores. :contentReference[oaicite:7]{index=7}

6. **Clustering and Final Decision (K-Means, k=2)**
   - Cluster mean anomaly scores into two stylistic groups.
   - Interpret clusters as authentic vs potentially disputed, anchored by well-established works. :contentReference[oaicite:8]{index=8}

---

## Case Study and Dataset Summary

- **Target author**: Al-Jahiz (selected due to a stable core corpus plus disputed attributions). :contentReference[oaicite:9]{index=9}
- **Impostors**: 25 classical Arabic books from comparable eras and genres. :contentReference[oaicite:10]{index=10}
- **Test set**: 55 books attributed to Al-Jahiz, segmented into 90 test samples with size alignment to reduce length bias. :contentReference[oaicite:11]{index=11}

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

2. Open the main notebook in `Phase B/` (pipeline notebook location is documented in Phase B). :contentReference[oaicite:12]{index=12}

3. Mount Google Drive and follow the notebook cells in order:
   - Preprocess
   - Fine-tune AraBERT (optional but recommended in our setup)
   - Generate test embeddings
   - Train Siamese models per impostor pair
   - Generate signals
   - DTW
   - Isolation Forest
   - K-Means clustering and evaluation outputs :contentReference[oaicite:13]{index=13}

---

## Compute Notes

- Development was tested on Google Colab Pro+.
- Large-scale training was run on Lambda Cloud to reduce runtime across many impostor pairs. :contentReference[oaicite:14]{index=14}
- Long stages (training and DTW generation) were implemented as **resumable loops** to handle runtime interruptions safely. :contentReference[oaicite:15]{index=15}

---

## Outputs (What You Get)

The pipeline saves outputs into structured folders (Drive-based), including:

- `results/signals/` . 1D stylometric signals per (book, impostor pair)
- `results/dtw/` . DTW matrices and heatmaps
- `results/iforest/` . per-pair anomaly scores + aggregated score tables
- `results/kmeans/` . final cluster assignments and plots
- Evaluation artifacts such as confusion matrix and predicted vs reference labels. :contentReference[oaicite:16]{index=16}

---

## Results Summary (Phase B)

Our evaluation compares predicted cluster labels with reference labels derived from scholarly and historical sources. The confusion matrix and reported metrics are included in the Phase B report. :contentReference[oaicite:17]{index=17}

---

## References

- Volkovich, Z., & Avros, R. (2025). *Comprehension of the Shakespeare Authorship Question through Deep Impostors Approach*. Digital Scholarship in the Humanities. https://doi.org/10.1093/llc/fqaf009 :contentReference[oaicite:18]{index=18}
- OpenITI: The Open Islamicate Texts Initiative. https://openiti.github.io :contentReference[oaicite:19]{index=19}
- Shamela Library. https://shamela.ws :contentReference[oaicite:20]{index=20}
- Al-Jahiz (Wikipedia). https://en.wikipedia.org/wiki/Al-Jahiz :contentReference[oaicite:21]{index=21}

---

## Team

- Mayar Salih
- Bolos Khoury :contentReference[oaicite:22]{index=22}

Supervisors:
- Dr. Renata Avros
- Prof. Zeev Volkovich :contentReference[oaicite:23]{index=23}

---

## License

Add a license file if you plan to make this reusable externally (MIT or Apache-2.0 are common choices).
