# Skill: correlation_analysis

## Purpose
Analyze relationships between variables relevant to the research question.

## When to Apply
After descriptive_stats, for all question types.

## Method Selection Rules
- Both variables numeric, roughly normal - Pearson
- One or both variables skewed (skewness > 1) - Spearman
- One variable categorical (binary) - Point-biserial via Pearson
- Both variables categorical - Cramers V

## Code
```python
num_cols = df.select_dtypes("number")

# Pearson correlation matrix (numeric only)
corr_matrix = num_cols.corr(method="pearson")

# Show only top correlations (absolute value > 0.3)
corr_pairs = (corr_matrix
    .unstack()
    .reset_index()
    .rename(columns={"level_0": "var1", "level_1": "var2", 0: "correlation"})
    .query("var1 != var2")
    .assign(abs_corr=lambda x: x["correlation"].abs())
    .sort_values("abs_corr", ascending=False)
    .drop_duplicates(subset=["abs_corr"])
    .head(20)
)
print("=== Top Correlations (|r| > 0.3) ===")
print(corr_pairs[corr_pairs["abs_corr"] > 0.3].round(3))

# Spearman for skewed columns
skewed = num_cols.columns[num_cols.skew().abs() > 1].tolist()
if skewed and target_variable:
    print(f"\n=== Spearman Correlations with {target_variable} (skewed columns) ===")
    spearman = df[skewed + [target_variable]].corr(method="spearman")[target_variable].drop(target_variable)
    print(spearman.sort_values(key=abs, ascending=False).round(3))
```

## Interpretation Rules
- |r| > 0.7 - strong correlation, flag in findings
- |r| 0.3-0.7 - moderate correlation, note in findings
- |r| < 0.3 - weak correlation, omit from findings unless relevant to research question

## Notes
- Never print full correlation matrix - only top N pairs
- Always note which method was used and why

## Self-Improvement
Append new correlation patterns here.
