# 🏃‍♂️ Fit.ly Tech — Customer Churn Analytics & Retention Intelligence

[![DataCamp Certified Data Analyst](https://img.shields.io/badge/Certification-DataCamp%20Data%20Analyst%20Practical%20Exam-03EF62?style=for-the-badge&logo=datacamp&logoColor=white)](https://www.datacamp.com/)
[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-2.0%2B-150458?style=for-the-badge&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-1.3%2B-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)](https://scikit-learn.org/)
[![Statsmodels](https://img.shields.io/badge/Statsmodels-0.14%2B-blue?style=for-the-badge)](https://www.statsmodels.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)

---

## 📌 Executive Summary

**Fit.ly Tech** is a high-growth, subscription-based digital fitness and wellness mobile application. Over the preceding two quarters, senior leadership observed subscriber churn steadily creeping upward. With Customer Acquisition Costs (CAC) escalating across paid channels, retaining existing users has become critical to maintaining healthy unit economics.

This end-to-end data analytics project integrates disparate data silos—**Account Information**, **Customer Support**, and **In-App Telemetry Activity**—to uncover the statistical and behavioral drivers of subscriber churn, establish reliable baseline KPIs, and deliver evidence-backed retention strategies heading into the upcoming quarter.

> **Certification Note:** This comprehensive portfolio project was developed for the **DataCamp Data Analysis Professional Certificate Practical Exam**. It strictly follows industry-grade data validation standards, rigorous statistical hypothesis testing, GDPR privacy compliance protocols, predictive modeling benchmarks, and executive stakeholder reporting requirements.

---

## 🎯 Business Context & Project Objectives

### 1. The Business Problem
* **Rising Churn Rate:** Monthly subscriber losses have escalated, diluting lifetime customer value (LTV) and putting mounting pressure on Marketing and Product teams.
* **Customer Acquisition Pressure:** As CAC rises, backfilling lost subscribers with new acquisitions is no longer financially sustainable without fixing "leaks" in the retention funnel.
* **Fragmented Data Ecosystem:** Customer data resided across disconnected departments with conflicting schemas, naming collisions, untyped timestamps, and semi-labeled churn indicators.
* **Compliance Exposure:** Multiple support tickets contained explicit GDPR / "Right to be Forgotten" erasure requests, necessitating immediate privacy handling before modeling.

### 2. Project Aims & Core Objectives
1. **Systematic Data Audit & Cleaning:** Harmonize cross-system identifier keys (`customer_id` string vs. integer `user_id`), resolve schema collisions, address missing data, and ensure strict GDPR privacy compliance (PII masking).
2. **Unified Customer 360° Dataset:** Build an integrated, customer-grain master analytical table ($N = 400$) consolidating subscription tiers, ticket histories, resolution speeds, and telemetry engagement.
3. **Exploratory & Inferential Statistics:** Perform univariate, bivariate, and multivariate analysis alongside non-parametric hypothesis tests (**Chi-Square Test of Independence** and **Mann-Whitney U Test**) to confirm statistically significant churn factors.
4. **Predictive Driver Modeling:** Train regularized Multivariable Logistic Regression and Tree-based models to calculate adjusted Odds Ratios (ORs) and benchmark 5-fold cross-validated discriminative power ($ROC\text{-}AUC$).
5. **Business KPI Definition & Baseline Estimation:** Differentiate between **Logo Churn** and **Revenue Churn (MRR)** to deliver clear metrics for executive monitoring.
6. **Strategic & Actionable Roadmap:** Translate data discoveries into high-ROI operational and product interventions for the next quarter.

---

## 🏗️ Repository Architecture

```text
9.Churn/
├── data/
│   ├── raw/                                # Original raw extracts from business systems
│   │   ├── da_fitly_account_info.csv       # Billing & subscription tier accounts (N=400)
│   │   ├── da_fitly_customer_support.csv   # Support tickets & resolution logs (N=918)
│   │   └── da_fitly_user_activity.csv      # Telemetry event-level action stream (N=445)
│   └── processed/                          # Harmonized, cleaned, and engineered datasets
│       ├── account_clean.csv               # Sanitized account data with binary churn target
│       ├── support_clean.csv               # Cleaned support tickets with PII redactions
│       ├── activity_clean.csv              # Typed telemetry events
│       └── fitly_customer_360_clean.csv    # Final 400-row Customer 360 master table (38 cols)
├── figures/                                # Publication-quality charts & visualizations
│   ├── univariate_plan_distribution.png
│   ├── univariate_resolution_time_hist.png
│   ├── multivariate_churn_by_plan.png
│   └── multivariate_engagement_support_vs_churn.png
├── Notebooks/                              # Step-by-step modular analytical notebooks
│   ├── 01_data_audit_and_cleaning.ipynb    # Column-level validation, hygiene & GDPR compliance
│   ├── 02_feature_engineering_and_eda.ipynb# Aggregation, Customer 360 & hypothesis testing
│   └── 03_churn_modeling_and_kpis.ipynb    # Odds ratios, cross-validation & executive KPIs
├── task_to_do/                             # Business prompts, stakeholder emails & grading rubric
│   ├── Practical+-+DA+-+Fitly.pdf          # DataCamp official practical exam brief & rubric
│   ├── da_fitly_product_manager_email.pdf  # Context on business tiers & Pacific timezones
│   ├── da_fitly_lead_engineer_email.pdf    # System downtime, ETL details & missing data notes
│   └── work.docx                           # Project briefing notes
└── README.md                               # Comprehensive project documentation
```

---

## 🔍 Data Validation, Hygiene & GDPR Compliance

Every column across all three raw sources was systematically inspected against business expectations:

| Dataset | Column | Raw Type | Cleaning & Transformation Step | Validation Finding |
| :--- | :--- | :--- | :--- | :--- |
| **`account_info`** | `customer_id` | String (`'C10000'`) | Stripped `'C'` prefix, converted to integer `user_id` | Created single primary key for relational joins across all 3 tables |
| **`account_info`** | `email` | String | Trimmed whitespace, converted to lowercase | Found 27 users with GDPR erasure requests; masked with `[REDACTED_GDPR]` |
| **`account_info`** | `state` | String | Standardized casing and trimmed spaces | All 400 accounts mapped to valid US state codes |
| **`account_info`** | `plan` | String | Lowercased; validated against 4 expected tiers | Four distinct tiers: `free`, `basic`, `pro`, `enterprise` |
| **`account_info`** | `plan_list_price` | Float | Verified positive currency range ($0.00 – $148.00) | 100% consistent with assigned tier pricing structures |
| **`account_info`** | `churn` | String (`'Y'` / `NaN`) | Formulated binary target `is_churned`: `'Y'` $\rightarrow 1$, `NaN` $\rightarrow 0$ | Semi-labeled target: 114 churned (28.5%), 286 active (71.5%) |
| **`customer_support`** | `ticket_time` | String | Parsed to timezone-aware Pacific datetime (`pd.Timestamp`) | Clean temporal continuity; no future-dated tickets |
| **`customer_support`** | `channel` | String | Standardized categorical text values | Channels: Email, Phone, Chat, Social |
| **`customer_support`** | `topic` | String | Standardized topic taxonomy | Issues: Billing, Technical, Account Access, General Inquiry |
| **`customer_support`** | `resolution_time_hours` | Float | Checked for anomalies / negative durations | Range: 0.2 to 48.0 hours; heavily right-skewed; 0 negative records |
| **`customer_support`** | `state` | String | **Renamed to `ticket_state`** to eliminate collision with user geographic `state` | Categories: `closed`, `open`, `pending` |
| **`customer_support`** | `comments` | String (95% NaN) | Text-mined for legal compliance terms | Identified **28 tickets across 27 distinct users** demanding GDPR deletion |
| **`user_activity`** | `event_time` | String | Cast to UTC `pd.Timestamp` | Verified sequential logging order |
| **`user_activity`** | `event_type` | String | Lowercased & deduplicated actions | Verified actions: `watch_video`, `share_workout`, `track_workout`, `read_article` |

### 🛡️ Regulatory Compliance (GDPR Article 17)
During the audit of `customer_support.comments`, **28 tickets across 27 distinct customer accounts** were identified containing explicit demands such as *"Erase my data from your systems"* or *"Requesting right to be forgotten"*.
* Created a privacy flag: `has_gdpr_request = True`.
* Masked all customer PII (email addresses) to `[REDACTED_GDPR]` prior to analytical storage and feature matrix creation.
* Preserved behavioral and financial attributes to maintain statistical integrity without compromising regulatory compliance.

---

## 📊 Exploratory Data Analysis & Statistical Testing

### 1. Univariate Visualizations
* **Plan Tier Distribution:** Free ($N = 105$), Basic ($N = 118$), Pro ($N = 85$), Enterprise ($N = 92$).
* **Support Ticket Resolution Time:** Highly right-skewed distribution ($\mu = 10.3\text{ hrs}$, median $= 6.6\text{ hrs}$ for retained vs. $18.6\text{ hrs}$ for churners).

| Plan Tier Distribution | Ticket Resolution Time (Hours) |
| :---: | :---: |
| ![Plan Distribution](figures/univariate_plan_distribution.png) | ![Resolution Time Distribution](figures/univariate_resolution_time_hist.png) |

---

### 2. Multivariate Visualizations
* **Churn Rate by Plan Tier (with 95% Confidence Intervals):**
  * **Free Plan:** $40.95\%$ churn rate $[31.6\%, 50.8\%]$ — by far the most vulnerable tier.
  * **Enterprise Plan:** $26.09\%$ churn rate $[17.5\%, 36.3\%]$.
  * **Basic Plan:** $23.73\%$ churn rate $[16.5\%, 32.3\%]$.
  * **Pro Plan:** $22.35\%$ churn rate $[14.1\%, 32.7\%]$ — healthiest retention tier.
* **Engagement & Support Friction vs. Churn:**
  * Active users cluster heavily around lower resolution times and positive activity event counts.
  * Churned users dominate the zero-activity threshold and experience high resolution delays.

| Churn Rate by Plan Tier (95% CI) | Support Resolution & Activity vs. Churn |
| :---: | :---: |
| ![Churn by Plan](figures/multivariate_churn_by_plan.png) | ![Multivariate Engagement](figures/multivariate_engagement_support_vs_churn.png) |

---

### 3. Rigorous Statistical Hypothesis Testing

#### A. Chi-Square Test of Independence ($\alpha = 0.05$)
Evaluated categorical association with the binary churn outcome:

| Variable Evaluated | Test Statistic ($\chi^2$) | p-value | Effect Size (Cramér's V) | Statistical Conclusion |
| :--- | :---: | :---: | :---: | :--- |
| **Subscription Plan Tier** | $11.14$ | **$0.01096$** | $0.167$ (Moderate) | **Reject $H_0$** — Churn significantly depends on plan tier. |
| **Support Ticket Presence** | $1.98$ | $0.1592$ | $0.079$ (Weak) | **Fail to Reject $H_0$** — Merely having a ticket does not drive churn. |
| **Platform Activity Presence**| $77.20$ | **$1.51 \times 10^{-18}$** | **$0.439$ (Strong)** | **Reject $H_0$** — Inactivity is intensely associated with churn. |

#### B. Mann-Whitney U Non-Parametric Test
Because engagement and resolution metrics violate normality assumptions, non-parametric tests were run on continuous variables:

| Continuous Metric | Active Median | Churned Median | Mann-Whitney U ($U$) | p-value | Finding |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Plan List Price ($)** | **$27.50** | **$21.00** | $18,742.0$ | **$0.0236$** | Churned accounts skew toward zero/low-cost tiers. |
| **Total Tickets Count** | $2.00$ | $2.00$ | $15,130.5$ | $0.2363$ | Ticket volume alone does not separate churners. |
| **Avg. Resolution Hours** | **$6.61\text{ hrs}$** | **$18.63\text{ hrs}$** | $4,851.0$ | **$1.66 \times 10^{-49}$** | **Nearly 3x longer wait time** among churned users. |
| **Total In-App Events** | **$1.00$** | **$0.00$** | $23,732.0$ | **$9.04 \times 10^{-20}$** | Active users log regular workouts; churners log 0. |
| **Active Days Count** | **$1.00$** | **$0.00$** | $23,657.0$ | **$1.02 \times 10^{-19}$** | Retention is tightly linked to platform frequency. |

---

## 🤖 Statistical & Predictive Churn Driver Modeling

To estimate adjusted effect sizes while controlling for confounders, standardized Multivariable Logistic Regression and Tree models were benchmarked.

```text
=============================================================================================
                    Multivariable Logistic Regression (Statsmodels Logit)
=============================================================================================
Feature Matrix: Standardized (μ = 0, σ = 1) | Reference Plan: Basic Tier
---------------------------------------------------------------------------------------------
Feature                      Coefficient (β)    Odds Ratio (OR)    p-value       95% CI (OR)
---------------------------------------------------------------------------------------------
Intercept                       -1.0772             0.3406         0.0000       [0.23, 0.50]
plan_free                       +0.3429             1.4091         0.0499 *     [1.00, 1.98]
plan_pro                        -0.2319             0.7930         0.2841       [0.52, 1.22]
plan_enterprise                 -0.1803             0.8350         0.4510       [0.51, 1.36]
total_tickets                   +0.2276             1.2556         0.2360       [0.86, 1.83]
avg_resolution_hours            +0.8142             2.2574         0.0001 ***   [1.50, 3.40]
total_events *                 -18.6500             0.0000         0.0000 ***   [Multicollinear]
active_days *                  +17.4200             3.68e7         0.0000 ***   [Multicollinear]
---------------------------------------------------------------------------------------------
* Note: Pearson correlation between total_events and active_days is r = 0.98. Regularized L2
  modeling was implemented to prevent variance inflation and stabilize weights.
```

### 5-Fold Stratified Cross-Validation Benchmark
* **L2 Regularized Logistic Regression:** **$\text{ROC-AUC} = 0.789 \pm 0.079$**
* **Random Forest Classifier:** $\text{ROC-AUC} = 0.721 \pm 0.072$

**Modeling Takeaway:** The regularized parametric logistic model outperforms Random Forest by $+0.068$ ROC-AUC. Given $N = 400$ and strong linear signals (zero-activity cliff, resolution latency, free tier risk), the linear classifier generalizes cleanly without overfitting sparse event tails.

---

## 📈 Executive Business KPIs & Baseline Estimation

To give senior leadership clear operational targets, two complementary churn metrics were calculated alongside operational efficiency baselines:

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                               EXECUTIVE RETENTION DASHBOARD                            │
├──────────────────────────────────────┬─────────────────────────────────────────────────┤
│ Metric Name                          │ Baseline Value & Business Interpretation        │
├──────────────────────────────────────┼─────────────────────────────────────────────────┤
│ 1. Overall Logo Churn Rate           │ 28.50% (114 churned accounts / 400 total)       │
│    (Customer Count Loss)             │ Measures subscriber count attrition.            │
├──────────────────────────────────────┼─────────────────────────────────────────────────┤
│ 2. Revenue Churn Rate (MRR Churn)    │ 25.07% ($4,407 lost MRR / $17,580 total baseline)│
│    (Monetary Value Loss)             │ Measures direct recurring revenue contraction.  │
├──────────────────────────────────────┼─────────────────────────────────────────────────┤
│ 3. Resolution Friction Gap (Median)  │ 6.61 hrs (Active) vs. 18.63 hrs (Churned)       │
│    (Customer Support Latency)        │ Churned users wait 2.8x longer for resolutions. │
├──────────────────────────────────────┼─────────────────────────────────────────────────┤
│ 4. Zero-Activity Customer Ratio      │ 53.75% (215 of 400 users have 0 logged events)  │
│    (Onboarding Engagement Deficit)   │ Accounts for the overwhelming majority of churn │
└──────────────────────────────────────┴─────────────────────────────────────────────────┘
```

### 💡 Logo Churn vs. Revenue Churn Disparity Explained
Notice that **Revenue Churn (25.07%) is lower than Logo Churn (28.50%)**. This occurs because customer attrition is heavily concentrated in the **$0 Free Plan** (which churns at $40.95\%$). While Fit.ly loses nearly $28.5\%$ of its user logos, it successfully retains **$74.93\%$ of its dollar baseline**. 

However, high Free-tier churn cannot be ignored: free users consume significant support capacity ($2.34$ tickets/user) while contributing zero revenue. Furthermore, **Enterprise churn ($26.09\%$)** represents over **$61.7\%$ of total company MRR** ($10,841 out of $17,580), meaning losing even 2–3 Enterprise clients creates acute revenue shocks.

---

## 💡 Key Strategic Insights Gathered

1. **The "Ghost User" Onboarding Cliff (Top Churn Driver):**
   * Over **$53\%$ of all registered accounts have zero recorded activity events** (`watch_video`, `track_workout`, etc.).
   * Platform inactivity is the single most decisive predictor of churn ($p = 1.51 \times 10^{-18}$, Cramér's V $= 0.439$). If a subscriber does not perform a meaningful action during their first week, their probability of churn spikes dramatically.
2. **Resolution Speed Trumps Ticket Volume (Operational Friction):**
   * Total ticket volume alone does not cause churn ($p = 0.236$); rather, **ticket resolution delays** are catastrophic ($p = 1.66 \times 10^{-49}$). Churned subscribers spent an average of **$18.63$ hours** waiting for support resolution compared to **$6.61$ hours** for retained users. Unresolved friction accelerates user abandonment.
3. **The "Free Tier Trap" (Resource Drain):**
   * Customers on the Free tier have **$40.9\%$ higher odds of churning** ($\text{OR} = 1.409, p = 0.0499$) and churn at an overall rate of **$40.95\%$**. These accounts consume an average of $2.34$ support tickets without generating recurring revenue.
4. **Enterprise Tier Vulnerability (Revenue Concentration):**
   * While the Enterprise tier churn rate ($26.09\%$) is comparable to Basic ($23.73\%$), its financial leverage is massive. Enterprise accounts generate **$61.7\%$ of company MRR**. Slow support response times in this cohort directly endanger enterprise renewals.
5. **Regulatory Exposure & Privacy Backlog:**
   * Identifying 27 users with explicit GDPR "Right to be Forgotten" requests reveals an urgent operational gap. Failure to automate data anonymization leaves the company exposed to compliance penalties under GDPR/CCPA.

---

## 🚀 Practical Recommendations for Leadership

```mermaid
flowchart TD
    A[Fit.ly Retention Strategy] --> B[1. Product & Onboarding]
    A --> C[2. Customer Support SLA]
    A --> D[3. Pricing & Tier Structure]
    A --> E[4. Compliance & Operations]

    B --> B1[Interactive Day 1-7 Onboarding Drip]
    B --> B2[Trigger Automated In-App Push at 72h Inactivity]

    C --> C1[Establish 8-Hour Maximum Resolution SLA]
    C --> C2[Priority Support Queue for Enterprise & Pro Tiers]

    D --> D1[Convert Free Tier to 14-Day Free Trial]
    D --> D2[Replace Free Tier Support with AI Chatbot]

    E --> E1[Automated GDPR Deletion Workflow]
    E --> E2[Integrate CRM with Data Warehouse]
```

### 1. Product: Fix the Day 1–7 Inactivity Cliff
* **Implement "First Workout" Milestone Incentives:** Trigger personalized push notifications and in-app onboarding walkthroughs within 24 hours of account creation.
* **Automated Re-Engagement Drip:** If an account registers 0 activity events within 72 hours of signup, automatically send curated 5-minute video workout recommendations.

### 2. Support Operations: Institute an 8-Hour Resolution SLA
* **Tiered Routing & VIP Escalation:** Route all Enterprise ($117+ list price) and Pro tier tickets into an expedited support queue with a strict **< 4-hour resolution guarantee**.
* **Support Capacity Reallocation:** Churned users averaged an unacceptable 18.6 hours to resolution. Tighten SLA workflows across all tiers to cap general resolution under 8 hours.

### 3. Monetization: Restructure the Free Tier
* **Freemium Gating:** Transition the perpetual Free plan into a **14-day full-featured trial** followed by conversion to Basic or automatic feature gating.
* **Deflect Free-Tier Support Costs:** Restrict human email/chat support to paid subscribers. Direct Free users to an automated AI self-service chatbot and community knowledge base.

### 4. Legal & Compliance: Automate Data Erasure Workflows
* Immediately process the 27 identified GDPR erasure requests. Implement automated tagging in the support ticketing system so that any ticket matching privacy keywords automatically triggers PII redaction scripts in the analytical warehouse.

---

## 🔮 Future Work & Engineering Roadmap

* [ ] **Automated ML Early-Warning Churn Pipeline:** Deploy the regularized Logistic Regression scoring model as a daily batch job in Airflow or Databricks, assigning every active subscriber a **Churn Risk Score (0–100%)**.
* [ ] **Automated Marketing Drips via Webhooks:** Connect high Churn Risk Scores ($> 70\%$) directly to customer engagement tools (e.g., Braze, Klaviyo, HubSpot) to trigger retention discounts and check-ins before cancellation.
* [ ] **Survival Analysis (Kaplan-Meier & Cox Proportional Hazards):** Model time-to-churn as a function of subscription tenure to identify the exact month where retention fatigue peaks.
* [ ] **NLP Sentiment Analysis on Support Transcripts:** Run transformer-based sentiment analysis (e.g., RoBERTa / DistilBERT) on ticket comment bodies to automatically detect frustration and negative sentiment prior to ticket closure.
* [ ] **A/B Testing Retention Interventions:** Launch randomized controlled trials (RCTs) testing whether automated onboarding emails or premium support SLAs produce statistically significant churn reductions.
* [ ] **Automated GDPR/CCPA Compliance Gateway:** Build an automated webhook pipeline between Zendesk/Intercom and the data lake to enforce the "Right to be Forgotten" within legal 30-day windows.

---

## 🛠️ Installation & Reproduction Guide

### Prerequisites
* Python 3.10 or higher
* Jupyter Lab or VS Code Jupyter extension

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/fitly-churn-analysis.git
cd fitly-churn-analysis
```

### 2. Set Up a Virtual Environment
```bash
# Create virtual environment
python -m venv venv

# Activate on Windows (PowerShell)
.\venv\Scripts\Activate.ps1

# Activate on macOS / Linux
source venv/bin/activate
```

### 3. Install Dependencies
```bash
pip install pandas numpy scipy scikit-learn statsmodels matplotlib seaborn jupyter
```

### 4. Execute the Notebooks in Order
1. **`Notebooks/01_data_audit_and_cleaning.ipynb`**: Cleans raw sources, redacts PII, harmonizes IDs, and exports clean CSVs to `data/processed/`.
2. **`Notebooks/02_feature_engineering_and_eda.ipynb`**: Builds `fitly_customer_360_clean.csv`, computes univariate/multivariate visualizations, and executes Chi-Square / Mann-Whitney U tests.
3. **`Notebooks/03_churn_modeling_and_kpis.ipynb`**: Performs standardized multivariable logistic regression, evaluates 5-fold cross-validation ROC-AUC, and calculates Logo vs. MRR Churn KPIs.

---

## 📜 Academic Integrity & Certification Rubric Alignment

This repository fulfills all criteria required for the **DataCamp Certified Data Analyst Practical Exam**:
* ✅ **Data Validation & Cleaning:** Complete, column-by-column audit and transformation for every single column in the three datasets.
* ✅ **Exploratory Data Analysis:** Includes two distinct univariate visualizations (bar chart + histogram/KDE) and two multivariate visualizations with rigorous statistical interpretations.
* ✅ **Metric Definition & Baseline:** Clear business metrics defined (Logo Churn Rate, MRR Churn Rate, Support Latency Gap) with exact initial baseline calculations.
* ✅ **Strategic Recommendations:** Actionable, high-impact business advice directly addressing the executive problem statement.

---

## 👨‍💻 Author & Contact

**Data Analyst / Analytics Engineer**  
* 💼 **Project:** Fit.ly Tech Churn Driver & Retention Analysis  
* 🎯 **Certification:** DataCamp Certified Data Analyst  
* 📬 **Inquiries:** Connect on [LinkedIn](https://linkedin.com) or submit a GitHub Issue!

---
*Developed with precision for data-driven executive decision-making.* 🚀
