# Skill: export

## Purpose
Save results parquet and findings summary to disk.

## When to Apply
Always the last step after notebook_export.

## Code
```python
import pandas as pd
from pathlib import Path

output_base = Path(f"output/{dataset_name}")

# Save results as parquet
results_df = pd.DataFrame(analysis_results)
results_path = output_base / "data/results.parquet"
results_df.to_parquet(results_path, index=False)
print(f"Results saved: {results_path}")

# Save findings summary
findings_path = output_base / "findings.md"
findings_path.write_text(findings_content, encoding="utf-8")
print(f"Findings saved: {findings_path}")

# Validate
assert results_path.exists(), "results.parquet missing"
assert findings_path.exists(), "findings.md missing"

# File sizes
print(f"results.parquet: {results_path.stat().st_size / 1024:.1f} KB")
print(f"findings.md: {findings_path.stat().st_size / 1024:.1f} KB")

print("Analysis export complete")
```

## Notes
- analysis_results is a dict of key statistics collected during analysis
- findings_content is the full string from findings_summary skill
- Output directories are created by the manager - never create them here

## Self-Improvement
Append new export patterns here.
