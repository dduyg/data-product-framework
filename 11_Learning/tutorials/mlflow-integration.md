# 🤖 Integrating MLflow into Your Data Workflow

MLflow is a powerful open-source platform to manage the ML lifecycle, including experimentation, reproducibility, and deployment. In this tutorial, you'll learn how to integrate MLflow into your existing data pipelines.

---

## 🎯 Objectives

- Log parameters, metrics, and artifacts from experiments
- Track multiple runs and compare performance
- Organize runs with experiment names
- Enable reproducible model training and evaluation

---

## 🧱 Prerequisites

- Python 3.7+
- MLflow installed: `pip install mlflow`
- Familiarity with Pandas, Scikit-learn, or another ML library

---

## 🧪 Step 1: Basic Setup

```python
import mlflow
import mlflow.sklearn
from sklearn.ensemble import RandomForestClassifier
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split

X, y = load_iris(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(X, y)

with mlflow.start_run():
    clf = RandomForestClassifier(n_estimators=100, max_depth=5)
    clf.fit(X_train, y_train)
    
    acc = clf.score(X_test, y_test)

    mlflow.log_param("n_estimators", 100)
    mlflow.log_param("max_depth", 5)
    mlflow.log_metric("accuracy", acc)

    mlflow.sklearn.log_model(clf, "model")
```
---

## 🗂️ Step 2: Track Multiple Runs

You can organize related experiments under a single experiment name:

```python
mlflow.set_experiment("Iris_RF_Classifier")
```

This helps with version control and reproducibility.

---

## 📊 Step 3: View in UI

To launch the MLflow UI locally:

```bash
mlflow ui
```

Then navigate to: [http://localhost:5000](http://localhost:5000/)

---

## 💡 Bonus: Log Additional Artifacts

You can log confusion matrices, plots, or preprocessing artifacts:

```python
import matplotlib.pyplot as plt
from sklearn.metrics import ConfusionMatrixDisplay

ConfusionMatrixDisplay.from_estimator(clf, X_test, y_test)
plt.savefig("confusion_matrix.png")
mlflow.log_artifact("confusion_matrix.png")
```

---

## 🧰 Advanced Tip: Model Registry

You can register and version your models for deployment:

```python
result = mlflow.register_model(
    "runs:/<run_id>/model",
    "IrisClassifier"
)
```

You can then tag it as "Production", "Staging", etc.

---

## 📚 Resources

- [MLflow Docs](https://mlflow.org/docs/latest/index.html)
- [MLflow Tracking](https://mlflow.org/docs/latest/tracking.html)
- [MLflow Model Registry](https://mlflow.org/docs/latest/model-registry.html)
