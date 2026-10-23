# Topological Data Analysis (TDA) for Precision Agriculture

Official code repository for the manuscript:  
**"Unveiling Hidden Agricultural Domain Structures via Topological Data Analysis: An Explainable AI Framework for Soil, Crop Quality, and Microclimate Analytics"**  
*Submitted to: Computers and Electronics in Agriculture (Elsevier)*

---

## 📌 Overview

This repository provides an end-to-end, reproducible framework integrating **Topological Data Analysis (TDA)**—specifically Vietoris-Rips Persistent Homology ($H_0, H_1$), Persistence Entropy, and Takens Delay Coordinate Embedding—with state-of-the-art tree-based machine learning ensembles (**ExtraTrees, RandomForest, XGBoost, LightGBM, CatBoost**). 

The framework systematically extracts global geometric shape invariants across three primary agricultural domain modalities:
1. **Spatial Point Clouds** (*Soil Property & Nutrient Analytics*)
2. **Physicochemical Profiles** (*Crop Quality & Physiological Assessment*)
3. **Temporal Time-Series** (*Microclimate & Agro-Meteorology Forecasting*)

---

## 📁 Repository Structure

```text
.
├── data/
│   ├── raw/                  # Raw benchmark agricultural datasets
│   └── processed/            # Processed datasets enriched with TDA features
├── src/
│   ├── main_pipeline.py      # Main pipeline for data processing, TDA extraction, and model evaluation
│   ├── dataset_description.py# Metadata generator for benchmark datasets
│   └── visualize_tda_impact.py# Feature space visualization (Before vs. After TDA)
├── demo/
│   ├── run_demonstration.py  # Field operational walkthrough script
│   └── outputs/              # Generated VRA prescription maps, audits & alerts
├── results/
│   ├── tables/               # Benchmark evaluation summary CSV tables (R², RMSE, Latency)
│   └── figures/              # High-resolution (300 DPI) publication-ready figures
├── requirements.txt          # Python dependencies
└── README.md                 # Project documentation

```
---

## 🛠️ Installation & Prerequisites

Ensure you have **Python 3.8+** installed. Clone this repository and install the required dependencies using `pip`:

```bash
# Install required Python packages
pip install -r requirements.txt
```
## Key Dependencies
* **gudhi**: Topological Data Analysis & Simplex Tree Engine
* **scikit-learn**: Standard Machine Learning & Preprocessing
* **xgboost, lightgbm, catboost**: Gradient Boosted Tree Ensembles
* **pandas, numpy**: Data Processing
* **matplotlib, seaborn**: Data & Manifold Visualization

---

## 🚀 Usage & Reproduction Steps

To execute the entire experimental pipeline—including downloading datasets, constructing $k$-NN point clouds, extracting Vietoris-Rips persistent homology invariants, and benchmarking across all 5 decision tree regressors—run:

```bash
python src/main_pipeline.py
```
## Visualizing Topological Manifold Impact
To generate the 2D PCA feature-space comparison illustrating the manifold regularization effect (Before vs. After TDA enrichment):
```bash
python src/visualize_tda_impact.py
```
---
### 3. Run Field Operational Demonstration
To simulate the three-tier farm deployment (VRA nitrogen prescription map at 15 m grid, post-harvest quality classification audit, and Takens 3D frost anomaly alert):

```bash
python demo/run_demonstration.py
```
---
## 📊 Benchmark Results Summary (Strict 5-Fold Cross-Validation)

All architectures were benchmarked under identical 5-fold CV splits with fixed hyperparameters ($n_{\text{estimators}} = 200, \text{learning\_rate} = 0.05$).

