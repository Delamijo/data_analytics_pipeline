# Skill: memory_optimize

## Purpose
Optimize DataFrame memory through type downcast.

## When to Apply
Always run after categorical_profile, before export.

## Float Threshold
Always use Normal threshold (1e-6). This is decided by the manager - do not ask the user.

## Mixed Type Check
```python
for col in df.select_dtypes("object"):
    numeric_count = pd.to_numeric(df[col], errors="coerce").notna().sum()
    total = df[col].notna().sum()
    if 0 < numeric_count < total:
        print(f"{col}: mixed types detected ({numeric_count}/{total} numeric values)")
```

## DateTime Detection
```python
for col in df.select_dtypes("object"):
    try:
        parsed = pd.to_datetime(df[col], dayfirst=True, errors="coerce")
        if parsed.notna().mean() > 0.8:
            print(f"{col}: likely datetime, recommend conversion")
    except Exception:
        pass
```

## Code
```python
mem_before = df.memory_usage(deep=True).sum()

# Category conversion
for col in df.select_dtypes("object"):
    if df[col].nunique() / len(df) < 0.5:
        df[col] = df[col].astype("category")

# Integer downcast (no information loss)
for col in df.select_dtypes("int64"):
    df[col] = pd.to_numeric(df[col], downcast="integer")

# Float downcast with threshold 1e-6
threshold = 1e-6
float_log = []
for col in df.select_dtypes("float64"):
    diff = (df[col] - df[col].astype("float32")).abs().max()
    if diff < threshold:
        df[col] = df[col].astype("float32")
        float_log.append(f"- {col}: float64 -> float32")
    else:
        float_log.append(f"- {col}: kept float64 (max deviation: {diff:.2e})")

mem_after = df.memory_usage(deep=True).sum()
savings = (1 - mem_after / mem_before) * 100

print(f"Before: {mem_before / 1024**2:.2f} MB")
print(f"After:  {mem_after / 1024**2:.2f} MB")
print(f"Saved:  {savings:.1f}%")
print("\nFloat downcast log:")
for entry in float_log:
    print(entry)
print("\nNew dtypes:")
print(df.dtypes)
```

## Notes
- Always run after missing_values - NaN in int columns blocks downcast
- Store mem_before and mem_after for export skill

## Decision
Integer downcast and category conversion are applied automatically.
Float downcast uses threshold 1e-6 - decided by manager, applied automatically.
DateTime and mixed type corrections are flagged for the Cleaning Agent, never applied here.

## Self-Improvement
Append new downcast issues here.
