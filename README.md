# E-Commerce Customer Churn Analytics & Strategic Retention System

[](https://www.python.org/downloads/)
[](https://colab.research.google.com/)
[](https://opensource.org/licenses/MIT)
[](#)

An enterprise-grade customer churn analysis, behavioral diagnostics, and predictive retention decision-support framework built on 50,000 multi-market e-commerce customer records across 25 features.

---

## 📌 Executive Summary & Key Results

* **Customer Base & Churn Scale:** 50,000 customer accounts analyzed; **14,450 churned accounts** representing an overall platform churn rate of **28.90%**.
* **Financial Exposure:** Total platform Customer Lifetime Value (LTV) is **$72.03M**, with **$20.60M (28.60%) lost to churn**.
* **Asymmetric Value Concentration:** **62.6% ($12.90M) of all lost LTV** originates from just **4,956 accounts in the top 25% LTV tier (> $1,874)**—representing only 9.9% of the total customer population.
* **The "6-Call Friction Cliff":** Churn rate remains low and stable at $\le 5$ support calls (12.0%–16.1%), but jumps discontinuously to **40.6% at 6–8 calls** and **48.2% at 9+ calls** ($p < 10^{-300}$), pinpointing repeat tickets as an escalation failure loop.
* **The 60-Day Forgetting Curve:** Inactivity recency drives a monotonic surge in churn from **23.4% (<14 days)** to **38.7% (61–90 days)** and **66.4% (>180 days)**.
* **Cost-Optimal Predictive Modeling:** Standard Logistic Regression achieves a **ROC-AUC of 0.791**. Lowering the classification threshold from $0.50$ to **$0.30$** expands churner recall from **40.0% to 72.1%**, capturing 80% more at-risk accounts to maximize campaign ROI.
* **Financial ROI Sensitivity:** Targeting the 4,956 high-value at-risk accounts with a $20/contact bespoke concierge retention program yields an expected **18.5× Net ROI ($1.84M net preserved value)** under base-case assumptions (15% retention rate).

---

## 📂 Repository Structure

```text
├── eCommerce-dataset.csv                       # Raw customer transaction dataset (50,000 rows, 25 cols)
├── ecommerce_customer_churn_analysis.ipynb     # Complete end-to-end analytical & predictive notebook
├── ecommerce_customer_churn_business_brief.md  # Original business requirements and analytical brief
└── README.md                                   # Project documentation and strategic summary
```

---

## 🔬 Analytical Framework & Architecture

The analysis is structured around an end-to-end decision-support pipeline:

```text
BUSINESS PROBLEM
       ↓
DATA UNDERSTANDING & DICTIONARY (25 Features mapped across 7 business domains)
       ↓
DATA QUALITY ASSESSMENT (Duplicates, Missingness, Informative Signals, Boundaries)
       ↓
CLEANING PIPELINE (df -> df_clean, lineage preservation, feature engineering)
       ↓
EXPLORATORY CUSTOMER BASE ANALYSIS (Demographics, Geography, Tenure, Baskets)
       ↓
CHURN PATTERN DECOMPOSITION (Demographics, Geography, Recency, Friction)
       ↓
BEHAVIORAL COMPARISON MATRIX (Retained vs. Churned median divergence profiles)
       ↓
CUSTOMER LIFETIME VALUE EXPOSURE ANALYSIS (Top quartile LTV vulnerability)
       ↓
RFM SEGMENTATION ENGINE (Quintile scoring: R, F, M -> 7 Strategic Segments)
       ↓
MULTIVARIATE CHURN DRIVERS & MODELING (ROC-AUC 0.791, Cost-Optimal Threshold)
       ↓
RISK × VALUE PRIORITIZATION (2x2 Strategic Matrix: Quadrants 1 to 4)
       ↓
ACTIONABLE RECOMMENDATIONS & SCENARIO MODELING (P1-P3, Bear/Base/Bull ROI)
       ↓
EXECUTIVE SUMMARY & CONTINUOUS KPI GOVERNANCE (12 Metrics across 4 Pillars)
```

---

## 🛠️ Data Quality Engineering & Cleaning Decisions

| Issue / Observation | Detection | Treatment / Transformation | Analytical Rationale |
|---|---|---|---|
| **Informative Missingness** | `Session_Duration_Avg` (6.8% null), `Email_Open_Rate` (5.1% null) | Created binary flags (`Missing_*`) + median imputation | Churn rate among missing records is ~41% vs. 28% baseline; missingness reflects telemetry dropouts/app uninstalls. |
| **Non-Informative Missingness** | `Wishlist`, `Returns`, `Credit_Balance`, `Recency` | Median imputation | Churn distribution between observed and missing is identical (~28.9%); median protects against skewness. |
| **Invalid Biological Age** | 30 records < 18, 20 records > 100 (up to 200) | Imputed with valid median (38 yrs); categorized into `Age_Group` | Removes impossible typographical artifacts while retaining the customer record. |
| **Percentage Ceiling Violations** | 30 (`Cart_Abandonment`), 207 (`Discount_Usage`) > 100% | Right-capped at 100.0% | Restores mathematical percentage boundary while preserving extreme intensity. |
| **Negative Transaction Frequency** | 40 records with `Total_Purchases` < 0 | Left-clipped at 0.0 purchases | Resolves net accounting return adjustments without deleting customer entities. |
| **Extreme Basket Values** | 25 records with `Average_Order_Value` > $1,000 (up to $9,666) | Retained intact | Represents legitimate high-ticket/wholesale accounts essential for accurate LTV exposure sizing. |

---

## 🎯 Behavioral RFM Segmentation

Using Quintile scoring ($1–5$) across Recency ($R$), Frequency ($F$), and Monetary ($M$):

| Segment Name | Customer Count | % Base | Churn Rate | Avg LTV | Total Portfolio LTV | Lost Churned LTV | Strategic Priority |
|---|---:|---:|---:|---:|---:|---:|---|
| **Champions** | 7,048 | 14.1% | 28.4% | $2,372 | $16.72M | $5.14M | Loyalty & Advocacy |
| **Can Not Lose Them (VIP At-Risk)** | 6,758 | 13.5% | 29.4% | $2,361 | $15.96M | $5.04M | **P1 Critical Retention** |
| **Loyal Customers** | 9,914 | 19.8% | 19.3% | $1,693 | $16.78M | $3.93M | Value Protection |
| **At Risk** | 6,677 | 13.4% | 23.1% | $1,278 | $8.53M | $2.15M | Targeted Activation |
| **Recent Active / Potential** | 9,015 | 18.0% | 25.0% | $751 | $6.77M | $1.44M | Onboarding & Upsell |
| **Need Attention** | 4,055 | 8.1% | 32.8% | $748 | $3.03M | $0.86M | Incentive Calibration |
| **Hibernating / Lost** | 6,533 | 13.1% | 52.4% | $648 | $4.23M | $2.03M | Automated Low-Cost Win-Back |

---

## ⚖️ Strategic Risk × Value Prioritization Matrix

To avoid margin-diluting blanket discounts, customers are mapped into four distinct operational quadrants:

```text
                     HIGH CHURN RISK
                           ↑
                           |
            P3: MEDIUM     |    P1: CRITICAL
         Low-Mid Value     |    High Value (Top 25%)
         9,494 Customers   |    4,956 Customers
         $7.69M Exposure   |    $12.90M Exposure
         Automated Win-Back|    VIP Concierge Unit
                           |
    -----------------------+----------------------→ CUSTOMER VALUE
                           |
            P4: LOW        |    P2: HIGH
         Low-Mid Value     |    High Value (Top 25%)
         28,006 Customers  |    7,544 Customers
         $30.70M Base      |    $20.73M Base
         Standard Nurture  |    Loyalty & NPS Defense
                           |
                           ↓
                     LOW CHURN RISK
```

---

## 📈 Strategic Recommendations & ROI Sensitivity

### Prioritized Action Plan (P1–P3)
1. **P1 — Customer Service Circuit-Breaker:** Trigger an immediate routing rule to Senior Resolution Specialists upon a customer lodging their **4th support ticket**, armed with instant refund/appeasement authority before the 6-call cliff is reached.
2. **P1 — VIP Concierge Retention Unit:** Dedicated high-touch retention outreach for the 4,956 accounts in Quadrant 1 ($12.90M exposure) utilizing customized account managers, priority fulfillment, and bespoke perks.
3. **P1 — Automated Inactivity Lifeline:** Automated multi-channel triggers deploying between **Days 30 and 45 post-purchase** before cognitive habit decay solidifies.
4. **P2 — Dynamic Cart Recovery Engine:** Real-time exit-intent friction removal and automated shipping fee-waivers for cart abandonment sessions exceeding 65%.
5. **P2 — Youth Cohort (18–24) Lifecycle Overhaul:** Redesign mobile-first onboarding, introduce BNPL payment options, and deploy tiered gamified loyalty to address the **48.38% youth churn rate**.

### VIP Retention Campaign Financial Sensitivity Modeling

Target Population: **4,956 High-Value Accounts at Risk** | Total Exposure: **$12,904,219** | Intervention Cost: **$20.00 / contact** ($99,120 campaign budget)

| Scenario | Retention Success Rate | Retained VIP Accounts | Gross Preserved LTV | Campaign Cost | Net Value Created | Net ROI Multiple |
|---|---:|---:|---:|---:|---:|---:|
| **Bear Case (Pessimistic)** | 8.0% | 396 | $1,031,089 | $99,120 | $931,969 | **9.4×** |
| **Base Case (Expected)** | 15.0% | 743 | $1,934,594 | $99,120 | $1,835,474 | **18.5×** |
| **Bull Case (Optimistic)** | 25.0% | 1,239 | $3,226,059 | $99,120 | $3,126,939 | **31.5×** |

---

## 🚦 KPI Governance Framework

Continuous operational monitoring across four key pillars:

* **Customer Health:** Platform Churn Rate ($\le 22.0\%$), VIP Tier Churn Rate ($\le 15.0\%$), Reactivation Rate ($\ge 18.0\%$).
* **Service Friction:** Repeat Escalation Rate ($\ge 4$ calls $\le 8.0\%$), First Contact Resolution ($\ge 78.0\%$).
* **Engagement & Conversion:** Monthly Login Frequency ($\ge 12.0$ sessions), Email Open Rate ($\ge 24.0\%$), Cart Abandonment Rate ($\le 55.0\%$).
* **Economic Value:** Net Preserved LTV from P1 Interventions ($\ge \$1.5\text{M} / \text{quarter}$), Annual Order Frequency ($\ge 14.0\text{ orders/yr}$).

---

## 🚀 How to Run the Notebook

### Option A: Google Colab (Recommended)
1. Upload `ecommerce_customer_churn_analysis.ipynb` to [Google Colab](https://colab.research.google.com/).
2. Place `eCommerce-dataset.csv` in your Colab session root (`/content/eCommerce-dataset.csv`) or mount Google Drive.
3. Select **Runtime > Run all** (`Ctrl + F9`). The notebook includes automated environment detection and will execute end-to-end without manual intervention.

### Option B: Local Environment
Ensure you have Python 3.10+ installed with the following packages:
```bash
pip install pandas numpy matplotlib seaborn scipy
```
Launch JupyterLab or Notebook:
```bash
jupyter lab ecommerce_customer_churn_analysis.ipynb
```

---

## 📄 License & Attribution

This project is released under the [MIT License](https://www.google.com/search?q=LICENSE). Built for enterprise analytics teams, product leaders, and marketing strategists seeking data-driven customer retention modeling.
