# 🧬 Spatial-Transcriptomics-Deconvolution

**Spatial-Transcriptomics-Deconvolution** is a deep-learning-based framework for **cell-type deconvolution** of spatial transcriptomics data using an **attention-regularized regression model** with **spatial Laplacian smoothing** and **ridge penalization**.  
It integrates spatial context, transcriptomic signatures, and attention-weighted similarity between neighboring spots.

---

## 🔍 Model Overview

The goal is to infer the **cell-type proportion matrix** ![X](https://latex.codecogs.com/svg.image?\dpi{120}&space;\color{white}X\in\mathbb{R}^{n\times%20K})  
from the **spatial transcriptomics matrix** ![Y](https://latex.codecogs.com/svg.image?\dpi{120}&space;\color{white}Y\in\mathbb{R}^{n\times%20G})  
and the **reference signature matrix** ![S](https://latex.codecogs.com/svg.image?\dpi{120}&space;\color{white}S\in\mathbb{R}^{K\times%20G}).

The forward model is:

<p align="center">
  <img src="https://latex.codecogs.com/svg.image?\dpi{150}&space;\color{white}Y=XS+\varepsilon" />
</p>

where ![ε](https://latex.codecogs.com/svg.image?\dpi{120}&space;\color{white}\varepsilon) represents Gaussian noise.

---

## ⚙️ Optimization Objective

The **Attention Regression Deconvolution** model estimates ![X](https://latex.codecogs.com/svg.image?\dpi{120}&space;\color{white}X) by solving:

<p align="center">
  <img src="https://latex.codecogs.com/svg.image?\dpi{150}&space;\color{white}\min_{X\ge0}\;\|Y-XS\|_F^2+\lambda_r\|X\|_F^2+\lambda_c\,\mathrm{Tr}(X^T L X)" />
</p>

where:

- ![Y-XS](https://latex.codecogs.com/svg.image?\dpi{120}&space;\color{white}\|Y-XS\|_F^2): **reconstruction loss**  
- ![λr](https://latex.codecogs.com/svg.image?\dpi{120}&space;\color{white}\lambda_r\|X\|_F^2): **ridge penalty** for regularization  
- ![L](https://latex.codecogs.com/svg.image?\dpi{120}&space;\color{white}\mathrm{Tr}(X^T L X)): **spatial smoothing term**, with ![L](https://latex.codecogs.com/svg.image?\dpi{120}&space;\color{white}L=D-W) being the graph Laplacian of adjacency matrix ![W](https://latex.codecogs.com/svg.image?\dpi{120}&space;\color{white}W).

---

## 💡 Attention-Weighted Laplacian

Spatial weights are modulated by an attention mechanism:

<p align="center">
  <img src="https://latex.codecogs.com/svg.image?\dpi{150}&space;\color{white}W_{ij}=\exp(-\alpha\,d_{ij}^2)\cdot\mathrm{Attn}(Y_i,Y_j)" />
</p>

Where:
- ![dij](https://latex.codecogs.com/svg.image?\dpi{120}&space;\color{white}d_{ij}) is the spatial distance between spots.
- ![Attn](https://latex.codecogs.com/svg.image?\dpi{120}&space;\color{white}\mathrm{Attn}(Y_i,Y_j)) is a learnable similarity score between the transcriptomic profiles of spots `i` and `j`.  
This adaptively strengthens edges between **biologically similar** and **spatially close** spots.

---

## 🧩 Gauss–Seidel Update (Iterative Solver)

At each iteration ![t](https://latex.codecogs.com/svg.image?\dpi{120}&space;\color{white}t), the cell-type proportions are updated as:

<p align="center">
  <img src="https://latex.codecogs.com/svg.image?\dpi{150}&space;\color{white}X^{(t+1)}=(SS^T+\lambda_r I+\lambda_c L)^{-1}SY^T" />
</p>

Optionally, attention weights are re-estimated at each epoch to improve convergence.

---
