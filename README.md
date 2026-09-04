# Survey-Weighted Clustering Ensembles for Population-Representative Metabolic Phenotyping

## Overview

This repository contains the analysis code and reproducibility materials for the manuscript:

**Survey-Weighted Clustering Ensembles for Population-Representative Metabolic Phenotyping**

**Authors:** Afees Odebode, Stephen Swift, and Mahir Arzoky

This study develops a survey-weighted clustering ensemble framework for population-representative metabolic phenotyping in complex health survey data. The framework combines multiple clustering algorithms through a survey-weighted co-association graph and spectral consensus clustering.

The method was evaluated using data from the **National Health and Nutrition Examination Survey (NHANES) 2015–2018 fasting subsample**. The final complete-case analytic cohort contained **4,326 adults**.

Five complementary clustering approaches were incorporated into the ensemble:

- Standard k-means
- Survey-weighted k-means
- Gaussian mixture modeling
- Spectral clustering
- Ward agglomerative clustering

The study evaluates phenotype stability, agreement with alternative clustering strategies, leave-one-method-out robustness, sensitivity to inclusion of sex in phenotype derivation, population-prevalence recovery under controlled simulations, and cross-sectional associations with physician-diagnosed diabetes and cardiovascular disease.

A central finding of the study is that **clustering stability and population representativeness are distinct objectives**. Survey weighting did not uniformly improve individual-level geometric cluster recovery, but substantially improved recovery of target-population phenotype prevalence when sampling was unequal.

---

## Repository Contents

The main analyses are organized as seven Jupyter notebooks intended to be run approximately in numerical order.

### `01_primary_analysis.ipynb`

Performs the primary NHANES analysis, including:

- acquisition and preparation of NHANES 2015–2018 data;
- construction of the fasting-subsample analytic cohort;
- complete-case restriction;
- preprocessing and standardization;
- generation of base clustering partitions;
- survey-weighted co-association construction;
- spectral consensus clustering;
- selection of the primary phenotype solution;
- survey-weighted phenotype summaries; and
- export of the main analysis artifacts.

The primary phenotype derivation uses **10 metabolic and clinical variables**, with sex excluded from the clustering feature set and retained for phenotype characterization and downstream statistical adjustment.

### `02_sex_sensitivity.ipynb`

Evaluates the effect of including sex directly in phenotype derivation.

The notebook compares:

- the primary 10-variable sex-excluded specification; and
- an 11-variable specification including binary-coded sex.

It examines changes in the selected number of clusters, agreement between solutions, phenotype crosswalks, bootstrap stability, and the extent to which inclusion of sex subdivides otherwise similar metabolic profiles.

### `03_benchmark_ablation.ipynb`

Benchmarks the final survey-weighted consensus solution against alternative clustering strategies:

- standard k-means;
- survey-weighted k-means;
- Gaussian mixture modeling;
- spectral clustering;
- Ward agglomerative clustering; and
- unweighted consensus clustering.

The notebook evaluates agreement using Adjusted Rand Index (ARI) and Normalized Mutual Information (NMI), together with conventional geometric clustering criteria.

It also performs **leave-one-method-out (LOMO) ablation analysis** to determine the sensitivity of the final consensus solution to removal of individual component algorithms.

### `04_simulation_validation.ipynb`

Performs the controlled simulation study used to evaluate the methodological contribution of survey weighting under known ground truth.

The simulation varies:

- cluster separation: moderate and strong;
- sampling imbalance: none, moderate, and severe.

Four methods are compared:

- standard k-means;
- survey-weighted k-means;
- unweighted consensus clustering; and
- survey-weighted consensus clustering.

Cluster-assignment recovery is evaluated using **ARI and NMI**, while target-population phenotype-prevalence recovery is evaluated using **mean absolute error (MAE) and root mean squared error (RMSE)**.

Each of the six simulation conditions is repeated 50 times.

### `05_clinical_associations.ipynb`

Prepares the final phenotype assignments and clinical outcomes for survey-design analysis.

Clinical associations are evaluated for:

- physician-diagnosed diabetes; and
- cardiovascular disease (CVD).

The analysis incorporates NHANES:

- fasting-subsample weights;
- survey strata; and
- primary sampling units (PSUs).

Models are adjusted for age and sex.

Because HbA1c and fasting glucose contribute to phenotype derivation, physician-diagnosed diabetes is interpreted as **supportive clinical characterization rather than an independent validation outcome**. CVD provides a more independent outcome assessment because it was not used in phenotype derivation.

The design-based logistic regression component uses the R `survey` package.

### `06_phenotype_heatmap.ipynb`

Generates the survey-weighted phenotype heatmap used to visualize the metabolic characteristics of the final five phenotypes.

The heatmap summarizes phenotype-specific metabolic profiles relative to the overall analytic population.

### `07_clinical_forest_plot.ipynb`

Generates the manuscript forest plot showing adjusted associations between the five metabolic phenotypes and:

- physician-diagnosed diabetes; and
- cardiovascular disease.

The figure is based on estimates from the survey-design regression analysis.

---

## Repository Structure

