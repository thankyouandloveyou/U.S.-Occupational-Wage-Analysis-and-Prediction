# U.S. Occupational Wage Analysis and Prediction

Analyze U.S. occupational wage differences using statistical analysis, multiple linear regression, and machine learning to identify key factors and predict mean annual wages.

## Project Overview

Which factors best explain wage differences across U.S. labor markets, and how accurately can wages be predicted?

This project analyzes U.S. occupational wage estimates to understand how occupation, geographic location, and employment size relate to annual wages. I use exploratory data analysis, statistical testing, multiple linear regression, and machine learning to identify key patterns and evaluate predictive performance.

The goal is to demonstrate how data analysis can support high-level labor-market benchmarking, workforce planning, and evidence-based decision-making.

## Business Objectives

- Identify which factors are most strongly associated with differences in mean annual wages.
- Quantify the additional explanatory value of geography and employment size after accounting for occupation.
- Compare predictive models and evaluate their performance on unseen test observations.
- Translate statistical findings into actionable business insights while acknowledging analytical limitations.

## Dataset

**Source:** U.S. Bureau of Labor Statistics (BLS), Occupational Employment and Wage Statistics (OEWS), May 2024 state estimates.

The analysis focuses on major occupation groups across U.S. states and territories.

**Target variable**
- `A_MEAN`: Estimated mean annual wage.

**Predictor variables**
- `OCC_TITLE`: Occupation title.
- `AREA_TITLE`: Geographic area.
- `LOG_TOT_EMP`: Natural logarithm of total employment plus one, derived from `TOT_EMP`.

The dataset contains 1,188 initial observations. After converting wage values to numeric format and removing two observations with missing target values, the final analytical dataset contains **1,186 observations**.

Each observation represents an occupation–geographic area estimate, not an individual worker's salary.

## Methodology

### 1. Data Cleaning and Exploratory Data Analysis
- Inspected data types, missing values, and data quality.
- Converted wage and employment variables to appropriate numeric formats.
- Examined wage distributions and differences across occupations and geographic areas.
- Evaluated the relationship between employment size and mean annual wages using Pearson correlation.

### 2. Statistical Analysis
- Used Pearson correlation to assess the direction, strength, and statistical significance of the linear relationship between employment size and wages.
- Fit standalone regression models to examine the explanatory value of individual predictors.
- Compared progressively specified multiple linear regression models to evaluate the incremental contribution of additional predictors.

### 3. Multiple Linear Regression
Built nested models by adding predictors in stages:

1. Occupation
2. Occupation + geography
3. Occupation + geography + log employment

Evaluated model fit using R², adjusted R², and Akaike Information Criterion (AIC).

### 4. Predictive Modeling
Compared four approaches:

- Linear Regression
- Decision Tree Regressor
- Random Forest Regressor
- Dummy Regressor baseline

Categorical predictors were one-hot encoded. The data was split into training and test sets using an 80/20 split and `random_state=42`.

Used five-fold cross-validation and `GridSearchCV` to tune the tree-based models, with mean absolute error (MAE) as the selection metric.

Final predictive performance was evaluated on the held-out test set using MAE, root mean squared error (RMSE), and R².

## Key Findings

### 1. Occupation Explains Most of the Wage Variation
Occupation alone explained approximately **83.8%** of the observed variation in mean annual wages.

This suggests that occupational differences are strongly associated with wage differences in the analyzed sample.

### 2. Geography Adds Substantial Explanatory Value
Adding geographic area increased R² from approximately 0.838 to 0.932, an improvement of **9.4 percentage points**.

This indicates that geographic location provides substantial additional explanatory information after accounting for occupation.

### 3. Employment Size Adds Limited Explanatory Power
Adding log employment increased R² from approximately 0.932 to 0.937, an additional **0.5 percentage points**.

In the standalone correlation analysis, the relationship between employment size and mean annual wages was negligible and not statistically significant (Pearson's \(r = 0.015\), \(p = 0.607\)).

However, its relationship changes after controlling for occupation and geography in the multiple regression model. This illustrates why a standalone correlation and a conditional regression coefficient can tell different stories.

### 4. Linear Regression Achieved the Best Test-Set Performance
Linear Regression outperformed the evaluated tree-based models and the dummy baseline on the held-out test set.

| Model | Test MAE | Test RMSE | Test R² |
|---|---:|---:|---:|
| Linear Regression | $5,002.99 | $7,068.91 | 0.939 |
| Random Forest | $5,657.66 | $8,287.97 | 0.916 |
| Decision Tree | $6,985.80 | $10,385.45 | 0.868 |
| Dummy Regressor | $22,885.97 | $28,627.40 | -0.001 |

Linear Regression achieved a test MAE of approximately **$5,003**, meaning its predictions differed from the observed mean annual wages by about $5,003 on average in absolute terms.

The results show that a relatively simple model can perform well when occupation and geography capture much of the structure in the data.

## Business Recommendations

- **Use occupation and geography as primary benchmarking dimensions.** Both are strongly associated with wage variation and should be considered when comparing labor-market wage levels.
- **Use the model for high-level planning.** The results may help inform preliminary workforce planning, salary benchmarking, and broad labor-market comparisons.
- **Avoid treating estimates as individual salary predictions.** The data describes mean wages for occupation–area groups and does not capture individual experience, seniority, education, company, or specific job responsibilities.
- **Validate before operational deployment.** Further testing is needed to assess performance on entirely new geographic areas, occupation groups, or future years.

## Limitations

- The analysis uses aggregate occupation–area wage estimates rather than individual-level compensation data.
- The findings describe statistical associations and do not establish causal relationships.
- The random train/test split evaluates performance on held-out observations drawn from the same cross-sectional dataset. It does not establish performance on future years or entirely unseen occupations and geographic areas.
- The dataset contains a limited set of predictors, so some important determinants of wages are not captured.
- The model's test-set performance should not be interpreted as a guarantee of accuracy in a different labor market or use case.

## Tools and Technologies

- **Python**
- **Pandas & NumPy** — data cleaning and transformation
- **Matplotlib & Seaborn** — exploratory data analysis and visualization
- **SciPy** — statistical analysis
- **Statsmodels** — statistical inference and multiple linear regression
- **Scikit-learn** — preprocessing, predictive modeling, cross-validation, hyperparameter tuning, and model evaluation
- **Jupyter Notebook** — reproducible analysis

## Conclusion

This project combines exploratory analysis, statistical inference, regression modeling, and predictive evaluation to investigate wage variation across U.S. labor markets.

The analysis identifies occupation and geography as the main explanatory dimensions in the evaluated models. It also demonstrates that a more complex machine learning model does not necessarily outperform a simpler statistical model.

The broader takeaway is that model selection should be driven by validation results, interpretability, and the intended business application—not by model complexity alone.
