#  Customer Churn Prediction for Subscription Business

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.8+-blue.svg" alt="Python">
  <img src="https://img.shields.io/badge/scikit--learn-1.0+-orange.svg" alt="scikit-learn">
  <img src="https://img.shields.io/badge/Status-Production%20Ready-green.svg" alt="Status">
</p>

##  Business Problem

Subscription-based businesses face a critical challenge: **customer churn**. Acquiring new customers costs 5-25x more than retaining existing ones, making churn reduction a top priority for maximizing profitability.

**Challenge:** Identify customers at high risk of canceling their subscription 30 days in advance to enable targeted retention campaigns.

**Goal:** Optimize retention spend by balancing campaign costs against customer lifetime value (CLV), maximizing net business impact.

---

##  Business Impact

| Metric | Value |
|--------|-------|
| **Model ROC-AUC** | 0.977 |
| **Optimal Precision** | 84% |
| **Optimal Recall** | 92% |
| **Projected Annual Net Benefit** | Rs7,121,000 for 7,000 customer base |
| **Customers Retained** | 148 |
| **ROI** | 22:1 (₹22 return per ₹1 spent on campaigns) |

### Key Insights
- **Month-to-month contract holders** are 2.33x more likely to churn than annual contract customers
- **New customers (0-12 months)** have 3.04x higher churn risk than loyal customers (36+ months)
- **Low engagement customers** (0-1 active services) churn at 1.58x the rate of high engagement customers (4-6 services)

---

##  Features

### Technical Capabilities
-  **Advanced Feature Engineering** - 7 business-relevant features extracted from transactional data
-  **Imbalanced Data Handling** - SMOTE oversampling for minority class
-  **Cost-Benefit Optimization** - Decision threshold tuned for business economics, not just accuracy
-  **Customer Segmentation** - High-risk groups identified for targeted marketing
-  **Production Ready** - Serialized model artifacts for deployment
-  **Comprehensive Evaluation** - ROC-AUC, precision-recall curves, confusion matrix, feature importance

### Business Features
-  Dynamic decision threshold based on campaign costs and CLV
-  Top 100 high-risk customer export for immediate action
-  Segment-specific churn drivers for personalized retention strategies
-  Executive-ready visualizations and business case

---


##  Methodology

### 1. Feature Engineering

Created 7 business-relevant features from raw transaction data:

| Feature | Description | Business Rationale |
|---------|-------------|-------------------|
| **Engagement Score** | Count of active services (0-6) | Low engagement signals weak product-market fit |
| **Avg Monthly Spend** | Total charges / tenure | Identifies high-value customers worth retaining |
| **Tenure Category** | New / Mid / Loyal customer lifecycle stage | Recent customers have higher churn risk |
| **Contract Risk** | Binary flag for month-to-month contracts | No commitment = higher churn likelihood |
| **Payment Risk** | Binary flag for electronic check payments | Less stable payment method |
| **Service Density** | Services per dollar spent | Value perception metric |
| **Revenue Velocity** | Spending rate over time | Rapid spenders may have buyer's remorse |

### 2. Model Selection

**Algorithm:** Random Forest Classifier

**Why Random Forest?**
- Handles non-linear relationships between features
- Robust to outliers and missing data
- Provides feature importance for business interpretability
- Strong baseline performance without extensive hyperparameter tuning

**Alternative models tested:** Logistic Regression (0.82 AUC), XGBoost (0.86 AUC)

### 3. Handling Class Imbalance

**Problem:** Only 26.5% of customers churn (imbalanced dataset)

**Solution:** SMOTE (Synthetic Minority Oversampling Technique)
- Generates synthetic samples of minority class (churners)
- Balances class distribution to 50-50 in training set
- Prevents model from defaulting to "predict no churn" for everyone


### 4. Model Evaluation

Standard metrics (accuracy) are misleading for imbalanced data. We use:

- **ROC-AUC:** Overall model discrimination ability
- **Precision:** Of customers we contact, how many actually would churn? (reduces wasted spend)
- **Recall:** Of customers who will churn, how many do we catch? (maximizes retained value)
- **Confusion Matrix:** Full picture of TP, FP, TN, FN

### 5. Cost-Benefit Analysis

**Business Economics:**
- Retention campaign cost: ₹500 per customer contacted
- Customer Lifetime Value (CLV): ₹50,000 per customer
- Campaign success rate: 40% (industry benchmark)

**Optimization:**
Instead of using default threshold (0.5), we find the threshold that maximizes:

```
Net Benefit = (True Positives × Success Rate × CLV) - (All Contacted × Campaign Cost)
```

**Result:** Optimal threshold = 0.38 (more aggressive than default)

---

##  Results

### Model Performance

| Metric | Value |
|--------|-------|
| ROC-AUC | 0.977 |
| Precision (at optimal threshold) | 58.3% |
| Recall (at optimal threshold) | 99% |
| F1-Score | 0.88 |
| Cross-Validation ROC-AUC | 0.988 ± 0.007 |



### Business Impact (7,000 Customer Base(Assumed)

**Without Model (Reactive Approach):**
- Lose ~1,850 customers/year (26.5% churn rate)
- 
**With Model (Proactive Retention):**
- Contact 638 customers out of 1407
- Campaign cost:  Rs319,000
- Retain ~ 148 customers (40% of 1,400)
- Value retained: Rs37,014,925
- **Net benefit: Rs35,427,861** (after campaign costs)
- **ROI: 22:1**

---




---

##  Acknowledgments

- **Dataset:** [IBM Telco Customer Churn](https://www.kaggle.com/datasets/blastchar/telco-customer-churn) (Kaggle)

---
