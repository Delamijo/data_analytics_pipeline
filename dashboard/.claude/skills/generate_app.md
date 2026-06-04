# Skill: generate_app

## Purpose
Generate a self-contained Plotly Dash app based on the chart plan.

## When to Apply
After plan_dashboard.

## App Template
```python
app_template = '''
import pandas as pd
import plotly.express as px
from dash import Dash, dcc, html
from pathlib import Path

# Load data
base = Path(__file__).parent.parent
df = pd.read_parquet(base / "data/cleaned.parquet")
results = pd.read_parquet(base / "data/results.parquet")

app = Dash(__name__)

# ── Charts ──────────────────────────────────────────────────────────────────

{chart_code}

# ── Layout ──────────────────────────────────────────────────────────────────

app.layout = html.Div([
    html.H1("{research_question}", style={{"fontFamily": "sans-serif", "padding": "20px"}}),
    html.H3("Dataset: {dataset_name}", style={{"fontFamily": "sans-serif", "paddingLeft": "20px", "color": "#666"}}),

    {layout_components}

    html.Footer(
        "{dataset_name} | Generated: {generation_date}",
        style={{"fontFamily": "sans-serif", "padding": "20px", "color": "#999", "fontSize": "12px"}}
    )
])

if __name__ == "__main__":
    app.run(debug=False, port=8050)
'''
```

## Chart Code Templates

### Histogram (source: cleaned)
```python
fig_hist_{col} = px.histogram(df, x="{col}", title="Distribution of {col}")
```

### Bar Chart (source: cleaned)
```python
fig_bar_{col} = px.bar(
    df["{col}"].value_counts().reset_index(),
    x="{col}", y="count",
    title="Frequency of {col}"
)
```

### Correlation Heatmap (source: cleaned)
```python
fig_heatmap = px.imshow(
    df[num_cols].corr().round(2),
    title="Correlation Matrix",
    color_continuous_scale="RdBu_r",
    zmin=-1, zmax=1
)
```

### Feature Importance Bar (source: results)
```python
fig_importance = px.bar(
    results.sort_values("importance", ascending=True).tail(10),
    x="importance", y="feature",
    orientation="h",
    title="Top 10 Feature Importances"
)
```

### Scatter Actual vs Predicted (source: results)
```python
fig_scatter = px.scatter(
    results,
    x="actual", y="predicted",
    title="Actual vs Predicted"
)
```

### Group Comparison Bar (source: cleaned)
```python
fig_group = px.box(
    df, x="{cat_col}", y="{num_col}",
    title="{num_col} by {cat_col}"
)
```

## Layout Component Template
```python
layout_component = '''
    html.Div([
        dcc.Graph(figure=fig_{name})
    ], style={{"padding": "20px"}}),
'''
```

## Notes
- Generate entire app.py as a single string, write in one pass
- Use plotly.express only - no plotly.graph_objects
- Max 6 charts per dashboard
- Always use relative path via Path(__file__) for data loading
- df = cleaned.parquet, results = results.parquet

## Self-Improvement
Append new chart templates here.
