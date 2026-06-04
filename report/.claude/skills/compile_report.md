# Skill: compile_report

## Purpose
Compile a final human-readable report from findings.md and cleaning_log.md.

## When to Apply
First and main step. Read input files once, write report in one pass.

## Code
```python
from pathlib import Path
from datetime import datetime

output_base = Path(f"output/{dataset_name}")

# Read input files once
findings = (output_base / "findings.md").read_text(encoding="utf-8")
cleaning_log_path = output_base / "cleaning_log.md"
cleaning_summary = cleaning_log_path.read_text(encoding="utf-8") if cleaning_log_path.exists() else "Cleaning log not available."
```

## Report Template
```python
report = f"""# Analysis Report: {dataset_name}
Generated: {datetime.now().strftime('%Y-%m-%d %H:%M')}
Research question: "{research_question}"

---

## Executive Summary
[3-5 sentences summarizing the most important findings in plain language.
Reference the research question directly. No jargon.]

---

## Dataset Overview
[Brief description of the dataset - rows, columns, key variable types.
Sourced from findings.md Overview section.]

---

## Data Quality and Cleaning
[Summary of what was found and fixed during EDA and cleaning.
Sourced from cleaning_log.md. Keep brief - 5-10 bullet points max.]

---

## Key Findings
[Numbered list of findings with supporting statistics.
Sourced from findings.md Key Findings section.
Write in plain language.]

---

## ML Results
[Only include if predictive or explanatory question type.
Summarize test run results and model recommendation.
Include link to notebook for full training code.]

---

## Limitations
[Honest assessment of what the analysis cannot conclude.
Sourced from findings.md Limitations section.
Never omit this section.]

---

## Recommended Next Steps
[Concrete actions the user can take based on findings.
Include reference to notebooks for full training code if applicable.]

---

*Full analysis code available in:*
*- notebooks/01_eda.ipynb*
*- notebooks/02_cleaning.ipynb*
*- notebooks/03_analysis.ipynb*
*- notebooks/04_report.ipynb*
"""
```

## Notes
- Executive Summary must directly answer the research question
- Never add findings not present in findings.md
- Limitations section is mandatory

## Self-Improvement
Append new report structure patterns here.
