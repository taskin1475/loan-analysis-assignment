# AN7914 — Lending Club Interest Rate Analysis

**Module:** AN7914: Data Analytics and Modelling A (25/26)  
**University:** University of Winchester  
**Student ID:** 2506630  

---

## Project Overview

This project analyses the determinants of interest rates charged on the Lending Club peer-to-peer lending platform. Using a dataset of 10,000 loan observations, the analysis employs a structured pipeline of data cleaning, exploratory data analysis, and OLS regression modelling to identify the key borrower and loan characteristics that predict the interest rate assigned.

The central finding is that **loan grade** — Lending Club's internal credit quality score — is by far the dominant predictor of interest rate, with Grade G borrowers paying approximately 23.8 percentage points more than Grade A borrowers. Credit utilisation ratio, debt-to-income ratio, and the number of recent credit checks are also statistically significant predictors in the full specification.

---

## Dataset

| Property | Detail |
|---|---|
| File | `loans_dataset_4473645.csv` |
| Raw observations | 10,000 rows × 55 columns |
| After cleaning | 9,976 rows × 17 columns |
| Target variable | `interest_rate` |
| Source | Lending Club (provided by module) |

---

## Repository Structure

```
AN7914-Lending-Club-Analysis/
│
├── AN7914_Lending_Club_Analysis.ipynb   # Main notebook (Parts A, B, C)
├── loans_dataset_4473645.csv            # Dataset — place in same folder as notebook
├── README.md                            # This file
│
└── figures/                             # Auto-generated when notebook is run
    ├── fig1_interest_rate_hist.png
    ├── fig2_annual_income_hist.png
    ├── fig3_scatter_ir_income.png
    ├── fig4_scatter_ir_dti.png
    ├── fig5_boxplot_grade.png
    ├── fig6_boxplot_verified.png
    └── fig7_boxplot_homeownership.png
```

---

## Analysis Pipeline

### Part A — Data Preparation
- Subsetted the raw 55-column dataset to 15 required variables
- Renamed `inquiries_last_12m` → `credit_checks`
- Handled missing values:
  - `emp_length` (817 missing, 8.17%) → imputed with median (6 years)
  - `debt_to_income` (24 missing, 0.24%) → rows dropped
- Final dataset: **9,976 observations × 15 variables**

### Part B — Exploratory Data Analysis
- Descriptive statistics for key numerical variables
- Frequency tables for `grade`, `verified_income`, `homeownership`
- 7 visualisations: 2 histograms, 2 scatterplots, 3 boxplots
- Feature engineering:
  - `credit_util` = total_credit_utilized / total_credit_limit (mean = 40.30%)
  - `bankruptcy_dummy` = 1 if public_record_bankrupt ≥ 1 (12.16% positive)

### Part C — Regression Analysis (5 OLS Models)

| Model | Specification | R² |
|---|---|---|
| Model 1 | `interest_rate ~ debt_to_income` | 0.020 |
| Model 2 | `interest_rate ~ bankruptcy_dummy` | 0.002 |
| Model 3 | `interest_rate ~ verified_income dummies` | 0.061 |
| Model 4 | `interest_rate ~ DTI + credit_util + bankruptcy` | 0.078 |
| Model 5 | Full specification (all controls + grade dummies) | **0.952** |

Model 5 achieves R² = 0.952 with F-statistic = 7,276.74 (p < 0.0001).  
Loan grade is the dominant predictor; Grade G borrowers pay ~23.8 pp more than Grade A.

---

## Requirements

### Python Version
Python 3.10 or higher (developed on Python 3.12)

### Dependencies

```bash
pip install pandas numpy matplotlib seaborn statsmodels scipy jupyter
```

| Library | Purpose |
|---|---|
| `pandas` | Data manipulation and cleaning |
| `numpy` | Numerical operations |
| `matplotlib` | Base plotting |
| `seaborn` | Statistical visualisations |
| `statsmodels` | OLS regression models |
| `scipy` | Statistical utilities |
| `jupyter` | Notebook environment |

---

## How to Run

1. **Clone the repository:**
   ```bash
   git clone <your-github-link>
   cd AN7914-Lending-Club-Analysis
   ```

2. **Place the dataset** in the root directory:  
   `loans_dataset_4473645.csv` must be in the same folder as the notebook.

3. **Install dependencies:**
   ```bash
   pip install pandas numpy matplotlib seaborn statsmodels scipy jupyter
   ```

4. **Launch Jupyter:**
   ```bash
   jupyter notebook AN7914_Lending_Club_Analysis.ipynb
   ```

5. **Run all cells:**  
   `Kernel → Restart & Run All`  
   All 7 figures will be automatically saved to the `figures/` directory.

---

## Key Findings

- **Loan grade is by far the strongest predictor:** Grade G borrowers pay ~23.8 pp more than Grade A (Model 5, β = 23.775***)
- **Credit utilisation significantly increases rates:** β = 0.323 in Model 5 (p < 0.0001)
- **DTI has a small but significant positive effect:** β = 0.003 in Model 5 (p < 0.001)
- **Bankruptcy becomes insignificant** once grade is controlled for (β = 0.020, p = 0.561), suggesting grade already prices in bankruptcy risk
- **Homeownership and employment length are not significant** in the full model
- **Annual income has a small negative effect** (β ≈ −0.0000005), consistent with theory

---

## Author

**Student ID:** 2506630  
**Module:** AN7914 — Data Analytics and Modelling A  
**University of Winchester**
