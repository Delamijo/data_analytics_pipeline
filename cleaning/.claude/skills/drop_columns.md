# Skill: drop_columns

## Purpose
Drop confirmed columns from the DataFrame.

## When to Apply
First cleaning step. Only drop columns explicitly confirmed by the manager.

## Code
```python
from datetime import datetime

# Only drop columns confirmed by manager
cols_to_drop = [c for c in confirmed_drop_columns if c in df.columns]
skipped = [c for c in confirmed_drop_columns if c not in df.columns]

df = df.drop(columns=cols_to_drop)

print(f"Dropped: {cols_to_drop}")
if skipped:
    print(f"Skipped (not found): {skipped}")
print(f"Remaining columns: {df.shape[1]}")
```

## Log Entry
```
## DROP COLUMNS - [TIMESTAMP]
Action: dropped confirmed columns
Columns dropped: [list]
Columns skipped: [list or "none"]
Remaining shape: [rows] x [cols]
Decision source: manager confirmed
```

## Notes
- Never drop columns not in confirmed_drop_columns
- Log skipped columns without raising errors

## Self-Improvement
Append new drop patterns here.
