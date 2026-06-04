# Skill: imputation

## Purpose
Impute missing values using manager-confirmed methods.

## When to Apply
Sixth and last cleaning step, after outlier_handling.

## Code
```python
for col, method in confirmed_imputations.items():
    if col not in df.columns:
        print(f"SKIP: {col} not found")
        continue

    missing_before = df[col].isna().sum()
    if missing_before == 0:
        print(f"{col}: no missing values, skipping")
        continue

    if method == "median":
        fill_value = df[col].median()
    elif method == "mode":
        fill_value = df[col].mode().iloc[0]
    elif method == "mean":
        fill_value = df[col].mean()

    df[col] = df[col].fillna(fill_value)
    missing_after = df[col].isna().sum()

    print(f"{col} ({method}): {missing_before} -> {missing_after} missing, fill value: {fill_value:.4f}")
```

## Log Entry
```
## IMPUTATION - [TIMESTAMP]
Action: imputed missing values
Columns imputed: [col: method, fill_value, n_filled]
Missing remaining: [n or "0"]
Decision source: manager confirmed
```

## Notes
- Always impute last - after outlier handling so fill values are not skewed by outliers
- Never impute columns not in confirmed_imputations
- Log fill values explicitly for reproducibility

## Self-Improvement
Append new imputation patterns here.
