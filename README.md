# Survey-Weighted Clustering Ensembles for Population-Representative Metabolic Phenotyping

This repository contains the code, analysis scripts, and reproducibility materials for the study:

**“Survey-Weighted Clustering Ensembles for Population-Representative Metabolic Phenotyping.”**

The study develops a survey-weighted clustering ensemble framework for identifying stable metabolic phenotypes from complex health survey data while accounting for unequal population representation. The framework is evaluated using NHANES 2015–2018 data and controlled simulation experiments.

## Abstract

Metabolic health is heterogeneous, yet data-driven phenotyping often relies on single clustering algorithms or samples that may not adequately represent the target population. We developed a survey-weighted clustering ensemble framework and applied it to 4,326 fasting-eligible adults from the National Health and Nutrition Examination Survey (NHANES) 2015–2018.

Five clustering algorithms were integrated through a survey-weighted co-association graph and spectral consensus clustering. The primary analysis identified five stable phenotypes: **Metabolically Favorable (25.5%)**, **Older High-HDL (27.0%)**, **Dyslipidemic (28.0%)**, **Severe Adiposity (17.0%)**, and **Severe Hyperglycemia (2.5%)**. The consensus solution was stable under bootstrap resampling (mean ARI = 0.941) and robust to omission of individual base clustering methods (ARI = 0.916–0.962).

Controlled simulations separately evaluated cluster-assignment recovery and target-population prevalence recovery. Under strong cluster separation and severe sampling imbalance, prevalence MAE decreased from 0.2507 with unweighted consensus to 0.0116 with survey-weighted consensus, without a corresponding improvement in cluster-assignment accuracy. This indicates that the primary benefit of survey weighting under unequal sampling was improved recovery of population phenotype prevalence rather than uniformly improved geometric cluster recovery.

In survey-design regression adjusted for age and sex, the phenotypes showed distinct cross-sectional associations with physician-diagnosed diabetes (F(4,24) = 89.35, p < 0.001) and cardiovascular disease (F(4,24) = 6.86, p < 0.001). Because glycemic measures contributed to phenotype derivation, diabetes was interpreted as supportive clinical characterization, whereas cardiovascular disease provided a more independent outcome assessment.

Overall, the framework combines heterogeneous clustering with survey weighting to support stable metabolic phenotype discovery and population prevalence estimation in complex survey data.

## Study Overview

The study addresses two related but distinct questions:

1. Can a heterogeneous clustering ensemble identify stable and interpretable metabolic phenotypes?
2. Does incorporating survey weights improve representation of these phenotypes at the population level?

The analysis consists of four main components:

- survey-weighted consensus phenotype derivation;
- robustness and sensitivity analyses;
- controlled simulation experiments with known ground truth; and
- survey-design analysis of phenotype associations with diabetes and cardiovascular disease.

## Data

The analysis uses the **2015–2016 and 2017–2018 NHANES cycles**.

The primary analytic cohort consists of **4,326 fasting-eligible adults aged 18 years or older** with complete data for the phenotype variables.

The primary clustering analysis uses 10 metabolic and demographic measures:

- Age
- Body mass index (BMI)
- Waist circumference
- Systolic blood pressure
- Diastolic blood pressure
- HbA1c
- Fasting glucose
- Triglycerides (log-transformed)
- HDL cholesterol
- LDL cholesterol

Sex is not included in the primary clustering feature matrix. It is retained for post-hoc phenotype characterization and adjustment in the clinical outcome analyses.

NHANES fasting subsample weights, strata, and primary sampling units (PSUs) are used where appropriate to account for the complex survey design.

## Methodology

### Base Clustering Ensemble

The ensemble contains five clustering methods:

- K-means
- Survey-weighted K-means
- Gaussian Mixture Models (GMM)
- Spectral clustering
- Ward agglomerative clustering

Candidate cluster numbers range from **K = 3 to 6**, with multiple initializations, producing **40 base partitions**.

### Survey-Weighted Consensus

Base partitions are combined using a survey-weighted co-association representation. Survey weights are incorporated into pairwise affinity construction, followed by graph sparsification and normalized spectral clustering.

The final number of phenotypes is selected using the eigengap criterion.

The primary analysis identifies **K = 5**.

### Validation and Sensitivity Analysis

Robustness is evaluated using:

- Adjusted Rand Index (ARI)
- Normalized Mutual Information (NMI)
- bootstrap resampling
- initialization stability
- leave-one-method-out ablation
- weighted versus unweighted consensus comparison
- comparison with individual clustering algorithms
- sensitivity to inclusion of sex in phenotype derivation

Leave-one-method-out analysis evaluates whether the consensus solution is disproportionately driven by any individual base clustering method.

### Simulation Validation

Controlled simulations evaluate performance when the true cluster assignments and target-population prevalences are known.

The simulation design combines:

- two levels of cluster separation: **moderate** and **strong**;
- three levels of sampling imbalance: **none**, **moderate**, and **severe**;
- four clustering strategies; and
- 50 independent replications per condition.

The four methods are:

1. K-means
2. Survey-weighted K-means
3. Unweighted consensus
4. Survey-weighted consensus

This produces **1,200 method-level simulation results**.

Cluster-assignment recovery is evaluated using **ARI and NMI**, while target-population prevalence recovery is evaluated using **mean absolute error (MAE) and root mean squared error (RMSE)**.

The simulations are designed to distinguish improvements in cluster recovery from improvements in population prevalence estimation.

### Survey-Design Outcome Analysis

Cross-sectional associations between the derived phenotypes and physician-diagnosed diabetes and cardiovascular disease (CVD) are evaluated using survey-design logistic regression.

Models account for:

- NHANES survey weights
- strata
- primary sampling units (PSUs)
- age
- sex

Because HbA1c and fasting glucose contribute to phenotype derivation, diabetes is treated as **supportive clinical characterization rather than independent validation**. CVD provides a more independent outcome assessment because CVD status is not used in phenotype derivation.

## Key Findings

The primary analysis identified five population-weighted metabolic phenotypes:

| Phenotype | Weighted Prevalence |
|---|---:|
| Metabolically Favorable | 25.5% |
| Older High-HDL | 27.0% |
| Dyslipidemic | 28.0% |
| Severe Adiposity | 17.0% |
| Severe Hyperglycemia | 2.5% |

The consensus solution showed:

- **Bootstrap stability:** mean ARI = 0.941
- **Leave-one-method-out robustness:** ARI = 0.916–0.962
- **Weighted vs. unweighted consensus agreement:** ARI = 0.839, NMI = 0.832

The weighted and unweighted NHANES solutions retained broadly similar metabolic profiles, with relatively modest differences in estimated phenotype prevalence. Controlled simulations showed that the benefit of survey weighting became substantially larger when unequal sampling distorted the observed population composition.

These findings support a distinction between **cluster-assignment recovery** and **population-prevalence recovery**: survey weighting does not necessarily improve geometric clustering accuracy, but it can substantially reduce prevalence bias under unequal sampling.

## Repository Structure

```text
├── data/                  # Processed data and data-preparation documentation
├── scripts/               # Core clustering and analysis scripts
├── notebooks/             # Jupyter notebooks for analyses and validation
├── simulation/            # Simulation design and validation
├── figures/               # Manuscript and supplementary figures
├── results/               # Clustering, robustness, and regression outputs
├── R/                     # Survey-design regression analyses
├── requirements.txt       # Python dependencies
└── README.md              # Repository overview
