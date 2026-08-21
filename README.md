<div align="center">

# Mental Health & Academic Performance Analysis (Excel)

An Excel-based exploratory and statistical analysis of college students to examine the relationship between **depression, anxiety, perceived stress and academic performance**, while also assessing the role of academic, lifestyle and other student-level factors.

</div>

<br>
  
## 1. Research Objective

The primary objective is to examine how mental health factors are associated with academic performance among college students.

The analysis focuses on:
- **PHQ-9** — Depression
- **GAD-7** — Anxiety
- **PSS-10** — Perceived Stress
- **CGPA** — Academic performance

It also evaluates whether study hours, class attendance, sleep, screen time, physical activity, financial stress and part-time employment provide additional explanatory value beyond mental-health measures alone.

#### Research Question

> **How are depression, anxiety and perceived stress associated with students' academic performance, and do these factors remain important after accounting for academic and lifestyle characteristics?**

#### Supporting Questions

1. Is higher depression associated with lower CGPA?
2. Is higher anxiety associated with lower CGPA?
3. Is higher perceived stress associated with lower CGPA?
4. Which mental-health factor shows the strongest individual relationship with CGPA?
5. Do depression, anxiety and stress collectively explain variation in CGPA?
6. Do academic and lifestyle factors add explanatory value beyond mental-health measures?
7. Which variables remain statistically significant in the full regression model?

<br>

## 2. Dataset

The dataset contains **500 college student records** with 18 features covering mental health, daily habits, academic behaviour and academic performance during the 2022–2023 hybrid-learning period.

#### Main Variables

| Variable | Description |
|---|---|
| `student_id` | De-identified participant ID |
| `age` | Student age |
| `gender` | Self-reported gender |
| `year_of_study` | Undergraduate year |
| `major_category` | Broad academic discipline |
| `daily_sleep_hours` | Average sleep per night |
| `daily_screen_time_hours` | Recreational screen time per day |
| `physical_activity_days_per_week` | Days with ≥30 minutes of moderate activity |
| `phq9_score` | PHQ-9 depression score (0–27) |
| `gad7_score` | GAD-7 anxiety score (0–21) |
| `perceived_stress_scale` | PSS-10 perceived stress score (0–40) |
| `weekly_study_hours` | Weekly study hours |
| `class_attendance_percent` | Estimated class attendance |
| `online_learning_satisfaction` | Hybrid-learning satisfaction score (1–5) |
| `has_part_time_job` | Whether the student has a part-time job |
| `financial_stress` | Financial pressure score (1–5) |
| `semester_type` | Fall, Spring or Summer |
| `cgpa` | Cumulative GPA (0.5–4.3) |

#### Dataset Characteristics

- **500 total student records**
- PHQ-9 and GAD-7 contain missing observations
- CGPA contains **12 missing observations (2.4%)**
- Missing CGPA values are concentrated among **Year 1 students**
- PHQ-9 and GAD-7 missing values occurred in the same records

<br>

## 3. Dashboard

The project includes an Excel dashboard summarizing the major descriptive and statistical findings.

The dashboard contains:
- Student population KPIs
- Average PHQ-9, GAD-7 and PSS-10 scores
- Severe depression, anxiety and stress counts
- Gender, major and year distributions
- Mental-health severity distributions
- Academic and lifestyle indicators
- Part-time employment and CGPA analysis
- Regression model comparison
- Key predictors of CGPA
- Mental-health and part-time employment analysis

#### Dashboard Preview

<img width="1703" height="1046" alt="image" src="https://github.com/user-attachments/assets/b46c3f1a-d6ce-48ed-8b9a-1b58019a5a16" />


<br> <br>

## 4. Analytical Approach

The analysis follows:

```text
Data Cleaning
      ↓
Exploratory Data Analysis
      ↓
Correlation Analysis
      ↓
Multicollinearity / VIF
      ↓
Individual Regression Models
      ↓
Combined Mental-Health Regression
      ↓
Full Regression Model
      ↓
Model Comparison
      ↓
Insights & Dashboard
```

The analysis was performed entirely in **Microsoft Excel**.

<br>

## 5. Data Cleaning & Preparation

The original dataset was preserved separately from the cleaned analysis dataset.

#### Cleaning steps
- Standardized inconsistent categorical values
- Checked missing values
- Reviewed numerical ranges
- Checked variable consistency
- Created a binary variable for part-time employment
- Prepared complete-case datasets for regression

