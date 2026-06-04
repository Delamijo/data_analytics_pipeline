# Skill: notebook_export

## Purpose
Save all executed cleaning code as a Jupyter notebook.

## When to Apply
Always run after all cleaning steps, before export.

## Code Storage Convention
During each skill execution, store the executed code block in a variable:
- drop_code = "..."
- duplicates_code = "..."
- type_code = "..."
- consistency_code = "..."
- outlier_code = "..."
- imputation_code = "..."

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
    nbformat.v4.new_markdown_cell(f"# Cleaning: {dataset_name}\nGenerated: {datetime.now().strftime('%Y-%m-%d %H:%M')}"),
    nbformat.v4.new_markdown_cell("## 1 - Drop Columns"),
    nbformat.v4.new_code_cell(drop_code),
    nbformat.v4.new_markdown_cell("## 2 - Remove Duplicates"),
    nbformat.v4.new_code_cell(duplicates_code),
    nbformat.v4.new_markdown_cell("## 3 - Type Correction"),
    nbformat.v4.new_code_cell(type_code),
    nbformat.v4.new_markdown_cell("## 4 - Value Consistency"),
    nbformat.v4.new_code_cell(consistency_code),
    nbformat.v4.new_markdown_cell("## 5 - Outlier Handling"),
    nbformat.v4.new_code_cell(outlier_code),
    nbformat.v4.new_markdown_cell("## 6 - Imputation"),
    nbformat.v4.new_code_cell(imputation_code),
]

nb.cells = cells

notebook_path = Path(f"output/{dataset_name}/notebooks/02_cleaning.ipynb")
with open(notebook_path, "w", encoding="utf-8") as f:
    nbformat.write(nb, f)

print(f"Notebook saved: {notebook_path}")
```

## Notes
- Notebook is write-once, never re-execute to verify
- Only include steps that were actually executed

## Self-Improvement
Append new notebook patterns here.
