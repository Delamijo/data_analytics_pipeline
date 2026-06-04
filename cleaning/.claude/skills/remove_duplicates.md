# Skill: remove_duplicates

## Purpose
Remove duplicate rows confirmed by the manager.

## When to Apply
Second cleaning step, after drop_columns.

## Code
```python
rows_before = len(df)

# Use key column if specified, otherwise all columns
if confirmed_duplicate_key:
    df = df.drop_duplicates(subset=[confirmed_duplicate_key], keep="first")
else:
    df = df.drop_duplicates(keep="first")

rows_removed = rows_before - len(df)
print(f"Rows removed: {rows_removed}")
print(f"Remaining rows: {len(df)}")
```

## Log Entry
```
## REMOVE DUPLICATES - [TIMESTAMP]
Action: removed duplicate rows
Key column: [column or "all columns"]
Rows before: [n]
Rows removed: [n]
Rows after: [n]
Decision source: manager confirmed
```

## Notes
- Always use keep="first" unless manager specifies otherwise
- If no duplicates found, log and continue without error

## Self-Improvement
Append new duplicate patterns here.
