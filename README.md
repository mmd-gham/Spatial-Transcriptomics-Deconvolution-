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

## 🛠️ Installation

To run this project, you will need Python and the necessary dependencies. Clone the repository:



```bash
git clone https://github.com/mmd-gham/Spatial-Transcriptomics-Deconvolution.git
cd Spatial-Transcriptomics-Deconvolution
```

🚀 Usage
Data Preparation

You will need to prepare the following data before running the model:

Single-cell RNA-seq data (sc_data): A pandas DataFrame or a NumPy array containing gene expression data for each single cell.

Spatial transcriptomics data (st_data): A pandas DataFrame or a NumPy array containing gene expression data for each spatial spot.

Spatial coordinates (st_coordinates): A pandas DataFrame containing the x and y coordinates for each spatial spot in the transcriptomics data.

Cell-type labels (celltype): A pandas Series or DataFrame containing the cell-type labels corresponding to the single-cell RNA-seq data.

Running the Model

Once your data is prepared, you can run the deconvolution model using the following Python code:

from attention_regression_deconv import AttentionRegressionDeconv

# Prepare your data: single-cell RNA-seq, spatial transcriptomics, and coordinates
sc_data = # your single-cell RNA-seq data
st_data = # your spatial transcriptomics data
st_coordinates = # coordinates for spatial spots
celltype = # your cell-type information (DataFrame or Series)


```bash
# Initialize the model
deconv = AttentionRegressionDeconv(
    sc_data=sc_data,
    st_data=st_data,
    st_coordinates=st_coordinates,
    celltype=celltype,
    phi=0.8,  # Initial spatial autocorrelation
    lambda_cell=1.0,  # Cell-level regularization
    lambda_spatial=1.0,  # Spatial smoothing penalty
    lambda_ridge=1.0,  # Ridge regularization
    max_cells_per_type=1000,  # Max number of cells per type
    n_iter=5,  # Number of iterations
    n_jobs=-1,  # Number of parallel jobs
    k_neighbors=6  # Number of spatial neighbors
)

# Run the model to obtain deconvolved cell-type proportions
deconv_celltypes, elapsed_time = deconv.run()

# Evaluate the performance using ground-truth data if available
metrics = deconv.evaluate(deconv_celltypes, result_x)
print(f"Elapsed time: {elapsed_time:.2f} sec")
print("Evaluation metrics:", metrics)
```


