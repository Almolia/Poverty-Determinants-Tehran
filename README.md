# Poverty Determinants in Tehran: A Statistical Analysis

[![Python](https://img.shields.io/badge/Python-3.11+-blue.svg)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

An in-depth statistical analysis of household poverty factors in Tehran using **Logistic Regression** and **Generalized Additive Models (GAMs)** to identify key socio-economic determinants of poverty across three years (1399-1401 Persian calendar / 2020-2022).

## 📊 Project Overview

This research project analyzes household survey data from Tehran to understand the primary drivers of poverty using advanced statistical modeling techniques. The analysis uses **relative poverty** measures (60% of median equivalized expenditure) and accounts for survey weights to provide representative estimates.

### Key Research Questions
- What are the primary socio-economic predictors of household poverty in Tehran?
- How do education, employment status, and household composition affect poverty risk?
- Are there non-linear relationships between predictors and poverty outcomes? 

---

## 🎯 Key Findings

### Major Poverty Risk Factors (Increasing Poverty)

| Factor | Odds Ratio | Marginal Effect | Interpretation |
|--------|------------|-----------------|----------------|
| **Illiteracy** | 6.78x | +27.1% | Illiterate household heads have **6.8 times higher odds** of poverty |
| **Less than Diploma** | 2.70x | +14.1% | Under-diploma education increases poverty odds by 170% |
| **Unemployed Head** | 2.54x | +13.2% | Unemployment increases poverty probability by 13.2 percentage points |
| **Housekeeper Head** | 2.50x | +13.0% | Households headed by housekeepers face 2.5x poverty risk |
| **High Dependency Ratio** | 1.66x | +7.2% | Each unit increase in employment dependency ratio increases poverty by 7.2% |

### Major Protective Factors (Reducing Poverty)

| Factor | Odds Ratio | Marginal Effect | Interpretation |
|--------|------------|-----------------|----------------|
| **Master's/PhD Education** | 0.15x | -26.8% | Advanced degrees reduce poverty odds by **85%** |
| **Bachelor's Degree** | 0.39x | -13.5% | Bachelor's education reduces poverty probability by 13.5% |
| **Home Ownership (Other)** | 0.24x | -20.5% | Certain ownership types reduce poverty odds by 76% |
| **Urban Residence** | 0.37x | -14.1% | Urban households have 63% lower poverty odds than rural |
| **Imputed Rent Income** | 0.36x | -14.6% | Higher imputed rent (home value) strongly reduces poverty |
| **Tenant Status** | 0.59x | -7.5% | Renting reduces poverty compared to other arrangements |

### Model Performance

#### Logistic Regression Models
- **Pseudo R² (McFadden's)**: 0.295 - 0.297
- **Sample Size**: 4,275 households
- **Significant Predictors**: 15 out of 36 variables
- **Model Selection**: Stepwise removal of non-significant variables (household size, marital status, gender)

**⚠️ Limitation**: All logistic models failed the **Hosmer-Lemeshow goodness-of-fit test** (p < 0.005), indicating that linear assumptions don't fully capture the data complexity.

#### Generalized Additive Model (GAM)
- **Pseudo R²**: 0.413 (significantly improved)
- **AIC**: 5,111.63
- **Effective DoF**: 148. 05
- **All Features**:  Statistically significant (p < 0.001)

**✅ Key Advantage**: GAM captures **non-linear relationships** that logistic regression misses, particularly for: 
- **Age effects**:  Poverty risk decreases non-linearly with household head age
- **Income shares**: Non-linear effects of different income sources
- **Dependency ratios**: Complex interactions with poverty outcomes

---

## 🛠️ Technical Stack

### Core Technologies
```python
# Statistical Modeling
- statsmodels          # Logistic regression, model diagnostics
- pygam                # Generalized Additive Models
- scipy                # Statistical tests

# Data Processing
- pandas               # Data manipulation
- numpy                # Numerical computations
- scikit-learn         # Preprocessing (StandardScaler)

# Visualization
- matplotlib           # Plotting
- seaborn             # Statistical graphics
```

### Key Methodological Features

1. **Survey Weights Implementation**
   - Proper handling of household weights for representative estimates
   - Weighted median calculation for poverty line definition
   - Weight normalization for regression analysis

2. **Relative Poverty Definition**
   - **Poverty line**:  60% of weighted median equivalized expenditure
   - **OECD Modified Scale**:  Accounts for household composition
   - Year-specific poverty thresholds (inflation-adjusted)

3. **Model Diagnostics**
   - **Variance Inflation Factor (VIF)**: Multicollinearity detection (all VIF < 8)
   - **Hosmer-Lemeshow Test**:  Goodness-of-fit assessment
   - **Marginal Effects**: Interpretable effect sizes
   - **Robust Standard Errors**: HC1 heteroskedasticity-consistent covariance

4. **GAM Features**
   - **Spline smoothing**: s(x) for continuous variables
   - **Factor terms**: f(x) for categorical variables
   - **Grid search optimization**: Automatic lambda tuning
   - **Partial dependence plots**: Visual interpretation of effects

---

## 📁 Project Structure

```
Poverty-Determinants-Tehran/
│
├── Data/
│   ├── Tehran_1399.zip              # 2020 household survey data
│   ├── Tehran_1400.zip              # 2021 household survey data
│   ├── Tehran_1401.zip              # 2022 household survey data
│   ├── Tehran_1399_prepared.csv     # Processed 2020 data
│   ├── Tehran_1400_prepared.csv     # Processed 2021 data
│   ├── Tehran_1401_prepared. csv     # Processed 2022 data
│   ├── Tehran_all_prepared.csv      # Combined dataset
│   └── Data Preparation.ipynb       # Data preprocessing pipeline
│
├── Model/
│   ├── Code. ipynb                   # Main analysis notebook
│   └── Code. html                    # HTML export of analysis
│
├── Utils/
│   └── Sharif_logo.png              # University logo
│
├── Report.pdf                       # Detailed research report
└── references.txt                   # Bibliography
```

---

## 🚀 Getting Started

### Prerequisites
```bash
pip install pandas numpy statsmodels scikit-learn scipy pygam matplotlib seaborn
```

### Data Preparation
```bash
# Run the data preparation notebook
jupyter notebook Data/Data\ Preparation.ipynb
```

This will:
1. Extract household survey data from zip files
2. Process income breakdowns into meaningful categories
3. Calculate dependency ratios and equivalence scales
4. Generate prepared CSV files for analysis

### Running the Analysis

```bash
# Open the main analysis notebook
jupyter notebook Model/Code. ipynb
```

The notebook performs:
1. **Section 1**: Logistic Regression model building and selection
2. **Section 2**:  GAM fitting with automatic hyperparameter tuning
3. Comprehensive model diagnostics and visualization

---

## 📈 Variables Used

### Dependent Variable
- `Is_Poor`: Binary indicator (1 = household below poverty line)

### Independent Variables

**Household Head Demographics:**
- `Head_Age`: Categorical (20-30, 30-40, 40-50, 50-60, 60-70, 70+)
- `Head_Age_`: Continuous age (used in Model 3)
- `Head_Sex`: Male/Female
- `Head_Education_Level`: Illiterate, Under_Diploma, Diploma, Bachelor, Masters_PhD
- `Head_Activity_Status`: Employed, Unemployed, Housekeeper, Other, Income_without_Work
- `Head_Marital_Status`: Married, Bachelor, Divorced, Widowed

**Household Structure:**
- `Household_Size`: 1-7, Above 7
- `Dependency_Ratio_Age`: (Age <15 or >64) / Working age population
- `Dependency_Ratio_Employment`: Non-employed / Employed members

**Income Composition (shares of total income):**
- `Aid_Income_Share`: Subsidies, aid, transfers, donations
- `Labor_Income_Share`: Wages and salaries
- `Pension_Income_Share`: Retirement income
- `Capital_Income_Share`: Rent, interest
- `Imputed_Rent_Share`: Estimated value of owner-occupied housing
- `Other_NonCash_Income_Share`: Other non-cash benefits

**Housing & Location:**
- `Tenure_Type`: Owner, Tenant, Other (free/service)
- `Urban_Rural`: Urban/Rural
- `Year`: 1399, 1400, 1401 (Persian calendar)

---

## 📊 Model Evolution

### Model 1: Full Specification
- **All variables included**
- **Result**: Many non-significant predictors (household size, marital status, gender)
- **Pseudo R²**: 0.297

### Model 2: Refined Model
- **Removed**: `Head_Sex`, `Head_Marital_Status`, `Household_Size`, `Year`
- **Result**: Similar performance, better parsimony
- **Pseudo R²**: 0.295

### Model 3: Non-linear Age
- **Added**: `Head_Age` (continuous) + `Head_Age²`
- **Result**: Captures non-linear age effects
- **Pseudo R²**: 0.295
- **Key insight**:  Poverty decreases with age but effect diminishes

### Model 4: GAM (Final Model)
- **Approach**: Splines for continuous vars, factors for categorical
- **Result**: Best fit, captures complexity
- **Pseudo R²**:  0.413 ⭐
- **Advantage**: No failed goodness-of-fit, flexible functional forms

---

## 📚 Data Sources

The analysis uses household income and expenditure survey data from Tehran province (1399-1401 / 2020-2022). Data includes: 
- Household composition and member characteristics
- Detailed income sources and amounts
- Expenditure patterns
- Housing characteristics
- Survey weights for population representation

---

## 👥 Authors

| Name | Student ID | Major |
|------|------------|-------|
| 👨🏻 Ali M. Shabestari | 401106482 | Computer Engineering |
| 🧔🏻‍♂️ Arshia Dehghan | 401100382 | Computer Science |
| 🧔🏻 Abolfazl Moslemi | 401100506 | Computer Science |

**Institution**:  Sharif University of Technology  
**Course**:  Regression Analysis - Summer 2025

---

## 📄 Full Report

For detailed methodology, theoretical framework, and extended results, see [`Report.pdf`](Report.pdf).

---

## 🔍 Interpretation Guide

### Understanding Odds Ratios
- **OR > 1**: Factor increases poverty risk
- **OR < 1**: Factor decreases poverty risk
- **OR = 1**: No effect

**Example**:  Illiteracy OR = 6.78 means illiterate heads have 6.78 times the odds of poverty compared to the reference group (Diploma holders).

### Understanding Marginal Effects
Marginal effects show the **percentage point change** in poverty probability from a one-unit increase in the predictor (for continuous) or switching from the reference category (for categorical).

**Example**: Unemployment has a marginal effect of +13.2%, meaning unemployed households are 13.2 percentage points more likely to be poor than employed ones.

---

## 📖 References

See [`references.txt`](references.txt) for complete bibliography.

---

## 📝 License

This project is part of academic research at Sharif University of Technology. 

---

## 🤝 Contributing

This is a completed academic project. For questions or collaborations, please contact the authors.

---

**Note**: All poverty measurements are based on **relative poverty** (60% of median), not absolute poverty lines. 
