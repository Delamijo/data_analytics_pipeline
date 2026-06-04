# Skill: ingest_parquet

## Purpose
Read a Parquet file token-efficiently and return basic dataset info.

## When to Apply
Dataset is a .parquet file.

## Code
```python
import pandas as pd
import pyarrow.parquet as pq
from pathlib import Path

path = Path("data/[dataset]")

# Check schema before loading
schema = pq.read_schema(path)
print(f"Schema: {schema}")

# Full read
df = pd.read_parquet(path)

print(f"Shape: {df.shape}")
print(f"Memory: {df.memory_usage(deep=True).sum() / 1024**2:.2f} MB")
print(df.dtypes)
```

## Notes
- Parquet preserves types - dtypes are often already optimal
- Use columns=[...] to load only needed columns
- Prefer pyarrow over fastparquet
- Never rebuild file contents as a string in code, always use pd.read_parquet

## Self-Improvement
Append new Parquet-specific patterns here.
