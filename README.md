# Customer Churn Prediction — A Data-Driven Retention Strategy for Telecom

**GCI World 2026 April · Final Business Proposal**
**Matsuo-Iwasawa Lab, The University of Tokyo**
**Author:** Esraa

**In-Class Competition Result: 🏆 0.84182 — Rank 264 / ~3,500**

---

## 📌 Overview

This project builds an end-to-end machine learning pipeline to predict customer churn for a telecom company, then translates the model's predictions into a concrete, quantified retention strategy for the business.

The work covers the full data science lifecycle: exploratory data analysis, feature engineering, model selection & tuning, evaluation, and finally a business proposal backed by ROI analysis — submitted as the final assignment for the **GCI World 2026 April** program.

## 🎯 Problem Statement

Predict whether a telecom customer will churn (leave) based on their usage, billing, and service-quality data, so the company can proactively intervene with retention offers rather than reactively losing customers.

## 📊 Dataset

- **100,000 customer records**, ~230 raw features
- Nearly balanced classes: 50.4% stayed, 49.6% churned
- Source: provided via GCI World 2026 April course materials (Omnicampus)

## 🔍 Exploratory Data Analysis — Key Findings

- 10 high-missingness columns (>20%) identified and dropped
- **Equipment age is the #1 predictive signal** — churners carry devices ~58 days older on average (421d vs 363d)
- Refurbished-device customers churn at ~53% vs ~49% for new-device customers
- Churners show a steeper decline in monthly minutes of use (MOU)

## 🛠️ Feature Engineering

12 engineered signals built from raw telecom data, including:

| Feature | Description |
|---|---|
| `eqpdays` | Equipment age in days (top-ranked feature) |
| `totmrc_Mean` | Mean monthly recurring charge |
| `rev_per_min` | Revenue efficiency (revenue ÷ minutes-of-use) |
| `mou_rev_sync` | Usage-revenue alignment |
| `usage_momentum` | Rolling MOU trend — declining = churn warning signal |
| `failure_rate` | Call failure rate (service dissatisfaction proxy) |
| `call_completion_rate` | Network quality proxy |

*(Full list of 12 features in the report.)*

## 🤖 Model Selection

| Model | OOF AUC |
|---|---|
| Logistic Regression | Baseline |
| Random Forest | Ensemble |
| LightGBM v3 (hand-picked) | 0.6939 |
| LightGBM v3 (ensemble ×4) | 0.6980 |
| **LightGBM v4 + Optuna (30 trials)** ✅ | **0.6967 (Final)** |

The final model is a **Tuned LightGBM** with hyperparameters optimized via **Optuna** (30 trials) — chosen for its balance of reproducibility, single-model simplicity, and strong recall.

## 📈 Model Performance

- **Test AUC-ROC:** 0.698
- **Recall @ threshold 0.35:** 90.1% (catches 9 in 10 churners before they leave)
- **Precision @ threshold 0.35:** 56%
- Decision threshold set at **0.35** (not the default 0.5) because the business cost of a missed churner far outweighs the cost of unnecessary outreach

## 🧩 Risk Segmentation

Model scores were translated into three actionable customer tiers:

| Tier | Score Range | Actual Churn Rate | Action |
|---|---|---|---|
| 🔴 High Risk | 60–100% | ~70% | Immediate outreach + upgrade offer |
| 🟠 Medium Risk | 30–60% | ~47% | Loyalty incentive + proactive check-in |
| 🟢 Low Risk | <30% | ~22% | Monitor only |

## 💰 Business Impact

| Metric | Value |
|---|---|
| Revenue Retained | +$6.70M |
| Campaign Cost | −$1.20M |
| **Net Annual Impact** | **+$5.50M** |
| **ROI** | **5.6×** |
| Break-even offer success rate | ~8% |

*Assumptions: avg. monthly revenue $28/customer · retention offer cost $48/customer · 25% offer success rate.*

## 🚀 Next Steps (Proposed)

- Deploy model to production CRM, scoring all customers weekly
- A/B test retention offers (upgrade voucher vs. loyalty discount) on the High-Risk segment
- Monitor actual vs. predicted churn monthly; retrain quarterly
- Explore SHAP explanations for per-customer interpretability

## 🏆 Competition Result

This project was also submitted to the **GCI World 2026 April In-Class Competition** (a parallel NFL Draft Prediction task using the same ML workflow), achieving:

- **Score: 0.84182**
- **Rank: 264 out of ~3,500 participants**

## 🧰 Tech Stack

- Python, Pandas, NumPy
- LightGBM, Optuna
- Matplotlib / Seaborn for visualization
- Scikit-learn for evaluation metrics

## 📁 Repository Structure

```
├── notebook.ipynb          # Full analysis: EDA → feature engineering → modeling → evaluation
├── report.pdf              # Final business proposal deck
├── competition_tutorial.pdf # In-class competition tutorial/reference
└── README.md
```

## 📚 References

1. GSMA Intelligence (2024). *State of Mobile Economy 2024.*
2. Harvard Business Review (2014). *The Value of Keeping the Right Customers.*
3. Statista (2024). *U.S. Telecom Industry Monthly Churn Rate.*
4. Ke, G. et al. (2017). *LightGBM: A Highly Efficient Gradient Boosting Decision Tree.* NeurIPS 2017.
5. Akiba, T. et al. (2019). *Optuna: A Next-generation Hyperparameter Optimization Framework.* KDD 2019.

---

*Completed as part of the GCI World 2026 April program at Matsuo-Iwasawa Lab, The University of Tokyo.*
