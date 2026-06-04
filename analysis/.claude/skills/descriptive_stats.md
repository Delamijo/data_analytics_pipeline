# Skill: descriptive_stats

## Purpose
Run descriptive statistics relevant to the research question.

## When to Apply
Always run after load_data, for all question types.

## Code
```python
num_cols = df.select_dtypes("number")
cat_cols = df.select_dtypes(["object", "category"])

# Numeric summary
print("=== Numeric Summary ===")
print(num_cols.describe().T.round(3))

# Distribution shape (skewness, kurtosis)
print("\n=== Distribution Shape ===")
shape = pd.DataFrame({
    "skewness": num_cols.skew().round(3),
    "kurtosis": num_cols.kurt().round(3)
})
print(shape)

# Categorical summary
if not cat_cols.empty:
    print("\n=== Categorical Summary ===")
    for col in cat_cols.columns:
        print(f"\n{col}:")
        print(df[col].value_counts(normalize=True).round(3).head(10))
```

## Decision Rules
- Skewness > 1 or < -1 - flag as highly skewed, note in findings
- Kurtosis > 3 - flag as heavy-tailed, note in findings
- For predictive questions - always describe target variable distribution separately

```python
# Target variable distribution (predictive only)
if question_type == "predictive" and target_variable:
    print(f"\n=== Target Variable: {target_variable} ===")
    if df[target_variable].dtype in ["object", "category"]:
        print(df[target_variable].value_counts(normalize=True).round(3))
    else:
        print(df[target_variable].describe().round(3))
        print(f"Skewness: {df[target_variable].skew():.3f}")
```

## Notes
- Never print full DataFrames, only summaries
- Flag skewed distributions in findings - they affect model choice

## Self-Improvement
Append new descriptive patterns here.
