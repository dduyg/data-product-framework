# 🧪 ETL to ML Pipeline: A Full Tutorial

### Overview
Building a machine learning system isn't just about training models—it starts with collecting and preparing the data. This tutorial walks you through the **complete journey**:

1. **ETL Process**:
    - **Extract**: Gather data from various sources.
    - **Transform**: Clean, enrich, and restructure it.
    - **Load**: Store the processed data in a usable format.
2. **Machine Learning Pipeline**:
    - Exploratory Data Analysis (EDA)
    - Feature Engineering
    - Model Training
    - Evaluation
    - Deployment basics

We'll build a pipeline using Python with libraries like `pandas`, `scikit-learn`, and optionally `SQL`, `Airflow`, or `Prefect` for orchestration.

---

## 🧱 Step 1: Extract

### 🎯 Goal: Gather raw data from source(s)

Data can come from:

- APIs
- Databases (SQL, NoSQL)
- CSV/Excel files
- Cloud storage (S3, GCS, Azure Blob)

### ✅ Example: Load a CSV file

```python
import pandas as pd

# Extract
raw_data = pd.read_csv("https://raw.githubusercontent.com/datasciencedojo/datasets/master/titanic.csv")
print(raw_data.head())

```

If pulling from SQL:

```python
import sqlalchemy

engine = sqlalchemy.create_engine("postgresql://user:password@host:5432/db")
raw_data = pd.read_sql("SELECT * FROM passengers", con=engine)

```

---

## 🔧 Step 2: Transform

### 🎯 Goal: Clean, enrich, and reshape the data

Key tasks:

- Handle missing values
- Encode categorical variables
- Normalize/standardize features
- Feature extraction and selection

### ✅ Example: Clean the Titanic dataset

```python
# Drop columns that won't help
data = raw_data.drop(['PassengerId', 'Name', 'Ticket', 'Cabin'], axis=1)

# Fill missing values
data['Age'].fillna(data['Age'].median(), inplace=True)
data['Embarked'].fillna(data['Embarked'].mode()[0], inplace=True)

# Convert categoricals to numeric
data = pd.get_dummies(data, columns=['Sex', 'Embarked'], drop_first=True)

# View transformed data
print(data.head())

```

---

## 📥 Step 3: Load

### 🎯 Goal: Store clean data in a structured, accessible way

Options:

- Save as a CSV/parquet file
- Load into a database table
- Upload to cloud storage
- Use in-memory for ML pipeline

### ✅ Example: Save transformed data

```python
data.to_csv("cleaned_titanic.csv", index=False)

```

Or load into PostgreSQL:

```python
data.to_sql("cleaned_passengers", con=engine, if_exists='replace')

```

---

## 📊 Step 4: Exploratory Data Analysis (EDA)

### 🎯 Goal: Understand the data before modeling

```python
import matplotlib.pyplot as plt
import seaborn as sns

# Correlation heatmap
plt.figure(figsize=(10, 6))
sns.heatmap(data.corr(), annot=True)
plt.show()

# Survival rates by class
sns.barplot(x='Pclass', y='Survived', data=data)
plt.show()

```

---

## 🧠 Step 5: Train a Machine Learning Model

### 🎯 Goal: Build a predictive model

Split features and target:

```python
from sklearn.model_selection import train_test_split

X = data.drop('Survived', axis=1)
y = data['Survived']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

```

Train a simple model:

```python
from sklearn.ensemble import RandomForestClassifier

model = RandomForestClassifier(n_estimators=100, random_state=42)
model.fit(X_train, y_train)

```

---

## 📈 Step 6: Evaluate the Model

```python
from sklearn.metrics import classification_report, confusion_matrix

y_pred = model.predict(X_test)

print(confusion_matrix(y_test, y_pred))
print(classification_report(y_test, y_pred))

```

---

## ⚙️ (Optional) Step 7: Model Tuning

Use grid search or automated tools like Optuna:

```python
from sklearn.model_selection import GridSearchCV

params = {'n_estimators': [50, 100, 200], 'max_depth': [None, 5, 10]}
grid = GridSearchCV(RandomForestClassifier(), params, cv=5)
grid.fit(X_train, y_train)

print(grid.best_params_)

```

---

## 🚀 Step 8: Deploy (Simplified Overview)

You could:

- Save the model with `joblib` or `pickle`
- Serve via a Flask/FastAPI app
- Deploy to AWS SageMaker, Azure ML, or Vertex AI

```python
import joblib

joblib.dump(model, "rf_model.pkl")

```

Basic API example (Flask):

```python
from flask import Flask, request, jsonify
import joblib
import pandas as pd

app = Flask(__name__)
model = joblib.load("rf_model.pkl")

@app.route('/predict', methods=['POST'])
def predict():
    data = pd.DataFrame(request.json)
    prediction = model.predict(data)
    return jsonify(prediction.tolist())

# Run with: flask run

```

---

## 🔁 Automation with ETL Orchestration Tools

To scale the process:

- Use **Apache Airflow**, **Prefect**, or **Dagster**
- Build DAGs to automate: extract → transform → train → deploy

Example DAG with Airflow (pseudo-code):

```python
with DAG("etl_ml_pipeline") as dag:
    t1 = PythonOperator(task_id="extract", python_callable=extract_data)
    t2 = PythonOperator(task_id="transform", python_callable=transform_data)
    t3 = PythonOperator(task_id="train", python_callable=train_model)
    t1 >> t2 >> t3

```

---

## 🧩 Bonus: Add MLOps & Monitoring

For production:

- Track experiments (e.g., MLflow)
- Monitor data drift
- Schedule retraining jobs
- Use CI/CD for ML pipelines

---

## ✅ Recap

| Step | Purpose |
| --- | --- |
| Extract | Load raw data from sources |
| Transform | Clean and prepare data |
| Load | Store or serve clean data |
| EDA | Understand patterns and features |
| Model | Train and tune ML models |
| Evaluate | Test model accuracy |
| Deploy | Serve model predictions |
| Automate | Schedule workflows and retraining |

---

## 🧰 Tools & Libraries Reference

| Task | Tools |
| --- | --- |
| ETL | pandas, SQLAlchemy, Spark, Airflow |
| ML | scikit-learn, XGBoost, LightGBM |
| EDA | pandas, seaborn, matplotlib |
| Deployment | Flask, FastAPI, Docker, SageMaker |
| Automation | Airflow, Prefect, Dagster |
| Tracking | MLflow, DVC |
