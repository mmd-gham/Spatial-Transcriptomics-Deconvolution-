# 🧬 Spatial-Transcriptomics-Deconvolution

**Spatial-Transcriptomics-Deconvolution** is a deep learning-based framework designed for **cell-type deconvolution** of spatial transcriptomics data. The framework employs an **attention-regularized regression model** with **spatial Laplacian smoothing** and **ridge penalization** to infer the proportions of different cell types across spatial regions in tissue samples. This model integrates **spatial context**, **transcriptomic signatures**, and **attention-weighted similarity** between neighboring spots, providing a robust solution for deconvolving cell-type proportions in complex tissue environments.

---

## 🔍 Model Overview

The primary goal of **Spatial-Transcriptomics-Deconvolution** is to estimate **cell-type proportions** in spatial spots from spatial transcriptomics data, leveraging single-cell RNA-seq data as reference. The model works by building spatial and transcriptomic matrices, and then inferring the relative contributions of each cell type in the spatial tissue regions using an **attention mechanism**.

### Model Components:
- **Single-cell RNA-seq data** (`sc_data`): Provides a reference for cell-type signatures.
- **Spatial transcriptomics data** (`st_data`): Offers gene expression data across spatial spots.
- **Spatial coordinates** (`st_coordinates`): Defines the positions of the spatial spots.
- **Attention-regularized regression**: Used to impose spatial relationships between neighboring spots based on both their spatial proximity and transcriptomic similarity.

The framework uses a **CARD-inspired** method for type-level deconvolution and iteratively refines the results using **Gauss-Seidel updates** at the cell level, allowing for high accuracy in cell-type proportion estimation.

---

## ⚙️ Key Features

- **Attention Regularization**: The model integrates spatial context and transcriptomic data to apply an attention-based regularization that adjusts the weights of neighboring spots. This allows the model to account for both spatial proximity and transcriptomic similarity, ensuring more accurate deconvolution.
  
- **Multi-step Deconvolution**: 
  - **Step 1**: The model first estimates cell-type proportions at the type level using a **CARD-inspired** approach that applies a spatial Laplacian smoothing and ridge regularization.
  - **Step 2**: After estimating the proportions at the type level, the model distributes these proportions to the individual cells using **Gauss-Seidel updates**. This step refines the deconvolution to ensure accurate cell-type proportions in each spatial spot.

- **Flexible Hyperparameters**: The model allows for the adjustment of several parameters, such as:
  - Number of iterations for the deconvolution process (`n_iter`)
  - Regularization strengths for ridge regularization (`lambda_ridge`) and spatial smoothing (`lambda_spatial`)
  - The number of neighbors used for the spatial graph (`k_neighbors`), providing flexibility in the spatial smoothing process.

- **GPU/CPU Agnostic**: The model is designed to run on both CPU and GPU, using `cupy` (if available) for GPU acceleration, making it scalable for large datasets.

---

git clone https://github.com/yourusername/Spatial-Transcriptomics-Deconvolution.git
cd Spatial-Transcriptomics-Deconvolution
