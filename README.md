# Credit Risk Modeling & Policy Optimization
**LendingClub Loan Default Prediction**

## Overview
This project builds an end-to-end credit risk modeling pipeline using historical LendingClub loan data.

The objective is not only to predict loan default risk, but to translate model outputs into actionable credit policies using cost-sensitive decisioning and Expected Loss (EL) analysis.

Unlike typical classification-focused projects, this work emphasizes:
- Leakage-aware modeling
- Business-aligned decision thresholds
- Probability calibration
- Expected Loss–driven approval policies

## Problem Statement
Given borrower and loan characteristics available at loan origination, predict whether a loan will default and design a decision framework that balances:
- Loan approval volume
- Credit losses from defaulted loans

## Repository Structure
```
credit-risk-modeling/
├── notebooks/credit_risk_model.ipynb   # full pipeline: cleaning -> models -> thresholds -> EL policy -> SHAP
├── data/README.md                      # how to download the dataset (data is not committed)
├── requirements.txt
├── LICENSE
└── README.md
```

## How to Run
1. Clone the repo and install dependencies: `pip install -r requirements.txt`
2. Download the dataset from Kaggle (see `data/README.md`) and place `loan.csv` in `data/`.
3. Open `notebooks/credit_risk_model.ipynb` and run all cells.

> The notebook was developed on Google Colab, so data-loading cells use Colab paths and Kaggle secrets. Adjust the file path in the loading cell when running locally.

## Dataset
- **Source:** LendingClub (Kaggle: https://www.kaggle.com/datasets/gabrieloliveirasan/lending-club-credit-risk)
- **Observations:** ~815k loans after filtering to the labelled statuses (~163k in the 20% stratified test set)
- **Class balance:** ~6.4% default / ~93.6% non-default
- **Target Variable:** `loan_status`
  - `0` → Non-default (Fully Paid)
  - `1` → Default (Charged Off / Default)

> Note: Only features known before loan issuance were retained.

## Key Methodological Choices

### Target Construction
Loan status categories were mapped into a binary credit risk label to reflect real-world default outcomes.

### Leakage Prevention
All post-origination variables were removed, including:
- Payment dates
- Recoveries
- Collections activity

This ensures:
- Honest model evaluation
- Realistic deployability

## Data Preprocessing
- Median imputation (numerical features)
- Most-frequent imputation (categorical features)
- One-Hot Encoding for categorical variables
- Feature standardization for numerical variables
- Memory-safe sparse preprocessing

## Models Trained

| Model | Purpose |
|------|--------|
| Logistic Regression | Interpretable baseline |
| XGBoost | Non-linear high-performance model |

## Validation Strategy
- 80/20 stratified train/test split
- Test ROC-AUC: XGBoost ~0.78, Logistic Regression ~0.75

## Cost-Sensitive Decisioning
Decision thresholds were optimized using a cost-based framework:
- False Negatives assigned higher cost than False Positives
- Threshold chosen to minimize expected misclassification cost

This aligns model behavior with real-world credit risk priorities.

## Probability Calibration
XGBoost probabilities were calibrated using Isotonic Regression to produce reliable Probability of Default (PD) estimates.

Benefits:
- Interpretable PD values
- Meaningful downstream financial calculations

## Expected Loss Framework
Expected Loss is computed as:

Expected Loss = PD × LGD × EAD

Assumptions:
- LGD approximated for unsecured personal loans
- EAD proxied by loan amount

This enables policy-level decision-making rather than pure classification.

## Credit Policy Analysis
Approval policies were evaluated using Expected Loss to study trade-offs between:
- Approval rate
- Bad loan approval rate

A policy frontier enables selection of conservative or aggressive credit strategies.

## Results Summary
- Cost-optimized approval rate: ~72%
- Significant reduction in high-cost default approvals
- Stable policy behavior across thresholds
- No evidence of leakage-driven performance inflation

## Key Takeaways
- Credit risk modeling is not just about AUC
- Business costs must drive decision thresholds
- Probability calibration is critical
- Expected Loss bridges ML and real credit policy

## Limitations & Future Work
- Simplified LGD assumptions
- No macroeconomic stress testing
- No time-based (vintage) validation

Potential extensions:
- Explicit LGD modeling
- Stress testing under adverse scenarios
- Time-aware validation

## Tools & Libraries
- Python
- Pandas, NumPy
- Scikit-learn
- XGBoost
- Matplotlib

## Final Note
This project emphasizes realistic, deployable credit risk modeling with strong business alignment rather than leaderboard optimization.
