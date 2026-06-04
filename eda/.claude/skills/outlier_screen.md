# Skill: outlier_screen

## Purpose
Detect outliers and implausible values token-efficiently.

## When to Apply
Always run after missing_values, before categorical_profile.

## Code
```python
num_cols = df.select_dtypes("number")

# Overview
overview = pd.DataFrame({
    "min": num_cols.min(),
    "max": num_cols.max(),
    "negative_values": (num_cols < 0).sum(),
    "zeros": (num_cols == 0).sum(),
    "mean": num_cols.mean().round(2),
    "std": num_cols.std().round(2)
})
print(overview)

# IQR-based outlier count
Q1 = num_cols.quantile(0.25)
Q3 = num_cols.quantile(0.75)
IQR = Q3 - Q1
outliers_iqr = ((num_cols < (Q1 - 1.5 * IQR)) |
                (num_cols > (Q3 + 1.5 * IQR))).sum()

print("\nOutliers per IQR:")
print(outliers_iqr[outliers_iqr > 0])
```

## Outlier Pattern Check
For columns with critical outliers, check if they cluster in specific groups:

```python
for col in critical_outlier_cols:
    is_outlier = (df[col] < (Q1[col] - 1.5 * IQR[col])) | \
                 (df[col] > (Q3[col] + 1.5 * IQR[col]))
    for cat_col in df.select_dtypes(["object", "category"]).columns:
        if df[cat_col].nunique() <= 20:
            print(f"\nOutliers in {col} by {cat_col}:")
            print(df.groupby(cat_col)[col].apply(lambda x: is_outlier.loc[x.index].sum()))
```

## Evaluation Rules
- Negative values in age, price, weight - likely data error
- IQR outliers > 5% of rows - flag for manager decision
- min == max - column is constant, likely useless
- Values outside domain plausibility (age > 120) - flag as data error

## Decision
Never clip, drop, or modify outliers automatically.
All outlier decisions go to the manager via the Cleaning Agenda.

## Output for Manager
Flag all columns with critical outliers as requiring manager decision.
Never remove or clip automatically, pass recommendations to eda_report.

## Self-Improvement
Append domain-specific plausibility rules here when known.
