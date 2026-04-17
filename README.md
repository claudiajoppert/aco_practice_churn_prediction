# ACO Practice Churn Prediction

Predicting which healthcare practices will leave an Accountable Care Organization — before they do.

**500 practices · 150K patients · 5-year synthetic dataset · 78% ROC-AUC**

---

## The Problem

When practices leave ACOs, patient care is disrupted and shared savings targets collapse. By the time a practice signals it's leaving, it's usually too late to act. This project identifies at-risk practices **12 months in advance** so retention teams can intervene early.

---

## Approach

Built on a normalized 6-table schema (practices, patients, financials, operations, market context, churn labels) designed to reflect how ACO data actually lives in a warehouse — not a flat CSV.

The pipeline runs end-to-end across ten steps:

1. **Data modeling & synthetic generation** — churn probabilities derived from a logistic function encoding known ACO exit drivers, so the data has realistic, learnable structure
2. **EDA** — churn rates segmented by practice size, tenure, risk track, and financial performance
3. **Statistical testing** — t-tests, chi-square, and logistic regression (odds ratios) to confirm which drivers are signal vs. noise
4. **Survival analysis** — Kaplan-Meier curves showing *when* practices exit, stratified by shared savings status and risk track (log-rank p < 0.05)
5. **ML modeling** — Logistic Regression, Random Forest, and Gradient Boosting compared; Random Forest selected at 78% AUC, 75% recall, 82% precision
6. **SHAP interpretability** — practice-level explanations of model predictions for operational use
7. **K-means segmentation** — four practice archetypes with distinct churn profiles and targeted intervention strategies
8. **Scenario analysis** — model used as a policy simulator to estimate churn reduction from three interventions

---

## Key Findings
<img width="1440" height="1460" alt="image" src="https://github.com/user-attachments/assets/d7588171-1640-459e-9b73-1e288a70a1b5" />

- **Benchmark gap is the #1 driver** — practices spending >5% above their CMS benchmark churn at 3× the baseline rate
- **Downside risk accelerates exit** — two consecutive loss years push 12-month churn probability to ~70%
- **Small practices are structurally disadvantaged** — churn at 2× the rate of large practices due to resource constraints
- **Shared savings is the strongest retention lever** — practices earning >$100K/year in savings have ~85% 5-year retention

---

## Tech Stack

`Python` · `PySpark` · `scikit-learn` · `SHAP` · `lifelines` · `statsmodels` · `Databricks`

---

## About

**Claudia Joppert** · Senior Data Analyst · Healthcare Analytics  
[LinkedIn](https://www.linkedin.com/in/claudia-joppert-888568161) · [GitHub](https://github.com/claudiajoppert)
