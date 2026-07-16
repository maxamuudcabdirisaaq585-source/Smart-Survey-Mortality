# Predicting Household Mortality from Survey Data

## Group Members
1.Fuad Abdikarim Moalim Abukar
2.Abdishakur Adam Mohamud
3.Mohamed Muse Farah
4.Mohamud Abdirizak Abdullahi
5.Osama Mohamed Ahmed
6.Mohamed Abdirahman Mohamud
7.Mohamed Ali Ahmed
8.Salim Mohamed Hussain


## Project Overview
This project analyzes household mortality survey data from Somalia (2018–2023) to identify individuals at higher risk of death and to build a supervised machine learning system capable of flagging at-risk cases from routine survey data.

The work is organized into three parts:

**Part 1 — Data Preparation**
Two raw survey extracts — a household-level mortality dataset and a person-level (individual) dataset — were merged on a shared household identifier (`household_uid`). This produced:
- `individual_level_merged.csv` — one row per surveyed person, enriched with household-level context (household size, births, deaths, joins/departures).
- `household_aggregate.csv` — the household-level mortality summary, kept as a standalone aggregate view.

**Part 2 — Machine Learning**
Three classification algorithms were trained to predict an individual's death outcome (`died_flag`) from demographic and household features (age, sex, household size, district, survey month/year/round, household births/joins):
- **Logistic Regression** — an interpretable statistical baseline.
- **Random Forest** — a bagging ensemble that captures non-linear feature interactions.
- **Gradient Boosting** (scikit-learn `HistGradientBoostingClassifier`, used as a substitute for XGBoost, which could not be installed in this offline environment) — a boosting ensemble that typically performs strongly on structured/tabular data.

Support Vector Machine, Naive Bayes, and K-Nearest Neighbours were considered but excluded, as they scale poorly to a dataset of this size (175,302 records) or make assumptions (feature independence) that don't hold for correlated demographic data.

**Part 3 — Evaluation**
Each model was evaluated on a held-out test set using Accuracy, Recall, Precision, F1-Score, AUC, and the Confusion Matrix. Because deaths make up only 0.36% of records, particular attention is given to **Recall** as the most operationally important metric — missing an at-risk individual (a false negative) is far more costly in a public-health context than a false alarm.

## Key Finding
Gradient Boosting achieved the best overall balance of Recall and AUC, making it the most suitable candidate model for an operational early-warning tool, with Logistic Regression serving as an interpretable secondary check.

## Repository Contents
| File | Description |
|---|---|
| `individual_level_merged.csv` | Merged person-level dataset (model input) |
| `household_aggregate.csv` | Household-level aggregate mortality data |
| `mortality_ml_report.docx` | Full written report: methodology, model comparison, evaluation metrics, and figures |
| `README.md` | This file |

## Methodology Notes
- Age values above 110 were treated as data-entry errors and set to missing.
- Fields only populated for deceased individuals (cause/location of death) were excluded from the model features to prevent data leakage.
- Data was split 80/20 (train/test), stratified on the outcome to preserve the 0.36% death rate in both sets.
- All models were trained with balanced class weighting to address the severe class imbalance.
