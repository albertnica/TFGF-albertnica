# Redshift Prediction via Machine Learning Models

This repository contains multiple machine learning models benchmarked against each other to predict cosmological redshift from astronomical optical spectra obtained from the Sloan Digital Sky Survey (SDSS). A comprehensive analysis is presented in the accompanying thesis report: [Spanish Original](<TFG_Fisica_Alberto_Nieto_Cardoso.pdf>) / [English Translation](<TFG_Fisica_Alberto_Nieto_Cardoso_en.pdf>).

## Description

The project implements and evaluates various machine learning architectures for cosmological redshift ($z$) prediction using wavelength-flux data (with sky background noise pre-subtracted by SDSS pipelines). The architectures included in this project are:

- **MLP (Multi-Layer Perceptron):** Deep feed-forward neural networks that capture non-linear relationships across the entire spectral feature space.
- **CNN (Convolutional Neural Network):** 1D convolutional architectures designed to extract localized spectral features, emission lines, and continuum slopes.
- **TRA (Transformer):** Sequence-based self-attention architecture based on "Attention Is All You Need", modeling long-range dependencies across spectral wavelengths.

## Project Structure

- **/data:** Directory containing the training and test datasets.
- **[/extra](extra):** Auxiliary directory containing fitted scalers, batch indices, and intermediate serialization files.
- **[/SciServer](SciServer):** Vendored SciServer Python library modules (`Authentication.py`, `CasJobs.py`, `Config.py`) for direct execution without external package installation.
- **/spectrums:** Directory containing downloaded sample spectra (`.fits`) obtained via [Fits Retriever](<1 Fits Retriever.ipynb>) for individual model inference (last cell of each model notebook).
- **/storage:** Storage directory for trained PyTorch checkpoints (`.pth`) and evaluation arrays (`.npy`).
- **[1 Fits Retriever](<1 Fits Retriever.ipynb>):** Queries the SDSS SkyServer/CasJobs database and concurrently downloads `.fits` spectral files.
- **[2 Spectra Processor](<2 Spectra Processor.ipynb>):** Core preprocessing program that balances dataset distributions across redshift bins to counteract severe sample skew, particularly ensuring representation for $z \ge 4$.
- **[3 Dataset Builder](<3 Dataset Builder.ipynb>):** Standard dataset standardization pipeline that interpolates spectra onto a fixed grid of 5,000 flux + 5,000 wavelength points, applies standard scaling, and builds PyTorch datasets and memory maps.

<br />

- **[4.1 Model MLP 600k](<4.1 Model MLP 600k.ipynb>), [4.1 Model MLP 1M](<4.1 Model MLP 1M.ipynb>):** Multi-Layer Perceptron models evaluated on 600k and 1M sample regimes.

<br />

- **[4.2 Model CNN 600k 0](<4.2 Model CNN 600k 0.ipynb>), [4.2 Model CNN 1M 0](<4.2 Model CNN 1M 0.ipynb>):** Baseline 1D convolutional neural network.
- **[4.2 Model CNN 600k 1](<4.2 Model CNN 600k 1.ipynb>), [4.2 Model CNN 1M 1](<4.2 Model CNN 1M 1.ipynb>):** CNN variant introducing dropout in the final linear layers.
- **[4.2 Model CNN 600k 2](<4.2 Model CNN 600k 2.ipynb>), [4.2 Model CNN 1M 2](<4.2 Model CNN 1M 2.ipynb>):** CNN variant with extended convolutional depth.
- **[4.2 Model CNN 600k 3](<4.2 Model CNN 600k 3.ipynb>), [4.2 Model CNN 1M 3](<4.2 Model CNN 1M 3.ipynb>):** CNN variant integrating a self-attention module.
- **[4.2 Model CNN 600k 4](<4.2 Model CNN 600k 4.ipynb>), [4.2 Model CNN 1M 4](<4.2 Model CNN 1M 4.ipynb>):** Regularized CNN with dropout applied across all convolutional and linear blocks.

<br />

- **[4.3 Model TRA 600k](<4.3 Model TRA 600k.ipynb>), [4.3 Model TRA 1M](<4.3 Model TRA 1M.ipynb>):** Transformer-based models for 600k and 1M datasets.

<br />

- **[5 Others Evaluations and Errors](<5 Others Evaluations and Errors.ipynb>):** Evaluation suite for testing models on external datasets (NASA/NED, stellar contaminant spectra) and calculating comprehensive error statistics ($\sigma_{\text{NMAD}}$, outlier fractions, linear correlation).

## Usage

1. Clone the repository:
   ```bash
   git clone https://github.com/albertnica/TFGF-albertnica
   ```
2. Install dependencies (Python 3.13):
   ```bash
   pip install -r requirements.txt
   ```
