# Bayesian Survival Analysis of Patient Recovery Times in Primary Biliary Cirrhosis (PBC)

**ECE225A – Probability & Statistics for Data Science**

---

## 1. Project Overview

This project performs a **Bayesian survival analysis** of patient survival and mortality outcomes using the **Mayo Clinic Primary Biliary Cirrhosis (PBC)** dataset.

We develop both **non-hierarchical** and **hierarchical Bayesian survival models** using **Exponential likelihoods** (with optional Weibull comparison) to study how demographic factors and clinical biomarkers influence patient survival under right-censoring.

### Key Objectives
- Estimate covariate effects and **hazard ratios** with credible intervals  
- Compare **hierarchical vs. non-hierarchical** survival models  
- Perform **prior and posterior predictive checks**  
- Conduct **sensitivity analysis** to prior choices  
- Evaluate model fit using **LOO-CV** and **WAIC**

---

## 2. Dataset

**Source:** Mayo Clinic Primary Biliary Cirrhosis Dataset (`pbc2`)  
**Observations:** 1945 rows across 312 patients

### Key Variables
- **Survival:** `years`, `status`, `status2`
- **Demographics:** `age`, `sex`
- **Treatment:** `drug`
- **Biomarkers:**  
  `serBilir`, `serChol`, `albumin`, `alkaline`, `SGOT`,  
  `platelets`, `prothrombin`
- **Clinical Indicators:**  
  `ascites`, `hepatomegaly`, `spiders`, `edema`, `histologic`

See `data/data_dictionary.md` for full variable descriptions.

---

## 3. Repository Structure
pbc-bayesian-survival-analysis/
├── data/ # Raw and processed data
├── notebooks/ # Jupyter notebooks for analysis
├── src/ # Python scripts for EDA and modeling
├── models/ # PyMC models
├── results/ # Figures, tables, diagnostics
├── report/ # Final report
├── environment/ # environment.yml and requirements.txt
└── README.md

---

## 4. Analysis Pipeline

### 1. Exploratory Data Analysis (EDA)
- Summary statistics (mean, SD, median, range)
- Histograms and density plots for biomarkers
- Kaplan–Meier survival curves stratified by:
  - Treatment (`drug`)
  - Sex
  - Edema status
- Log-rank tests
- Correlation heatmaps

---

### 2. Bayesian Survival Models

#### Model 1: Non-Hierarchical Survival Model
- **Likelihood:** Exponential  
- **Linear Predictor:**
log(λ_i) = α + β₁·age + β₂·drug + β₃·sex + β₄·serBilir + ...

- **Priors:**  
`α, β ~ Normal(0, 1)`

**Outputs**
- Posterior distributions
- Hazard ratios `exp(β)`
- Credible intervals

---

#### Model 2: Hierarchical Survival Model
- Random intercepts (e.g., patient-level or clinical grouping)
- **Priors:**
μ_α ~ Normal(0, 1)
σ_α ~ Exponential(1)

---

### 3. Prior Predictive Checks
- Sample survival times from priors
- Validate plausible survival ranges
- Visualize prior-based survival curves

---

### 4. Posterior Inference
- **Sampler:** HMC / NUTS  
- **Sampling:** 4 chains × 2000 draws

**Diagnostics**
- R-hat (< 1.1)
- Effective Sample Size (ESS)
- Divergence checks
- Trace plots

---

### 5. Posterior Predictive Checks (PPC)
- Generate survival curves from posterior samples
- Compare with empirical Kaplan–Meier curves

---

### 6. Model Comparison
- Leave-One-Out Cross-Validation (LOO-CV)
- WAIC
- Compare:
- Hierarchical vs Non-hierarchical

---

### 7. Sensitivity Analysis
- Alternative priors:
- Half-Normal(1)
- Student-t(3)
- More informative Normal priors
- Assess posterior robustness

---

### 8. Interpretation & Reporting
- Credible intervals for coefficients
- Forest plots for hazard ratios
- Comparison of hierarchical vs non-hierarchical effects
- Clinical interpretation of biomarker effects

---

## 5. Reproducibility

### Environment Setup

**Using Conda**
```bash
conda env create -f environment/environment.yml
conda activate pbc-env

**Using Pip**
pip install -r environment/requirements.txt

Key Dependencies
- Python 3.10+
- NumPy, pandas
- ArviZ
- PyMC 
- Matplotlib, Seaborn
- Lifelines (Kaplan–Meier curves)
- scikit-survival 

6. How to Run the Project
Data Preparation
python src/data_prep.py
Exploratory Analysis
python notebooks/01_exploratory_analysis.ipynb
Bayesian Modeling
python src/model1_non_hierarchical.py
python src/model2_hierarchical.py
Diagnostics & Posterior Predictive Checks
python src/posterior_checks.py
Model Evaluation
python src/evaluation.py

7. Results
All outputs are stored under:
results/
├── figures/
├── tables/
└── diagnostics/
Includes:

Kaplan–Meier curves

Posterior distributions

Trace and diagnostic plots

LOO / WAIC comparison tables

Hazard ratio summaries

