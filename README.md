# Telecom_customer_churn_analysis
A business analyst project analyzing customer churn patterns for a telecom company, identifying key churn drivers, quantifying revenue impact, and delivering actionable retention recommendations.

## Problem Statement

The company is losing customers at a rate of 26.5%, directly impacting recurring revenue. This project investigates why customers churn, which segments are highest risk, and how much revenue is at stake — translating raw data into business recommendations.

## Dataset

- *Source:* Telco Customer Churn dataset (Kaggle)
- *Size:* 7,043 customers, 21 features
- *Features:* Demographics (gender, senior citizen, partner, dependents), account info (tenure, contract, payment method), services subscribed (internet, streaming, tech support), and billing (monthly/total charges)

## Methodology

1. *Data Cleaning*
   - Converted TotalChargesnew from object to numeric
   - Diagnosed 11 missing values as structurally tied to customers with tenure = 0 (new, unbilled customers) — imputed with 0 instead of blind mean/median imputation
   - Standardized redundant categories (e.g., "No internet service" → "No") across service columns

2. *Exploratory Data Analysis*
   - Univariate analysis: distribution of tenure, monthly charges, total charges, churn rate
   - Bivariate analysis: churn rate by tenure, contract type, payment method, gender, senior citizen status, and internet service
   - Correlation analysis across numerical features

3. *Segment-Level Risk Analysis*
   - Multi-factor segmentation (Contract × Internet Service × Payment Method) to identify compounding risk factors
   - Filtered for statistically meaningful segment sizes (≥30 customers)

4. *Revenue Impact Quantification*
   - Calculated monthly and annualized revenue lost to churn
   - Broke down revenue loss by contract type to prioritize retention budget

5. *Predictive Modeling*
   - Logistic Regression with class_weight='balanced' to address class imbalance
   - Evaluated using classification report (precision/recall/F1) and ROC-AUC rather than accuracy alone, given the imbalanced target

## Key Findings

- *Overall churn rate:* 26.5%
- *Tenure is the strongest churn indicator:* churn drops from 47.4% (0–12 months) to 25.5% (13–36 months) to 11.9% (37+ months), even as average monthly charges increase with tenure — showing price alone doesn't drive churn; early-stage dissatisfaction does
- *Highest-risk segment:* Month-to-month contract + Fiber optic internet + Electronic check payment → *60.37% churn rate* across 1,307 customers (more than 2x the overall average)
- *Senior citizens churn at ~42%*, nearly double the ~24% rate of non-senior citizens
- *Electronic check users churn at ~45%*, roughly 3x the rate of automatic payment methods
- *Revenue at risk:* ₹1,39,130.85 in monthly recurring revenue (30.5% of total monthly revenue), an estimated *₹16.7 lakh annually*

## Model Performance

| Metric | Class 0 (No Churn) | Class 1 (Churn) |
|---|---|---|
| Precision | 0.90 | 0.50 |
| Recall | 0.72 | 0.79 |
| F1-score | 0.80 | 0.62 |

*ROC-AUC: 0.838*

class_weight='balanced' was used instead of SMOTE, since the dataset is predominantly categorical — SMOTE's interpolation between minority-class samples is less meaningful for categorical features than for continuous numerical ones. Recall was prioritized over precision, since missing an actual churner is costlier to the business than a false alarm.

## Business Recommendations

1. *Contract upgrades:* Incentivize month-to-month customers to switch to annual contracts, especially within their first 6 months
2. *Early-tenure onboarding:* Strengthen onboarding and proactive check-ins in the first 12 months, where churn risk is highest regardless of price tier
3. *Autopay adoption:* Encourage Electronic check users to switch to automatic payment methods via small incentives
4. *Targeted retention campaign:* Prioritize the compound high-risk segment (month-to-month + fiber optic + electronic check) rather than broad, undifferentiated offers
5. *Senior citizen support:* Investigate simplified plans or dedicated support for senior citizens, given their significantly higher churn rate.