#### Part-Time Job Encoding

```text
Yes = 1
No  = 0
```

#### Missing Data Treatment

Missing values were **not automatically replaced with mean or median values** for regression.
Instead, **complete-case analysis** was used. Records missing variables required for a particular regression model were excluded from that model.

#### CGPA Missingness

The dataset contained **12 missing CGPA observations (2.4%)**, concentrated among **Year 1 students**.
Because CGPA is the dependent variable in the regression analysis, these observations were excluded rather than imputed.

The cleaned dataset retains the records so the EDA represents the full dataset.

<br>

## 6. Exploratory Data Analysis

EDA was performed to understand the dataset before statistical modelling.

It included:
- Mean and variance
- Numerical distributions
- Missing-value assessment
- Student demographics
- Academic-year distribution
- Major distribution
- Mental-health severity distribution
- Lifestyle indicators
- Academic performance characteristics

#### Mental-Health Profile

The analysis examined:
- Depression severity using PHQ-9
- Anxiety severity using GAD-7
- Perceived stress using PSS-10

#### Missingness Analysis

An important pattern was identified:

> **CGPA missingness was concentrated among Year 1 students.**

#### EDA Preview

<img width="1815" height="579" alt="image" src="https://github.com/user-attachments/assets/f29e4c45-d0f9-48f7-a39e-c6d088f9bf24" />

<br>

## 7. Correlation Analysis

Pearson correlation was used to examine linear relationships between numerical variables.

#### Correlation with CGPA

| Variable | Correlation with CGPA |
|---|---:|
| **GAD-7** | **−0.334** |
| **PHQ-9** | **−0.307** |
| **PSS-10** | **−0.244** |

All three mental-health variables show negative correlations with CGPA.

#### Mental-Health Intercorrelations

| Variable Pair | Correlation |
|---|---:|
| **PHQ-9 ↔ GAD-7** | **0.872** |
| **PHQ-9 ↔ PSS-10** | **0.849** |
| **GAD-7 ↔ PSS-10** | **0.807** |

The strong positive correlations indicate substantial overlap between the mental-health measures and motivated the VIF analysis.

#### Correlation Matrix
<img width="1750" height="387" alt="image" src="https://github.com/user-attachments/assets/be4a51ad-b44d-4637-9d6a-8308cad54637" />


<br> <br>

## 8. Multicollinearity / VIF Analysis

Because PHQ-9, GAD-7 and PSS-10 are strongly correlated, VIF was calculated before interpreting the combined mental-health regression.

For each predictor, a regression was run against the other two mental-health variables.

Example:

```text
PHQ-9 = GAD-7 + PSS-10
```

The resulting R² was used as:

```text
VIF = 1 / (1 - R²)
```

#### VIF Results

| Predictor | R² | VIF | Interpretation |
|---|---:|---:|---|
| PHQ-9 | 0.818 | **5.49** | Elevated |
| GAD-7 | 0.772 | **4.39** | Moderate |
| PSS-10 | 0.739 | **3.83** | Moderate |

#### Interpretation

PHQ-9 shows elevated multicollinearity with GAD-7 and PSS-10, while GAD-7 and PSS-10 show moderate overlap.

The variables were retained because they represent distinct mental-health dimensions relevant to the research question. Individual coefficients in the combined model should be interpreted cautiously.

#### VIF Analysis

<img width="1222" height="1008" alt="image" src="https://github.com/user-attachments/assets/35c0b35f-16b6-4eb1-9a8c-924565a1fa06" />


<br> <br>

## 9. Regression Analysis

Linear regression was used to examine relationships between mental-health measures and CGPA.

The analysis included:
- **Model 1A — PHQ-9 only**
- **Model 1B — GAD-7 only**
- **Model 1C — PSS-10 only**
- **Model 1 — Combined mental-health model**
- **Model 2 — Full model**

The regression analysis used **475 complete observations**.

### Model 1A — PHQ-9

```text
CGPA ~ PHQ-9
```

| Metric | Result |
|---|---:|
| Observations | 475 |
| R² | **9.45%** |
| Adjusted R² | **9.26%** |
| PHQ-9 coefficient | **−0.0237** |
| p-value | **<0.001** |

**Interpretation:** PHQ-9 has a statistically significant negative association with CGPA. A one-point increase in PHQ-9 is associated with approximately a **0.024-point lower CGPA** in this individual model.

### Model 1B — GAD-7

```text
CGPA ~ GAD-7
```

