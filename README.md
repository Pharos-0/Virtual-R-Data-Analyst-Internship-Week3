# Virtual R Data Analyst Internship

**Week 3 of 4 – Statistical Analysis and Predictive Modeling Using R**

This repository contains the code, data and outputs of the Week 3 submission: the R code (including the Week 1 import and cleaning steps needed to recreate the cleaned data), the dataset, statistical and model outputs, diagnostics, console transcripts and rendered screenshots. The written report (Word document) is submitted separately through the internship portal.

## Project Overview

The internship project analyses customer churn for a telecommunications company using the IBM Telco Customer Churn dataset. The work is split into four weekly submissions, each in its own repository:

| Week | Topic | Repository |
|---|---|---|
| 1 | Data cleaning and preliminary analysis | [Virtual-R-Data-Analyst-Week1](https://github.com/Pharos-0/Virtual-R-Data-Analyst-Week1) |
| 2 | Data visualisation and insight communication | [Virtual-R-Data-Analyst-Week2](https://github.com/Pharos-0/Virtual-R-Data-Analyst-Week2) |
| **3 (this repository)** | Statistical analysis and predictive modelling | [Virtual-R-Data-Analyst-Week3](https://github.com/Pharos-0/Virtual-R-Data-Analyst-Week3) |
| 4 | Comprehensive final report | [Virtual-R-Data-Analyst-Week4](https://github.com/Pharos-0/Virtual-R-Data-Analyst-Week4) |

## Dataset

- **Name:** Telco Customer Churn (IBM sample data describing a fictional telecommunications company)
- **Source:** [IBM/telco-customer-churn-on-icp4d](https://github.com/IBM/telco-customer-churn-on-icp4d/blob/master/data/Telco-Customer-Churn.csv) – Apache License 2.0
- **Size:** 7,043 rows × 21 columns; target `Churn` (1,869 churned, 26.5%)

## Objectives

1. Formulate and test hypotheses about churn with appropriate tests, checking their assumptions.
2. Build and validate a churn classification model (logistic regression) and compare it with a random forest.
3. Evaluate the models with a confusion matrix, accuracy, precision, recall, F1, ROC-AUC, calibration and threshold analysis, and interpret the results.

## Week 1 – Data Cleaning and Preliminary Analysis

Import, inspection, missing values, duplicates, type conversion, outliers, scaling, encoding, descriptive statistics and correlation. See the [Week 1 repository](https://github.com/Pharos-0/Virtual-R-Data-Analyst-Week1).

## Week 2 – Data Visualization

Eleven `ggplot2` charts organised around six questions. See the [Week 2 repository](https://github.com/Pharos-0/Virtual-R-Data-Analyst-Week2).

## Week 3 – Statistical Analysis and Predictive Modeling (this repository)

- **Report:** `Week_3_Report.docx`, submitted through the internship portal (not stored in this repository)
- **Code:** [`R/05_statistical_analysis.R`](R/05_statistical_analysis.R), [`R/06_predictive_model.R`](R/06_predictive_model.R) (after `R/01_data_import.R` and `R/02_data_cleaning.R`), run by [`run_all.R`](run_all.R)
- **Outputs:** test results (`outputs/tables/w3_hypothesis_test_summary.csv`), model metrics (`outputs/tables/w3_test_metrics.csv`, `w3_cv_summary.csv`), odds ratios, importance, calibration, gains and test-set predictions (`outputs/tables/w3_*.csv`), figures (`figures/w3_*.png`), console transcripts (`outputs/logs/`), screenshots (`screenshots/`)

**Statistical analysis.** Eight hypotheses (chi-square, Welch t-test, Wilcoxon rank-sum, Welch ANOVA/Kruskal–Wallis, correlation and two-proportion tests), with normality (Anderson–Darling, Q-Q plots), equal-variance (Levene), independence and class-balance checks, Holm-adjusted p-values and effect sizes.

**Predictive modelling.** Stratified 80/20 split (seed 42); 10-fold stratified cross-validation with identical folds for all models; centring/scaling learned inside the folds; multicollinearity checked with VIF; logistic regression (17 predictors) and random forest (`ranger`, tuned over 16 settings); threshold chosen on out-of-fold predictions and applied once to the test set.

## Week 4 – Comprehensive Final Report

Integrated final report. See the [Week 4 repository](https://github.com/Pharos-0/Virtual-R-Data-Analyst-Week4).

## Technologies

R 4.3.3 · `caret` · `ranger` · `pROC` · `car` · `nortest` · `ResourceSelection` · `broom` · `ggplot2` · `dplyr` · `tidyr` · `readr` · `forcats` · `purrr` · `highr` (screenshots). The analysis was run with `Rscript`; RStudio was not available, so the screenshots are renderings of the saved R console transcripts.

## Repository Structure

```text
Virtual-R-Data-Analyst-Week3/
├── README.md
├── run_all.R                    # runs every script in R/ and saves console transcripts
├── R/                           # analysis scripts
│   ├── 00_setup.R
│   ├── 01_data_import.R
│   ├── 02_data_cleaning.R
│   ├── 05_statistical_analysis.R
│   ├── 06_predictive_model.R
│   ├── render_screenshots.R
│   └── screenshot_specs.R
├── data/
│   ├── README.md
│   ├── raw/                     # Telco-Customer-Churn.csv + Apache-2.0 licence
│   └── processed/               # telco_clean.csv (cleaned data)
├── outputs/
│   ├── tables/                  # 18 CSV tables
│   ├── metrics/                 # 4 JSON files with the key results
│   └── logs/                    # console transcripts, run summary, session info, package versions
├── figures/                     # 11 charts (PNG, 300 dpi)
├── screenshots/                 # 31 renderings of R console output + index
└── requirements/
    └── R_packages.txt           # packages and versions used
```

## Methodology

Tests were chosen by variable type and assumption checks (Welch versions where variances differ, rank tests where data are strongly non-normal); every test reports an effect size because with 7,043 customers even small differences are significant. The models were built to avoid leakage: the identifier was removed, redundant category levels were recoded, all tuning and threshold choices used cross-validation on the training set only, and the test set was used once. The full method is described in the Week 3 report.

## Key Findings

- **7 of 8** null hypotheses rejected (Holm-adjusted). Contract type: Cramér's V = 0.41; tenure: median shift 19 months (rank-biserial r = 0.48); gender: no association (p = 0.487).
- Test ROC-AUC: logistic regression **0.853**, random forest **0.855** – no significant difference (DeLong p = 0.617).
- Logistic regression at threshold 0.5: accuracy 81.4%, precision 66.8%, recall 59.8%, F1 0.631; at the CV-chosen threshold 0.33: recall 78.6%, precision 56.6%.
- Strongest odds ratios: two-year contract 0.25, fibre optic 2.35, tech support 0.66, online security 0.71.
- The 20% of test customers with the highest predicted risk contain **53%** of the churners.
- Diagnostics: Hosmer–Lemeshow p = 0.252; no bins outside the binned-residual band; largest Cook's distance 0.0030.

## Reproducibility

```bash
Rscript run_all.R
```

Runs the import, cleaning, statistics and modelling scripts in order (seed 42; about four minutes, mostly random-forest tuning), writes console transcripts to `outputs/logs/` and re-renders the screenshots (requires Google Chrome or Chromium). Model objects are saved to `outputs/models/` but not committed (size). Packages and versions: [`requirements/R_packages.txt`](requirements/R_packages.txt). Install them with the command at the top of that file. A single script can be re-run with, for example, `Rscript run_all.R 02`; the R session information and package versions of the last run are saved in `outputs/logs/`.

**About the screenshots:** RStudio was not available, so the images in `screenshots/` are renderings of genuine R console transcripts saved by `run_all.R`, not RStudio window captures.

## Dataset Source

IBM, *Telco Customer Churn* sample data, repository [IBM/telco-customer-churn-on-icp4d](https://github.com/IBM/telco-customer-churn-on-icp4d), file `data/Telco-Customer-Churn.csv`. Apache License 2.0; see [`data/raw/LICENSE-Apache-2.0_IBM.txt`](data/raw/LICENSE-Apache-2.0_IBM.txt).

## Author

Pharos Sophy Samuel T J – Virtual R Data Analyst Internship, Week 3 submission.
