# Capstone Project : Analysis of Authorship in Arabic Sources

This repository contains the implementation of our capstone project for Arabic authorship verification using a Deep Impostors inspired framework. The system reformulates authorship verification as a **signal comparison** and **unsupervised anomaly detection** problem, targeting **classical Arabic** with a case study on **Al-Jahiz**. 

---

## Project Goals

- Build a reproducible pipeline for **authorship verification** in Arabic.
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
  - Capstone Project Phase A Book (DOCX & PDF)
  - Capstone Project Phase A Presentation (PPTX)
  - Contains the theoretical background, problem definition, and proposed methodology.

- `Phase B/`
  - `Pipeline Setup/`
    - `Analysis_of_Authorship_in_Arabic_sources.ipynb` . Main pipeline notebook
    - `impostors_data.zip` . Impostor author texts
    - `test_data.zip` . Test texts (Al-Jahiz corpus)
    - `test_data_table.csv` . Mapping of test files to book names
    - `test_data_table_no_duplicates.csv` . Cleaned index table

  - `Project Documentation/`
    - Capstone Project Phase B Book (DOCX & PDF) - Read for user's Guide
    - `poster.pdf` . Project poster
    - `projectVideo.mp4` . Explanation video


---
## Quick Start (Recommended . Google Colab)

1. Clone the repository or download it:
   - https://github.com/mayarsaleh4/CapstoneProject

2. Open the main notebook:
   - `Phase B/Pipeline Setup/Analysis_of_Authorship_in_Arabic_sources.ipynb`

3. Prepare the data:
   - Unzip `impostors_data.zip` and `test_data.zip`.
   - Create a folder named `capstone` in Google Drive.
   - Move all extracted folders and CSV files into `capstone`.

4. Open the notebook in Google Colab:
   - Enable a GPU runtime.
   - Mount Google Drive when prompted.

5. Run the notebook cells sequentially from top to bottom following the pipeline stages.

All outputs are automatically saved to structured folders in Google Drive.

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
