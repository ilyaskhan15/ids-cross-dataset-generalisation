# ids-cross-dataset-generalisation

## Research question
Do machine-learning intrusion detection models trained on one network
traffic dataset still detect attacks when tested on a different dataset?

## One-page idea
Most IDS papers train and test on the same dataset, which gives very
high accuracy (often 99%+). In practice, a deployed IDS sees traffic
from a different network, with different tools and attack styles.

This project:
1. Trains standard classifiers (Random Forest, Logistic Regression,
   XGBoost/Gradient Boosting) on one dataset (e.g. CIC-IDS2017).
2. Tests them on a different dataset (e.g. UNSW-NB15) using shared features.
3. Compares within-dataset vs cross-dataset performance
   (F1, precision, recall, per-attack-type recall).
4. Analyses why performance drops (feature distribution shift,
   label mismatch, class imbalance) and tests simple fixes
   (feature normalisation, feature selection, domain-robust features).

## Expected contribution
A clear, reproducible measurement of the generalisation gap and
which preprocessing choices reduce it.

## Datasets
- CIC-IDS2017
- UNSW-NB15
- (optional) CSE-CIC-IDS2018

## Tools
Python, pandas, scikit-learn, matplotlib, Jupyter

## Status
Day 1: project setup. Work in progress.

## Structure
data/ (not committed), notebooks/, src/, results/
