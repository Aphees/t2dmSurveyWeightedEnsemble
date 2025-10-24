# t2dmSurveyWeightedEnsemble
Survey-Weighted Ensemble Clustering for Population-Level Diabetes Phenotyping This repository hosts the code, scripts, and supplementary materials for the conference paper "Survey-Weighted Ensemble Clustering Reveals Population-Level Diabetes Subgroups."  

# Abstract
Type 2 diabetes mellitus (T2DM) differs widely among individuals, creating challenges for accurate patient
classification and personalized treatment. Traditional clustering methods for T2DM phenotyping often
depend on single algorithms or clinic-based datasets, which limit reproducibility and generalizability.
Moreover, analyses of large-scale survey data such as US National Health and Nutrition Examination Survey
(NHANES) frequently neglect survey sampling design, introducing bias into phenotype discovery. We present
a Population-Weighted Partitioning Framework to address these limitations, integrating survey sampling
weights into the clustering process through weighted co-association matrices. Applied to NHANES data, the
framework uncovers robust, population-representative T2DM phenotypes, validated across multiple imputed
datasets, survey cycles, and regression-based outcome analyses. Comparative experiments demonstrate that
population-weighted partitioning enhances cluster stability and improves alignment with actual population
characteristics. By combining unsupervised learning with survey-aware design, this framework strengthens
methodological rigor in health data mining. It delivers a scalable, reproducible approach for precision
diabetes epidemiology in complex survey settings.
## Repository Structure

├── data/                  # Example or synthetic NHANES-like data
├── scripts/               # Core scripts for weighted ensemble clustering
├── notebooks/             # Jupyter notebooks for analysis and figures
├── figures/               # Generated figures (eigengap, forest plots, etc.)
├── results/               # Output summaries and regression tables
├── requirements.txt       # Python dependencies
└── README.md              # Project overview (this file)

## Installation

Clone the repository and install dependencies:

git clone https://github.com/afeesodebode/survey-weighted-ensemble-clustering.git
cd survey-weighted-ensemble-clustering
pip install -r requirements.txt

## Quick Start

Run the full pipeline:

python scripts/run_weighted_ensemble.py
Or explore the interactive workflow:
jupyter notebook notebooks/SurveyWeightedClustering.ipynb


## The pipeline:

- Generates base clusterings (k-means, GMM, spectral, hierarchical)

- Builds a survey-weighted co-association matrix

- Performs consensus spectral clustering

- Determines cluster number via eigengap heuristic

- Evaluates cluster associations with survey-weighted regression

## Key Findings

Two stable metabolic clusters identified:

- Cluster 0: Favorable metabolic measures (lower HbA1c, glucose, BP)

- Cluster 1: Elevated HbA1c, fasting glucose, and blood pressure

- Weighted prevalence: 51.1% vs. 48.9%

- Cluster 1 associated with higher odds of hypertension and diabetes

Findings remained significant after Bonferroni correction

## Methodological Highlights

- Survey-weighted co-association matrix: Integrates design weights into affinity computation

- Spectral consensus clustering: Uses eigengap to determine optimal cluster number

- Validation: Survey-weighted logistic regression confirms robustness

- Sensitivity analysis: Varying neighbors (25/50/100) and ensemble size (20/40/80) yielded ARI ≥ 0.86

## Reproducibility & Data Availability

Due to NHANES data use restrictions, raw data are not redistributed here.
- To ensure reproducibility:
Standard errors were estimated via Taylor series linearization using strata and PSU design variables.

All analyses used survey-weighted procedures in Python.
