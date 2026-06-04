# Skill: group_comparison

## Purpose
Compare numeric variables across categorical groups relevant to the research question.

## When to Apply
After correlation_analysis, when categorical columns exist and research question implies group differences.

## Method Selection Rules
- 2 groups, normal distribution - t-test
- 2 groups, skewed - Mann-Whitney U
- 3+ groups, normal - ANOVA
- 3+ groups, skewed - Kruskal-Wallis

## Code
```python
from scipy import stats

cat_cols = df.select_dtypes(["object", "category"]).columns
num_cols = df.select_dtypes("number").columns

# Only compare groups for low-cardinality categoricals
for cat_col in cat_cols:
    if df[cat_col].nunique() > 10:
        continue

    groups = df[cat_col].dropna().unique()
    n_groups = len(groups)

    for num_col in num_cols:
        if num_col == target_variable:
            continue

        group_data = [df[df[cat_col] == g][num_col].dropna() for g in groups]
        skewed = abs(df[num_col].skew()) > 1

        if n_groups == 2:
            if skewed:
                stat, p = stats.mannwhitneyu(*group_data, alternative="two-sided")
                method = "Mann-Whitney U"
            else:
                stat, p = stats.ttest_ind(*group_data)
                method = "t-test"
        else:
            if skewed:
                stat, p = stats.kruskal(*group_data)
                method = "Kruskal-Wallis"
            else:
                stat, p = stats.f_oneway(*group_data)
                method = "ANOVA"

        if p < 0.05:
            print(f"{num_col} by {cat_col} ({method}): p={p:.4f} - significant")
```

## Interpretation Rules
- p < 0.05 - statistically significant difference, flag in findings
- p < 0.01 - highly significant, flag prominently
- Always report which test was used and why

## Notes
- Skip high-cardinality categoricals (nunique > 10)
- Only report significant results in findings

## Self-Improvement
Append new group comparison patterns here.
