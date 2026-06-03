# Skill: notebook_export

## Purpose
Save all executed EDA code as a Jupyter notebook.

## When to Apply
Always run after eda_report, before export.

## Code Storage Convention
During each skill execution, store the executed code block in a variable:
- ingest_code = "..." 
- missing_code = "..."
- outlier_code = "..."
- categorical_code = "..."
- memory_code = "..."

These must be available when notebook_export runs.

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
    nbformat.v4.new_markdown_cell(f"# EDA: {dataset_name}\nGenerated: {datetime.now().strftime('%Y-%m-%d %H:%M')}"),
    nbformat.v4.new_markdown_cell("## 1 - Ingestion"),
    nbformat.v4.new_code_cell(ingest_code),
    nbformat.v4.new_markdown_cell("## 2 - Missing Values"),
    nbformat.v4.new_code_cell(missing_code),
    nbformat.v4.new_markdown_cell("## 3 - Outlier Screening"),
    nbformat.v4.new_code_cell(outlier_code),
    nbformat.v4.new_markdown_cell("## 4 - Categorical Profile"),
    nbformat.v4.new_code_cell(categorical_code),
    nbformat.v4.new_markdown_cell("## 5 - Memory Optimization"),
    nbformat.v4.new_code_cell(memory_code),
]

nb.cells = cells

notebook_path = Path(f"output/{dataset_name}/notebooks/01_eda.ipynb")
with open(notebook_path, "w", encoding="utf-8") as f:
    nbformat.write(nb, f)

print(f"Notebook saved: {notebook_path}")
```

## Notes
- Store each skill's code block in a variable (ingest_code, missing_code, etc.) during execution
- Notebook is write-once - never re-execute to verify
- Install nbformat if missing: pip install nbformat

## Self-Improvement
Append new notebook structure patterns here.
