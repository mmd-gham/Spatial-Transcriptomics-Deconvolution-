# 🧬 Spatial-Transcriptomics-Deconvolution

**Spatial-Transcriptomics-Deconvolution** is a deep learning-based framework designed for **cell-type deconvolution** of spatial transcriptomics data. This framework uses an **attention-regularized regression model** with **spatial Laplacian smoothing** and **ridge penalization** to infer the proportions of different cell types across spatial regions in tissue samples. The model integrates **spatial context**, **transcriptomic signatures**, and **attention-weighted similarity** between neighboring spots.

---

## 🔍 Model Overview

This framework aims to deconvolve spatial transcriptomics data by estimating the **cell-type proportions** in each spatial spot based on single-cell RNA-seq data and spatial transcriptomics measurements. It employs an **attention mechanism** to assign weights to neighboring spots in the spatial matrix based on both their spatial proximity and transcriptomic similarity.

---

## ⚙️ Key Features

- **Attention Regularization**: The model integrates spatial and transcriptomic data to create a spatially-aware, attention-weighted regularization for more accurate deconvolution.
- **Multi-step Deconvolution**: The algorithm first estimates cell-type proportions at the type level using a CARD-inspired approach, then distributes the proportions to the cell level using a Gauss-Seidel update.
- **Flexible Parameters**: You can adjust parameters such as the number of iterations, the regularization strength, and the number of neighbors used for spatial smoothing.

---

