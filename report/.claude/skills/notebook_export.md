# Skill: notebook_export

## Purpose
Save a pipeline summary notebook linking all previous notebooks.

## When to Apply
After compile_report, before export.

## Code
```python
try:
    import nbformat
except ImportError:
    import subprocess
    subprocess.run(["pip", "install", "nbformat", "--break-system-packages"])
    import nbformat

from pathlib import Path
from datetime import datetime

nb = nbformat.v4.new_notebook()

cells = [
    nbformat.v4.new_markdown_cell(
        f"# Pipeline Summary: {dataset_name}\n"
        f"Research question: {research_question}\n"
        f"Generated: {datetime.now().strftime('%Y-%m-%d %H:%M')}"
    ),
    nbformat.v4.new_markdown_cell(
        "## Pipeline Notebooks\n"
        "- [01_eda.ipynb](01_eda.ipynb) - Exploratory Data Analysis\n"
        "- [02_cleaning.ipynb](02_cleaning.ipynb) - Data Cleaning\n"
        "- [03_analysis.ipynb](03_analysis.ipynb) - Analysis and ML\n"
        "- [04_report.ipynb](04_report.ipynb) - This summary\n"
    ),
    nbformat.v4.new_markdown_cell("## Key Findings"),
    nbformat.v4.new_markdown_cell(findings_summary),
    nbformat.v4.new_markdown_cell("## Full Report"),
    nbformat.v4.new_markdown_cell("> See report.md for the full human-readable report."),
]

nb.cells = cells

notebook_path = Path(f"output/{dataset_name}/notebooks/04_report.ipynb")
with open(notebook_path, "w", encoding="utf-8") as f:
    nbformat.write(nb, f)

print(f"Notebook saved: {notebook_path}")
```

## Notes
- findings_summary is the Key Findings section extracted from findings.md
- Notebook is write-once, never re-execute to verify
- This notebook serves as a navigable index for the full pipeline

## Self-Improvement
Append new notebook patterns here.
