# Dataset-Cleaner
Dataset-Cleaner, agentic AI agent built for 2025 Google Kaggle adk course
# Problem:
As an AI enthusiast, I understand the importance of the quality of an AI model. Collected datasets before cleaning are often inconsistent, missing data, biased, and full of outliers or filler data. Data scientists often spend weeks or even months on complex datasets before they attempt to feed it into a model.
As a result, I created this general data processor in an attempt to shorten the arduous development cycle.
# Solution:
This is a 10-

a
# Architecture:
a
# Instructions for setup:
a

Standardize: Fix types, remove currency symbols, unify text casing.
Date: Extract features (Year/Month) from timestamps.
Duplicates: Remove exact row matches (Safety: <1%).
Grouper: Group rare categories into "Other" (Safety: <1%).
Nulls: Drop bad cols/rows or Impute (Median/Mode).
Correlation: Drop redundant features (Correlation > 95%).
Skew: Log-transform positive outliers.
Encoding: Convert categories to numbers (One-Hot vs Label).
Scaler: Split Train/Test and Standardize (Prevent Leakage).
Bias: Fixes Statistical Bias and Class Imbalance on Train set.
Mastermind: orchestrates the code of these 10 agents to work in sequential order on a given dataset.
Note: In cases where bias is part of the data and prediction for a model, we can simply just remove the agent tool. The output is 2 files: test_df.pkl and train_df.pkl.
Result:
The model transformed the imbalanced, messy dataset of ~1005 rows into a robust training set of ~1650 samples. It fixed common data inconsistencies, extracted features, removed unnecessary information, compressed skewed distributions, and encoded the data to be model-ready.
