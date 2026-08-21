# Statistical Methodology

### Mental Health & Academic Performance Analysis

This document describes **how the research was conducted in Microsoft Excel**. It focuses on the data preparation, analytical methods, statistical tests, model specifications, formulas and decision rules used in the project.

It intentionally does **not** present the project's conclusions or business insights. Those are documented separately in `README.md` and the `Insights` worksheet.

<br> <br>

## 1. Research Objective

The analysis was designed to investigate the relationship between student mental-health measures and academic performance.

### Primary research question

> How are depression, anxiety and perceived stress associated with students' academic performance, and do academic and lifestyle factors provide additional explanatory value?

### Outcome Variable

**CGPA**

### Primary Mental-Health Predictors

- **PHQ-9** — Depression
- **GAD-7** — Anxiety
- **PSS-10** — Perceived Stress

### Additional Predictors

- Daily sleep
- Daily screen time
- Physical activity
- Weekly study hours
- Class attendance
- Financial stress
- Part-time employment

<br>

## 2. Analytical Workflow

The analysis followed the sequence below:

```text
Raw Data
   ↓
Data Cleaning
   ↓
Missingness Assessment
   ↓
Exploratory Data Analysis
   ↓
Correlation Analysis
   ↓
Multicollinearity / VIF
   ↓
Individual Mental-Health Regression Models
   ↓
Combined Mental-Health Regression
   ↓
Full Regression Model
   ↓
Model Comparison
   ↓
Statistical Interpretation
```

The order was chosen so that the regression models were developed only after examining the data quality, distributions and relationships among the variables.

<br>

## 3. Dataset Preparation

The original dataset was retained in the `Raw Data` worksheet.

A separate `Cleaned Data` worksheet was created for analysis.

### Preparation steps

1. Reviewed variable names and data types.
2. Checked numerical ranges.
3. Identified missing values.
4. Standardized inconsistent categorical values.
5. Created derived variables required for analysis.
6. Prepared the complete-case regression dataset.
7. Preserved the original records rather than overwriting the raw data.

<br>

## 4. Categorical Data Standardization

Categorical variables were reviewed for inconsistent representations.

For example, gender values such as:

```text
Male
male
M
```

were standardized so that the same category would not be counted as separate groups in Pivot Tables and charts.

The same principle was applied to other categorical variables where inconsistent formatting was present.

<br>

## 5. Part-Time Employment Encoding

The `has_part_time_job` variable was converted into a binary numerical variable for regression.

```text
No  = 0
Yes = 1
```

This was necessary because Excel regression requires numerical predictor values.

The `No` category was treated as the reference group.

Therefore, the coefficient for the binary variable represents the estimated difference in CGPA between students with and without a part-time job, conditional on the other predictors included in the model.

<br>

## 6. Missing Data Assessment

Missingness was assessed before performing statistical modelling.

Excel functions such as:

```text
COUNTBLANK()
COUNT()
```

were used to identify missing observations.

Missingness was also examined by variable and, where relevant, across student groups.

<br>

### 6.1 PHQ-9 and GAD-7

PHQ-9 and GAD-7 contained missing observations.

The missing PHQ-9 and GAD-7 observations occurred in the same student records.

These records were excluded from regression models requiring the three mental-health variables.

<br>

### 6.2 CGPA

CGPA contained **12 missing observations (2.4%)**.

The missing CGPA values were concentrated among Year 1 students.

Because CGPA is the dependent variable in the regression models, missing CGPA values were not replaced with mean or median values.

Instead, those records were excluded from the regression analysis.

### Reason for not imputing CGPA

Mean or median imputation would create artificial outcome values and could alter the distribution and variance of CGPA.

Because the amount of missing CGPA data was relatively small, complete-case analysis was selected.

<br>

## 7. Complete-Case Dataset

For regression analysis, observations were retained only when all variables required for the relevant model were available.

The final regression dataset contained:

```text
N = 475
```

The same complete set of 475 students was used across the regression models so that model comparisons were performed on the same observations.

This is particularly important when comparing R² values between nested models.

<br>

## 8. Exploratory Data Analysis

