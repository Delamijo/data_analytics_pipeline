# Skill: ingest_csv

## Purpose
Read a CSV file token-efficiently and return basic dataset info.

## When to Apply
Dataset is a .csv file.

## Code
```python
import pandas as pd
from pathlib import Path

path = Path("data/[dataset]")

# Detect separator and encoding
for enc in ["utf-8", "latin-1", "cp1252"]:
    for sep in [",", ";"]:
        try:
            df = pd.read_csv(path, encoding=enc, sep=sep, nrows=5)
            if df.shape[1] > 1:
                encoding_used = enc
                sep_used = sep
                break
        except Exception:
            continue
    else:
        continue
    break

# Full read
df = pd.read_csv(path, encoding=encoding_used, sep=sep_used)

print(f"Shape: {df.shape}")
print(f"Memory: {df.memory_usage(deep=True).sum() / 1024**2:.2f} MB")
print(f"Encoding: {encoding_used} | Separator: {sep_used!r}")
print(df.dtypes)
```

## Notes
- For files > 100MB, test with nrows=1000 first
- German CSVs typically use sep=";" and encoding="latin-1"
- Never rebuild file contents as a string in code, always use pd.read_csv

## Self-Improvement
Append new encoding or separator patterns here.
