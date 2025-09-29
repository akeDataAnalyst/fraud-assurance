# Mobile Money Fraud & Leakage Assurance System

## Project Overview

This project delivers a high-recall machine learning system to detect fraud (SIM Swap/Account Takeover, Agent Collusion) and financial leakage (system errors) within the mobile money platform. The primary goal was to minimize **False Negatives (missed fraud)**, creating a critical safety net for the Assurance department.

* **Model Used:** Class-Weighted Logistic Regression (Chosen for interpretability).
* **Key Result (Recall):** $\approx 99\%$. The model successfully catches virtually all fraud cases, minimizing financial loss.
* **Performance (AUC-ROC):** $0.9805$ (Excellent predictive power).

---

## Key Features & Actionable Insights

The model's success is driven by custom engineered features and the interpretation of their risk impact (Odds Ratios).

### Engineered Risk Factors:
1.  **Txn\_Count\_Sender\_1H:** (Velocity/SIM Swap Risk) Counts transactions by the Sender ID in the last 60 minutes.
2.  **Agent\_Cust\_Pair\_Count\_7D:** (Collusion Risk) Tracks repeated transactions between the same Agent/Customer pair.

### Top Strategic Recommendations:

The model provides clear rules for where to focus security controls:

1.  **Critical Risk: P2P Transfers**
    * **Impact:** Odds of fraud are $\mathbf{13,249}$ times higher.
    * **Action:** Implement mandatory 2FA/step-up authentication for high-value P2P transactions.

2.  **Engineering Priority: System Leakage**
    * **Risk Factor:** `Billing_Status_TIMEOUT`
    * **Impact:** $\mathbf{51.5\%}$ increase in risk.
    * **Action:** Launch urgent investigation to stabilize the billing system, as timeouts are a quantifiable source of leakage.

3.  **Value-Based Control**
    * **Risk Factor:** `Amount_ETB`
    * **Impact:** Odds increase $\mathbf{4.2}$ times for every standard unit increase.
    * **Action:** Institute dynamic velocity limits based on transaction value.

---

## Project Files

* **Trained Model:** `logistic_regression_fraud_model.joblib`
* **Fitted Scaler:** `standard_scaler.jobjob` (Essential for scoring new data)
* **Analysis:** `final_odds_ratio_analysis.csv` (The final list of ranked risk drivers)
* **Documentation:** `[notebooks]` containing all EDA, feature engineering, and training steps.

### Operational Strategy

The model is optimized for detection over efficiency. All transactions scoring $\mathbf{\geq 0.5}$ must be routed to a Manual Analyst Review Queue. Automatic blocking is not recommended due to the high False Positive rate.
