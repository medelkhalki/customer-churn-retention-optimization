# Customer Churn Prediction & Retention Optimization

An end-to-end Data Science project focused on predicting customer churn and translating model outputs into actionable retention decisions.

---

## Project Overview

Customer churn directly affects recurring revenue, acquisition costs, and long-term customer value.

The objective of this project is to go beyond simple churn prediction and answer a more practical business question:

> **Which customers are most likely to churn, which ones should be prioritized for retention, and what could be the financial impact of a retention campaign?**

The project covers the complete Data Science workflow, from data preparation to model explainability and business-oriented customer prioritization.

---

## Business Objectives

The project aims to:

- Predict customer churn probability.
- Identify the main factors associated with churn.
- Segment customers by risk level.
- Estimate annual revenue at risk.
- Prioritize customers for retention actions.
- Simulate the potential financial return of retention campaigns.

---

## Dataset

The project uses the **Telco Customer Churn** dataset.

- Total customers: **7,043**
- Target variable: **Churn**
- Problem type: **Binary classification**

The dataset includes customer information related to:

- demographics,
- tenure,
- contract type,
- internet and phone services,
- payment method,
- monthly charges,
- total charges.

---

## Project Workflow

The project follows this workflow:

**Raw Data → Data Cleaning → Exploratory Data Analysis → Feature Engineering → Train / Validation / Test Split → Preprocessing → Model Comparison → Threshold Optimization → Final Test Evaluation → SHAP Explainability → Customer Scoring → Risk Segmentation → Revenue at Risk → Retention Prioritization → ROI Simulation**

---

## Data Preparation

The main data-quality steps included:

- checking missing values,
- checking duplicates,
- correcting data types,
- converting `TotalCharges` to numeric,
- handling missing `TotalCharges`,
- validating categorical values,
- preparing the churn target.

Missing `TotalCharges` values were linked to customers with zero tenure, so they were treated as contextually explainable instead of applying an arbitrary statistical imputation.

---

## Feature Engineering

Several business-oriented variables were created to improve the analytical value of the dataset:

- new customer indicator,
- monthly contract indicator,
- automatic payment indicator,
- number of subscribed services,
- binary churn target.

These features were designed to capture customer engagement, commitment, payment behavior, and service adoption.

---

## Train / Validation / Test Strategy

To avoid overestimating performance, the data was split into three independent subsets:

- **Training set:** approximately 70%
- **Validation set:** approximately 15%
- **Final test set:** approximately 15%

The training set was used to learn model parameters.

The validation set was used to compare models and optimize the decision threshold.

The final test set was used only once for the final performance evaluation.

---

## Models Compared

Six classification models were evaluated:

| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| XGBoost | 0.799 | 0.660 | 0.505 | 0.573 | **0.831** |
| Logistic Regression | 0.795 | 0.642 | 0.516 | 0.572 | **0.830** |
| HistGradientBoosting | 0.801 | 0.661 | 0.520 | **0.582** | 0.828 |
| Random Forest | 0.789 | 0.638 | 0.477 | 0.546 | 0.815 |
| SVM | 0.794 | 0.665 | 0.452 | 0.538 | 0.790 |
| KNN | 0.763 | 0.559 | 0.505 | 0.531 | 0.774 |

---

## Final Model

### Logistic Regression

Although XGBoost achieved a slightly higher ROC-AUC on the validation set, the difference was negligible.

Logistic Regression was selected as the final model because it offered:

- competitive predictive performance,
- greater interpretability,
- easier communication with business stakeholders,
- transparent coefficients,
- straightforward integration with SHAP explanations.

The choice therefore reflects a balance between **performance and interpretability**.

---

## Threshold Optimization

The default classification threshold of 0.50 was not automatically retained.

Several thresholds were evaluated on the validation set.

The final selected threshold was:

**0.30**

This lower threshold increased the ability to detect customers who actually churned, which is particularly important in a retention context where missing an at-risk customer may be costly.

---

## Final Test Performance

The final Logistic Regression model was evaluated on previously unseen test data.

| Metric | Final Result |
|---|---:|
| Accuracy | **75.6%** |
| Precision | **52.7%** |
| Recall | **76.8%** |
| F1-score | **62.5%** |
| ROC-AUC | **85.0%** |

The final confusion matrix showed:

- **584** true negatives
- **193** false positives
- **65** false negatives
- **215** true positives

The model successfully detected approximately **76.8% of actual churners**.

---

## Model Explainability

Model explainability was an important part of the project.

Two approaches were used:

### Logistic Regression Coefficients

The coefficients helped identify variables associated with higher or lower churn risk.

### SHAP

SHAP was used to understand:

