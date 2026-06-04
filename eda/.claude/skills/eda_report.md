# Skill: eda_report

## Purpose
Generate a structured EDA report including a Cleaning Agenda for the manager.

## When to Apply
Always run after memory_optimize, before notebook_export.

## Output Format

```
# EDA Report: [dataset_name]
Generated: [timestamp]

## Overview
- Shape: X rows x Y columns
- Memory: X MB -> Y MB (Z% saved)

## Data Quality
- Missing values: [columns with > 5% missing, or "none critical"]
- Outliers: [critical columns per IQR, or "none critical"]
- Duplicates: X rows

## Categorical Columns
- High cardinality (> 80%): [columns] -> likely IDs
- Optimized to category dtype: [columns]

## Type Corrections Recommended
- [column]: from [dtype] to [dtype] - reason: [incorrectly read]

## Recommended Next Steps
1. [concrete cleaning action]
2. [concrete cleaning action]

## Open Questions
- [anything requiring domain knowledge]

---

## Cleaning Agenda
<!-- Recommendations for Cleaning Agent - not decisions -->

RECOMMENDATION DUPLICATES:
- rows: [n] duplicate rows found | method: drop_duplicates(keep="first")

RECOMMENDATION DROP:
- column: [name] | reason: [e.g. unique_pct > 80%]

RECOMMENDATION IMPUTATION:
- column: [name] | method: [median/mode] | reason: [missing_pct X%, numeric/categorical]

RECOMMENDATION TYPE CORRECTION:
- column: [name] | from: [dtype] | to: [dtype] | reason: [incorrectly read]

RECOMMENDATION VALUE CONSISTENCY:
- column: [name] | issue: [whitespace/case inconsistency] | fix: [strip/lower]

RECOMMENDATION OUTLIER:
- column: [name] | method: [IQR-clip/drop] | reason: [X% outliers]

```

## Notes
- Only populate Cleaning Agenda sections where issues were actually found
- Leave out empty sections
- Open Questions must be honest - never guess domain knowledge

## Type Correction Integration
Include all DateTime and mixed type findings from memory_optimize 
in the RECOMMENDATION TYPE CORRECTION section of the Cleaning Agenda.
Never leave detected type issues out of the report.

## Manager Summary
After writing the report, return this structured summary to the manager:

```
EDA COMPLETE
- dataset: [name]
- shape: [rows] x [columns]
- memory saved: [X]%
- critical issues: [list or "none"]
- cleaning agenda: output/[dataset_name]/eda_report.md#cleaning-agenda
```

## Self-Improvement
Append recurring report patterns here.
