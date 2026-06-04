# Skill: cleaning_log

## Purpose
Initialize and manage the cleaning log file.

## When to Apply
Initialize before first cleaning step. Append after every step.

## Initialization Code
```python
from pathlib import Path
from datetime import datetime

log_path = Path(f"output/{dataset_name}/cleaning_log.md")

header = f"""# Cleaning Log: {dataset_name}
Started: {datetime.now().strftime('%Y-%m-%d %H:%M')}
Source: output/{dataset_name}/data/optimized.parquet
Rows before cleaning: {rows_before}
Columns before cleaning: {cols_before}

---

"""
log_path.write_text(header, encoding="utf-8")
print(f"Cleaning log initialized: {log_path}")
```

## Append Function
```python
def append_log(log_path, entry):
    with open(log_path, "a", encoding="utf-8") as f:
        f.write(entry + "\n\n")
```

## Notes
- Initialize once at the start, never overwrite
- Use append_log after every cleaning step
- Log only deltas - never write full DataFrame state

## Self-Improvement
Append new log format patterns here.
