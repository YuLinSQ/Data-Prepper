# Data-Prepper
Data-Prepper is an agentic AI system built to clean and transform data into a model-ready state. It was built as a capstone project for the 2025 Google Kaggle ADK course.  

# Problem:
As an AI enthusiast, I understand that the quality of an AI model depends heavily on its input data. Raw datasets are often inconsistent, contain missing data, exhibit bias, and are full of outliers or filler information. Data scientists often spend weeks or even months on complex datasets before they can attempt to feed them into a model.  

# Solution:
I created this general data processor to shorten this arduous development cycle. This is an 11-agent pipeline managed by a "Mastermind" agent. The process starts by generating a pseudo-random dataset, transforming it into a dataframe, and then feeding it into the pipeline. The Mastermind agent orchestrates the workflow: fixing inconsistent data, extracting features, grouping rare categories, handling missing data, removing duplicate rows, and eliminating redundant features (Correlation Analysis). It then skews and encodes data, splits it into test and train sets, and removes biases. While designed for general use cases, this process can be easily customized to suit various datasets and model specifications.  

# Architecture:
To demonstrate true generalization, I avoided loading a static dataset. Instead, I used a pseudo-randomized dataset generator. This showcases the robustness of the agent pipeline and simulates performance on real, raw data.  
The agents are as follows:  
Standardize: Fixes types based on appearance percentage and similarity; removes currency symbols and unifies text casing.  
Date: Extracts features (Year/Month) via splitting from timestamps (necessary for later steps).  
Duplicates: Removes exact row matches (only if <1% of data, for safety).  
Grouper: Groups rare categories into "Other" (if <1% total and <0.5% individually) to reduce dimensionality.  
Nulls: Drops bad columns/rows or Imputes (Median/Mode) using total thresholding to prevent cascading data loss.  
Correlation: Drops redundant features (Correlation > 95%) using a correlation matrix.  
Skew: Log-transforms positive numeric outliers (log(x+1)) while excluding key features/IDs.  
Encoding: Convert categories to numbers (One-Hot vs Label) based on cardinality and ordinality, avoids key features.  
Scaler: Splits Train/Test and Standardizes (Preventing Leakage) using Z-score standardization, then transforms the test set using precalculated values.  
Bias: Fixes Statistical Bias and Class Imbalance on the Train set; avoids overfitting by introducing jittering/noise injection and protects target variables.  
Mastermind: Orchestrates the code of these 10 agents to work in sequential order on a given dataset.  

I have also included 2 comparison examples at the end, which serves as both a debugging tool and a metric system to measure the pipeline's performance.  

Note: In cases where bias is part of the data and prediction for a model, we can simply just remove the bias agent tool. The output is 2 files: test_df.pkl and train_df.pkl.  

# Instructions for setup:
Remember to set up secrets in kaggle notebook, after that just do "Run All" and it should work! If you have a specific dataset to use, load it into a dataframe and save it as a pickle file: df.to_pickle('/kaggle/working/df.pkl').
Lastly, you can try out different models by replacing gemini-2.5-flash with gemini-2.0-flash (or -lite). Based on what I observed, there isn't too much of a noticeable difference, though the 2.5 version seems to follow instructions on safety better.

# Result:
The pipeline transformed an imbalanced, messy dataset of ~1005 rows into a robust training set of ~1650 samples. It fixed common data inconsistencies, extracted features, removed unnecessary information, compressed skewed distributions, and encoded the data to be model-ready.
