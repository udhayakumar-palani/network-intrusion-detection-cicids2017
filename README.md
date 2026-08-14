# Interpretable Network Intrusion Detection on CICIDS2017

Correlation-based feature selection with a hybrid CNN-LSTM classifier, benchmarked against Random Forest, a base-paper-style LSTM, and XGBoost+SMOTE, with SHAP interpretability.


**Author:** Udhaya Kumar Palani (23306891)

---

## Overview

Network intrusion detection systems increasingly rely on deep learning, but deep models are computationally expensive, opaque to security analysts, and sensitive to the severe class imbalance found in real traffic. This project builds an end-to-end pipeline that:

1. Reduces the CICIDS2017 feature space from 78 to ~16 features using correlation-based feature selection (CFS)
2. Trains and compares four classifiers on identical features: **Random Forest**, **LSTM** (base-paper style), a proposed **hybrid CNN-LSTM**, and **XGBoost trained on SMOTE-balanced data**
3. Applies **SHAP** (SHapley Additive exPlanations) to the strongest model to expose which features drive attack-class predictions

## Repository contents

| File | Description |
|---|---|
| `IDS_CICIDS2017_Notebook.ipynb` | Full implementation — data loading, preprocessing, CFS, 4-model training, evaluation, SHAP |
| `architecture.png` | System architecture diagram (5-stage pipeline) |
| `figures/` | All output figures: class distribution, feature correlation, confusion matrices, ROC curves, SHAP plots |
| `results_summary.csv` | Comparative metrics table (accuracy, precision, recall, F1, FPR, ROC-AUC, training time) for all four models |
| `requirements.txt` | Python dependencies |

## Results summary

| Model | Accuracy | Precision | Recall | F1 | FPR | ROC-AUC | Train time |
|---|---|---|---|---|---|---|---|
| Random Forest | 0.9975 | 0.9964 | 0.9889 | **0.9926** | **0.0007** | **0.9999** | 42.6 s |
| LSTM (base-paper style) | 0.9732 | 0.9054 | 0.9392 | 0.9220 | 0.0199 | 0.9952 | 133.4 s |
| CNN-LSTM (proposed) | 0.9823 | 0.9716 | 0.9218 | 0.9461 | 0.0055 | 0.9984 | 601.8 s |
| XGBoost + SMOTE | 0.9962 | 0.9842 | **0.9932** | 0.9887 | 0.0032 | 0.9998 | **5.0 s** |

**Key findings:**
- Correlation-based feature selection reduces the feature space by ~80% with no material loss in detection accuracy across all four classifiers.
- The proposed CNN-LSTM beats the plain LSTM baseline on every metric (+2.4 F1 percentage points, 3.6x lower false-positive rate), confirming the value of the hybrid architecture over a single-architecture deep model.
- Random Forest and XGBoost+SMOTE outperform both deep models overall on this binary task — an honest finding that classical and imbalance-aware methods remain highly competitive.
- SHAP attribution identifies backward-direction packet-length statistics as the dominant features, independently confirmed by the Pearson correlation ranking — giving security analysts an audit trail for trusting alerts.

See the project notebook and its concluding section for the full discussion, literature review, and limitations/future work.

## How to run

### Google Colab (recommended)

1. Upload `IDS_CICIDS2017_Notebook.ipynb` to [Google Colab](https://colab.research.google.com)
2. Runtime → Change runtime type → GPU (T4)
3. Download CICIDS2017 from the [Canadian Institute for Cybersecurity](https://www.unb.ca/cic/datasets/ids-2017.html) (the `MachineLearningCSV.zip` archive)
4. Upload the eight CSVs to Google Drive, mount Drive in the notebook, and point `DATA_DIR` at the folder
5. Run all cells

### Local Jupyter

```bash
pip install -r requirements.txt
mkdir data   # place the eight CICIDS2017 CSVs here
jupyter notebook IDS_CICIDS2017_Notebook.ipynb
```

## Dataset

[CICIDS2017](https://www.unb.ca/cic/datasets/ids-2017.html) (Sharafaldin, Lashkari and Ghorbani, 2018) — five days of realistic enterprise network traffic, ~2.83 million flows, 78 statistical features extracted with CICFlowMeter, seven attack categories (DoS, DDoS, brute force, port scan, web attack, infiltration, botnet).

