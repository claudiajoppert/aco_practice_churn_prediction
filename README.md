# ACO Patient Churn Prediction

## A portfolio case study on identifying at-risk Medicare patients before they leave

> **About this project:** This is a portfolio analysis built on **synthetic Medicare ACO data** (1,000 simulated patients, ~10,000 simulated encounters). The patients, encounters, and revenue figures are not real. The methodology, model architecture, and recommended workflow are designed to mirror what a production deployment in a real ACO setting could look like.

---

## The Problem

Each year, a meaningful share of Medicare patients in an ACO stop using the network. They either go inactive for 6+ months or shift their care out-of-network. Either way, the practice loses them — along with the revenue attributed to those patients.

In most workflows, this churn is only noticed after the patient is already gone. The goal of this project is to flag the risk early enough to do something about it.

---

## What the Project Built

A predictive model that identifies which patients are likely to churn before they actually do.

The dataset (synthetic) covered 1,000 patients and roughly 10,000 encounters, with features spanning:

- Visit frequency patterns
- PCP relationship strength
- Out-of-network utilization trends
- Tenure in the ACO network

On a held-out test set, the model reached **98.5% accuracy** and recalled **87.5%** of churners — i.e., it caught roughly 9 out of 10 patients who went on to churn in the simulation.

---

## What the Model Found (in the synthetic data)

### 74 patients were flagged as high-risk

The model identified **74 high-risk patients** with a predicted churn probability around 99%. In the simulated test outcomes, 73 of those 74 churned without intervention.

| Risk Level | Patients | Predicted Churn Rate | Interpretation |
|:-----------|:--------:|:--------------------:|:---------------|
| 🔴 **High Risk** | 74 | 99% | Likely to leave without action |
| 🟡 **Medium Risk** | 7 | 71% | Trending toward high risk |
| 🟢 **Low Risk** | 911 | 0.2% | Stable and engaged |

---

## Why They're Leaving (signals from the model)

Four signals dominated the model's predictions.

### 1. Long gaps since last visit

Churned patients in the simulation averaged **172 days** since their last visit, versus **43 days** for active patients. That ~4-month gap was the single strongest signal in the model.

### 2. Rising out-of-network utilization

Higher out-of-network share suggests the patient is establishing relationships with other providers. By the time it shows up clearly, the patient has often already mentally left.

### 3. Weak PCP relationships

Churned patients had a **44.6%** PCP engagement rate, versus **52.3%** for active patients. Patients who don't see their assigned PCP don't build the relationship that anchors them in-network.

### 4. Newer ACO members are more vulnerable

Newer members were over-represented in the churn group. The first few months of enrollment matter — patients who don't engage with their PCP early are more likely to drift out.

---

## Why This Matters for a Real Deployment

The model produces something most operational dashboards don't: **advance warning**. The patterns above show up well before the 6-month inactive threshold and before the patient has fully shifted to out-of-network care, leaving a window for outreach and intervention.

### Plausible impact in a real ACO

If a deployment of this approach prevented even 50–60 of the simulated 73 high-risk churns, the organizational impact would include:

- Protected attributed revenue
- Improved quality scores from fewer inactive patients
- Stronger network retention overall

The exact dollar impact depends on the specific ACO's attributed PMPM and shared-savings structure, so any ROI estimate should be modeled against real contract terms rather than copied from this case study.

---

## Suggested Action Plan (template for a real deployment)

### Week 1

**Pull the list. Start calling.**

- [ ] Export the high-risk cohort from the model
- [ ] Assign a care coordinator to each patient
- [ ] Schedule urgent PCP wellness visits within 2 weeks
- [ ] For no-shows, diagnose the barrier:
  - Transportation
  - Access / scheduling
  - Dissatisfaction with assigned PCP
  - Insurance change

### Month 1

**Strengthen onboarding.**

- [ ] Audit the new-member onboarding workflow
- [ ] Identify drop-off points in the first 90 days
- [ ] Schedule new members with their assigned PCP within 30 days of enrollment
- [ ] Build a meaningful "welcome to the ACO" touchpoint

### Quarter 1

**Operationalize the model.**

- [ ] Stand up a monitoring dashboard that refreshes monthly
- [ ] Re-score all patients every 30 days
- [ ] Catch patients sliding into the medium-risk band early
- [ ] Automate high-risk alerts to the care coordination team
- [ ] Track intervention outcomes and feed them back into the model

---

## How the Model Works

A Random Forest classifier was trained on 15 patient engagement features:

- Demographics (age, gender, chronic conditions)
- Geographic access (distance to PCP)
- Engagement patterns (visit frequency, PCP relationship)
- Network utilization (in-network vs. out-of-network)
- Care continuity (provider fragmentation, preventive care)

### What drives predictions

| Feature | Importance | Interpretation |
|:--------|:----------:|:---------------|
| **Recent visit activity** | 44% | The dominant signal |
| **Tenure in ACO** | 9% | Newer members are more vulnerable |
| **PCP engagement** | 7% | Relationship strength matters |
| **Primary care ratio** | 6% | Share of care delivered in-network |
| **Out-of-network rate** | 5% | Early sign of provider shopping |

### Model performance (on held-out synthetic test set)

```
Accuracy:        98.5%
Recall:          87.5%   (catches ~9 of 10 churners)
Precision:       93.3%   (few false alarms)
AUC-ROC:         0.979   (near-perfect class separation)
```

A note on these numbers: performance this strong is partly a feature of synthetic data, where the underlying churn process is more learnable than reality. A real-world deployment should expect more noise and should re-validate against the ACO's actual historical churn before being trusted operationally.

---

## What Success Could Look Like

### 3 months in

- 50+ of the high-risk patients have had meaningful PCP touchpoints
- 40–50 predicted churns prevented through intervention
- Monthly monitoring dashboard live
- Care coordinators have a clear high-risk outreach workflow

### 6 months in

- Churn rate trending from ~8% toward 5–6%
- New-member 90-day engagement up 20%+
- Measurable revenue retention
- Quality scores reflect improved continuity

### 12 months in

- Predictive monitoring is standard operating procedure
- At-risk patients are caught well before the 6-month inactive mark
- The model is refined with real intervention outcomes
- Retention becomes a measurable competitive advantage

---

## Anticipated Questions

**Q: How confident should anyone be in these 74 patients?**
A: Very, *within the synthetic dataset*. The model is 98.5% accurate with 93% precision on held-out test data. Real-world precision will be lower and should be validated before clinical operations rely on it.

**Q: What if there aren't enough care coordinators?**
A: Start with the highest-risk 20–30 patients. Even partial coverage produces measurable ROI and builds the case for scaling.

**Q: Will patients respond to outreach?**
A: Some won't. The economics still work if even half of high-risk churns are prevented, given typical ACO attributed revenue.

**Q: How often should the model re-run?**
A: Monthly. Risk scores shift as patient behavior shifts, and the goal is to catch slides into high risk before they're irreversible.

**Q: What's the investment required?**
A: Mostly care coordinator time. The model itself is built. Execution is low-tech, high-touch — phone calls, scheduling, barrier removal.

---

## Bottom Line

The model produces a named list of high-risk patients before they actually churn. That capability — advance warning at the patient level — is the part that's transferable from this synthetic case study to a real ACO deployment. The hard part isn't building the model; it's standing up the operational workflow that acts on it.

---

*Methodology developed and validated on 1,000 synthetic Medicare ACO patients with 10,054 simulated encounters and an 8% baseline churn rate. All patients, encounters, and revenue figures are synthetic. Model code and notebook are reproducible.*
