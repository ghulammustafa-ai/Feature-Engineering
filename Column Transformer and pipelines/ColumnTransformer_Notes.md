
# 📘 ColumnTransformer – Complete Notes

---

## 🔶 1. Why Do We Need ColumnTransformer?

In real-world datasets, we often have **different types of columns**:

- Numerical columns (e.g., Age, Salary)
- Categorical columns (e.g., Gender, City)
- Text, datetime, etc.

Each type of column needs **different preprocessing**:
- Scale numeric features
- Encode categorical features
- Pass-through or drop some columns

Manually applying transformations to each type is hard and error-prone.  
That’s where **ColumnTransformer** comes in – it allows you to apply different preprocessing steps **to different columns** in one unified way.

---

## 🔶 2. What is ColumnTransformer?

`ColumnTransformer` is a class in `sklearn.compose` that lets you:

✅ Apply different transformations to **different subsets of features**  
✅ Combine preprocessing steps into a pipeline  
✅ Automatically return a transformed NumPy array  

---

## 🔶 3. Import Statement

```python
from sklearn.compose import ColumnTransformer
```

---

## 🔶 4. Basic Syntax

```python
ColumnTransformer(
    transformers=[
        ('name1', transformer1, [column_list1]),
        ('name2', transformer2, [column_list2]),
        ...
    ],
    remainder='drop' or 'passthrough'
)
```

### Parameters:
- **`transformers`**: A list of tuples.
  - Each tuple contains:  
    `("name", transformer, columns_to_apply_on)`
- **`remainder`**: What to do with columns not listed.
  - `'drop'`: drop them (default)
  - `'passthrough'`: keep them as-is

---

## 🔶 5. Example Use Case

### Dataset:
```python
import pandas as pd

df = pd.DataFrame({
    'age': [25, 30, 45],
    'gender': ['Male', 'Female', 'Female'],
    'city': ['Lahore', 'Karachi', 'Islamabad']
})
```

### Transformers:
- Scale `age`
- One-Hot Encode `gender` and `city`

---

### Full Code:

```python
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import StandardScaler, OneHotEncoder

transformer = ColumnTransformer(transformers=[
    ("num", StandardScaler(), ["age"]),
    ("cat", OneHotEncoder(drop="first"), ["gender", "city"])
], remainder="passthrough")

transformed = transformer.fit_transform(df)
print(transformed)
```

✅ This will:
- Scale the "age" column
- One-hot encode "gender" and "city"
- Keep any remaining columns as-is

---

## 🔶 6. Output Explanation

The result is a **NumPy array**, not a DataFrame.

You can convert it back using:

```python
import numpy as np
pd.DataFrame(transformed)
```

Or, use `get_feature_names_out()` for proper column names if needed.

---

## 🔶 7. Using ColumnTransformer in a Pipeline

ColumnTransformer works well inside a pipeline with a model:

```python
from sklearn.pipeline import Pipeline
from sklearn.linear_model import LogisticRegression

pipe = Pipeline(steps=[
    ("preprocessor", transformer),
    ("model", LogisticRegression())
])

pipe.fit(X_train, y_train)
```

✅ This makes your **entire workflow clean and reusable**.

---

## 🔶 8. Advantages of ColumnTransformer

✅ Clean and modular preprocessing  
✅ Easy to scale or encode specific columns  
✅ Works well with pipelines  
✅ Avoids data leakage by separating preprocessing for train/test sets  
✅ Highly compatible with cross-validation

---

## 🔶 9. Tips

- You can use it with **SimpleImputer, StandardScaler, OneHotEncoder, OrdinalEncoder**, etc.
- Combine with `Pipeline` for full automation
- Use `remainder="passthrough"` to retain untouched columns

---

## 🔶 10. Summary

| Feature        | Description                                          |
|----------------|------------------------------------------------------|
| Purpose        | Apply different transformers to different columns    |
| Syntax         | List of (name, transformer, columns)                 |
| Remainder      | 'drop' or 'passthrough'                              |
| Used With      | Pipelines, models                                    |
| Input/Output   | Takes DataFrame, returns NumPy array                 |

---
