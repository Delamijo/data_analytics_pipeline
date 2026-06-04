# Skill: notebook_export

## Purpose
Save all analysis code and full ML training code as a Jupyter notebook.

## When to Apply
Always run after findings_summary, before export.

## Code Storage Convention
During each skill execution, store the executed code block in a variable:
- load_code = "..."
- descriptive_code = "..."
- correlation_code = "..."
- group_code = "..."
- ml_prep_code = "..."
- ml_test_code = "..."

## Full Training Code Template
Always include this section in the notebook even if test run was not executed:

```python
full_training_template = """
# Full Model Training
# Run this section manually after reviewing test run results

from sklearn.ensemble import RandomForestClassifier  # or Regressor
from sklearn.model_selection import GridSearchCV, cross_val_score

# Uncomment and adjust parameters based on test run results
param_grid = {
    'n_estimators': [100, 200, 300],
    'max_depth': [3, 5, 10, None],
    'min_samples_split': [2, 5, 10]
}

# Use full dataset for final training
X_full_train, X_full_test, y_full_train, y_full_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

grid_search = GridSearchCV(
    RandomForestClassifier(random_state=42),
    param_grid,
    cv=5,
    scoring='accuracy',  # adjust for regression: 'r2'
    n_jobs=-1
)
grid_search.fit(X_full_train, y_full_train)

print(f"Best params: {grid_search.best_params_}")
print(f"Best score: {grid_search.best_score_:.4f}")
"""
```

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
    nbformat.v4.new_markdown_cell(f"# Analysis: {dataset_name}\nResearch question: {research_question}\nGenerated: {datetime.now().strftime('%Y-%m-%d %H:%M')}"),
    nbformat.v4.new_markdown_cell("## 1 - Load Data"),
    nbformat.v4.new_code_cell(load_code),
    nbformat.v4.new_markdown_cell("## 2 - Descriptive Statistics"),
    nbformat.v4.new_code_cell(descriptive_code),
    nbformat.v4.new_markdown_cell("## 3 - Correlation Analysis"),
    nbformat.v4.new_code_cell(correlation_code),
    nbformat.v4.new_markdown_cell("## 4 - Group Comparison"),
    nbformat.v4.new_code_cell(group_code),
    nbformat.v4.new_markdown_cell("## 5 - ML Preparation"),
    nbformat.v4.new_code_cell(ml_prep_code),
    nbformat.v4.new_markdown_cell("## 6 - ML Test Run (20% sample)"),
    nbformat.v4.new_code_cell(ml_test_code),
    nbformat.v4.new_markdown_cell("## 7 - Full Model Training\n> Run this section manually after reviewing test run results above."),
    nbformat.v4.new_code_cell(full_training_template),
]

nb.cells = cells

notebook_path = Path(f"output/{dataset_name}/notebooks/03_analysis.ipynb")
with open(notebook_path, "w", encoding="utf-8") as f:
    nbformat.write(nb, f)

print(f"Notebook saved: {notebook_path}")
```

## Notes
- Always include full training code section even if no ML was run
- Notebook is write-once, never re-execute to verify
- Skip ML cells if question type is descriptive

## Self-Improvement
Append new notebook patterns here.
