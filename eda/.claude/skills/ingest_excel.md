# Skill: ingest_excel

## Purpose
Read an Excel file token-efficiently and return basic dataset info.

## When to Apply
Dataset is a .xlsx or .xls file.

## Code
```python
import pandas as pd
from pathlib import Path

path = Path("data/[dataset]")

# Check sheet names before loading
xl = pd.ExcelFile(path)
print(f"Sheets: {xl.sheet_names}")

# Load first sheet by default
df = pd.read_excel(path, sheet_name=0)

print(f"Shape: {df.shape}")
print(f"Memory: {df.memory_usage(deep=True).sum() / 1024**2:.2f} MB")
print(df.dtypes)
```

## Notes
- Always check sheet names before loading
- Use header=1 if column names are not in row 1
- Use skiprows=N for files with metadata at the top
- Never rebuild file contents as a string in code, always use pd.read_excel

## Self-Improvement
Append new Excel-specific patterns here.
