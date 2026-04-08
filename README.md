# Coefficient Stability in Credit Default Models

### Overview
This project examines how correlated predictors affect both predictive performance and coefficient interpretation in credit default models. Financial variables (e.g., credit limits, balances) and behavioral variables (e.g., repayment history) are not independent signals; they encode overlapping information about borrower risk.

The analysis focuses on how this overlap alters model behavior, rather than on optimizing predictive accuracy alone. In particular, it studies how coefficients change across specifications and what those changes imply about the underlying structure of information.

### Questions
- Do financial variables retain explanatory power after controlling for repayment behavior?
- Can repayment history be represented using lower-dimensional summary features?
- How do coefficient estimates change across model specifications?
- What does “variable importance” mean when predictors are correlated?

### Data
- Source: UCI Default of Credit Card Clients
- Observations: 30,000
- Target: Default in the following month (binary)

##### Feature groups:
- Financial variables: credit limit, bill amounts, payment amounts
- Behavioral variables: repayment status over time (PAY_0–PAY_6)
- Demographic variables (marriage, sex, age)
- The dataset is drawn from 2005. While absolute levels of default risk may differ in current settings, the analysis focuses on structural relationships between variables.

### Approach

##### Exploratory Analysis
Exploration is conducted iteratively to identify key patterns:
- A strong monotonic relationship between delinquency and default
- High correlation within financial variables and within repayment variables
- Evidence of redundancy across predictors
##### Feature Construction
Repayment behavior is summarized into:
- ANY_DELAY
- MAX_DELAY
- NUM_DELAY_MONTHS
These features capture the presence, severity, and persistence of delinquency while reducing dimensionality.

##### Model Specifications
Models are estimated using different covariate sets:
- Financial variables only
- Behavioral variables only (full repayment history)
- Behavioral variables only (summary features)
- Financial + behavioral variables
- Financial + summary behavioral features
##### Evaluation
- Metric: out-of-sample AUC
- Method: train/test split
- Focus: comparison across specifications, not model optimization
##### Interpretation

Regression is treated as a projection rather than a structural model:
Coefficients represent conditional contributions. A variable’s coefficient reflects what it explains beyond what is already captured by other predictors. This distinction is central in the presence of correlated covariates.

### Results
##### Predictive Performance
Behavioral variables substantially outperform financial variables in isolation. Combined models yield only modest improvements, indicating that much of the predictive content of financial variables overlaps with repayment behavior.

##### Feature Representation
Summary measures of delinquency perform comparably to the full repayment history. This suggests that the behavioral signal is effectively low-dimensional.

##### Coefficient Stability

Financial variables appear important in isolation but weaken when behavioral variables are included. This reflects multicollinearity and the conditional nature of coefficient interpretation.

##### Regime Dependence

Variable importance depends on borrower state:
- In non-delinquent regimes, financial variables provide more information
- In delinquent regimes, behavioral variables dominate

### Takeaways
- Default risk is primarily driven by realized delinquency
- Financial variables act as indirect or leading indicators of risk
- Coefficient estimates are sensitive to specification under correlated predictors
- Interpretation is more meaningful at the level of variable groups than individual coefficients

##### Repository Structure
├── data/
├── notebooks/
│   ├── stat230a_project_eda.ipynb
├── src/
├── figures/
├── report/
│   └── Coefficient_Stability_in_Credit_Default_Models.pdf
└── README.md
Usage

Install dependencies:

`pip install -r requirements.txt`

Run notebook:
`stat230a_project_eda.ipynb`



This project emphasizes the distinction between predictive performance and interpretation. In settings with correlated predictors, understanding how information is distributed across variables is often more important than marginal improvements in predictive accuracy.
