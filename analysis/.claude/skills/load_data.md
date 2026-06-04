# Skill: load_data

## Purpose
Load and verify the cleaned dataset before analysis.

## When to Apply
Always the first step. Read cleaned.parquet once - never reload during analysis.

## Code
```python
import pandas as pd
from pathlib import Path

parquet_path = Path(f"output/{dataset_name}/data/cleaned.parquet")

if not parquet_path.exists():
    raise FileNotFoundError(f"cleaned.parquet not found: {parquet_path}")

df = pd.read_parquet(parquet_path)

print(f"Shape: {df.shape}")
print(f"Memory: {df.memory_usage(deep=True).sum() / 1024**2:.2f} MB")
print(df.dtypes)

# Verify target variable exists if predictive question
if question_type == "predictive" and target_variable:
    if target_variable not in df.columns:
        print(f"WARNING: target variable '{target_variable}' not found - switching to descriptive")
        question_type = "descriptive"
```

## Notes
- Store df in memory for the entire analysis session
- Never reload from disk after this step
- Log warning if target variable missing and switch question type

## Self-Improvement
Append new load patterns here.