| Metric | Result |
|---|---:|
| Observations | 475 |
| R² | **11.18%** |
| Adjusted R² | **10.99%** |
| GAD-7 coefficient | **−0.0329** |
| p-value | **<0.001** |

**Interpretation:** GAD-7 has a statistically significant negative association with CGPA. A one-point increase in GAD-7 is associated with approximately a **0.033-point lower CGPA**. GAD-7 has the highest R² among the three individual mental-health models.

### Model 1C — PSS-10

```text
CGPA ~ PSS-10
```

| Metric | Result |
|---|---:|
| Observations | 475 |
| R² | **5.96%** |
| Adjusted R² | **5.76%** |
| PSS-10 coefficient | **−0.0162** |
| p-value | **<0.001** |

**Interpretation:** PSS-10 has a statistically significant negative association with CGPA. A one-point increase in PSS-10 is associated with approximately a **0.016-point lower CGPA**.

<br>

## 10. Model 1 — Combined Mental-Health Model

#### Model

```text
CGPA ~ PHQ-9 + GAD-7 + PSS-10
```

#### Overall Results

| Metric | Result |
|---|---:|
| Observations | **475** |
| R² | **11.78%** |
| Adjusted R² | **11.22%** |
| Significance F | **<0.001** |

#### Individual Predictors

| Predictor | Coefficient | p-value | Significant? |
|---|---:|---:|---|
| PHQ-9 | −0.0117 | 0.136 | No |
| **GAD-7** | **−0.0308** | **0.0006** | **Yes** |
| PSS-10 | +0.0091 | 0.107 | No |

#### Interpretation

The three mental-health variables collectively have a statistically significant association with CGPA and explain **11.78% of observed CGPA variation**.

When considered simultaneously, only **GAD-7 remains statistically significant**. PHQ-9 and PSS-10 were significant individually but not in the combined model.

This result should be interpreted alongside the VIF analysis because the three measures contain substantial overlapping information.

#### Combined Mental-Health Regression

<img width="935" height="569" alt="image" src="https://github.com/user-attachments/assets/6b2d4242-5fd3-49b2-aa25-fc9877708a4a" />


<br> <br>

## 11. Model 2 — Full Regression Model

The full model adds academic, lifestyle and other student-level factors.

#### Model

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

#### Model Comparison

| Metric | Model 1 | Model 2 |
|---|---:|---:|
| Observations | 475 | 475 |
| R² | **11.78%** | **41.46%** |
| Adjusted R² | **11.22%** | **40.19%** |
| Significance F | <0.001 | <0.001 |
| ΔR² | — | **+29.67 percentage points** |

#### Model Improvement

Adding academic and lifestyle variables increased explained variation in CGPA by **29.67 percentage points**.

The model improvement was statistically significant:

> **F-change ≈ 33.58, p < 0.001**

#### Full Model Results

| Predictor | Coefficient | p-value | Result |
|---|---:|---:|---|
| **Daily Sleep** | **+0.0649** | **<0.001** | Significant |
| Daily Screen Time | +0.0048 | 0.316 | Not significant |
| Physical Activity | +0.0022 | 0.783 | Not significant |
| PHQ-9 | −0.0093 | 0.150 | Not significant |
| **GAD-7** | **−0.0226** | **0.002** | Significant |
| PSS-10 | +0.0071 | 0.127 | Not significant |
| **Weekly Study Hours** | **+0.0161** | **<0.001** | Significant |
| **Class Attendance** | **+0.0088** | **<0.001** | Significant |
| Financial Stress | −0.0057 | 0.613 | Not significant |
| **Part-time Job** | **−0.1000** | **<0.001** | Significant |

#### Interpretation

The full model explains **41.46% of observed variation in CGPA**.

Among the three mental-health measures, **GAD-7 is the only one that remains statistically significant** after accounting for academic and lifestyle factors.

Significant non-mental-health predictors are:
- Sleep: positive association
- Weekly study hours: positive association
- Class attendance: positive association
- Part-time employment: negative association

PHQ-9, PSS-10, screen time, physical activity and financial stress do not show statistically significant independent associations with CGPA in the full model.

#### Full Regression Analysis

<img width="1428" height="708" alt="image" src="https://github.com/user-attachments/assets/24c993ba-7dcd-4d14-97a2-1c68139d511b" />


<br> <br>

## 12. Key Findings & Insights

### Significant Findings

#### 1. Mental health is associated with academic performance

