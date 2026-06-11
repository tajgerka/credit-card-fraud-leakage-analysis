# Credit Card Fraud: Leakage Analysis and Dataset Artifacts

## Project Overview
This project explores fraud detection using a simulated credit card transaction dataset. 
The goal is not only to build a fraud classification model, but also to investigate why unusually high model performance may appear in synthetic fraud datasets.

The project focuses on:
- time-based validation
- class imbalance
- PR-AUC and ROC-AUC evaluation
- Logistic Regression baseline
- CatBoost nonlinear model
- feature engineering experiments
- leakage and dataset artifact analysis

## Business Problem
Fraud detection models are used to identify suspicious transactions and support operational decisions such as:
- auto-approving low-risk transactions
- sending medium-risk transactions to manual review
- declining high-risk transactions

In fraud detection, high accuracy is not enough. The model must be evaluated carefully because fraud is rare, false positives create customer friction, and leakage can make results look unrealistically good.

## Dataset
The project uses a simulated credit card transaction dataset with transaction-level records from 2019–2020.

The target variable is binary:
- `0` — legitimate transaction
- `1` — fraudulent transaction

Fraud rate is highly imbalanced at approximately **0.58%**, so accuracy is not used as the main metric.

## Validation Strategy
A time-based validation strategy is used to better simulate real-world fraud detection.
The model is trained on earlier transactions and validated on later transactions.  
This helps reduce the risk of future information leaking into the training process.

## Methodology

### 1. Exploratory Data Analysis
The EDA focuses on:
- fraud rate by transaction category
- fraud rate by hour
- transaction amount distribution
- customer age groups
- merchant and state-level fraud patterns

### 2. Baseline Model
A Logistic Regression model is used as a simple and interpretable baseline.

Baseline results:
| Model | PR-AUC | ROC-AUC |
|---|---:|---:|
| Logistic Regression | 0.115 | 0.826 |

The baseline model captures some fraud signal, but precision remains low because of strong class imbalance and limited feature interactions.

### 3. Nonlinear Model
CatBoost is trained using the same core feature set as the baseline.

| Model | PR-AUC | ROC-AUC |
|---|---:|---:|
| CatBoost | 0.92 | 0.998 |

The very strong performance suggests that the dataset contains synthetic patterns that are easy for nonlinear models to capture.

### 4. Feature Engineering and Leakage Analysis
Several feature groups were tested:
- transaction amount features
- time-based features
- category features
- customer age features
- novelty features
- geographic features

Velocity features were explored but removed from the final version because some aggregation strategies may introduce temporal leakage.

## Key Findings
- Fraud is extremely rare, so PR-AUC is more informative than accuracy.
- Fraud risk has clear temporal patterns, especially during late night and early morning hours.
- Fraud rate differs across transaction categories.
- Transaction amount, category, and time-related features carry strong predictive signal.
- CatBoost achieves near-perfect ROC-AUC, which is a warning sign rather than just a success.
- The dataset likely contains synthetic decision rules that make fraud easier to detect than in real-world production systems.

## Final Takeaway
This project shows that strong model metrics do not automatically mean a model is production-ready.
The main lesson is that fraud detection requires careful validation, leakage checks, and business interpretation.  
In this dataset, CatBoost performs extremely well, but the results should be interpreted cautiously because the data likely contains synthetic artifacts.

## Tech Stack
- Python
- pandas
- numpy
- scikit-learn
- CatBoost
- matplotlib
- seaborn

## Repository Structure

```text

credit-card-fraud-leakage-analysis/
├── README.md
├── notebooks/
│   └── credit-card-fraud-leakage-analysis.ipynb
├── images/
├── requirements.txt
├── .gitignore
└── LICENSE

