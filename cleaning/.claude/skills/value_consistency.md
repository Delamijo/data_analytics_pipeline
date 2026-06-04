# Skill: value_consistency

## Purpose
Fix whitespace and case inconsistencies in categorical columns.

## When to Apply
Fourth cleaning step, after type_correction.

## Code
```python
for col, fixes in confirmed_consistency_fixes.items():
    if col not in df.columns:
        print(f"SKIP: {col} not found")
        continue

    before_unique = df[col].nunique()

    if "whitespace" in fixes:
        df[col] = df[col].str.strip()

    if "case" in fixes:
        df[col] = df[col].str.lower()

    after_unique = df[col].nunique()
    print(f"{col}: {before_unique} -> {after_unique} unique values")
```

## Log Entry
```
## VALUE CONSISTENCY - [TIMESTAMP]
Action: fixed whitespace and case inconsistencies
Columns affected: [list]
Unique value changes: [col: before -> after]
Decision source: manager confirmed
```

## Notes
- Only apply fixes explicitly listed in confirmed_consistency_fixes
- Log reduction in unique values as confirmation that fix worked

## Self-Improvement
Append new consistency patterns here.
