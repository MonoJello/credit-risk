# Something about credit risk






This project compares logistic regression and XGBoost models for predicting credit default, demonstrating the trade-off between interpretability and predictive performance. Using the Statlog German Credit dataset, logistic regression with Weight of Evidence achieves near-equivalent performance (test AUC: .772) to XGBoost (.786) while offering superior explainability for regulatory and stakeholder audiences.


# Problem Statement

Banks need to assess credit default risk quickly and accurately, but must also explain their lending decisions to stakeholders and regulators. This project evaluates whether the interpretability gains of simpler models justify accepting modestly lower predictive performance compared to black-box alternatives.


# Dataset


This project uses the Statlog German Credit Dataset from the UCI Machine Learning Repository. The dataset contains 1,000 loan applications with 20 features capturing consumer demographics, financial history, and loan characteristics. The target variable is binary: good credit (700 cases) or bad credit (300 cases), representing whether a borrower defaulted on their loan.



# Tools and Technologies



- Python
- pandas
- NumPy
- Toad
- statsmodels
- Matplotlib
- Seaborn
- Optuna
- scikit-learn
- Jupyter Notebook



# Project Structure


```text
credit-risk/
├── code/
│   ├── utils/ (contains functions used in notebooks)
│   │   ├── __init__.py
│   │   ├── describe.py
│   │   ├── model.py
│   │   └── results.py
│   ├── 00 get data.ipynb
│   ├── 01 descriptive.ipynb
│   ├── 02a model logi.ipynb
│   ├── 02b model xgb woe.ipynb
│   ├── 02c model xgb raw.ipynb
│   ├── 03 results.ipynb
│   └── requirements.txt/
├── data/ (00 notebook pulls data from OpenML website, data is publicly available)
├── README.md
└── .gitignore
```








# Methodology

## 1. Exploratory Data Analysis


## 2. Training and Test Split

The dataset was split 50-50 into training and test sets due to the smaller sample size (1,000 observations). When comparing descriptive statistics across samples, lower sample rates showed somewhat meaninful differences.

## 3. Feature Selection

For the logistic regression model, features were selected based on statistical significance and correlation criteria inline with assumptions of logistic regression. Weight of Evidence (WoE) transformations were computed using the Toad library to bin categorical and continuous variables, improving model interpretability while reducing multicollinearity. For XGBoost models, all available features were retained to allow the algorithm to perform its own feature importance weighting.

## 4. Model Development

Three following classification models were trained and compared with out of sample data.

- Logistic
- XGBoost with WoE
- XGBoost with raw input


## 5. Hyperparameter Optimization

Optuna was used to optimize the hyperparameters of XGB models. The optimization objective was to maximize AUC.


## 6. Model Evaluation
Models were evaluated on both training and test sets using AUC and Kolmogorov-Smirnov (KS) statistic. AUC measures the model's ability to rank-order risk across the full probability distribution. KS captures the maximum separation between the cumulative distributions of good and bad accounts, providing a single summary metric commonly used in credit risk modeling.


| Model | Train AUC | Test AUC | Train KS | Test KS |
|-------|-----------|----------|----------|---------|
| Logistic Regression (WoE) | .815 | .772 | .505 | .467 |
| XGBoost (WoE) | .883 | .782 | .600 | .438 |
| XGBoost (Raw) | .892 | .786 | .610 | .438 |




# Key Findings

**InterpretabilityPerformance Trade-off:** The logistic regression model with Weight of Evidence transformations achieves a test AUC of .772, only ~1.5% lower than XGBoost with raw features (.786), while offering substantially greater interpretability. For credit risk applications where regulatory scrutiny and stakeholder explainability are critical, this modest performance gap is likely a worthwhile trade-off.

**XGBoost Overfitting:** Both XGBoost models show notable gaps between training and test performance (Train AUC: .892–.893 vs. Test AUC: .786), suggesting some overfitting. This may be due to the smaller sample sizes and some slight differences in attributes across them. In contrast, logistic regression generalizes more consistently (Train AUC: .815 vs. Test AUC: .772), a characteristic that may be preferable for production credit models.

**Feature Engineering Matters:** XGBoost performance is largely equivalent whether using transformed or raw features, suggesting that tree-based models naturally capture nonlinear relationships. WoE transformations are therefore most valuable for linear models where explicit feature binning and monotonic relationships are needed.

# Limitations


- Smaller total sample makes it difficult to have similar attributes in training and test data.
- Different transformations such as polynomials and interactions may fit the model better.
- Lack of features, with only 20 features there aren't many initial transformations that seem useful.
- 

