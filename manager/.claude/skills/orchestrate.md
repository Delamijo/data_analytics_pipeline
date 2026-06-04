# Skill: orchestrate

## Purpose
Run the full analytics pipeline from research question to dashboard output.

## When to Apply
User provides a research question and a dataset filename.

## Step 1a - Receive and Cache
Extract and cache the following from the user input:
- dataset filename
- dataset path: `data/[dataset]`
- dataset name (stem, without extension)
- research question (store as exact string)

Never ask the user for this information again during the pipeline run.

## Step 1b - Research Question Validation
Before proceeding, verify the research question contains:
- A clear outcome or goal ("predict", "identify", "compare", "explain")
- At least one implied variable or dimension

If the question is too vague, ask the user once:
```
Your research question is a bit broad. Could you tell me more specifically 
what you are trying to find out? For example: are you trying to predict 
a specific outcome, or understand relationships between variables?
```

## Step 1c - Analyze Research Question
Before starting the pipeline, derive the following from the research question:

- Question type: descriptive / predictive / explanatory
- Target variable (if predictable): which column is being predicted?
- Suggested analysis methods based on question type:
  - descriptive -> distributions, correlations, group comparisons
  - predictive -> regression (continuous target) or classification (categorical target)
  - explanatory -> feature importance, correlation analysis
- Key columns likely relevant to the question

Store this as a short analysis plan and pass it explicitly to the Analysis agent later.
Log the plan to the orchestration log.

## Step 1d - Confirm with User
Present the following to the user before starting:
```
I will run the following analysis:
Dataset: [dataset]
Research question: "[research question]"
Analysis plan:

Question type: [descriptive/predictive/explanatory]
Target variable: [variable or "none identified"]
Methods: [list of methods]

All outputs will be saved to output/[dataset_name]/
Does this look correct? Should I adjust anything before I start?
```
Wait for confirmation before continuing to Step 2.

## Step 2 - Prepare Output Structure

```python
from pathlib import Path
import json
from datetime import datetime

dataset_name = Path("data/[dataset]").stem
output_base = Path(f"output/{dataset_name}")

# Create folder structure
(output_base / "notebooks").mkdir(parents=True, exist_ok=True)
(output_base / "data").mkdir(parents=True, exist_ok=True)
(output_base / "dashboard").mkdir(parents=True, exist_ok=True)

# Initialize orchestration log
log_path = output_base / "orchestration_log.md"
log_path.write_text(f"# Orchestration Log: {dataset_name}\nStarted: {datetime.now().strftime('%Y-%m-%d %H:%M')}\n\n", encoding="utf-8")

print(f"Output structure ready: {output_base}")
```

## Step 3 - Run EDA Agent
Inform the user:
```
Starting EDA stage. This will profile the dataset and identify data quality issues.
```

Delegate to `../eda/CLAUDE.md`. Pass:
- dataset path: `data/[dataset]`
- output path: `output/[dataset_name]/`

After completion, append to orchestration log:
```
## EDA - [TIMESTAMP]
Decision: EDA completed successfully
Reason: baseline profiling required before any transformation
User consulted: no
```

## Step 4 - Review and Decide
Read only the Cleaning Agenda section from `output/[dataset_name]/eda_report.md`.

For each irreversible recommendation, ask the user once:
```
The EDA found [issue] in column "[column]". 
I recommend [action] because [reason].
Should I proceed with this?
```

Wait for answer before moving to the next item.

Append all decisions to orchestration log.

## Step 5 - Run Cleaning Agent
Inform the user:
```
Starting Cleaning stage based on your confirmed decisions.
```

Delegate to `../cleaning/CLAUDE.md`. Pass:
- optimized parquet path: `output/[dataset_name]/data/optimized.parquet`
- confirmed cleaning decisions (not the full EDA report)
- output path: `output/[dataset_name]/`

After completion, briefly summarize what was cleaned and ask:
```
Cleaning is complete. Here is a summary of what was changed:
[summary]

Does this look correct before I start the analysis?
```

## Step 6 - Run Analysis Agent
Inform the user:
```
Starting Analysis stage. Research question: "[research question]"
```

Delegate to `../analysis/CLAUDE.md`. Pass:
- cleaned parquet path: `output/[dataset_name]/data/cleaned.parquet`
- research question (explicitly, as a string)
- analysis plan derived in Step 1b (question type, target variable, methods)
- output path: `output/[dataset_name]/`

## Step 7 - Run Report Agent
Delegate to `../report/CLAUDE.md`. Pass:
- findings summary only (not full results parquet)
- output path: `output/[dataset_name]/`

## Step 8 - Run Dashboard Agent
Delegate to `../dashboard/CLAUDE.md`. Pass:
- results parquet path: `output/[dataset_name]/data/results.parquet`
- findings summary
- output path: `output/[dataset_name]/dashboard/`

## Step 9 - Pipeline Complete
Verify all outputs exist:

```python
required = [
    output_base / "report.md",
    output_base / "orchestration_log.md",
    output_base / "data/cleaned.parquet",
    output_base / "dashboard/app.py",
]

missing = [str(p) for p in required if not p.exists()]
notebooks = list((output_base / "notebooks").glob("*.ipynb"))

if missing:
    print(f"Missing outputs: {missing}")
else:
    print("Pipeline complete. All outputs verified.")
    print(f"Notebooks generated: {len(notebooks)}")
```

Then present to the user in plain language:
```
The analysis is complete. Here is what was found:
[key findings in 3-5 sentences, no jargon]

Output files are saved to output/[dataset_name]/
- report.md - full analysis report
- orchestration_log.md - all decisions made during the pipeline
- dashboard/app.py - run with: python dashboard/app.py
- notebooks/ - [n] jupyter notebooks with full code
```

## Self-Improvement
Append new orchestration patterns or edge cases to manager/CLAUDE.md after each run.
