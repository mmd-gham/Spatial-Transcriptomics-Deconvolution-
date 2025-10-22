# 🧬 Spatial-Transcriptomics-Deconvolution

**Spatial-Transcriptomics-Deconvolution** is a deep-learning-based framework for **cell-type deconvolution** of spatial transcriptomics data.  
It uses an **attention-regularized regression model** with **spatial Laplacian smoothing** and **ridge penalization** to integrate spatial context, transcriptomic signatures, and attention-weighted similarity between neighboring spots.

---

## 🔍 Model Overview

The goal is to infer the **cell-type proportion matrix** `X`  
from the **spatial transcriptomics matrix** `Y`  
and the **reference signature matrix** `S`.

The forward model is:

**Y = X * S + ε**

where `ε` represents Gaussian noise.

---

## ⚙️ Optimization Objective

The **Attention Regression Deconvolution** model estimates `X` by solving:

**min_{X ≥ 0} || Y - X * S ||₂² + λ_r * ||X||₂² + λ_c * Tr(Xᵀ L X)**

Where:
- `|| Y - X * S ||₂²`: **Reconstruction loss**
- `λ_r * ||X||₂²`: **Ridge penalty** for regularization
- `Tr(Xᵀ L X)`: **Spatial smoothing term**, with `L = D - W` being the graph Laplacian of adjacency matrix `W`.

---

## 💡 Attention-Weighted Laplacian

Spatial weights are modulated by an attention mechanism:

**W_ij = exp(-α * d_ij²) * Attn(Y_i, Y_j)**

Where:
- `d_ij`: Spatial distance between spots `i` and `j`.
- `Attn(Y_i, Y_j)`: Learnable similarity score between transcriptomic profiles of spots `i` and `j`.  
This adaptively strengthens edges between **biologically similar** and **spatially close** spots.

---

## 🧩 Gauss–Seidel Update (Iterative Solver)

At each iteration, the cell-type proportions are updated using an iterative solver to minimize the loss function. This process includes the regularization terms and the spatial constraints.

Optionally, attention weights are re-estimated at each epoch to improve convergence.

---