```text
survey-weighted-metabolic-phenotyping/
│
├── README.md
├── LICENSE
├── CITATION.cff
├── requirements.txt
│
├── notebooks/
│   ├── 01_primary_analysis.ipynb
│   ├── 02_sex_sensitivity.ipynb
│   ├── 03_benchmark_ablation.ipynb
│   ├── 04_simulation_validation.ipynb
│   ├── 05_clinical_associations.ipynb
│   ├── 06_phenotype_heatmap.ipynb
│   └── 07_clinical_forest_plot.ipynb
│
├── data/
│   ├── README.md
│   ├── raw/
│   └── processed/
│
└── results/
```

Generated data and result files may be excluded from version control where appropriate and reproduced by running the notebooks.

---

## Data

### Data source

The empirical analysis uses publicly available data from the **U.S. National Health and Nutrition Examination Survey (NHANES)**.

Two survey cycles are pooled:

- NHANES 2015–2016
- NHANES 2017–2018

The analysis uses the NHANES fasting subsample and the corresponding fasting-subsample survey weights.

Raw NHANES files are not redistributed as part of this repository. They can be obtained from the publicly available NHANES data portal maintained by the U.S. Centers for Disease Control and Prevention.

### Analytic cohort

The study includes adults aged 18 years or older.

The fasting-eligible sample contained **4,766 participants**. After complete-case restriction across the variables required for phenotype derivation, the final analytic cohort contained:

**n = 4,326**

This represents approximately **90.8% of fasting-eligible participants**.

### Variables

The candidate metabolic and demographic variables include:

- Age
- Sex
- Body mass index (BMI)
- Waist circumference
- Systolic blood pressure
- Diastolic blood pressure
- HbA1c
- Fasting plasma glucose
- Triglycerides
- HDL cholesterol
- LDL cholesterol

The **primary clustering analysis excludes sex from phenotype derivation**, resulting in a 10-variable clustering feature set. Sex is retained for post-hoc phenotype characterization and adjustment in downstream survey-design regression.

Clinical outcome analyses additionally use questionnaire-derived information on:

- physician-diagnosed diabetes; and
- cardiovascular disease.

### Missing data

Residual missingness among fasting-eligible participants was low, with no phenotype variable having 10% or greater missingness. The highest variable-specific missingness was approximately **4.49% for waist circumference**.

The primary analysis therefore uses a complete-case cohort rather than median imputation.

---

## Installation

### Clone the repository

```bash
git clone https://github.com/YOUR-USERNAME/survey-weighted-metabolic-phenotyping.git
cd survey-weighted-metabolic-phenotyping
```

Replace `YOUR-USERNAME` with the GitHub account or organization hosting the repository.

### Option 1: Python virtual environment

Create a virtual environment:

```bash
python -m venv .venv
```

Activate it on macOS/Linux:

```bash
source .venv/bin/activate
```

or on Windows:

```bash
.venv\Scripts\activate
```

Install the required Python packages:

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

### Option 2: Conda

Create a dedicated environment:

```bash
conda create -n survey-phenotyping python=3.11
conda activate survey-phenotyping
pip install -r requirements.txt
```

Install Jupyter if it is not already available:

```bash
pip install jupyter
```

Then launch:

```bash
jupyter notebook
```

or:

```bash
jupyter lab
```

---

## Reproducing the Analysis

For a complete reproduction, run the notebooks in the following order.

### Step 1 — Primary phenotype analysis

```text
notebooks/01_primary_analysis.ipynb
```

Constructs the NHANES analytic cohort and reproduces the primary survey-weighted clustering ensemble.

### Step 2 — Sex-sensitivity analysis

```text
notebooks/02_sex_sensitivity.ipynb
```

Evaluates the effect of including sex directly in phenotype derivation.

### Step 3 — Benchmark and ablation analysis

```text
notebooks/03_benchmark_ablation.ipynb
```

Compares the final consensus with alternative clustering strategies and performs leave-one-method-out robustness analysis.

### Step 4 — Simulation validation

```text
notebooks/04_simulation_validation.ipynb
```

Reproduces the controlled simulation study under different levels of cluster separation and sampling imbalance.

### Step 5 — Clinical associations

```text
notebooks/05_clinical_associations.ipynb
```

Prepares the phenotype-outcome dataset and reproduces the survey-design clinical association analysis.

### Step 6 — Phenotype heatmap

```text
notebooks/06_phenotype_heatmap.ipynb
```

Generates the survey-weighted metabolic phenotype heatmap.

### Step 7 — Clinical forest plot

```text
notebooks/07_clinical_forest_plot.ipynb
```

Generates the forest plot for the adjusted diabetes and cardiovascular disease associations.

---

## Main Expected Results

A successful reproduction should recover the principal findings reported in the manuscript.

### Primary phenotype solution

Final analytic cohort:

```text
n = 4,326
```

Selected number of phenotypes:

```text
K = 5
```

The five survey-weighted phenotypes are:

| Phenotype | Estimated prevalence |
|---|---:|
| Metabolically Favorable | 25.5% |
| Older High-HDL | 27.0% |
| Dyslipidemic | 28.0% |
| Severe Adiposity | 17.0% |
| Severe Hyperglycemia | 2.5% |

