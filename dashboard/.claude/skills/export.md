# Skill: export

## Purpose
Save app.py and requirements.txt to the dashboard folder.

## When to Apply
Always the last step after generate_app.

## Code
```python
from pathlib import Path
from datetime import datetime

dashboard_dir = Path(f"output/{dataset_name}/dashboard")

# Save app.py
app_path = dashboard_dir / "app.py"
app_path.write_text(app_content, encoding="utf-8")
print(f"App saved: {app_path}")

# Save requirements.txt
requirements = """dash>=2.0.0
plotly>=5.0.0
pandas>=1.3.0
pyarrow>=6.0.0
"""
req_path = dashboard_dir / "requirements.txt"
req_path.write_text(requirements, encoding="utf-8")
print(f"Requirements saved: {req_path}")

# Completion check
required = [app_path, req_path]
missing = [str(p) for p in required if not p.exists()]

if missing:
    print(f"ERROR: Missing outputs: {missing}")
else:
    print("Dashboard export complete - all outputs verified")
    print(f"Run with: python {app_path}")
```

## Notes
- app_content is the full generated app string from generate_app skill
- Output directories are created by the manager - never create them here
- requirements.txt pins minimum versions only

## Self-Improvement
Append new export patterns here.
