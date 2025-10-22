# 🧬 Spatial-Transcriptomics-Deconvolution

**Spatial-Transcriptomics-Deconvolution** is a deep-learning-based framework for **cell-type deconvolution** of spatial transcriptomics data. The model integrates single-cell RNA-seq data with spatial transcriptomics data, applying **attention-regularized regression**, **spatial Laplacian smoothing**, and **ridge penalization** to infer cell-type proportions in spatial spots.

---

## 🔍 Project Overview

This framework estimates **cell-type proportions** in spatial transcriptomics data by leveraging single-cell RNA-seq data. The model uses an attention-weighted regularization term to account for spatial dependencies and transcriptomic signatures, providing more accurate deconvolution results.

---

## ⚙️ Key Features

- **Attention-based Regularization**: Integrates attention mechanisms to modulate spatial relationships between spots.
- **Spectral Deconvolution**: Employs a CARD-inspired method for estimating cell-type proportions.
- **Optimized for Multi-core and GPU**: Supports multi-threading with `n_jobs` and GPU computation with `cupy` (if available).
- **Flexible Parameters**: Allows adjustment of regularization strengths, spatial penalties, and number of iterations.
- **Cross-validation and Evaluation**: Built-in support for evaluating model performance using multiple metrics.

---

