# Dataset from https://www.kaggle.com/datasets/marcobeyer/bondora-p2p-loans?select=LoanData.csv


# Bondora P2P Loan Interest Rate Prediction

## 1\. Project Overview

This project is the capstone for the "Loan Interest Rates" course. It simulates a data science role within a fintech startup aimed at optimizing customer loan offers. By analyzing the **Bondora P2P Loans dataset**, the core objective is to:

1.  Identify the key factors influencing a loan's interest rate.
2.  Develop a robust **Ordinary Least Squares (OLS) regression model** to predict personalized interest rates for new applications.

## 2\. Business Problem

The challenge is to balance profitability with risk management. The analysis seeks to provide actionable insights by:

  * Identifying "high-risk" customer segments based on debt and employment stability.
  * Quantifying the impact of demographic (e.g., Education) and financial history (e.g., Previous Loans) features on the interest rate.
  * Delivering a statistically sound model to personalize loan terms and maximize efficiency.

## 3\. Dataset & Key Features

The analysis utilizes the Bondora P2P Loans dataset. The most relevant features are:

| Feature | Description | Type |
| :--- | :--- | :--- |
| `LoanId` | Unique identifier (used as the DataFrame index) | Identifier |
| `Interest` | **Target Variable:** The interest rate of the loan (%) | Numeric |
| `Rating` | Bondora credit rating issued by their internal model (AA, B, C, D, E, F, HR) | Categorical |
| `AppliedAmount` | Amount the borrower requested | Numeric |
| `LoanDuration` | The term of the loan (months) | Numeric |
| `Education` | Education level of the borrower (e.g., Vocational, Higher) | Categorical |
| `IncomeTotal` | Total borrower income | Numeric |
| `EmploymentDuration...` | Employment time with current employer | Categorical |
| `DebtToIncome` | Calculated feature: Ratio of Amount to IncomeTotal | Numeric |
| `AmountOfPreviousLoansBeforeLoan` | Value of previous loans held | Numeric |

## 4\. Methodology & Findings

### 4.1. Data Preparation and Initial Metrics

  * **Average Interest Rate (Sample Mean):** **27.29%**
  * **Loan Fulfillment:** Initial analysis showed 10,543 cases where the borrower received a lower amount than they applied for.
  * **Confidence Interval:** The 95% confidence interval for the proportion of under-fulfilled loans was calculated to be between **2.7% and 2.8%**.

### 4.2. Risk Segmentation

A custom flag, `IsRisky`, was engineered to isolate high-risk borrowers:

$$
\text{IsRisky} = 
\begin{cases} 
\text{True} & \text{if } (\text{DebtToIncome} \ge 0.35) \text{ AND } (\text{EmploymentDurationCurrentEmployer} \in \{\text{TrialPeriod}, \text{UpTo1Year}\}) \\
\text{False} & \text{otherwise}
\end{cases}
$$

  * **Result:** High-risk loans (15.88% of the data) carried an average interest rate of **28.86%**, demonstrating a clear risk premium compared to non-risky loans (26.99%).

### 4.3. Predictive Modeling (OLS Regression)

Two models were tested, with the complex model proving superior:

| Model | Predictors | $R^2$ Score | Key Insight |
| :--- | :--- | :--- | :--- |
| **Simple Linear Reg.** | `AmountOfPreviousLoansBeforeLoan` | 0.031 | Explained only 3.1% of variance. Insufficient. |
| **Complex Linear Reg. (Final)** | `Rating`, `Education`, `LoanDuration`, `DebtToIncome`, etc. | **0.604** | Explains 60.4% of variance. Statistically robust. |

**Final Model Summary:**

The **Bondora Rating** was the most influential variable. The coefficient analysis showed that moving from a top rating (AA) to the riskiest rating (HR) dramatically increased the interest rate component:

  * `Rating_AA` Coefficient: **-3.47** (reduces interest)
  * `Rating_HR` Coefficient: **+62.28** (drastically increases interest)

## 5\. Technology Stack

  * **Language:** Python 3
  * **Data Manipulation:** `pandas`
  * **Statistical Modeling:** `statsmodels` (for OLS regression)
  * **Visualization:** `matplotlib.pyplot`, `seaborn`
  * **Statistical Methods:** `scipy.stats`

