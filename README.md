# Week 05 & 06 — Fraud Detection for Adey Innovations Inc.

## Overview

This project implements an **end-to-end Fraud Detection System** for **e-commerce and bank transactions** at **Adey Innovations Inc.**  
It covers the full machine learning lifecycle:

- Data cleaning & exploratory analysis  
- Feature engineering & geolocation integration  
- Handling extreme class imbalance  
- Model building, evaluation, and comparison  
- Model explainability using **SHAP (Explainable AI)**  
- Business-driven recommendations  

The goal is to **accurately detect fraudulent transactions** while balancing **security, interpretability, and customer experience**.

---

## Table of Contents

1. [Business Objective](#business-objective)  
2. [Datasets](#datasets)  
3. [Environment Setup](#environment-setup)  
4. [Project Structure](#project-structure)  
5. [Task 1: Data Analysis & Preprocessing](#task-1-data-analysis--preprocessing)  
6. [Task 2: Model Building & Evaluation](#task-2-model-building--evaluation)  
7. [Task 3: Model Explainability (SHAP)](#task-3-model-explainability-shap)  
8. [Results Summary](#results-summary)  
9. [Usage](#usage)  
10. [Key Lessons Learned](#key-lessons-learned)  

---

## Business Objective

Adey Innovations Inc. aims to:

- Detect fraudulent transactions in **e-commerce** and **bank credit card data**
- Minimize **false negatives** (missed fraud)
- Control **false positives** to protect user experience
- Build **interpretable models** suitable for real-world deployment
- Enable **data-driven fraud prevention strategies**

---

## Datasets

### 1. Fraud_Data.csv (E-commerce Transactions)

| Column | Description |
|------|-------------|
| user_id | Unique user identifier |
| signup_time | Account creation timestamp |
| purchase_time | Transaction timestamp |
| purchase_value | Transaction amount |
| device_id | Device identifier |
| source | Traffic source |
| browser | Browser used |
| sex | User gender |
| age | User age |
| ip_address | Transaction IP |
| class | Target: 1 = Fraud, 0 = Legit |

### 2. IpAddress_to_Country.csv

Used to map IP ranges to countries for **geolocation fraud analysis**.

### 3. creditcard.csv (Bank Transactions)

| Column | Description |
|------|-------------|
| Time | Seconds since first transaction |
| V1–V28 | PCA-anonymized features |
| Amount | Transaction amount |
| Class | Target: 1 = Fraud, 0 = Legit |

⚠️ **Both datasets are highly imbalanced**, which strongly influences modeling choices.

---

## Environment Setup

```bash
python -m venv .venv
.venv\Scripts\activate      # Windows
pip install --upgrade pip
pip install -r requirements.txt
jupyter notebook
```

---

## Project Structure

```
fraud-detection/
│
├── data/
│   ├── raw/                # Original datasets (gitignored)
│   └── processed/          # Cleaned & feature-engineered data
│
├── notebooks/
│   ├── eda-fraud-data.ipynb
│   ├── eda-creditcard.ipynb
│   ├── feature-engineering.ipynb
│   ├── modeling.ipynb
│   └── shap-explainability.ipynb
│
├── models/                 # Saved trained models
├── src/                    # Reusable scripts (optional)
├── tests/                  # Unit tests (future extension)
├── requirements.txt
├── .gitignore
└── README.md
```

---

## Task 1: Data Analysis & Preprocessing

### Key Steps

**Data Cleaning**
- Removed duplicates
- Corrected data types
- Handled missing values with justified strategies

**Exploratory Data Analysis**
- Univariate & bivariate analysis
- Fraud vs non-fraud distribution
- Behavioral and monetary patterns

**Feature Engineering**
- `time_since_signup`
- `hour_of_day`, `day_of_week`
- Transaction frequency per user
- IP-to-country geolocation features

**Transformation**
- One-hot encoding of categorical variables
- Feature scaling for numerical variables

**Class Imbalance Handling**
- Applied **SMOTE only on training data**
- Preserved realistic test distribution

---

## Task 2: Model Building & Evaluation

### Models Trained

**Baseline Model**
- Logistic Regression (interpretable benchmark)

**Ensemble Model**
- Random Forest Classifier
- Basic hyperparameter tuning (`n_estimators`, `max_depth`)

### Evaluation Metrics (Imbalanced-Aware)

- **AUC–PR**
- **F1-Score**
- **Confusion Matrix**
- Stratified 5-Fold Cross-Validation

### Model Selection

The final model was chosen based on:
- Superior fraud recall
- Strong precision–recall trade-off
- Stability across folds
- Interpretability support via SHAP

---

## Task 3: Model Explainability (SHAP)

### Explainability Techniques

- Built-in Random Forest feature importance
- SHAP TreeExplainer for global & local explanations

### Visualizations

- SHAP Summary Plot (global importance)
- SHAP Force Plots for:
  - True Positive (correct fraud)
  - False Positive (legitimate flagged)
  - False Negative (missed fraud)

### Insights

- Temporal behavior and transaction velocity are strong fraud drivers
- High-value transactions shortly after signup increase risk
- Certain geolocations show elevated fraud likelihood

---

## Results Summary

| Model | AUC-PR | F1-Score | Interpretability |
|------|-------|---------|----------------|
| Logistic Regression | Moderate | Moderate | High |
| Random Forest | High | High | Medium (SHAP) |

✅ **Random Forest** selected as the final model.

---

## Usage

1. Activate environment:
```bash
.venv\Scripts\activate
```

2. Run notebooks in order:
- `eda-fraud-data.ipynb`
- `feature-engineering.ipynb`
- `modeling.ipynb`
- `shap-explainability.ipynb`

3. Review SHAP plots and business insights.

---

## Key Lessons Learned

- Fraud detection requires **precision–recall trade-offs**, not accuracy
- Class imbalance handling is critical
- Behavioral features outperform raw transaction values
- Explainability builds **business trust** and actionable insight
- SHAP is powerful but computationally expensive — sampling is essential

---

## Author

**Nigus Dibekulu**  
Artificial Intelligence Mastery Program  
Week 05 & 06 — Fraud Detection Project
