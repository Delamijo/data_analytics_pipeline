# Skill: export

## Purpose
Save the compiled report to disk and verify all outputs.

## When to Apply
Always the last step after notebook_export.

## Code
```python
from pathlib import Path

output_base = Path(f"output/{dataset_name}")
report_path = output_base / "report.md"

# Save report
report_path.write_text(report_content, encoding="utf-8")
print(f"Report saved: {report_path}")

# Completion check
required = [
    report_path,
    output_base / "notebooks/04_report.ipynb"
]
missing = [str(p) for p in required if not p.exists()]

if missing:
    print(f"ERROR: Missing outputs: {missing}")
else:
    print("Report export complete - all outputs verified")
    print(f"Report size: {report_path.stat().st_size / 1024:.1f} KB")
```

## Notes
- report_content is the full report string from compile_report skill
- Output directories are created by the manager - never create them here

## Self-Improvement
Append new export patterns here.
