# 🛡️ Credit Card Fraud Detection: Data Engineering & Behavioral Analytics

This repository houses an end-to-end data engineering pipeline and a behavioral analytics framework designed to detect fraudulent credit card transactions. By processing over 1.29 million transactions and enforcing strict data quality checks, this project uncovers subtle fraud signals that traditional methods often miss, enabling early detection and reducing financial losses.

---

## 🏗️ Data Engineering Architecture

The pipeline transforms raw transactional data into analytics-ready tables, with automated data quality validation acting as a strict gatekeeper before data enters the warehouse.

**Pipeline Flow:**
`Raw Data` ➔ `Pandas Transformation` ➔ `Great Expectations Validation` ➔ `Data Warehouse` ➔ `BI / Analytics`

### 1. Ingestion (Raw Layer)

* **Objective:** Ingest source data in its original, unmodified format for auditability.
* **Storage:** `data/raw/transaction_raw.csv`
* **Rationale:** Preserves the original schema and allows for easy pipeline reprocessing.

### 2. Transformation (Processed Layer)

* **Objective:** Clean, standardize, and model the data into analytical fact tables.
* **Tool:** `pandas`
* **Operations:** Data type casting, missing value handling, deduplication, business rule validation, and feature standardization.
* **Storage:** `data/processed/fact_transaction.csv`

### 3. Data Quality Validation

* **Objective:** Ensure processed data meets predefined business rules before downstream consumption.
* **Tool:** `Great Expectations`
* **Process:** Validates uniqueness, null constraints, numeric ranges, and boolean fields.
* **Gatekeeper:** The pipeline operates on a "fail-fast" principle; if validation fails, the pipeline halts immediately, preventing bad data from entering the warehouse.

### 4. Orchestration

* **Objective:** Automate and monitor pipeline execution.
* **Tool:** `Apache Airflow`
* **Task Flow:** `extract` ➔ `transform` ➔ `validate` ➔ `load_to_dwh`

---

## 💻 Technology Stack

| Layer | Technology |
| --- | --- |
| **Ingestion & Processing** | Python, pandas |
| **Data Quality** | Great Expectations |
| **Orchestration** | Apache Airflow |
| **Storage** | CSV, Data Warehouse |
| **Visualization** | Matplotlib, Seaborn, Looker Studio |

---

## 📊 Analytics & Key Findings

We conducted exploratory data analysis (EDA) on a 2019-2020 fiscal year dataset containing 1,296,675 transactions across 983 users.

**High-Level Metrics:**

| Metric | Value |
| --- | --- |
| **Total Transaction Value** | $91.2 Million |
| **Overall Fraud Rate** | 0.58% |
| **Users Involved in Fraud** | 77.52% (762 / 983) |

### Crucial Behavioral Signals

**1. Fraud is Habitual, Not Isolated**
Fraud is highly concentrated at the user level. The average fraudulent user commits roughly 10 fraudulent transactions. Detection must focus on user-level behavioral patterns rather than isolated single transactions.

**2. Calculated Evasion in Transaction Amounts**
Fraudulent transactions are typically 7-8x higher in value than legitimate ones (Median: $396.51 vs. $47.28). However, fraudsters actively avoid extreme maximums to bypass automated blocking thresholds.

**3. The Late-Night Vulnerability Window**
Temporal analysis reveals a massive spike in fraudulent activity late at night. Approximately **51% of all fraud** occurs within a two-hour window between 10:00 PM and 12:00 AM.

**4. Age as a Vulnerability Proxy**
Prime-age groups (26-50) exhibit the lowest fraud rates (~0.47%). Conversely, users aged 50+ record the highest fraud rate (0.74%), likely due to susceptibility to social engineering and scams.

---

## 🚨 Rule-Based Detection Strategy

To mitigate fraud efficiently while managing operational costs (e.g., OTP SMS budgets capped at 20% of transaction volume), we implemented targeted, rule-based interventions.

**Selected Strategy: High-Risk Demographic Targeting**
Targeting the oldest user demographic (Ages 61-95) provided the highest optimization of our security budget.

* **Coverage:** ~19.65% of transactions (Fits within 20% budget)
* **Fraud Uplift:** 24.84% improvement in detection

```python
# Demographic OTP Trigger Logic
def check_demographic_risk(user_age):
    if 61 <= user_age <= 95:
        trigger_otp_verification()

```

---

## 🚀 Next Steps: Machine Learning Integration

While rule-based systems are effective for immediate mitigation, our long-term strategy involves transitioning to Machine Learning to capture non-linear fraud patterns.

### 1. Multi-Signal Rule Enhancement (Immediate)

Combining temporal and demographic rules to flag distinct risk tiers.

```python
def evaluate_transaction_risk(user_age, transaction_timestamp):
    age_risk = user_age >= 61
    time_risk = 22 <= transaction_timestamp.hour <= 23
    
    if age_risk and time_risk:
        return "HIGH_RISK"
    elif age_risk or time_risk:
        return "MEDIUM_RISK"
    return "LOW_RISK"

```

### 2. Predictive Modeling (Long-Term)

We plan to utilize `XGBoost` or `Random Forest` to handle the severe class imbalance (0.58% fraud rate).

**Planned Feature Engineering:**

* `user_fraud_count_historical`
* `avg_transaction_amount_last_30_days`
* `merchant_fraud_rate`
* `time_since_last_transaction_minutes`

**Target Evaluation Metrics:** Precision, Recall, F1-Score, and overall Fraud Uplift %.

## 🔗 Resources
Documentation
GitHub Repository
Interactive Dashboard

## 🤝 Contributing
This project is part of the CODA RMT 013 data engineering and analytics portfolio. For questions or collaboration:

Author: Arief Bagus Nugraha, Paulus Marpaung, Dhias Renaldy, Nabilah Astiarini, Sinta Ahwalisa
LinkedIn:: linkedin.com/in/ariefbn13
LinkedIn:: linkedin.com/in/withpaulusmarpaung
LinkedIn:: linkedin.com/in/dhiasrenaldy)
LinkedIn: linkedin.com/in/sintaahwalisa
LinkedIn: linkedin.com/in/nabilahastiarini