All three mental-health measures show statistically significant negative associations with CGPA when examined individually.

| Factor | Correlation with CGPA | Individual R² |
|---|---:|---:|
| **GAD-7** | **−0.334** | **11.18%** |
| PHQ-9 | −0.307 | 9.45% |
| PSS-10 | −0.244 | 5.96% |

#### 2. Anxiety shows the strongest and most consistent relationship

GAD-7 has:
- The strongest correlation with CGPA
- The highest individual-model R²
- A significant coefficient in the combined mental-health model
- A significant coefficient in the full model

#### 3. Depression, anxiety and stress are highly interconnected

- PHQ-9 ↔ GAD-7 = **0.872**
- PHQ-9 ↔ PSS-10 = **0.849**
- GAD-7 ↔ PSS-10 = **0.807**

The VIF analysis confirms substantial overlap, making independent contributions harder to isolate in the combined model.

#### 4. Academic and Lifestyle Factors Substantially Improve the Model

Adding academic and lifestyle variables substantially improved the ability of the regression model to explain variation in CGPA.

| Model | Predictors | R² | Adjusted R² | Model Significance |
|---|---|---:|---:|---|
| **Model 1 — Mental Health Only** | PHQ-9 + GAD-7 + PSS-10 | **11.78%** | **11.22%** | p < 0.001 |
| **Model 2 — Full Model** | Mental health + academic + lifestyle factors | **41.46%** | **40.19%** | p < 0.001 |
| **Change** | Additional academic & lifestyle factors | **+29.67 pp** | **+28.97 pp** | Significant |

The full model explains substantially more variation in CGPA than the mental-health-only model.


#### 5. Key Academic and Lifestyle Predictors of CGPA

| Predictor | Coefficient (β) | p-value | Direction | Interpretation |
|---|---:|---:|---|---|
| **Weekly Study Hours** | **+0.0161** | **<0.001** | Positive | Higher study hours are associated with higher CGPA |
| **Class Attendance** | **+0.0088** | **<0.001** | Positive | Higher attendance is associated with higher CGPA |
| **Daily Sleep** | **+0.0649** | **<0.001** | Positive | Higher reported sleep is associated with higher CGPA |
| **Part-time Job** | **−0.1000** | **<0.001** | Negative | Students with a part-time job have a lower estimated CGPA |

All four predictors are statistically significant at the 5% significance level.


#### 6. Interpretation of Significant Predictors

| Factor | Statistical Finding | Interpretation |
|---|---|---|
| **Study Hours** | β = **+0.0161**, p < 0.001 | Each additional weekly study hour is associated with approximately **0.016 higher CGPA**, holding other variables constant. |
| **Class Attendance** | β = **+0.0088**, p < 0.001 | Each 1-percentage-point increase in attendance is associated with approximately **0.009 higher CGPA**, holding other variables constant. |
| **Daily Sleep** | β = **+0.0649**, p < 0.001 | Each additional hour of reported daily sleep is associated with approximately **0.065 higher CGPA**, holding other variables constant. |
| **Part-time Job** | β = **−0.1000**, p < 0.001 | Students with a part-time job have an estimated **0.10-point lower CGPA** than students without one, holding other variables constant. |

> **Note:** These coefficients represent statistical associations within the sample and should not be interpreted as causal effects.

#### 7. Overall Contribution of Academic and Lifestyle Factors

| Finding | Result |
|---|---:|
| Mental-health-only R² | **11.78%** |
| Full-model R² | **41.46%** |
| Increase in explained variation | **+29.67 percentage points** |
| Significant academic/lifestyle predictors | **Study Hours, Attendance, Sleep, Part-time Job** |
| Full-model observations | **475** |

The results indicate that **academic behaviour and lifestyle factors contribute substantially to explaining variation in CGPA beyond mental-health measures alone**.

#### 8. Part-time Employment and CGPA

| Part-time Employment | Regression Coding | Association with CGPA |
|---|---:|---:|
| **No** | 0 | Reference group |
| **Yes** | 1 | **−0.1000 CGPA** |

Students reporting a part-time job had an estimated **0.10-point lower CGPA** compared with students without a part-time job, after controlling for the other variables included in the full regression model.

This finding identifies an association rather than establishing that having a part-time job causes lower academic performance.

### Non-Significant Findings

In the full model, the following variables did not show statistically significant independent associations with CGPA at the 5% level:

- PHQ-9
- PSS-10
- Daily screen time
- Physical activity
- Financial stress

