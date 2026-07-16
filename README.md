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
This project analyzes household mortality survey data from Somalia, collected between 2018 and 2023, with the goal of identifying individuals at higher risk of death and building a machine learning system that can flag at-risk cases from routine survey data. The work began with a data preparation stage in which two raw survey extracts, a household-level mortality dataset and a person-level dataset, were merged using a shared household identifier. This produced a merged individual-level file, where each row represents a surveyed person enriched with household context such as household size, births, deaths, and departures, alongside a separate household-level aggregate file retained as a standalone summary view.
Building on this merged dataset, three classification algorithms were trained to predict whether an individual died, using demographic and household features including age, sex, household size, district, survey timing, and household-level births and departures. Logistic Regression was used as an interpretable statistical baseline, Random Forest as a bagging ensemble capable of capturing non-linear feature interactions, and Gradient Boosting, implemented through scikit-learn's HistGradientBoostingClassifier as a substitute for XGBoost since that library could not be installed in this offline environment, as a boosting ensemble expected to perform strongly on structured, tabular data of this kind. Support Vector Machine, Naive Bayes, and K-Nearest Neighbours were considered but ultimately excluded, since they either scale poorly to a dataset of this size, at over 175,000 records, or rely on independence assumptions that don't hold well for correlated demographic variables.
Each of the three trained models was then evaluated on a held-out test set using Accuracy, Recall, Precision, F1-Score, AUC, and the Confusion Matrix. Because recorded deaths make up only about 0.36 percent of the dataset, particular weight was given to Recall as the most operationally meaningful metric, since failing to identify a genuinely at-risk individual carries a much higher real-world cost in a public-health context than a false alarm does. Across the three models, Gradient Boosting produced the strongest overall balance of recall and discriminative power as measured by AUC, making it the most promising candidate for an operational early-warning tool, while Logistic Regression remains valuable as a simpler, more interpretable secondary check on its predictions.s were trained with balanced class weighting to address the severe class imbalance.
