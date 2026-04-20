# ACO Patient Churn Prediction

> Predicting which Medicare patients will leave the network, before they do.

---

## The problem

ACOs lose attributed revenue and quality scores every time a Medicare patient goes inactive, shifts care out-of-network, or stops seeing their assigned PCP. By the time it's obvious, it's too late. This project identifies at-risk patients early enough to intervene.

---

## Business impact

```
1,000 patients analyzed   →   74 flagged as high-risk   →   73 churns preventable
```

Catching 87.5% of churning patients before they leave means care coordinators can be dispatched while the relationship is still recoverable, protecting attributed revenue and CMS quality scores.

---

## How churn is defined

A patient is marked churned if any of the following is true:

- **6+ months** since last encounter (and has a history of visits)
- **60%+ of recent care** happened out-of-network
- **Zero PCP visits** in the last 6 months (with at least 3 recent encounters)

---

## What the data shows

| Signal | Churned patients | Active patients |
|---|---|---|
| Days since last visit | **172 days** | 43 days |
| PCP engagement rate | 44.6% | 52.3% |
| Out-of-network rate | Higher | Lower |
| New member (<6 mo) | Over-represented | — |

The gap in recency is the loudest signal. If someone hasn't shown up in six months, the model has already flagged them.

---

## Model performance

Built with Random Forest on 15 behavioral and demographic features.

| Metric | Score |
|---|---|
| AUC-ROC | **0.979** |
| Accuracy | **98.5%** |
| Recall (churn caught) | **87.5%** |
| False alarms | 1 of 200 |

AUC of 0.979 means near-perfect separation between patients who will churn and those who won't. For context: 0.5 is random guessing, 1.0 is perfect.

---

## Top predictive features

```
Recent encounters       ████████████████████████████████████████████  44%
ACO tenure              ████████                                        9%
PCP engagement rate     ███████                                         7%
Primary care ratio      ██████                                          6%
Out-of-network rate     █████                                           5%
```

Current engagement level dominates the model. The best predictor of future absence is recent absence.

---

## Risk tiers and action plan

| Tier | Patients | Churn rate | Action |
|---|---|---|---|
| 🔴 High risk | **74** | 98.6% | Immediate outreach: assign care coordinator, PCP visit within 2 weeks |
| 🟡 Medium risk | 7 | 71.4% | Proactive monitoring: preventive care reminders, PCP touchpoint within 60 days |
| 🟢 Low risk | 911 | 0.2% | Standard wellness programs, quarterly monitoring |

---

## Technical approach

- **Data:** 1,000 synthetic Medicare patients, 10,000+ encounters over 3 years (ages 65–95, 68% with chronic conditions)
- **Platform:** PySpark on Databricks for feature engineering at scale, scikit-learn for modeling
- **Features engineered:** in-network utilization rate, PCP engagement rate, care fragmentation index, recent vs. historical encounter patterns, new member flag
- **Model:** Random Forest (100 estimators, max depth 10, stratified 80/20 split)

---

## Roadmap

**Now**
- Score all 1,000 patients, surface top 74 to care coordination team
- Assign dedicated coordinators, schedule wellness visits within 2 weeks

**Next quarter**
- Build PCP onboarding program targeting new members (<6 months)
- Reduce out-of-network leakage with proactive referral management

**6–12 months**
- Automate monthly scoring pipeline in Databricks
- Build operational dashboard for care managers
- A/B test intervention strategies, track ROI on prevented churns

---

## Files

```
ACO_Patient_Churn_Prediction.ipynb   # Full analysis: data generation, EDA, modeling, risk tiers
README.md                            # This file
```

---

## Tech Stack

`Python` · `PySpark` · `scikit-learn` · `SHAP` · `lifelines` · `statsmodels` · `Databricks`

---

## About

**Claudia Joppert** · Senior Data Analyst · Healthcare Analytics  
[LinkedIn](https://www.linkedin.com/in/claudia-joppert-888568161) · [GitHub](https://github.com/claudiajoppert)
