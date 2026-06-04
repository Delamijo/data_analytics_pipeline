# Skill: export

## Purpose
Save the cleaned DataFrame as Parquet and finalize the cleaning log.

## When to Apply
Always the last step after notebook_export.

## Code
```python
from pathlib import Path

output_base = Path(f"output/{dataset_name}")
parquet_path = output_base / "data/cleaned.parquet"

# Save cleaned parquet
df.to_parquet(parquet_path, index=False)
print(f"Parquet saved: {parquet_path}")

# Validate
test = pd.read_parquet(parquet_path)
assert test.shape == df.shape, "Parquet validation failed - shape mismatch"
print(f"Validated: {test.shape}")

# File size
file_size = parquet_path.stat().st_size / 1024**2
print(f"File size: {file_size:.2f} MB")

# Completion check
required = [parquet_path, output_base / "cleaning_log.md"]
missing = [str(p) for p in required if not p.exists()]
if missing:
    print(f"ERROR: Missing outputs: {missing}")
else:
    print("Cleaning export complete - all outputs verified")

# Final stats
print(f"Rows before: {rows_before}")
print(f"Rows after:  {len(df)}")
print(f"Columns:     {df.shape[1]}")
```

## Notes
- Never overwrite optimized.parquet - always save to cleaned.parquet
- rows_before must be carried over from Pre-flight Check
- Output directories are created by the manager - never create them here

## Self-Improvement
Append new export patterns here.
