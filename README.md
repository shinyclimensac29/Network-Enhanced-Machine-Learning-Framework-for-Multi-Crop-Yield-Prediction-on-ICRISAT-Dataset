# Network-Enhanced Machine Learning for Multi-Crop Yield Prediction

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![DOI](https://img.shields.io/badge/DOI-10.xxxx%2Fxxxxx-blue)](https://doi.org/10.xxxx/xxxxx)

A comprehensive machine learning framework for predicting crop yields across six major Indian crops using temporal features, diversification indices, and network-based approaches.

## 📋 Overview

This repository contains the complete implementation and research article for crop yield prediction using:
- **52 years of agricultural data** (1966-2017)
- **311 districts** across 20 Indian states
- **6 major crops**: Rice, Wheat, Maize, Groundnut, Cotton, Sugarcane
- **Advanced ML models**: Random Forest, Gradient Boosting, Ridge Regression
- **Network analysis**: District similarity networks with multiple centrality measures

## 🎯 Key Findings

- **Exceptional Accuracy**: All crops achieve **R² > 0.94** with Random Forest
  - Rice: R² = 0.988, MAE = 35.88 kg/ha (2.7% MAPE)
  - Wheat: R² = 0.976, MAE = 54.08 kg/ha (2.8% MAPE)
  - Maize: R² = 0.971, MAE = 58.39 kg/ha (2.9% MAPE)
  - Sugarcane: R² = 0.986, MAE = 129.68 kg/ha (8.1% MAPE)

- **Temporal Features Dominate**: Lag-1 features alone explain 30-50% of variance
- **Network Features Minimal Impact**: Network centrality measures contribute <1% to predictions
- **75-85% Error Reduction** compared to naive baselines (statistically significant, p < 0.05)
- **Temporally Stable**: Models maintain performance across different time periods

## 📁 Repository Structure

```
├── code/
│   ├── main_pipeline.py              # Complete prediction pipeline
│   ├── data_preprocessing.py         # Data cleaning and feature engineering
│   ├── network_construction.py       # District and crop networks
│   ├── models.py                     # ML model implementations
│   └── evaluation.py                 # Metrics and statistical tests
│
├── data/
│   ├── icrisat_raw.csv              # Raw ICRISAT dataset (not included - see Data section)
│   └── icrisat_cleaned_with_features.csv  # Processed dataset with features
│
├── results/
│   ├── model_performance_summary.csv     # Performance metrics by model
│   ├── best_models_advanced_only.csv     # Best model per crop
│   ├── statistical_significance_tests.csv # Statistical test results
│   ├── temporal_stability_analysis.csv    # Temporal stability metrics
│   ├── feature_importance_summary.csv     # Feature importance rankings
│   └── diagnostics/                       # Diagnostic plots (PDFs)
│
├── paper/
│   ├── main.tex                     # LaTeX manuscript
│   ├── references.bib               # Bibliography
│   ├── figures/                     # Figures for paper
│   └── tables/                      # Tables for paper
│
├── notebooks/
│   └── exploratory_analysis.ipynb   # Jupyter notebook for EDA
│
├── requirements.txt                 # Python dependencies
├── README.md                       # This file
└── LICENSE                         # MIT License
```

## 🚀 Quick Start

### Prerequisites

```bash
Python 3.8+
```

### Installation

```bash
# Clone repository
git clone https://github.com/yourusername/crop-yield-prediction.git
cd crop-yield-prediction

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### Running the Pipeline

```python
# Run complete pipeline
python code/main_pipeline.py

# Output: All results saved to results/ directory
```

## 📊 Dataset

This research uses the **ICRISAT District-Level Database** for Indian agriculture:
- **Source**: [ICRISAT VDSA](http://vdsa.icrisat.ac.in/vdsa-database.aspx)
- **Coverage**: 1966-2017 (52 years)
- **Spatial**: 311 districts across 20 states
- **Crops**: 23 crops (6 analyzed: Rice, Wheat, Maize, Groundnut, Cotton, Sugarcane)
- **Variables**: Yield (kg/ha), Area (ha), Production

**Note**: Raw data not included due to size. Download from ICRISAT website and place in `data/` folder.

## 🔬 Methodology

### Feature Engineering
- **Temporal Features**: 
  - Lag features (1, 2, 3 years)
  - Rolling mean (3-year window)
  - Year-over-year changes
  - Rolling volatility (std)

- **Diversification Indices**:
  - Simpson diversity index
  - Number of active crops

- **Network Features** (6 measures):
  - Degree, Strength, Closeness
  - Betweenness, Eigenvector, Clustering

### Network Construction
- **District Similarity Network**: 
  - Cosine similarity on yield profiles (2008-2017)
  - Threshold: 0.80 (high similarity)
  - Top-k pruning: k=15 edges per node
  - Result: 311 nodes, 2,996 edges, 6.2% density

- **Crop Co-occurrence Network**:
  - 23 crops, 253 edges
  - Based on joint cultivation patterns

### Models Evaluated
1. **Baselines**: Naive (lag-1), Rolling Mean (3-year)
2. **Advanced**: 
   - Ridge Regression (α=10.0, RobustScaler)
   - Random Forest (300 trees)
   - Gradient Boosting (default params)

### Evaluation Protocol
- **Time-Series Cross-Validation**: 3 folds with expanding window
- **Metrics**: MAE, RMSE, MAPE, MedAE, R²
- **Statistical Tests**: Paired t-tests (α=0.05)
- **Temporal Stability**: MAE/R² trends across folds
- **Diagnostics**: Residual analysis, Q-Q plots

## 📈 Results Summary

### Model Performance

| Crop | Best Model | MAE (kg/ha) | RMSE (kg/ha) | MAPE (%) | R² |
|------|-----------|-------------|--------------|----------|-----|
| Rice | RandomForest | 35.88 ± 9.87 | 88.23 ± 22.79 | 2.69 | 0.988 |
| Wheat | RandomForest | 54.08 ± 13.72 | 133.55 ± 35.44 | 2.77 | 0.976 |
| Maize | RandomForest | 58.39 ± 22.80 | 163.40 ± 43.30 | 2.94 | 0.971 |
| Groundnut | RandomForest | 42.11 ± 24.12 | 87.46 ± 34.18 | 4.59 | 0.946 |
| Cotton | RandomForest | 13.77 ± 5.90 | 24.97 ± 8.40 | 4.47 | 0.969 |
| Sugarcane | RandomForest | 129.68 ± 19.33 | 308.99 ± 36.80 | 8.08 | 0.986 |

### Network Feature Contribution

| Crop | Network Importance | Top Temporal Feature |
|------|-------------------|---------------------|
| Rice | 0.1% | lag1 (42.3%) |
| Wheat | 0.2% | lag1 (38.7%) |
| Maize | 0.0% | lag1 (45.1%) |
| Groundnut | 0.0% | rolling_mean (31.2%) |
| Cotton | 0.3% | lag1 (29.8%) |
| Sugarcane | 0.9% | lag1 (48.6%) |

### Statistical Significance

- **Network Features**: p > 0.05 for all crops (not significant)
- **Advanced vs. Naive**: p < 0.05 for 5/6 crops (significant)
- **Improvement**: 75-85% error reduction vs. baselines

## 📄 Research Paper

### Compiling the LaTeX Manuscript

```bash
cd paper/
pdflatex main.tex
bibtex main
pdflatex main.tex
pdflatex main.tex
```

Or use **Overleaf** by uploading all files from `paper/` directory.

### Paper Structure
- **30 pages** (Springer format)
- **6 sections**: Introduction, Related Work, Methodology, Results, Discussion, Conclusion
- **11 tables**, **13 figures**, **3 algorithms**
- **70+ references** with DOIs (2000-2025)

## 🛠️ Dependencies

```
numpy>=1.21.0
pandas>=1.3.0
matplotlib>=3.4.0
seaborn>=0.11.0
scikit-learn>=1.0.0
networkx>=2.6.0
scipy>=1.7.0
```

See `requirements.txt` for complete list with versions.

## 📊 Visualizations

The pipeline generates:
- **Performance comparison plots** (bar charts, line plots)
- **Network visualizations** (district and crop networks)
- **Feature importance charts** (horizontal bar charts)
- **Diagnostic plots** (4-panel: predicted vs. observed, residuals, distribution, Q-Q)
- **Temporal stability plots** (MAE/R² trends)

All saved in `results/` directory as PDF/PNG.

## 🔄 Reproducibility

To reproduce all results:

```bash
# 1. Download ICRISAT data
# Place in data/icrisat_raw.csv

# 2. Run pipeline
python code/main_pipeline.py

# 3. Results saved to results/
# 4. Compare with provided results/ files
```

**Random seed**: 42 (set for reproducibility)

## 🤝 Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit changes (`git commit -am 'Add improvement'`)
4. Push to branch (`git push origin feature/improvement`)
5. Open a Pull Request

## 📝 Citation

If you use this code or methodology in your research, please cite:

```bibtex
@article{yourname2025crop,
  title={Network-Enhanced Machine Learning Framework for Multi-Crop Yield Prediction: A Comprehensive Analysis of Indian Agricultural Data},
  author={Your Name and Co-Authors},
  journal={Computers and Electronics in Agriculture},
  year={2025},
  volume={XXX},
  pages={XXXXX},
  doi={10.XXXX/XXXXX}
}
```

## 👥 Authors

- **Author 1** - *Research Scholar* - [Email](mailto:author1@email.com)
- **Author 2** - *Research Scholar* - [Email](mailto:author2@email.com)
- **Author 3** - *Professor* - [Email](mailto:author3@email.com)

**Affiliation**: Department, University/Institution, Country

## 📧 Contact


## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **ICRISAT** for providing the district-level agricultural database
- **[Funding Agency]** for financial support (Grant No. XXXX)
- **Computational resources** provided by [Institution]

## 📚 References

Full bibliography available in `paper/references.bib` (70+ references)

Key papers:
- van Klompenburg et al. (2020). Crop yield prediction using machine learning: A systematic literature review. *Computers and Electronics in Agriculture*.
- Khaki & Wang (2019). Crop yield prediction using deep neural networks. *Frontiers in Plant Science*.
- Jeong et al. (2016). Random forests for global and regional crop yield predictions. *PLOS ONE*.

---

**Last Updated**: November 2025  
**Status**: ✅ Complete | 📝 Paper Under Review | 🚀 Code Production-Ready
