# Skill: categorical_profile

## Purpose
Analyze categorical columns token-efficiently.

## When to Apply
Always run after outlier_screen, before memory_optimize.

## Code
```python
cat_cols = df.select_dtypes(["object", "category"])

# Cardinality overview
cardinality = pd.DataFrame({
    "unique": cat_cols.nunique(),
    "unique_pct": (cat_cols.nunique() / len(df) * 100).round(2),
    "top_value": cat_cols.mode().iloc[0],
    "top_frequency": [df[col].value_counts(normalize=True).iloc[0].round(3)
                      for col in cat_cols.columns]
})
print(cardinality)

# Top values only for low cardinality columns
for col in cat_cols.columns:
    if cat_cols[col].nunique() <= 20:
        print(f"\n{col}:")
        print(df[col].value_counts(normalize=True).round(3))

# Duplicates
print(f"\nDuplicates: {df.duplicated().sum()}")
```

## Encoding Check
For each categorical column, check for common mojibake patterns:

```python
for col in cat_cols.columns:
    sample = df[col].dropna().astype(str)
    if sample.str.contains("Ã|â€|Â").any():
        print(f"{col}: possible encoding issue detected")
```

## Value Consistency Check
```python
for col in cat_cols.columns:
    stripped = df[col].str.strip()
    if (stripped != df[col]).any():
        print(f"{col}: leading/trailing whitespace found")
    
    lower = df[col].str.lower()
    if lower.nunique() < df[col].nunique():
        print(f"{col}: case inconsistency found (e.g. 'Yes' vs 'yes')")
```

## Evaluation Rules
- unique_pct > 80% - likely ID column, recommend dropping to manager
- unique_pct < 5% - good candidate for category dtype
- top_frequency > 95% - column nearly constant, likely useless

## Decision
Never drop, merge, or encode categories automatically.
All decisions go to the manager via the Cleaning Agenda.

## Output for Manager
Flag high cardinality columns as potential ID columns for manager decision.
Never drop automatically - pass recommendations to eda_report.

## Self-Improvement
Append domain-specific cardinality patterns here.