EDA was conducted before correlation and regression analysis.

The purpose of EDA was to understand:

- Central tendency
- Dispersion
- Variable ranges
- Missingness
- Distribution of mental-health scores
- Demographic composition
- Academic characteristics
- Lifestyle characteristics
- Potential outliers

<br>

### 8.1 Descriptive Statistics

For numerical variables, descriptive statistics included:

- Count
- Mean
- Variance
- Minimum
- Maximum

Excel functions used included:

```text
=COUNT(range)
=AVERAGE(range)
=VAR.S(range)
=MIN(range)
=MAX(range)
```

Where appropriate, Pivot Tables were used to summarize groups and categories.

<br>

### 8.2 Categorical Analysis

Pivot Tables were used to examine:

- Gender distribution
- Major distribution
- Year-of-study distribution
- Semester distribution
- Part-time employment
- Mental-health severity categories

Charts were then created from selected Pivot Table outputs for the dashboard.

<br>

## 9. Mental-Health Severity Categorization

The project used the established score ranges associated with:

- PHQ-9 depression severity
- GAD-7 anxiety severity
- PSS-10 perceived stress

The score categories were created as derived fields for descriptive analysis and dashboard visualization.

These categorical variables were used for **descriptive reporting**, not as replacements for the original continuous scores in the regression models.

The regression analysis retained the original numerical PHQ-9, GAD-7 and PSS-10 scores.

<br>

## 10. Correlation Analysis

Pearson correlation was used to examine linear relationships between numerical variables.

The Pearson correlation coefficient is:

```text
r = Cov(X,Y) / (SD(X) × SD(Y))
```

It ranges from:

```text
-1 to +1
```

#### Interpretation framework

| Correlation | General interpretation |
|---:|---|
| Close to +1 | Strong positive linear relationship |
| Close to 0 | Weak/no linear relationship |
| Close to −1 | Strong negative linear relationship |

In Excel, correlation was calculated using:

```text
=CORREL(range1, range2)
```

The Excel Data Analysis ToolPak can also be used to generate a correlation matrix.

<br>

## 11. Correlation Analysis for the Research Question

The primary correlation analysis examined:

```text
PHQ-9 ↔ CGPA
GAD-7 ↔ CGPA
PSS-10 ↔ CGPA
```

The correlation matrix also included other numerical variables to identify broader relationships and potential predictor overlap.

The mental-health variables were additionally examined against one another because strong predictor-to-predictor relationships can affect multiple regression estimates.

<br>

## 12. Multicollinearity Assessment

Because PHQ-9, GAD-7 and PSS-10 measure related aspects of mental health, multicollinearity was assessed before interpreting their combined regression coefficients.

Variance Inflation Factor (VIF) was used.

<br>

### 12.1 VIF Procedure in Excel

For each predictor:

#### Step 1

Select one predictor as the temporary dependent variable.

Example:

```text
Dependent variable: PHQ-9
Predictors: GAD-7 + PSS-10
```

#### Step 2

Run:

```text
Data → Data Analysis → Regression
```

#### Step 3

Record the resulting R².

#### Step 4

Calculate:

```text
VIF = 1 / (1 - R²)
```

Excel formula:

```text
=1/(1-R2)
```

The same process was repeated for:

```text
PHQ-9
GAD-7
PSS-10
```

<br>

### 12.2 VIF Interpretation Thresholds

The following practical thresholds were used:

| VIF | Interpretation |
|---:|---|
| < 5 | Generally acceptable |
| 5–10 | Elevated multicollinearity |
| > 10 | High multicollinearity |

VIF was used as a diagnostic rather than an automatic rule for removing variables.

<br>

## 13. Regression Framework

Linear regression was used because CGPA is a continuous numerical outcome.

The general multiple regression equation is:

```text
CGPA = β0 + β1X1 + β2X2 + ... + βkXk + ε
```

Where:

- `CGPA` = dependent variable
- `β0` = intercept
- `β1 ... βk` = regression coefficients
- `X1 ... Xk` = predictor variables
- `ε` = residual/error term

Excel's:

