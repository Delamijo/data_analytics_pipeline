# Skill: outlier_handling

## Purpose
Handle confirmed outliers through IQR clipping or row dropping.

## When to Apply
Fifth cleaning step, after value_consistency.

## Code
```python
for col, method in confirmed_outlier_handling.items():
    if col not in df.columns:
        print(f"SKIP: {col} not found")
        continue

    rows_before = len(df)
    before_stats = f"min={df[col].min():.2f}, max={df[col].max():.2f}"

    if method == "IQR-clip":
        Q1 = df[col].quantile(0.25)
        Q3 = df[col].quantile(0.75)
        IQR = Q3 - Q1
        lower = Q1 - 1.5 * IQR
        upper = Q3 + 1.5 * IQR
        df[col] = df[col].clip(lower=lower, upper=upper)

    elif method == "drop":
        Q1 = df[col].quantile(0.25)
        Q3 = df[col].quantile(0.75)
        IQR = Q3 - Q1
        mask = (df[col] >= Q1 - 1.5 * IQR) & (df[col] <= Q3 + 1.5 * IQR)
        df = df[mask]

    after_stats = f"min={df[col].min():.2f}, max={df[col].max():.2f}"
    rows_removed = rows_before - len(df)

    print(f"{col} ({method}): {before_stats} -> {after_stats}, rows removed: {rows_removed}")
```

## Log Entry
```
## OUTLIER HANDLING - [TIMESTAMP]
Action: [IQR-clip / drop] outliers
Columns affected: [list]
Before: [min/max per column]
After: [min/max per column]
Rows removed: [n or "0 (clipped)"]
Decision source: manager confirmed
```

## Notes
- IQR-clip preserves all rows, drop removes them permanently
- Always log before/after min/max as evidence of effect
- Never apply to columns not in confirmed_outlier_handling

## Self-Improvement
Append new outlier handling patterns here.
