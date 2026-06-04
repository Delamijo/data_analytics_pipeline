# Skill: plan_dashboard

## Purpose
Plan which charts to generate based on question type and available data.

## When to Apply
Always the first step before generating the app.

## Code
```python
import pandas as pd
from pathlib import Path

output_base = Path(f"output/{dataset_name}")

# Read both datasets once
df = pd.read_parquet(output_base / "data/cleaned.parquet")
results = pd.read_parquet(output_base / "data/results.parquet")
findings = (output_base / "findings.md").read_text(encoding="utf-8")

# Profile available columns
num_cols = df.select_dtypes("number").columns.tolist()
cat_cols = df.select_dtypes(["object", "category"]).columns.tolist()
results_cols = results.columns.tolist()

print(f"Cleaned - numeric: {num_cols}")
print(f"Cleaned - categorical: {cat_cols}")
print(f"Results columns: {results_cols}")
print(f"Cleaned shape: {df.shape}")
print(f"Results shape: {results.shape}")
```

## Chart Plan by Question Type

### Descriptive
```
charts = [
    {"type": "histogram", "col": col, "source": "cleaned"} for col in num_cols[:4],
    {"type": "bar", "col": col, "source": "cleaned"} for col in cat_cols[:2],
    {"type": "heatmap", "cols": num_cols, "source": "cleaned"}
]
```

### Predictive
```
charts = [
    {"type": "histogram", "col": target_variable, "source": "cleaned"},
    {"type": "feature_importance", "source": "results"},
    {"type": "scatter_actual_pred", "source": "results"},
    {"type": "confusion_matrix", "source": "results"} if classification
      else {"type": "residual_plot", "source": "results"}
]
```

### Explanatory
```
charts = [
    {"type": "feature_importance", "source": "results"},
    {"type": "heatmap", "cols": num_cols, "source": "cleaned"},
    {"type": "group_bar", "cat": cat_cols[0], "num": num_cols[0], "source": "cleaned"}
]
```

## Notes
- Limit to max 6 charts per dashboard for readability
- Skip chart types if required columns are missing in the source dataset
- Store chart plan and both DataFrames for generate_app skill

## Self-Improvement
Append new chart planning patterns here.