### 1. Cross-Validated Performance Across Modalities
| Benchmark Dataset | Samples | Evaluated Model | Baseline R² | Baseline RMSE | Proposed (+TDA) R² | Proposed (+TDA) RMSE | Performance Shift (ΔR²) |
| :--- | :---: | :--- | :---: | :---: | :---: | :---: | :---: |
| **Soil Property**<br>*(Spatial Point Cloud)* | N = 505 | ExtraTrees | 0.8829 | 3.0802 | 0.8742 | 3.2093 | -0.98% |
| | | RandomForest | 0.8698 | 3.2231 | 0.8668 | 3.2608 | -0.34% |
| | | XGBoost | 0.8689 | 3.1766 | 0.8644 | 3.2183 | -0.51% |
| | | LightGBM | 0.8710 | 3.2366 | 0.8582 | 3.3959 | -1.47% |
| | | CatBoost | **0.8889** | **3.0223** | 0.8816 | 3.1278 | -0.82% |
| **Crop Quality**<br>*(Multivariate Chemical)* | N = 4,897 | ExtraTrees | 0.5557 | 0.5901 | **0.5569** | **0.5894** | +0.21% |
| | | RandomForest | 0.5272 | 0.6088 | 0.5329 | 0.6051 | +1.09% |
| | | XGBoost | 0.4665 | 0.6467 | 0.4775 | 0.6399 | +2.37% (*p* < 0.01) |
| | | LightGBM | 0.4744 | 0.6418 | 0.4837 | 0.6361 | +1.96% |
| | | CatBoost | 0.4079 | 0.6812 | 0.4205 | 0.6738 | **+3.09%** (*p* < 0.001) |
| **Agro-Climate**<br>*(Temporal Time-Series)* | N = 3,647 | ExtraTrees | 0.5514 | 2.7218 | 0.5736 | 2.6523 | **+4.02%** (*p* < 0.001) |
| | | RandomForest | 0.5677 | 2.6717 | 0.5897 | 2.6020 | +3.88% (*p* < 0.01) |
| | | XGBoost | 0.5949 | 2.5858 | 0.5936 | 2.5901 | -0.22% |
| | | LightGBM | 0.5980 | 2.5759 | 0.6011 | 2.5663 | +0.52% |
| | | CatBoost | **0.6234** | **2.4932** | 0.6215 | 2.4997 | -0.31% |

---
### 2. Computational Latency & Edge Feasibility

| Processing Stage | Soil Property (N = 505) | Crop Quality (N = 4,897) | Agro-Climate (N = 3,647) |
| :--- | :---: | :---: | :---: |
| k-NN Spatial Graph Indexing | 0.016 s | 0.118 s | 0.088 s |
| Vietoris-Rips Simplex Filtration (H₀, H₁) | 0.512 s | 5.084 s | 3.792 s |
| **Per-Sample Extraction Latency** | **1.01 ms/sample** | **1.03 ms/sample** | **1.04 ms/sample** |
| Ensemble Inference Latency | 0.007 s | 0.032 s | 0.026 s |

---

## 💡 Key Domain Discoveries & XAI Insights

* **Dominant Soil Scale (ε ∈ [1.2, 2.5]):** Betti curves (β₀, β₁) identify the intrinsic spatial stability plateau corresponding to a physical distance of 14.8 m – 31.2 m. This defines an objective **15 m grid resolution** for Variable Rate Application (VRA) machinery.
* **Biochemical Equilibrium Loops:** In crop quality evaluation, persistent H₁ loop lifespans (`TDA_Feat_1`, Gini ≈ 0.070) rank fourth overall in feature importance, outperforming univariate indices like citric acid, residual sugar, and density.
* **Microclimate Attractor Tracking:** Takens 3D delay embedding (τ = 1 day, d = 3) reconstructs annual cyclic thermodynamic orbits, providing early detection of unseasonal frost anomalies 12–18 hours in advance.
* **Sample Density Boundaries:** On sparse spatial grids (N = 505), local simplex filtration induces over-smoothing (-0.34% to -1.47%), proving that TDA in sparse pedology is suited for management zone delineation rather than direct tabular regression input.

---
## 📜 License

This project is released under the **MIT License**. See the [`LICENSE`](LICENSE) file for details.