### Stability

Bootstrap stability of the primary consensus:

```text
Mean ARI = 0.941
```

Leave-one-method-out robustness:

```text
ARI = 0.916–0.962
```

These results indicate that the final consensus is not dependent on any single component clustering algorithm.

### Simulation validation

Under **strong cluster separation and severe sampling imbalance**:

```text
Unweighted consensus prevalence MAE = 0.2507
Survey-weighted consensus prevalence MAE = 0.0116
```

This corresponds to an approximately **95% reduction in population-prevalence error**.

Under the same condition:

```text
Unweighted consensus ARI = 0.944
Survey-weighted consensus ARI = 0.938
```

Thus, the primary benefit of survey weighting in this simulation condition is improved **target-population prevalence recovery**, rather than uniformly improved individual-level cluster assignment.

### Clinical associations

Survey-design regression adjusted for age and sex shows an overall association between phenotype membership and physician-diagnosed diabetes:

```text
F(4,24) = 89.35
p < 0.001
```

and cardiovascular disease:

```text
F(4,24) = 6.86
p < 0.001
```

These associations are cross-sectional and should not be interpreted as evidence of causality or prospective clinical utility.

---

## Reproducibility Notes

The analysis incorporates the NHANES complex survey design where appropriate.

Important methodological distinctions include:

1. Survey weights are incorporated into the consensus construction to reflect unequal population representation.
2. Survey weighting is not assumed to guarantee superior geometric clustering.
3. Controlled simulations separately evaluate cluster-assignment recovery and target-population prevalence recovery.
4. Downstream clinical regression uses the NHANES survey design, including fasting-subsample weights, strata, and PSUs.
5. Diabetes is treated as supportive clinical characterization rather than independent validation because HbA1c and fasting glucose contribute to phenotype derivation.
6. CVD provides a more independent outcome assessment because it was not used to derive the phenotypes.

Random seeds are fixed where applicable to facilitate reproducibility.

---

## Software

The primary computational environment uses:

```text
Python 3.11
```

Major Python libraries include:

- NumPy
- pandas
- SciPy
- scikit-learn
- Matplotlib

See `requirements.txt` for the complete package list and versions used for the archived release.

Survey-design regression is performed in **R** using the:

```text
survey
```

package.

The exact R and package versions used for the final reproducibility release should be recorded with the archived version of the repository.

---

## Computational Considerations

The ensemble analysis constructs multiple base clustering partitions and a sparse survey-weighted co-association representation. Runtime and memory requirements will therefore depend on the computing environment.

The repository is intended to reproduce the analysis reported in the accompanying manuscript rather than serve as a general-purpose clustering software library.

---

## Interpretation and Limitations

The identified phenotypes should be interpreted as **descriptive multivariable metabolic patterns**, not as clinically validated disease subtypes.

The study is cross-sectional and does not establish:

- causal relationships;
- prospective disease risk;
- treatment response;
- incremental diagnostic value; or
- prognostic utility.

The analysis is restricted to the NHANES fasting subsample and participants with complete phenotype variables. Although survey weighting improves population representation and controlled simulations support improved prevalence recovery under unequal sampling, external and longitudinal validation is required before broader clinical or prognostic conclusions can be drawn.

---

## Citation

If you use this repository or its methods, please cite the accompanying manuscript:

> Odebode, A., Swift, S., & Arzoky, M. **Survey-Weighted Clustering Ensembles for Population-Representative Metabolic Phenotyping.** *Scientific Reports*, manuscript under revision.

Please update the citation above with the final journal volume, article number, year, and DOI after publication.

### Software archive

The version of the code corresponding to the manuscript will be archived on Zenodo.

```text
Zenodo DOI: [TO BE ADDED]
```

Once the Zenodo DOI is assigned, users should cite the archived software release in addition to the manuscript.

---

## Code Availability

The repository contains the computational workflow used for data preprocessing, survey-weighted clustering ensemble construction, benchmark and robustness analyses, simulation validation, sensitivity analyses, visualization, and preparation of the survey-design clinical association analyses.

A versioned release corresponding to the manuscript will be permanently archived in Zenodo and assigned a DOI.

**GitHub repository:**  
`https://github.com/YOUR-USERNAME/survey-weighted-metabolic-phenotyping`

**Zenodo archive:**  
`[TO BE ADDED AFTER RELEASE]`

---

## Data Availability

NHANES data are publicly available from the U.S. Centers for Disease Control and Prevention.

The repository does not redistribute the original NHANES source data. Instructions and code required to obtain and process the relevant public NHANES files are provided as part of the reproducibility workflow.

---

## License

This project is released under the **MIT License**.

See the [`LICENSE`](LICENSE) file for details.

---

## Contact

**Afees Odebode**  
Department of Computer Science and Informatics  
Fort Hays State University  
Hays, Kansas, USA

For questions about the analysis or reproducibility materials, please open an issue in this GitHub repository.

---

## Acknowledgments

This repository accompanies research conducted using publicly available data from the National Health and Nutrition Examination Survey (NHANES).

The findings and interpretations presented in the accompanying manuscript are those of the authors.
