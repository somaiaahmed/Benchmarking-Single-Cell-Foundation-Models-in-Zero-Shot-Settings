# Benchmarking Single-Cell Foundation Models in Zero-Shot Settings

**Exploring the Power of Foundation Models in Genomics**

Somaia Ahmed & Yasmine Mahmoud 
Department of Systems and Biomedical Engineering, Cairo University

---

## Overview

Single-cell foundation models promise transferable, general-purpose representations of transcriptomic data — but how well do they actually hold up against simple, task-specific baselines? This project benchmarks **four single-cell foundation models** — **scGPT**, **UCE**, **SCimilarity**, and **Transcriptformer** — in a **zero-shot** setting (pretrained embeddings, no fine-tuning) across four downstream tasks:

1. **Cell Type Annotation**
2. **Human Data Integration**
3. **Cross-Species Integration**
4. **Protein Expression Prediction**

All models are evaluated using the *same* datasets, metrics, and experimental pipeline to enable a fair, consistent comparison — something existing benchmarks in the field often lack.

## Key Finding

> Foundation models do **not** consistently outperform traditional baselines. Their usefulness is highly **task-dependent**.

| Task | Winner | Takeaway |
|---|---|---|
| Cell Type Annotation | **Baseline (HVG + LogReg/KNN)** | Conventional features remain highly competitive within-dataset |
| Human Data Integration | **scVI (baseline)** | Specialized integration models still edge out foundation embeddings |
| Cross-Species Integration | **Foundation models (dataset-dependent)** | Bigger relative gains here, especially for cross-species transfer |
| Protein Expression Prediction | **Foundation models** | Clear, consistent improvement over HVG features |

## Models Benchmarked

| Model | Architecture | Pre-training Data | Embedding Dim. |
|---|---|---|---|
| scGPT | Transformer | 33M cells | 512 |
| Transcriptformer | Transformer | 112M cells, 12 species | 2048 |
| SCimilarity | Metric Learning + Autoencoder | 23.4M cells | 128 |
| UCE | Transformer | 36M cells, multi-species | 1280 |

Embeddings were extracted directly from pretrained checkpoints (**no fine-tuning**) to isolate the quality of the learned representations themselves, independent of any task-specific adaptation.

<!-- ## Datasets

Nine single-cell datasets spanning multiple tissues, species, and cell counts were used, some reused across tasks:

| Dataset | # Cells | Species | Tissue | Tasks Used In |
|---|---|---|---|---|
| 1k PBMCs | 1,000 | Human | Blood | Annotation |
| Lukassen et al. | 17,500 | Human | Bronchial Epithelium | Annotation |
| Baron et al. | 8,600 / 1,900 | Human / Mouse | Pancreas | Annotation, Human Integration, Cross-species |
| Madissoon et al. | 57,000 | Human | Lung | Human Integration |
| Vieira Braga et al. | 9,000 | Human | Lung | Human Integration |
| Tabula Sapiens | 26,000 | Human | Colon | Cross-species |
| NHPCA | 6,600 | Macaque | Colon | Cross-species |
| CBMCs | 8,600 | Human | Blood | Protein Expression |
| BMMCs | 20,000 | Human | Bone Marrow | Protein Expression | -->

## Methodology Summary

- **Cell Type Annotation** — Logistic Regression & KNN (k=15) trained on embeddings vs. HVG features, 5-fold stratified CV, evaluated with **Macro F1**.
- **Data Integration** — Foundation model embeddings compared against **scVI** (2 hidden layers, 30 latent dims, top 4,000 HVGs). Evaluated with **Silhouette Score** and **LISI** (cell-type & dataset/species mixing).
- **Label Transfer** — Classifier trained on a source dataset, evaluated on a target dataset (within- and cross-species), scored with **Macro F1**.
- **Protein Expression Prediction** — Linear Regression on CITE-seq datasets (paired RNA + protein), 5-fold CV, evaluated with **Pearson correlation** and **RMSE**.

## Repository Structure

```
BENCHMARKING-OF-TRANSCRIPTOMICS-FOUNDATION-MODELS/
├── scGPT/
│   ├── scGPT_annotation.ipynb
│   ├── scGPT_cross_species_integration.ipynb
│   ├── scGPT_data_integration.ipynb
│   └── scGPT_protein_expression_prediction.ipynb
├── Transcriptformer/
│   ├── Transcriptformer_annotation.ipynb
│   ├── Transcriptformer_cross_species_integration.ipynb
│   ├── Transcriptformer_integration.ipynb
│   └── Transcriptformer_protein_expression_prediction.ipynb
|
├── UCE/
│   ├── UCE_annotation.ipynb
│   ├── UCE_cross_species_integration.ipynb
│   ├── UCE_integration.ipynb
│   └── UCE_protein_expression_prediction.ipynb
|
├── SCimilarity/
│   ├── SCimilarity_annotation.ipynb
│   ├── SCimilarity_cross_species_integration.ipynb
│   ├── SCimilarity_integration.ipynb
│   └── SCimilarity_protein_expression_prediction.ipynb
└── README.md
```


## Results Summary

### 1. Cell Type Annotation — No Consistent Improvement
HVG-based baselines matched or outperformed most foundation models across PBMC, HBEC, and Pancreas datasets. Transcriptformer was the strongest foundation model but only marginally beat baseline in a few cases.

### 2. Data Integration — Limited Advantage over scVI
- **Within-species:** SCimilarity best balanced cell-type preservation and batch correction; scVI showed the strongest batch mixing overall.
- **Cross-species:** SCimilarity preserved cell-type structure best; scVI achieved stronger species alignment. Transcriptformer generally underperformed here.

### 3. Label Transfer
Within-species transfer was strong across all methods. Cross-species transfer showed larger gaps — Transcriptformer led Human→Mouse performance (asymmetrically), while SCimilarity and UCE were most consistent for Human↔Monkey.

### 4. Protein Expression Prediction — Clear Win for Foundation Models
All four foundation models outperformed the HVG baseline on both CBMC and Bone Marrow CITE-seq datasets. scGPT achieved the highest correlation on CBMCs; Transcriptformer led on Bone Marrow.


## Citation

If you use this work, please cite:

```
Mahmoud, Y. & Ahmed, S. "Exploring the Power of Foundation Models in Genomics."
Department of Systems and Biomedical Engineering, Cairo University, 2026.
```

## Contact
- Somaia Ahmed — somaia.ahmed03@gmail.com
- Yasmine Mahmoud — yasmine.fotouh03@eng-st.cu.edu.eg
