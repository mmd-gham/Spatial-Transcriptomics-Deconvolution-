# 🧬 Spatial-Transcriptomics-Deconvolution

**Spatial-Transcriptomics-Deconvolution** is a deep-learning-based framework for **cell-type deconvolution** of spatial transcriptomics data using an **attention-regularized regression model** with **spatial Laplacian smoothing** and **ridge penalization**.  
It integrates spatial context, transcriptomic signatures, and attention-weighted similarity between neighboring spots.

---

## 🔍 Model Overview

The goal is to infer the **cell-type proportion matrix** \( X \in \mathbb{R}^{n \times K} \)  
from the **spatial transcriptomics matrix** \( Y \in \mathbb{R}^{n \times G} \) and the **reference signature matrix** \( S \in \mathbb{R}^{K \times G} \).

The forward model is:

\[
Y = X S + \epsilon
\]

where \( \epsilon \) represents Gaussian noise.

---

## ⚙️ Optimization Objective

The **Attention Regression Deconvolution** model estimates \( X \) by solving:

\[
\min_{X \geq 0} 
\; \| Y - X S \|_F^2 
+ \lambda_r \| X \|_F^2
+ \lambda_c \, \mathrm{Tr}(X^T L X)
\]

where:

- \( \| Y - X S \|_F^2 \): reconstruction loss  
- \( \lambda_r \| X \|_F^2 \): **ridge penalty** for regularization  
- \( \mathrm{Tr}(X^T L X) \): **spatial smoothing term**,  
  with \( L = D - W \) being the graph Laplacian of the adjacency matrix \( W \).

---

## 💡 Attention-Weighted Laplacian

Spatial weights are modulated by an attention mechanism:

\[
W_{ij} = \exp(-\alpha \, d_{ij}^2) \cdot \mathrm{Attn}(Y_i, Y_j)
\]

where \( d_{ij} \) is spatial distance and \( \mathrm{Attn}(Y_i, Y_j) \) is a learnable similarity score between transcriptomic profiles.  
This adaptively strengthens edges between **biologically similar** and **spatially close** spots.

---

## 🧩 Gauss–Seidel Update (Iterative Solver)

At each iteration \( t \), the cell-type proportions are updated as:

\[
X^{(t+1)} = (S S^T + \lambda_r I + \lambda_c L)^{-1} S Y^T
\]

Optionally, attention weights are re-estimated at each epoch to improve convergence.

---