- global feature importance,
- the direction of each feature's impact,
- individual customer predictions.

Important churn-related signals included:

- customer tenure,
- contract type,
- internet service,
- monthly charges,
- total charges.

These relationships should be interpreted as model associations, not causal effects.

---

## Customer Scoring

The final model assigns a churn probability to each customer in the final test population.

These probabilities were converted into three risk levels:

| Risk Level | Probability Range |
|---|---|
| Low | 0% to 30% |
| Medium | 30% to 60% |
| High | 60% to 100% |

Final test risk distribution:

| Risk Level | Customers |
|---|---:|
| Low | **649** |
| Medium | **244** |
| High | **164** |

---

## Revenue at Risk

To connect churn predictions with financial impact, a simplified annual customer revenue estimate was used.

**Estimated Annual Revenue = Monthly Charges × 12**

Then:

**Revenue at Risk = Estimated Annual Revenue × Churn Probability**

This metric combines:

- customer financial value,
- churn probability.

It helps identify customers who are both likely to churn and financially important.

This is a simplified annual revenue proxy and not a full Customer Lifetime Value model.

---

## Retention Prioritization

Customers were prioritized using both churn probability and revenue at risk.

A high-priority customer is therefore not only likely to churn, but also represents significant financial exposure.

This approach helps retention teams focus resources on customers where intervention may have the greatest potential impact.

---

## Retention Campaign Simulation

A business simulation was created using the following assumptions:

- Contact cost: **10**
- Retention offer cost: **50**
- Total campaign cost per targeted customer: **60**

Three retention-success scenarios were evaluated:

| Scenario | Success Rate |
|---|---:|
| Pessimistic | 10% |
| Realistic | 30% |
| Optimistic | 50% |

---

## ROI Simulation

The simulation produced the following results:

| Scenario | Success Rate | Total Cost | Estimated Saved Revenue | Estimated Net Gain | Estimated ROI |
|---|---:|---:|---:|---:|---:|
| Pessimistic | 10% | 3,780 | 5,162.30 | 1,382.30 | **36.57%** |
| Realistic | 30% | 3,780 | 15,486.89 | 11,706.89 | **309.71%** |
| Optimistic | 50% | 3,780 | 25,811.49 | 22,031.49 | **582.84%** |

> These ROI values are simulated estimates based on explicit assumptions and are not observed production results.

---

## Business Value

The main value of the project is that it connects Machine Learning predictions with business decision-making.

The workflow moves from:

**Churn Prediction → Risk Segmentation → Financial Exposure → Retention Priority → Campaign Simulation**

Potential business applications include:

- early identification of at-risk customers,
- more targeted retention campaigns,
- better allocation of commercial resources,
- prioritization of high-value customers,
- estimation of potential financial exposure.

---

## Key Visualizations

The project includes the following key visualizations:

1. Project workflow / pipeline
2. Model comparison by ROC-AUC
3. ROC curve on the final test set
4. Confusion matrix
5. SHAP summary plot
6. Customer risk distribution
7. Top customers by revenue at risk
8. ROI comparison by scenario

---

## Technologies Used

- Python
- Pandas
- NumPy
- Scikit-learn
- XGBoost
- SHAP
- Matplotlib
- SciPy
- Jupyter Notebook

---

## Repository Structure

- `D_Science_Final.ipynb` — complete analysis and modeling notebook
- `README.md` — project documentation
- `requirements.txt` — project dependencies
- `data/` — dataset
- `outputs/` — scoring results and figures

---

## Limitations

Several limitations should be considered:

- The dataset is static and does not capture concept drift.
- No temporal validation was possible with the available data.
- Campaign costs are assumed rather than observed.
- Retention success rates are simulated.
- ROI values are illustrative rather than realized.
- The model was not deployed to production.
- Revenue at risk is based on a simplified annual revenue estimate.
- Model explanations show statistical associations, not causal relationships.

---

## Future Improvements

Possible future improvements include:

- probability calibration,
- temporal validation,
- Customer Lifetime Value modeling,
- cost-sensitive threshold optimization,
- uplift modeling,
- model deployment through an API,
- drift monitoring,
- CRM integration,
- A/B testing of retention strategies.

---

## Conclusion

This project demonstrates how a churn prediction model can be extended into a complete decision-support workflow.

The final Logistic Regression model achieved:

- **ROC-AUC: 0.850**
- **Recall: 76.8%**
- **F1-score: 62.5%**

More importantly, the project goes beyond model performance by connecting churn probability with customer value, financial exposure, retention prioritization, and campaign ROI simulation.

The result is an end-to-end Data Science project that combines:

**Machine Learning + Explainability + Business Prioritization + Financial Impact**
