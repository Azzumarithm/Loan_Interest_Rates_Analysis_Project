# Dataset from https://www.kaggle.com/datasets/marcobeyer/bondora-p2p-loans?select=LoanData.csv


Insights gathered:
<!-- ...existing code... -->

- Key risk indicator
  - DebtToIncome (Amount / IncomeTotal) >= 0.35 combined with very short employment (TrialPeriod or UpTo1Year) identifies a meaningful "risky" borrower segment. Use this as a quick underwriting flag for additional checks or higher pricing.

- Pricing / underwriting adjustments
  - Loans flagged as risky have a higher mean interest — consider:
    - Adding a pricing surcharge for flagged borrowers, or
    - Requiring stronger collateral/verification before approval.

- Rating is predictive
  - Rating (ordinal) is an important predictor of Interest in multivariate regression. Use rating to tier pricing and pre-approve limits.

- Prior loan history
  - AmountOfPreviousLoansBeforeLoan shows a statistically detectable relationship with Interest in the simple model. Incorporate prior-loan count/amount into risk scoring (not just binary history).

- Limited value from raw income alone
  - IncomeTotal has weaker correlation with Interest than DebtToIncome; prefer ratio-based features (DTI) over raw income for risk decisions.

- Education differences
  - Interest distributions vary by Education level. Use education as a segmentation variable for targeted offers or different risk models.

- Operational thresholds and monitoring
  - Monitor proportion of loans where Amount < AppliedAmount — a high rate could indicate origination process issues or credit tightening.
  - Track DTI distribution and share of "IsRisky" loans over time; set alerts if the risky share rises materially.

- Modeling & validation
  - Before deploying scoring/pricing changes:
    - Check regression diagnostics (residuals, heteroskedasticity), multicollinearity (VIF), and stability across time/rating cohorts.
    - Validate with cross-validated predictive models (e.g., logistic/gradient-boosted classifiers for default risk, or regression for interest prediction).
    - Consider ordinal encoding for Rating, and treat skewed predictors (log transforms) as needed.

- Quick next experiments
  - Build a classification model for default or delinquency using DTI, Rating, EmploymentDurationCurrentEmployer, and prior-loan features.
  - A/B test adjusted pricing for the flagged risky group to measure impact on default rates and yield.

