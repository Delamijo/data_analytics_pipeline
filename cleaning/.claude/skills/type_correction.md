# Skill: type_correction

## Purpose
Fix incorrectly read column types (object -> datetime, object -> numeric).

## When to Apply
Third cleaning step, after remove_duplicates.

## Code
```python
for col, target_type in confirmed_type_corrections.items():
    if col not in df.columns:
        print(f"SKIP: {col} not found")
        continue

    before_dtype = df[col].dtype

    try:
        if target_type == "datetime":
            df[col] = pd.to_datetime(df[col], dayfirst=True, errors="coerce")
        elif target_type in ["float", "int"]:
            df[col] = pd.to_numeric(df[col], errors="coerce")
        
        print(f"{col}: {before_dtype} -> {df[col].dtype}")

    except Exception as e:
        print(f"ERROR: {col} type correction failed - {e}")
```

## Log Entry
```
## TYPE CORRECTION - [TIMESTAMP]
Action: corrected column types
Columns corrected: [col: old_dtype -> new_dtype]
Columns failed: [list or "none"]
Decision source: manager confirmed
```

## Notes
- errors="coerce" turns unparseable values into NaT/NaN - log if this introduces new missing values
- Always check for new missing values after conversion

```python
# Check for new missing values after conversion
new_missing = df[confirmed_type_corrections.keys()].isna().sum()
if new_missing.any():
    print(f"New missing values introduced: {new_missing[new_missing > 0]}")
```

## Self-Improvement
Append new type correction patterns here.