```text
Data → Data Analysis → Regression
```

was used for each regression model.

<br>

## 14. Model Structure

The regression analysis was deliberately divided into several stages.

```text
Model 1A → PHQ-9 only
Model 1B → GAD-7 only
Model 1C → PSS-10 only
Model 1  → PHQ-9 + GAD-7 + PSS-10
Model 2  → Mental health + academic/lifestyle controls
```

This structure allows the individual and combined relationships to be examined separately.

<br>

## 15. Model 1A — PHQ-9

### Specification

```text
CGPA ~ PHQ-9
```

### Excel setup

- Input Y Range: `CGPA`
- Input X Range: `PHQ-9`
- Labels: checked
- Confidence Level: 95%
- Output: new worksheet

The following outputs were retained:

- R²
- Adjusted R²
- F-statistic
- Significance F
- Coefficient
- Standard Error
- t Stat
- p-value
- 95% confidence interval

<br>

## 16. Model 1B — GAD-7

### Specification

```text
CGPA ~ GAD-7
```

The same Excel regression procedure was used.

- Input Y Range: `CGPA`
- Input X Range: `GAD-7`

The same regression statistics were recorded.

<br>

## 17. Model 1C — PSS-10

### Specification

```text
CGPA ~ PSS-10
```

The same Excel regression procedure was used.

- Input Y Range: `CGPA`
- Input X Range: `PSS-10`

<br>

## 18. Model 1 — Combined Mental-Health Model

The three mental-health variables were then entered simultaneously.

### Specification

```text
CGPA ~ PHQ-9 + GAD-7 + PSS-10
```

### Excel setup

- Input Y Range: `CGPA`
- Input X Range: columns containing PHQ-9, GAD-7 and PSS-10
- Labels: checked
- Confidence Level: 95%

This model was designed to estimate the association of each mental-health measure while controlling for the other two.

Because the predictors are correlated, the coefficients were interpreted alongside the VIF results.

<br>

## 19. Model 2 — Full Model

The second-stage model added academic and lifestyle variables.

### Specification

```text
CGPA ~ PHQ-9
     + GAD-7
     + PSS-10
     + Weekly Study Hours
     + Class Attendance
     + Daily Sleep
     + Daily Screen Time
     + Physical Activity
     + Financial Stress
     + Part-time Job
```

The full model was estimated using the same 475 complete observations.

This model was designed to determine whether the relationship between mental-health variables and CGPA remained after accounting for other student-level characteristics.

<br>

## 20. Regression Output Metrics

The following regression statistics were recorded.

### R²

R² represents the proportion of observed variation in CGPA explained by the predictors included in the model.

```text
R² = Explained variation / Total variation
```

For example, an R² of 0.4146 corresponds to:

```text
41.46%
```

<br>

### Adjusted R²

Adjusted R² accounts for the number of predictors in the model.

It is useful when comparing models containing different numbers of predictors.

<br>

### Coefficient (β)

The coefficient represents the expected change in CGPA associated with a one-unit increase in the predictor, holding the other predictors constant.

<br>

### Standard Error

The standard error measures the estimated uncertainty around the regression coefficient.

<br>

### t Statistic

The t statistic is used to test whether an individual regression coefficient differs significantly from zero.

<br>

### p-value

The p-value was used to assess statistical evidence for individual predictors.

<br>

### F Statistic and Significance F

The F-test evaluates whether the regression model as a whole provides statistically significant explanatory information.

<br>

## 21. Statistical Significance Rule

The project used:

```text
α = 0.05
```

Decision rule:

```text
p-value < 0.05
→ statistically significant

p-value ≥ 0.05
→ not statistically significant
```

Statistical significance was interpreted as evidence of an association within the sample, not as evidence of causality.

<br>

## 22. Model Comparison

The combined mental-health model and full model were compared using:

- R²
- Adjusted R²
- Significance F
- Change in R²

#### Change in R²

The change in R² was calculated as:

```text
ΔR² = R²(Model 2) − R²(Model 1)
```

Using the displayed values:

```text
ΔR² = 41.46% − 11.78%
     = 29.68 percentage points
```

