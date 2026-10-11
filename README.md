# Venture Creed – Customer Propensity Modelling

## Project Overview

This project focuses on building a **Customer Propensity-to-Buy Model** to estimate the likelihood of customers making a future purchase. The analysis integrates customer-level Propensity Data with Salesforce (SF) Master order records to develop a machine learning solution for customer scoring and segmentation.

The objective is to identify customers with different purchase propensities using historical purchasing behaviour, statistical analysis, feature selection, and Logistic Regression.

## Objectives

- Perform Exploratory Data Analysis (EDA) to understand customer and order data.
- Clean and integrate customer and Salesforce order datasets.
- Handle missing values, duplicate records, and categorical inconsistencies.
- Define a future-purchase target variable using a cutoff date.
- Select relevant features using business reasoning and statistical analysis.
- Build a classification model to estimate customer purchase probabilities.
- Generate customer-level propensity scores.
- Segment customers into Low, Medium, and High propensity groups.
- Validate data quality, model outputs, and customer segmentation.

## Tech Stack

- **Programming Language:** Python
- **Data Analysis:** Pandas, NumPy
- **Data Visualization:** Matplotlib, Seaborn
- **Machine Learning:** Scikit-learn
- **Statistical Analysis:** Correlation Analysis, Chi-Square Test, IQR Method
- **Model:** Logistic Regression
- **Data Storage and Reporting:** Excel, Jupyter Notebook

## Project Workflow

1. **Data Loading:** Load the Propensity Data and SF Master datasets.
2. **Data Cleaning:** Identify duplicates, missing values, inconsistent categories, and invalid records.
3. **Data Integration:** Combine customer attributes and historical order information at the customer level.
4. **Exploratory Data Analysis:** Examine distributions, missing-value patterns, order activity, and feature correlations.
5. **Target Engineering:** Define future purchase behaviour using a cutoff date of 15 August 2021.
6. **Feature Selection:** Evaluate business relevance, feature redundancy, correlations, and categorical associations with the target.
7. **Model Development:** Train a Logistic Regression classifier using an 80/20 train-test split.
8. **Model Evaluation:** Evaluate classification performance using accuracy, precision, recall, F1-score, and ROC-AUC.
9. **Propensity Scoring:** Estimate the probability of a future completed purchase for each customer.
10. **Customer Segmentation:** Assign Low, Medium, and High propensity groups using empirical quartiles.
11. **Quality Validation:** Verify customer counts, missing scores, duplicate IDs, valid probability ranges, and segment totals.

## Target Variable Definition

The problem is formulated as a binary classification task.

- `y = 1`: The customer has at least one completed order after the cutoff date.
- `y = 0`: The customer has no completed order after the cutoff date.

Historical customer and order information is used to predict future purchasing behaviour.

## Feature Selection and Data Quality

Feature selection combines business relevance, statistical association, and redundancy checks.

Key considerations include:

- Excluding `Customer_ID` because it is an identifier rather than a predictive characteristic.
- Removing `Postal_Code` and `Country` from the final modelling features.
- Removing `completion_rate` as a derived or redundant feature.
- Reviewing highly correlated numerical variables.
- Evaluating categorical predictors using Chi-Square tests.
- Excluding target-related and post-outcome information to reduce data leakage risk.
- Reviewing missing values, duplicate records, and inconsistent categorical values.

Potential outliers in order activity are investigated in their business context rather than automatically removed.

## Machine Learning Model

**Algorithm:** Logistic Regression

Logistic Regression was selected because it estimates the probability of a customer belonging to the positive class.

The propensity score is defined as:

`Propensity Score = P(y = 1 | Customer Features)`

Each score ranges from 0 to 1, where higher values indicate a higher estimated likelihood of future purchase.

The model uses consistent numerical and categorical preprocessing and an 80/20 train-test split.

## Model Performance

The model achieved the following results on the test set:

| Evaluation Metric | Result |
|---|---:|
| Accuracy | 94.97% |
| Precision | 91% |
| Recall | 79% |
| F1-Score | 85% |
| ROC-AUC | 0.9866 |

These metrics evaluate the model's ability to classify future purchasing behaviour and distinguish between customers with different purchase outcomes.

## Customer Propensity Segmentation

Because the propensity score distribution is highly skewed, empirical quartiles are used instead of arbitrary probability thresholds.

| Segment | Score Rule | Customers | Share |
|---|---|---:|---:|
| Low | Score ≤ 0.0001626 | 920 | 25% |
| Medium | 0.0001626 < Score ≤ 0.1648688 | 1,840 | 50% |
| High | Score > 0.1648688 | 920 | 25% |
| **Total** | | **3,680** | **100%** |

This segmentation can support customer prioritisation and targeted marketing strategies.

## Outlier Detection

The Interquartile Range (IQR) method was used to identify potential outliers in `total_orders`.

- 43 observations were flagged as potential outliers.
- These represented approximately 3.94% of customers with historical order records.
- The observations were investigated as potential high-volume or bulk-order customers.
- They were retained after business review because they could represent genuine purchasing behaviour rather than data errors.

## Final Validation

The final solution was validated against several quality checks:

- 3,680 customers in the final dataset.
- Zero duplicate customer IDs after integration.
- Zero missing propensity scores.
- Zero missing target values.
- Zero missing propensity segments.
- All propensity scores within the valid 0–1 probability range.
- Customer segmentation totals equal to 3,680.

## Project Files

- `Proc_DNA_Case_Study.ipynb` — Jupyter Notebook containing data preparation, EDA, feature selection, model development, scoring, and segmentation.
- `ProcDNA_Propensity_Score.xlsx` — Excel workbook containing customer propensity scores and summary information.
- `ProcDNA_Venture_Creed_Propensity_Case_Study_Final.pptx` — Presentation summarising the analysis, methodology, results, and validation.

*Input datasets are not included in this repository. File names may differ depending on the local project setup.*

## How to Run the Project

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/venture-creed-propensity-modeling.git
cd venture-creed-propensity-modeling
```

### 2. Create a virtual environment

```bash
python -m venv .venv
```

Activate it on Windows:

```bash
.venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install pandas numpy matplotlib seaborn scikit-learn openpyxl pyarrow jupyter
```

### 4. Prepare the input datasets

Place the required input files in the project directory or update the notebook paths to match your local setup.

The notebook expects the Propensity Data and SF Master files in Feather format.

### 5. Launch Jupyter Notebook

```bash
jupyter notebook
```

Open `Proc_DNA_Case_Study.ipynb` and execute the cells in order.

## Key Takeaways

- Customer purchase propensity can be estimated using historical order behaviour and customer characteristics.
- Data cleaning and feature selection are important for reliable model development.
- Statistical analysis and business reasoning help identify useful predictive features.
- Probability-based customer scoring enables data-driven customer prioritisation.
- Quartile-based segmentation provides a practical approach when propensity scores are highly skewed.
- Quality validation helps ensure that customer scores and segments are complete and consistent.

## Conclusion

This project demonstrates an end-to-end customer propensity modelling workflow, from data preparation and exploratory analysis to machine learning, customer-level scoring, segmentation, and validation.

The Logistic Regression model achieved **94.97% accuracy and a ROC-AUC of 0.9866** on the test set. The final solution assigned propensity scores to 3,680 customers and grouped them into Low, Medium, and High propensity segments.

---

**Disclaimer:** This repository documents a data science case study for educational and portfolio purposes. Any public sharing of company-provided datasets, case materials, or confidential information must comply with the applicable permissions and submission guidelines.
