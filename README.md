# Patient Readmission Risk Analytics

> A full analytics pipeline on hospital EHR data: who gets readmitted within 30 days, why, and what it costs.

## Overview

This is the flagship project — a complete, report-style healthcare analytics workflow that goes from raw electronic health records to a costed executive recommendation. It links clinical risk to financial impact and defends each conclusion with statistical evidence and confounder checks.

## The data

| File | Rows | Description |
|------|------|-------------|
| `data/patient_outcome.csv` | 6,600 | Hospital admissions with clinical outcomes and charges |
| `data/patients.csv` | 100,000 | Patient demographics, vitals, and 15 comorbidity flags |

*Synthetic EHR data (2018–2024) — no real patient information.*

## Analytical pipeline

1. **Data screening** — structure, missingness, outlier decisions (extreme charges retained as clinically plausible).
2. **Feature engineering** — Charlson Comorbidity Index, DRG severity tiers (MCC/CC), risk stratification.
3. **Hypothesis testing** — 8 two-sample tests (readmission by risk tier, insurance, smoking, alcohol, exercise; charges by ICU status; length-of-stay by diagnosis) with explicit null/alternative statements.
4. **Bootstrap & Monte Carlo** — confidence intervals for population readmission rate and a cost simulation of readmission risk by tier.
5. **PCA** — dimensionality reduction on vitals/behavior features.
6. **Clustering** — K-Means patient segmentation (silhouette-tuned), integrated with A/B testing across clusters.
7. **Executive summary** — findings and recommended interventions.

## Key findings

- **The problem is expensive:** 14.3% 30-day readmission across 6,600 admissions ≈ **944 preventable readmissions** and **≈ $17M** burden at a mean index charge of **$18,090**.
- **Risk is actionable:** high-comorbidity patients (Charlson ≥ 3) are readmitted **+11.6 pp** more often than low-risk patients — **p < 0.0001**, roughly **5× higher** risk.
- **Not a confound:** the risk gap stays significant **within** individual DRGs (Heart Failure, Pneumonia, Joint Replacement, Esophagitis), so it's driven by comorbidity burden, not disease mix alone.
- **Quantified uncertainty:** bootstrap 95% CI for the Heart-Failure readmission rate is **14.31% (13.16%–15.46%)**.

## Recommended interventions

- **High-complexity discharge bundling** — assign intensive post-discharge coordinators to Charlson ≥ 3 patients before discharge.
- **Diagnosis-specific protocols** — differentiated (not one-size-fits-all) post-acute plans, with Heart Failure as the priority cohort.

## Repository structure

```
patient-readmission-risk-analytics/
├── patient_readmission_analytics.ipynb
├── data/
│   ├── patient_outcome.csv
│   └── patients.csv
├── requirements.txt
└── README.md
```

## How to run

```bash
pip install -r requirements.txt
jupyter notebook patient_readmission_analytics.ipynb
```

## Tech stack

Python · pandas · NumPy · SciPy · statsmodels · scikit-learn · factor_analyzer · Matplotlib · Seaborn

---
*Analytical project by Anchor Miao (Ching-Hung Miao), USC Marshall MSBA.*