The purpose of ΔR² was to quantify how much additional variation in CGPA was explained after adding the academic and lifestyle predictors.

<br>

## 23. Coefficient Direction

The sign of a regression coefficient was used to describe the direction of association.

| Coefficient | Direction |
|---|---|
| β > 0 | Positive association |
| β < 0 | Negative association |
| β ≈ 0 | Little linear association |

For example:

```text
β = −0.0226
```

indicates a negative association between the predictor and CGPA after controlling for the other variables in the model.

<br>

## 24. Binary Predictor Interpretation

For the part-time-job variable:

```text
No = 0
Yes = 1
```

the coefficient represents the expected difference between the `Yes` and `No` groups, conditional on the other predictors.

The reference group is:

```text
Students without a part-time job
```

<br>

## 25. Hypothesis Testing Framework

The analysis used the following hypotheses.

#### H1 — Depression

**H0:** PHQ-9 has no linear association with CGPA.

**H1:** Higher PHQ-9 is associated with lower CGPA.

#### H2 — Anxiety

**H0:** GAD-7 has no linear association with CGPA.

**H1:** Higher GAD-7 is associated with lower CGPA.

#### H3 — Perceived Stress

**H0:** PSS-10 has no linear association with CGPA.

**H1:** Higher PSS-10 is associated with lower CGPA.

#### H4 — Combined Mental Health

**H0:** PHQ-9, GAD-7 and PSS-10 collectively provide no explanatory value for CGPA.

**H1:** The combined mental-health model provides explanatory value for CGPA.

#### H5 — Additional Academic/Lifestyle Factors

**H0:** Adding academic and lifestyle factors does not improve the model.

**H1:** Adding academic and lifestyle factors improves the model.

<br>

## 26. Assumptions Considered

Because ordinary least squares linear regression was used, the following assumptions were considered:

#### Linearity

The relationship between continuous predictors and CGPA should be approximately linear.

#### Independence

Each student record represents an individual observation.

#### Homoscedasticity

The variance of regression residuals should be reasonably consistent across fitted values.

#### Normality of Residuals

Residuals should be approximately normally distributed for standard inferential procedures.

#### Multicollinearity

Predictors should not exhibit problematic levels of redundancy.

VIF was used specifically to assess this assumption.

#### Outliers and Influential Observations

Numerical distributions and regression outputs were reviewed for unusual observations that could disproportionately affect the results.

<br>

## 27. Excel Dashboard Preparation

The dashboard was created after completing the statistical analysis.

Pivot Tables and calculated summary tables were used to create:

- Student count KPI
- Average PHQ-9 KPI
- Average GAD-7 KPI
- Average PSS-10 KPI
- Severe depression count
- Severe anxiety count
- Severe stress count
- Gender distribution
- Major distribution
- Year-of-study distribution
- Mental-health severity distributions
- Study hours vs physical activity
- Sleep vs screen time
- Part-time employment vs CGPA
- Regression model comparison
- Key CGPA predictors
- Mental-health measures by part-time employment

The dashboard was designed to communicate the analytical results visually rather than to perform additional statistical testing.

<br>

## 28. Workbook Structure

The Excel workbook was organized into:

| Worksheet | Purpose |
|---|---|
| **Raw Data** | Original dataset |
| **Cleaned Data** | Standardized dataset used for analysis |
| **EDA** | Descriptive statistics, missingness and category analysis |
| **Correlation** | Pearson correlation matrix |
| **Statistical Analysis** | VIF and regression models |
| **Dashboard** | Visual presentation of key results |
| **Insights** | Research findings, hypotheses and conclusions |

<br>

## 29. Reproducibility

The analysis can be reproduced in Excel using:

1. The original dataset in `Raw Data`.
2. The cleaning rules described above.
3. The complete-case regression dataset.
4. Excel's Data Analysis ToolPak.
5. The model specifications documented in this file.
6. The formulas described for correlation and VIF.
7. The same significance threshold of α = 0.05.

The methodology is documented separately from the results so that another analyst can reproduce the statistical workflow without relying on the project's conclusions.

