# Skill: ml_test_run

## Purpose
Run a short ML test run to validate model feasibility. Max 5 minutes runtime.

## When to Apply
Only after ml_preparation, for predictive or explanatory question types.

## Code
```python
import time
from sklearn.ensemble import RandomForestClassifier, RandomForestRegressor
from sklearn.linear_model import LogisticRegression, LinearRegression
from sklearn.metrics import (accuracy_score, f1_score, r2_score,
                             mean_absolute_error, classification_report)

start_time = time.time()
results = {}

# Simple config for test run
rf_params = {"n_estimators": 50, "max_depth": 3, "random_state": 42, "n_jobs": -1}
lr_params = {"max_iter": 100, "random_state": 42}

if task_type == "classification":
    models = {
        "Random Forest": RandomForestClassifier(**rf_params),
        "Logistic Regression": LogisticRegression(**lr_params)
    }
    for name, model in models.items():
        if time.time() - start_time > 270:  # stop at 4.5 min
            print(f"TIMEOUT: skipping {name}")
            continue
        model.fit(X_train, y_train)
        y_pred = model.predict(X_test)
        results[name] = {
            "accuracy": accuracy_score(y_test, y_pred).round(4),
            "f1": f1_score(y_test, y_pred, average="weighted").round(4)
        }
        print(f"{name}: accuracy={results[name]['accuracy']}, f1={results[name]['f1']}")

elif task_type == "regression":
    models = {
        "Random Forest": RandomForestRegressor(**rf_params),
        "Linear Regression": LinearRegression()
    }
    for name, model in models.items():
        if time.time() - start_time > 270:
            print(f"TIMEOUT: skipping {name}")
            continue
        model.fit(X_train, y_train)
        y_pred = model.predict(X_test)
        results[name] = {
            "r2": r2_score(y_test, y_pred).round(4),
            "mae": mean_absolute_error(y_test, y_pred).round(4)
        }
        print(f"{name}: r2={results[name]['r2']}, mae={results[name]['mae']}")

# Feature importance (Random Forest only)
rf_model = models.get("Random Forest")
if rf_model and hasattr(rf_model, "feature_importances_"):
    importances = pd.Series(rf_model.feature_importances_, index=X.columns)
    print("\n=== Top 10 Feature Importances ===")
    print(importances.sort_values(ascending=False).head(10).round(4))

elapsed = time.time() - start_time
print(f"\nTest run completed in {elapsed:.1f}s")
```

## Interpretation Rules

### Classification
- accuracy > 0.85 - strong baseline, recommend full training
- accuracy 0.70-0.85 - decent baseline, recommend hyperparameter tuning
- accuracy < 0.70 - weak baseline, review features or class imbalance

### Regression
- r2 > 0.7 - strong baseline, recommend full training
- r2 0.4-0.7 - moderate baseline, recommend feature engineering
- r2 < 0.4 - weak baseline, review target variable and features

## Notes
- This is a test run only - results are indicative, not final
- Always write full training code to notebook regardless of test run result
- If timeout occurs, log and write code to notebook without metrics

## Self-Improvement
Append new model selection patterns here.
