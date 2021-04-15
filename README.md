# Passage-of-Time Dysphoria

This repository contains the scripts necessary to preprocess, analyze, print and plot the results reported in the following paper:

> Jangraw, Keren, Bedder, Rutledge, Pereira et al. (2021). "Passage-of-Time Dysphoria: A Highly Replicable, Gradual Decline in Subjective Mood Observed During Rest and Simple Tasks." BioRxiv.

## Datasets

Data from the online participants described in the paper can be found in the following repository on the Open Science Framework: https://osf.io/km69z/.

Data from the mobile app participants described in the paper are a subset of the dataset shared by Robb Rutledge's laboratory, now available at:

> Rutledge, Robb B. (2021), Risky decision and happiness task: The Great Brain Experiment smartphone app, Dryad, Dataset, https://doi.org/10.5061/dryad.prr4xgxkk

However, due to differences between the data as we used it and the data as shared, we are not currently able to share code that will reproduce our results. We are working to resolve these issues and will update the code here soon.

## Dependencies

Code for preprocessing, fitting mixed effects models, and analysis and plotting of results will work from a conda environment created with this command:
```
conda create -p potdys3 -c ejolly -c conda-forge python=3.8.8 numpy=1.19.2 pandas=1.1.5 pytest=6.2.2 joblib=1.0.1 rpy2=3.4.3 matplotlib=3.3.4 seaborn=0.11.1 scikit-learn=0.24.1 numexpr=2.7.3 patsy=0.5.1 statsmodels=0.12.2 openpyxl=3.0.7 pymer4=0.7.3 r-psych=2.1.3 xlrd=2.0.1
conda activate ./potdys3
pip install -e .
```

If you need an environment for running the pytorch models, please open an issue and we'll work on adding this.

## Usage
Data can be downloaded from the above locations to the Data folder of this repository. The scripts described below should then be run from the scripts folder. Processed data files will be saved to the Data/OutFiles folder. Figures will be saved to the Figures folder.
We have broken down processing into 3 processing stages, outlined below.

### Preprocessing
Converts raw data into preprocessed files that combine across cohorts (or "batches") of participants. Run the following:
- [ImportMmiData_May2020.py](scripts/ImportMmiData_May2020.py) to import online adult cohorts
- [ImportNimhMmiData.py](scripts/ImportNimhMmiData.py) to import online adolescent cohorts
- [AssembleMmiBatches.py](scripts/AssembleMmiBatches.py) to combine across online cohorts for grouped analyses
- [LoadRutledgeGbeData.py](scripts/LoadRutledgeGbeData.py) to import mobile app cohorts *Not currently working with public GBE data as shared.*
- [MakePytorchInputTable.py](scripts/MakePytorchInputTable.py) to format data for computational model fitting.

### Model Fitting
Fits the large-scale linear mixed effects (LME) model and the computational model described in the paper. The computational model scripts are computationally expensive and best run on a high-performance computing cluster. Run the following:
- [RunPymerOnCovidData_Aug2020.py](scripts/RunPymerOnCovidData_Aug2020.py) to run the LME model. Must be run from a python environment with pymer4 installed.
- [Tune_GBE_pytorch.py](scripts/Tune_GBE_pytorch.py) to tune the computational model's hyperparameters. Must have pytorch installed. *Not currently working with public GBE data as shared.*
- [Tune_GBE_pytorch_NoBetaT.py](scripts/Tune_GBE_pytorch_NoBetaT.py) to do the same without the time-responsive beta_T term. *Not currently working with public GBE data as shared.*
- [Tune_GBE_pytorch.py](scripts/Tune_GBE_pytorch.py) to run the model with given hyperparameters. Must have pytorch installed. *Not currently working with public GBE data as shared.*
- [Run_GBE_pytorch_NoBetaT.py](scripts/Tune_GBE_pytorch_NoBetaT.py) to do the same without the time-responsive beta_T term. *Not currently working with public GBE data as shared.*
- [CombineGbeConfirmResults.py](scripts/CombineGbeConfirmResults.py) to combine across computational model results for the confirmatory mobile app sub-cohorts, which were split apart to avoid memory errors.

### Analysis
Produces all the figures, tables, and printed results reporeted in the paper. Run the following:
- [ProduceAllResults.py](scripts/ProduceAllResults.py). All other scripts and functions in this folder will be called by this wrapper script.

### Data Naming Conventions

Cohorts were renamed after analysis to make them more intuitive in the paper. Here is a translation:

| Name in paper | Folder(s) in database |
| ------------- | ------------------ |
| 15sRestBetween | Recovery1, RecoveryInstructed1 |
| 30sRestBetween | RecoveryInstructed1Freq0p5 |
| 7.5sRestBetween | RecoveryInstructed1Freq2 |
| 60sRestBetween | RecoveryInstructed1Freq0p25 |
| AlternateRating | Numbers |
| Expectation-7mRest | Expectation-7min |
| Expectation-12mRest | Expectation-12min |
| RestDownUp | RestDownUp |
| Daily-Rest-01 | Stability01-Rest |
| Daily-Rest-02 | Stability02-Rest |
| Weekly-Rest-01 | COVID01 |
| Weekly-Rest-02 | COVID02 |
| Weekly-Rest-03 | COVID03 |
| Adolescent-01 | RecoveryNimh |
| Adolescent-02 | RecoveryNimh |
| Visuomotor | Motion |
| Visuomotor-Feedback | MotionFeedback |
| RestAfterWins | Return1 |
| Daily-Closed-01 | Stability01-closed |
| Daily-Closed-02 | Stability02-closed |
| App-Exploratory | *not shared* |
| App-Confirmatory | *not shared* |
