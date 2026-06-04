# Skill: ml_preparation

## Purpose
Prepare features and target for ML test run.

## When to Apply
Only for predictive or explanatory question types, after correlation_analysis.

## Model Selection Rules

### Regression (continuous target)
- Default: Random Forest Regressor
- If features < 10 and low complexity: Linear Regression as baseline
- Always run both for comparison in test run

### Classification (categorical target)
- Default: Random Forest Classifier
- If binary target and interpretability needed: Logistic Regression as baseline
- Always run both for comparison in test run

## Code
```python
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelEncoder, StandardScaler
import numpy as np

# Separate features and target
X = df.drop(columns=[target_variable])
y = df[target_variable]

# Encode categorical features
cat_cols = X.select_dtypes(["object", "category"]).columns
le = LabelEncoder()
for col in cat_cols:
    X[col] = le.fit_transform(X[col].astype(str))

# Encode target if classification
if y.dtype in ["object", "category"]:
    task_type = "classification"
    y = le.fit_transform(y.astype(str))
    print(f"Task type: classification | Classes: {np.unique(y)}")
else:
    task_type = "regression"
    print(f"Task type: regression | Target range: {y.min():.2f} - {y.max():.2f}")

# Train/test split - use 20% of data for test run
X_sample, _, y_sample, _ = train_test_split(X, y, train_size=0.2, random_state=42)
X_train, X_test, y_train, y_test = train_test_split(X_sample, y_sample, test_size=0.2, random_state=42)

print(f"Test run sample: {X_train.shape[0]} train, {X_test.shape[0]} test rows")
print(f"Features: {X.shape[1]}")
```

## Notes
- Always use only 20% of data for test run
- Store task_type, X, y, X_train, X_test, y_train, y_test for ml_test_run skill
- Log feature count and class distribution

## Self-Improvement
Append new preparation patterns here.
