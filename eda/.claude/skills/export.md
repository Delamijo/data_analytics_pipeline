# Skill: export

## Purpose
Save the optimized DataFrame as Parquet and write the EDA report to disk.

## When to Apply
Always the last step after notebook_export.

## Code
```python
from pathlib import Path
from datetime import datetime

output_base = Path(f"output/{dataset_name}")

# Save optimized parquet
parquet_path = output_base / "data/optimized.parquet"
df.to_parquet(parquet_path, index=False)
print(f"Parquet saved: {parquet_path}")

test = pd.read_parquet(parquet_path)
assert test.shape == df.shape, "Parquet validation failed - shape mismatch"
print(f"Parquet validated: {test.shape}")

file_size = parquet_path.stat().st_size / 1024**2
print(f"File size: {file_size:.2f} MB")

# Save EDA report
report_path = output_base / "eda_report.md"
report_path.write_text(eda_report_content, encoding="utf-8")
print(f"Report saved: {report_path}")

# Log completion
print(f"Memory before: {mem_before / 1024**2:.2f} MB")
print(f"Memory after:  {mem_after / 1024**2:.2f} MB")
print(f"Savings: {(1 - mem_after / mem_before) * 100:.1f}%")
```

## Completion Check
```python
required = [parquet_path, report_path]
missing = [str(p) for p in required if not p.exists()]
if missing:
    print(f"ERROR: Missing outputs: {missing}")
else:
    print("EDA export complete - all outputs verified")
```

## Notes
- mem_before and mem_after must be carried over from memory_optimize
- eda_report_content is the full report string from eda_report skill
- Output directories are created by the manager in Step 2 - never create them here
- Parquet preserves all type optimizations - never save back to CSV

## Self-Improvement
Append new export patterns here.
