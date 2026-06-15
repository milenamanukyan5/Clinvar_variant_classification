# ClinVar Genetic Variant Classification

### 🚀 Project Overview
This repository contains an end-to-end Machine Learning pipeline designed to predict whether a genetic variant submitted to **ClinVar (NCBI)** will have a **conflicting clinical classification** (`CLASS = 1`) or a consistent one (`CLASS = 0`). 

Resolving conflicting classifications is a critical challenge in medical genetics, as it directly impacts patient diagnosis and precision medicine workflows.

### 📊 Dataset & Problem Type
- **Source:** Kaggle (`kevinarvai/clinvar-conflicting`) containing clinical variants with human genomic annotations.
- **Problem Type:** Imbalanced Binary Classification.
- **Class Distribution:** ~75% Consistent (`CLASS = 0`), ~25% Conflicting (`CLASS = 1`).

### 🛠️ Key Pipeline Steps
1. **Exploratory Data Analysis (EDA):** Analyzed highly right-skewed allele frequencies (`AF_ESP`, `AF_EXAC`, `AF_TGP`) and diagnosed variant impact variations.
2. **Data Preprocessing:** Handled missing values through domain-specific imputation, performed scaling for distance-based models, and applied One-Hot Encoding for categorical features (e.g., `IMPACT`).
3. **Advanced Feature Engineering:** Created high-impact domain features to boost model predictability:
   - `af_max`: The maximum allele frequency across all reference populations.
   - `is_snv`: Boolean indicator distinguishing Single Nucleotide Variants from Indels.
   - `n_missing_annotations`: Count of missing annotations per variant as a proxy for rare/unstudied genomic regions.

### 🧠 Models Evaluated
To find the optimal decision boundary under class imbalance, **11 different Machine Learning algorithms** were trained and bench-marked, utilizing techniques like `class_weight="balanced"` and `scale_pos_weight` to address skewness.

| Model | Accuracy | Precision | Recall | F1-Score | ROC-AUC | PR-AUC |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **LightGBM** | **0.736** | **0.485** | **0.759** | **0.592** | **0.825** | **0.594** |
| XGBoost | 0.758 | 0.516 | 0.662 | 0.580 | 0.813 | 0.575 |
| Random Forest | 0.782 | 0.651 | 0.293 | 0.404 | 0.820 | 0.580 |
| Logistic Regression | 0.555 | 0.335 | 0.778 | 0.468 | 0.679 | 0.377 |
| Decision Tree | 0.724 | 0.451 | 0.443 | 0.447 | 0.631 | 0.340 |

### 🏆 Key Findings
- **Champion Model:** **LightGBM** emerged as the absolute winner, achieving the highest balance between Precision and Recall (**F1-Score = 0.592**) and an outstanding **ROC-AUC of 0.825**.
- **Clinical Relevance:** Since missing a conflicting variant (False Negative) is highly critical in medical diagnostics, high **Recall (75.9%)** makes LightGBM exceptionally suited for this domain.
- **Feature Importance:** The engineered features (`af_max` and `n_missing_annotations`) ranked among the top 5 most important features in tree-based ensembles, validating the custom feature engineering step.

### 💻 Installation & Usage

1. Clone the repository:
```bash
git clone [https://github.com/your-username/clinvar-variant-classification.git](https://github.com/your-username/clinvar-variant-classification.git)
cd clinvar-variant-classification
