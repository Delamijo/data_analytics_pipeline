# Skill: findings_summary

## Purpose
Write a structured findings summary for the manager and report agent.

## When to Apply
After all analysis steps, before notebook_export.

## Output Format

```markdown
# Findings: [dataset_name]
Research question: "[research question]"
Generated: [timestamp]

## Question Type
[descriptive / predictive / explanatory]

## Key Findings
1. [finding with supporting statistic]
2. [finding with supporting statistic]
3. [finding with supporting statistic]

## Descriptive Highlights
- [key distribution finding]
- [key correlation finding]
- [key group difference if found]

## ML Results (if applicable)
- Task type: [classification / regression]
- Best model (test run): [model name]
- Best metric: [metric: value]
- Top features: [list]
- Recommendation: [full training recommended / needs feature engineering / review target variable]

## Limitations
- [what the analysis cannot conclude]
- [data quality issues that may affect results]

## Recommended Next Steps
1. [concrete action]
2. [concrete action]
```

## Notes
- Key Findings must reference actual statistics, never vague statements
- Limitations section is mandatory - never omit
- ML Results section only if predictive or explanatory question type

## Self-Improvement
Append new findings patterns here.
