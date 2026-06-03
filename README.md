# Data Analytics Pipeline

A modular, multi-agent data analytics pipeline built with Claude Code. Each agent handles one stage of the analytics process, coordinated by a central manager agent.

## Concept

Most data analytics workflows require manual decisions at every step: loading, cleaning, analyzing, and visualizing data. This pipeline automates the technical decisions while keeping the user in control of meaningful, irreversible choices.

A manager agent receives a research question and a dataset, then orchestrates specialized sub-agents to handle each stage. The user is only asked when domain knowledge or irreversible decisions are required.

## Architecture

```
User: research question + dataset
              |
       Manager Agent
       /     |     \
     EDA   Clean  Analysis
              |
           Report
              |
          Dashboard
```

## Structure

```
data-analytics-pipeline/
├── data/                        <- input datasets
├── output/
│   └── [dataset_name]/          <- one folder per run
│       ├── notebooks/           <- jupyter notebooks per stage
│       ├── data/                <- intermediate and final data
│       ├── report.md            <- analysis report with cleaning agenda
│       └── dashboard/           <- plotly dash app
├── manager/                     <- orchestration and decision logic
│   ├── CLAUDE.md
│   └── .claude/skills/
├── eda/                         <- exploratory data analysis
│   ├── CLAUDE.md
│   └── .claude/skills/
├── cleaning/                    <- data cleaning based on eda report
│   ├── CLAUDE.md
│   └── .claude/skills/
├── analysis/                    <- descriptive statistics and ml models
│   ├── CLAUDE.md
│   └── .claude/skills/
├── report/                      <- structured report generation
│   ├── CLAUDE.md
│   └── .claude/skills/
└── dashboard/                   <- plotly dash app generation
    ├── CLAUDE.md
    └── .claude/skills/
```

## How It Works

The manager agent reads the research question and delegates tasks to sub-agents in order. Each sub-agent produces a structured output that the next agent uses as input.

**Decision logic:**
- Reversible technical decisions: sub-agents decide independently
- Irreversible data changes: manager decides
- Domain knowledge required: user is asked

The user is never asked directly by a sub-agent. All communication goes through the manager.

## Usage

Place your dataset in the `data/` folder, open the project in VS Code with Claude Code, and prompt the manager:

```
For Example:
Analyze customer churn in sales_data.csv. Research question: which factors predict churn?
```

All outputs are saved to `output/[dataset_name]/`.

## Agents

| Agent | Input | Output |
|---|---|---|
| Manager | research question + dataset path | orchestration log |
| EDA | raw dataset | eda report + optimized parquet + 01_eda.ipynb |
| Cleaning | eda report + parquet | cleaned parquet + cleaning log + 02_cleaning.ipynb |
| Analysis | cleaned parquet + research question | results parquet + findings + 03_analysis.ipynb |
| Report | all outputs | report.md |
| Dashboard | results parquet + findings | plotly dash app |

## Tech Stack

- [Claude Code]
- [pandas]
- [scikit-learn]
- [plotly dash] 
- [jupyter]
- Python 3.10+

## Future Plans

- Natural language report generation
- Automated model selection and hyperparameter tuning
- Support for SQL databases as input
- Export to Power BI compatible format

## License

MIT