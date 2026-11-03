# Aarohan AI: Progress Log

## Day 1: Setup, Data Cleaning and Checks

### 1. GitHub and environment setup

I started by setting up GitHub, then created a virtual environment (venv).

A venv is like a fresh, empty Python for this project only. It starts with none of my libraries, so I install pandas, numpy and the rest again, but only inside this box.

- **Benefit:** other projects on my laptop can't affect this one, and this one can't break them.
- **Bonus:** later I'll save the library list in `requirements.txt`, so anyone (or Streamlit Cloud) can rebuild the same environment with one command.
- **Quick check:** the terminal line should start with `(venv)`.

| Library | Job in the project |
|---|---|
| numpy | Fast math on arrays of numbers |
| pandas | Loading and manipulating tables (the CSV) |
| matplotlib, seaborn | Charts |
| scikit-learn | Machine learning models (Day 2) |
| joblib | Saving a trained model to a file |
| streamlit | Turning the code into a web app (Day 3) |
| jupyter, ipykernel | Lets VS Code run notebook files |

### 2. Reading the CSV

```python
df = pd.read_csv("../dataset/student_lifestyle_dataset.csv")
print(df.shape)
df.head()
```

### 3. Data cleaning notebook

**Rows, columns and data types.** I checked the shape, column names and data types with `df.info()`.
- If a numeric column has a wrong type, it shows as `object` in Dtype.
- If values are missing, Non-Null Count is lower than the total. For example, 1950 non-null instead of 2000 means 50 values are missing.
- (Already explained in the notebook.)

**Missing values.** `df.isnull().sum()` counts empty cells in each column. Missing values must be handled because they affect the ML model.

**Duplicate rows.** `df.duplicated().sum()` finds rows that are identical. Duplicates can bias the model and inflate its results.

**Quick statistics.** `df.describe()` gives count, mean, std, min and 25% / 50% / 75% / max. I use it to look for impossible values, such as negative study hours or a GPA outside the valid range.

### 4. Observations

- Many students have physical activity above 10 hours, which is practically impossible.
- To investigate, I checked whether the five daily hour columns add up to 24 for every student.
- **Result:** yes, every student's hours sum to exactly 24. The dataset was designed this way, so it appears synthetic.
- 25 rows have physical activity above 10 hours. I decided to keep them, because 25 out of 2000 rows (about 1%) will not affect the results much.
- `describe()` summarizes only numeric columns, so I use `value_counts()` to summarize `Stress_Level`.