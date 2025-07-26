
# 🧠 Machine Learning Pipelines – Complete Notes

---

## 📌 What is a Pipeline?

A **Pipeline** in machine learning is a way to streamline the **workflow of data preprocessing + model building** by chaining multiple steps together into a **single object**.

It ensures that the **exact same preprocessing steps** are applied to **both training and test data**, preventing **data leakage** and improving **code reproducibility** and **readability**.

---

## ✅ Why Use Pipelines?

| Reason | Explanation |
|--------|-------------|
| 🔁 Repeatability | Ensures same preprocessing steps on training and test data. |
| 💡 Clean Code | Makes code modular and readable. |
| 🚫 Prevent Data Leakage | Keeps preprocessing confined within cross-validation. |
| 🧪 Cross-validation Integration | Allows preprocessing during CV loops. |
| 📦 Deployment Ready | Easily export the whole process with `joblib` or `pickle`. |

---

## 🧱 Pipeline Structure

A pipeline is a sequence of steps where each step is a **tuple** of:

```
("step_name", transformer or estimator)
```

✔ Transformers: Modify data using `.fit()` and `.transform()`  
✔ Final Step: Must be an estimator (e.g., model) with `.fit()` and `.predict()`

---

## 📘 Basic Example

```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression

pipe = Pipeline([
    ('scaler', StandardScaler()),
    ('model', LogisticRegression())
])

pipe.fit(X_train, y_train)
pipe.predict(X_test)
```

---

## 🔀 Steps Typically Included in a Pipeline

1. **Imputation**: Handle missing values  
   `SimpleImputer`, `KNNImputer`

2. **Encoding**: Convert categories to numbers  
   `OneHotEncoder`, `OrdinalEncoder`

3. **Scaling**: Normalize numerical data  
   `StandardScaler`, `MinMaxScaler`, `RobustScaler`

4. **Feature Selection / Dimensionality Reduction**:  
   `PCA`, `SelectKBest`

5. **Model**: Estimator like  
   `LogisticRegression`, `RandomForestClassifier`, etc.

---

## ⚙️ Using `ColumnTransformer` with Pipeline

To apply different preprocessing to different columns:

```python
from sklearn.compose import ColumnTransformer
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import OneHotEncoder, StandardScaler
from sklearn.ensemble import RandomForestClassifier

numeric_features = ["Age", "Fare"]
numeric_transformer = Pipeline([
    ("imputer", SimpleImputer(strategy="mean")),
    ("scaler", StandardScaler())
])

categorical_features = ["Sex", "Embarked"]
categorical_transformer = Pipeline([
    ("imputer", SimpleImputer(strategy="most_frequent")),
    ("encoder", OneHotEncoder(drop="first"))
])

preprocessor = ColumnTransformer([
    ("num", numeric_transformer, numeric_features),
    ("cat", categorical_transformer, categorical_features)
])

clf_pipeline = Pipeline([
    ("preprocessor", preprocessor),
    ("classifier", RandomForestClassifier())
])

clf_pipeline.fit(X_train, y_train)
y_pred = clf_pipeline.predict(X_test)
```

---

## 🔁 Cross-Validation with Pipelines

```python
from sklearn.model_selection import cross_val_score

scores = cross_val_score(clf_pipeline, X, y, cv=5, scoring='accuracy')
print("CV Accuracy:", scores.mean())
```

✅ Automatically handles fitting and transforming inside each fold.

---

## 📦 Saving and Loading the Pipeline

```python
import joblib

# Save
joblib.dump(clf_pipeline, 'pipeline.pkl')

# Load
loaded_pipeline = joblib.load('pipeline.pkl')
loaded_pipeline.predict(X_test)
```

---

## 🧪 Grid Search with Pipelines

```python
from sklearn.model_selection import GridSearchCV

param_grid = {
    "classifier__n_estimators": [50, 100],
    "classifier__max_depth": [None, 10, 20]
}

grid = GridSearchCV(clf_pipeline, param_grid, cv=5)
grid.fit(X_train, y_train)
```

Note:  
Use `__` to access parameters of nested steps in the pipeline.

---

## 🧠 Best Practices

- Use pipelines to **avoid data leakage**.
- Always include preprocessing inside the pipeline.
- Use `ColumnTransformer` for handling multiple column types.
- Include all steps: imputation → encoding → scaling → modeling.
- Use grid search or cross-validation with pipelines for best results.

---

## 🧰 Summary

| Component | Role |
|-----------|------|
| `Pipeline()` | Chain steps into one object |
| `SimpleImputer()` | Fill missing values |
| `OneHotEncoder()` | Convert categories to binary |
| `StandardScaler()` | Normalize numeric data |
| `ColumnTransformer()` | Apply transformations to specific columns |
| `GridSearchCV()` | Tune hyperparameters with pipeline |
| `cross_val_score()` | Evaluate entire pipeline using CV |