This does not mean these factors have no relationship with academic performance. It means the analysis did not find sufficient evidence of an independent association after accounting for the other variables in the model.

<br>

## Hypothesis Testing Summary

| Hypothesis | Result | Evidence |
|---|---|---|
| **H1:** Higher depression is associated with lower CGPA | **Supported** | PHQ-9 β = −0.0237, p < 0.001 |
| **H2:** Higher anxiety is associated with lower CGPA | **Supported** | GAD-7 β = −0.0329, p < 0.001 |
| **H3:** Higher perceived stress is associated with lower CGPA | **Supported** | PSS-10 β = −0.0162, p < 0.001 |
| **H4:** Mental-health variables collectively relate to CGPA | **Supported** | Model p < 0.001, R² = 11.78% |
| **H5:** Mental health alone explains the major variation in CGPA | **Not supported** | Full model R² = 41.46%; ΔR² = +29.67 pp |

<br>

## Overall Conclusion

<mark> **Mental health is associated with academic performance, but it is not the only factor associated with CGPA.** </mark>



Depression, anxiety and perceived stress each show significant negative associations with CGPA when examined individually.

Among the three, **anxiety shows the strongest and most consistent independent relationship with academic performance**.

However, academic and lifestyle factors add substantial explanatory value. Adding study hours, attendance, sleep, screen time, physical activity, financial stress and part-time employment increased the model's R² from **11.78% to 41.46%**.

In the full model:
- **GAD-7** remains negatively associated with CGPA.
- **Study hours** are positively associated with CGPA.
- **Class attendance** is positively associated with CGPA.
- **Sleep** is positively associated with CGPA.
- **Part-time employment** is negatively associated with CGPA.

The findings suggest that academic performance is associated with a **combination of mental-health, academic behaviour and lifestyle factors**, rather than mental health alone.

> **Important:** These results describe statistical associations within the sample and do not establish causal relationships.

<br>

## Limitations

#### 1. Observational data

The dataset is observational, so regression results identify associations rather than causal relationships.
For example, the relationship between sleep and CGPA does not establish that increasing sleep will necessarily increase CGPA.

#### 2. Missing CGPA values

There were **12 missing CGPA observations (2.4%)**, concentrated among Year 1 students.
Because CGPA is the dependent variable, these observations were excluded rather than imputed.

#### 3. Multicollinearity

PHQ-9, GAD-7 and PSS-10 are strongly correlated.

PHQ-9 had an elevated VIF of approximately **5.49**.

Therefore, individual coefficients in the combined mental-health model should be interpreted cautiously.

#### 4. Self-reported variables

Variables such as sleep, study hours, screen time and physical activity are self-reported and may contain reporting or recall bias.

#### 5. Generalizability

The findings are based on the available student sample and should not automatically be generalized to all college students or other populations.

<br>

## Tools & Techniques

| Microsoft Excel | Statistical Techniques |
|---|---|
| Data cleaning | Descriptive statistics |
| Data validation | Exploratory Data Analysis |
| Pivot Tables | Pearson correlation |
| Descriptive statistics | Variance Inflation Factor (VIF) |
| Missingness analysis | Simple linear regression |
| Correlation analysis | Multiple linear regression |
| VIF calculation | R², Adjusted R² and ΔR²|
| Regression analysis | p-values |
| Dashboard development | Model comparison |

<br>

## Workbook Structure

| Sheet | Purpose |
|---|---|
| **Raw Data** | Original dataset |
| **Cleaned Data** | Cleaned and standardized dataset |
| **EDA** | Descriptive statistics, missingness and distributions |
| **Correlation** | Pearson correlation matrix |
| **Statistical Analysis** | VIF and regression analysis |
| **Dashboard** | Visual summary of the project |
| **Insights** | Hypotheses, findings and conclusions |

<br>

## Project Files

Recommended repository structure:

```text
mental-health-academic-performance-analysis/
│
├── README.md
│
├── data/
│   └── mental_health_survey.csv
│
├── excel/
│   └── Mental_Health_Student_Analysis.xlsx
│
└── dashboard/
    ├── dashboard.png
    ├── statistical_analysis.png
    └── insights.png


```

---

<br> <br> <br>

> [!NOTE]
> This project is intended as an analytical case study demonstrating exploratory analysis, statistical reasoning and Excel-based modelling.
> The findings represent associations observed within the dataset and should not be interpreted as medical, psychological or causal conclusions.


























