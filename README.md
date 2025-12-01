# Data-Prepper
Data-Prepper, agentic AI agent built for cleaning and transforming data to be model-ready. Built as capstone for 2025 Google Kaggle ADK course.

# Problem:
As an AI enthusiast, I understand the importance of the quality of an AI model. Collected datasets before cleaning are often inconsistent, missing data, biased, and full of outliers or filler data. Data scientists often spend weeks or even months on complex datasets before they attempt to feed it into a model.

# Solution:
As a result, I created this general data processor in an attempt to shorten the arduous development cycle. This is an 11-agent pipeline managed by a mastermind agent. It starts by generating a pseudo-random dataset, transform it into a dataframe, then feed it into the pipeline. The "Mastermind" agent then orchestrates the pipeline: fixing inconsistent data, extracting features, grouping rare categories, filling in or removing missing data, removing duplicate data and features (PCA), skewing and encoding data to be training ready, and splitting it into the test and train set while removing biases. This whole process is designed to work on a general use case, but can be easily customized to suit various datasets and model specifications. 

# Architecture:
Since ths is meant to be a general dataset, I avoided just loading in a static dataset, then convert it to a dataframe for processing. Instead, I decided to use a pseudo-randomized dataset generator. This truly showcases the robustness of the agent pipeline, and how it would perform on real, raw datasets.
The agents are as follows:
Standardize: Fix types based on appearance percentage and similarity, remove currency symbols, unify text casing.
Date: Extract features (Year/Month) via splittiing from timestamps, necessary for later steps.
Duplicates: Remove exact data and row matches, only if <1% for safety.
Grouper: Group rare categories into "Other" if <1% total and <0.5% individually, reduces dimensionality.
Nulls: Drop bad cols/rows or Impute (Median/Mode), total thresholding to prevent cascading data loss.
Correlation: Drop redundant features (Correlation > 95%), performs PCA with correlation matrix.
Skew: Log-transform positive numeric outliers, log(x+1), and avoids key features.
Encoding: Convert categories to numbers (One-Hot vs Label) based on cardinality and ordinality, avoids key features.
Scaler: Split Train/Test and Standardize (Prevent Leakage) using z-score standardization, then transforms test-set using precalculated values.
Bias: Fixes Statistical Bias and Class Imbalance on Train set, avoids overfitting by introducing jittering/noise injection, avoids key feature for classification tasks.
Mastermind: Orchestrates the code of these 10 agents to work in sequential order on a given dataset.

I have also included 2 comparison examples at the end, which serves as both a debugging tool and a metric system for us to see how well the piepeline did in transforming the data.

Note: In cases where bias is part of the data and prediction for a model, we can simply just remove the agent tool. The output is 2 files: test_df.pkl and train_df.pkl.

# Instructions for setup:
Rememeber to set up secrets in kaggle notebook, after that just do run all and it should work! If you have a specific dataset to use, transform it into a pickle file via: df = filepath and df.to_pickle('/kaggle/working/df.pkl').
Lastly, you can try out different models, simply replace all gemini-2.5-flash with gemini-2.0-flash (or -lite). Based on what I observed, there isn't too much of a noticible differene, though the 2.5 version seems to follow instructions on safety better.

# Result:
The model transformed the imbalanced, messy dataset of ~1005 rows into a robust training set of ~1650 samples. It fixed common data inconsistencies, extracted features, removed unnecessary information, compressed skewed distributions, and encoded the data to be model-ready.
