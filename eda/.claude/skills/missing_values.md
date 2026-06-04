# Skill: missing_values

## Purpose
Identify and evaluate missing values token-efficiently.

## When to Apply
Always run after ingestion, before outlier screening.

## Code
```python
missing = pd.DataFrame({
    "absolute": df.isna().sum(),
    "percent": (df.isna().mean() * 100).round(2)
}).query("absolute > 0").sort_values("percent", ascending=False)

print(missing if not missing.empty else "No missing values found")
print(f"Affected rows: {df.isna().any(axis=1).sum()}")
print(f"Complete rows: {df.notna().all(axis=1).sum()}")
```

## Evaluation Rules
- < 5% missing - flag in report, recommend median/mode imputation
- 5-20% missing - flag in report, recommend imputation with caution
- > 20% missing - flag in report, recommend dropping or advanced imputation
- > 50% missing - flag in report, recommend dropping

## Missing Pattern Check
For columns with > 5% missing, check if missingness correlates with other columns:

```python
# Check if missing values cluster in specific groups
for col in missing[missing["percent"] > 5].index:
    for other_col in df.select_dtypes("number").columns:
        correlation = df[col].isna().corr(df[other_col])
        if abs(correlation) > 0.3:
            print(f"{col} missing correlates with {other_col}: {correlation:.2f}")
```

Report finding as:
- "missing values in [col] appear random" - if no correlations found
- "missing values in [col] correlate with [other_col]" - if pattern found (likely MNAR)

## Decision
Never impute or drop automatically.
All missing value decisions go to the manager via the Cleaning Agenda.

## Output for Manager
Flag all columns above 20% threshold as requiring manager decision.
Never impute or drop automatically - pass recommendations to eda_report.

## Self-Improvement
Append new missing value patterns here.