3. If an NVIDIA GPU is available, install PyTorch with [CUDA support](https://developer.nvidia.com/cuda-gpus):
   ```bash
   pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu128
   ```
4. Download the preprocessed datasets and evaluation checkpoints:
   - [/data](https://unioviedo-my.sharepoint.com/:f:/g/personal/uo284776_uniovi_es/EiifkyAiWZ9HqcUwLZppl-4BypXdYoWkSBb3XhzWpCAhqw?e=ihWSAh)
   - [/spectrums](https://unioviedo-my.sharepoint.com/:f:/g/personal/uo284776_uniovi_es/EhBzxXiQb2pOuGz-wNQrxUwBxq-gw6r4Nu20r6s61r3LSg?e=C3JDgY)
   - [/storage](https://unioviedo-my.sharepoint.com/:f:/g/personal/uo284776_uniovi_es/EsZm738_E2FKoC7dOV0W8hIB3JHfctzO4ZNtGTlCgmFDTg?e=Sc1KSx)

## Brief Results and Conclusions

A comprehensive evaluation was performed on the full SDSS dataset comprising **3,372,890** spectra. Below is a summary of the quantitative benchmarks, architectural comparisons, and physical findings detailed in the thesis report ([`Chapter 5`](Overleaf/5.tex) & [`Chapter 6`](Overleaf/6.tex)).

### Quantitative Model Benchmark (Full SDSS Catalog)

| Model Architecture | Variant / Key Feature | Pearson Correlation ($r$) | Mean Absolute Error (MAE) | Negative Predictions ($z < 0$) |
| :--- | :--- | :---: | :---: | :---: |
| **MLP** | Baseline Fully-Connected | 0.477 | 0.128 | 2,181 |
| **CNN Variant 0** | Baseline 1D CNN | 0.011 | 0.098 | - |
| **CNN Variant 1** | Linear Layer Dropout | 0.009 | 0.134 | - |
| **CNN Variant 2** | Extended Conv Depth | 0.005 | 0.153 | - |
| **CNN Variant 4** | Dropout across All Layers | 0.001 | 0.356 | - |
| **CNN Variant 3** | **Conv + MultiHeadAttention** | **0.967** | **0.073** | **565** |
| **TRA** | Transformer (StandardScaler) | **0.965** | **0.039** | 1,570 |
| **TRA Renorm** | **Transformer (Shape-Preserving)** | **0.975** | **0.030** | **665** |

---

### Key Findings and Visual Analysis

#### 1. Dataset Scaling (600k vs 1M Samples)
Scaling the training dataset from 600,000 to 1,000,000 spectra directly improved performance in the sparse, high-redshift regime ($z_{\text{spec}} > 5$) without causing overfitting across mid-redshift intervals. High-$z$ MAE decreased from **3.7** down to **1.8**.

<p align="center">
  <img src="Overleaf/images/79.png" width="48%" alt="TRA 600k High Redshift" />
  <img src="Overleaf/images/80.png" width="48%" alt="TRA 1M High Redshift" />
  <br />
  <em>Figure: Logarithmic density heatmaps for high redshifts: TRA 600k (left) vs. TRA 1M (right). Predictions consolidate along the identity diagonal.</em>
</p>

#### 2. Architecture Comparison: Attention vs. Local Convolutions
- **MLP**: Demonstrated inadequate capacity to capture fine spectral features ($r = 0.477$).
- **Standard CNNs (0-2, 4)**: Captured localized emission profiles but suffered when aggressive dropout was introduced across convolutional stages.
- **CNN Variant 3 (Attention)**: Integrating `MultiHeadAttention` over convolutional feature maps enabled long-range feature correlation, driving linear correlation to $0.967$ and MAE to $0.073$.
- **Transformer (TRA)**: Achieved the highest fidelity across the full spectral range by directly modeling interactions between distant emission lines (e.g., [O III], H$\beta$, and Lyman-$\alpha$).

<p align="center">
  <img src="Overleaf/images/118.png" width="48%" alt="CNN Variant 3 Evaluation" />
  <img src="Overleaf/images/128.png" width="48%" alt="Transformer TRA Evaluation" />
  <br />
  <em>Figure: Full-range evaluation heatmaps for CNN Variant 3 with Attention (left) vs. pure Transformer TRA (right).</em>
</p>

#### 3. Influence of Input Normalization (Mitigating Data Drift)
Replacing scikit-learn's standard z-score normalization (`StandardScaler`) with a **shape-preserving normalization** (zero-centering wavelength, scaling wavelength to $[-1, 1]$, and normalizing flux to unit peak amplitude) reduced MAE from **0.039** to **0.030**, cut negative predictions by over 57%, and improved resilience to instrument domain shifts.

<p align="center">
  <img src="Overleaf/images/135.png" width="48%" alt="TRA StandardScaler" />
  <img src="Overleaf/images/145.png" width="48%" alt="TRA Shape-Preserving Normalization" />
  <br />
  <em>Figure: Prediction heatmaps using StandardScaler (left) vs. Shape-Preserving Normalization (right).</em>
</p>

#### 4. Challenges & Out-of-Distribution Limitations
- **Near-Zero Redshift ($z \approx 0$)**: On stellar spectra ($z \le 0.00462$), uniform absolute error metrics yield significant relative dispersion ($\text{MAE} \approx 0.628$), pointing to the need for adaptive redshift-weighted loss functions or dedicated low-$z$ sub-networks.
- **Instrument Transfer (Data Drift)**: Evaluating SDSS-trained models on out-of-distribution NASA/NED spectra exposed domain shift sensitivities stemming from different native spectrograph resolutions and line profile samplings:

<p align="center">
  <img src="Overleaf/images/155.png" width="48%" alt="Data Drift Unrenormalized" />
  <img src="Overleaf/images/156.png" width="48%" alt="Data Drift Renormalized" />
  <br />
  <em>Figure: Out-of-distribution evaluation on NASA/NED spectra: Unrenormalized (left, r=0.812, MAE=0.335) vs. Renormalized (right, r=0.720, MAE=0.483).</em>
</p>

---

### Conclusions

1. **Primacy of Attention Mechanisms**: Modeling long-range spectral correlations via self-attention is essential for astronomical redshift estimation, as single localized emission peaks are physically degenerate without their multiplet pairs.
2. **Generalization through Normalization**: Shape-preserving spectral normalization preserves critical line-to-continuum flux ratios and protects against instrument-level baseline shifts.
3. **Survey Utility**: High-performing attention models serve not only for rapid automated redshift estimation but also as robust quality-control filters to flag anomalous catalog entries and misclassified spectra in large astronomical surveys.