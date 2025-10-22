# 🧬 Spatial-Transcriptomics-Deconvolution

**Spatial-Transcriptomics-Deconvolution** is a deep-learning-based framework for **cell-type deconvolution** of spatial transcriptomics data using an **attention-regularized regression model** with **spatial Laplacian smoothing** and **ridge penalization**.  
It integrates spatial context, transcriptomic signatures, and attention-weighted similarity between neighboring spots.

---

## 🔍 Model Overview

The goal is to infer the **cell-type proportion matrix** <img src="https://latex.codecogs.com/svg.latex?X%20%5Cin%20%5Cmathbb%7BR%7D%5E%7Bn%20%5Ctimes%20K%7D" />  
from the **spatial transcriptomics matrix** <img src="https://latex.codecogs.com/svg.latex?Y%20%5Cin%20%5Cmathbb%7BR%7D%5E%7Bn%20%5Ctimes%20G%7D" /> and the **reference signature matrix** <img src="https://latex.codecogs.com/svg.latex?S%20%5Cin%20%5Cmathbb%7BR%7D%5E%7BK%20%5Ctimes%20G%7D" />.

The forward model is:

<p align="center">
  <img src="https://latex.codecogs.com/svg.latex?Y%20%3D%20XS%20%2B%20%5Cvarepsilon" />
</p>

where <img src="https://latex.codecogs.com/svg.latex?%5Cvarepsilon" /> represents Gaussian noise.

---

## ⚙️ Optimization Objective

The **Attention Regression Deconvolution** model estimates <img src="https://latex.codecogs.com/svg.latex?X" /> by solving:

<p align="center">
  <img src="https://latex.codecogs.com/svg.latex?%5Cmin_%7BX%20%5Cge%200%7D%20%5C%3B%20%5C%7C%20Y%20-%20XS%20%5C%7C_F%5E2%20%2B%20%5Clambda_r%20%5C%7C%20X%20%5C%7C_F%5E2%20%2B%20%5Clambda_c%20%5C%2C%20%5Cmathrm%7BTr%7D(X%5ETLX)" />
</p>

where:

- <img src="https://latex.codecogs.com/svg.latex?%5C%7C%20Y%20-%20XS%20%5C%7C_F%5E2" />: reconstruction loss  
- <img src="https://latex.codecogs.com/svg.latex?%5Clambda_r%20%5C%7C%20X%20%5C%7C_F%5E2" />: **ridge penalty** for regularization  
- <img src="https://latex.codecogs.com/svg.latex?%5Cmathrm%7BTr%7D(X%5ETLX)" />: **spatial smoothing term**, with <img src="https://latex.codecogs.com/svg.latex?L%20%3D%20D%20-%20W" /> being the graph Laplacian of adjacency matrix <img src="https://latex.codecogs.com/svg.latex?W" />.

---

## 💡 Attention-Weighted Laplacian

Spatial weights are modulated by an attention mechanism:

<p align="center">
  <img src="https://latex.codecogs.com/svg.latex?W_%7Bij%7D%20%3D%20%5Cexp(-%5Calpha%20d_%7Bij%7D%5E2)%20%5Ccdot%20%5C
