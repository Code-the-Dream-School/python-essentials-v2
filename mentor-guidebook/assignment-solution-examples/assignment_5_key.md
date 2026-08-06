# Assignment 5 Answer Key: Data Cleaning and Validation with Pandas

**Mentor note:** This lesson is **not** a `.py` file with PyTest — it is a **Kaggle Jupyter notebook** (`CTD_Assignment_5`) that uses pandas over datasets loaded from Kaggle. There is no automated test to lean on, so review by reading the notebook and, ideally, running "Run All" on the shared Kaggle link. The reference snippets below show one clean, correct approach; students will legitimately vary in style, variable names, and how they handle judgment calls. Most cleaning steps have a single expected operation (marked **Objective**); the reflection and final-project tasks are open-ended (**Subjective**). The data files live on Kaggle input datasets, not in the repo, so exact row counts depend on the dataset version — check the student's `info()` output rather than a memorized number.

---

## File Setup

- Work is a **Kaggle notebook** named `CTD_Assignment_5` (not a branch/PR in the python_homework repo).
- Each task should be introduced by a **Markdown cell** ("Task 1", etc.) followed by one or more **code cells**. Using `## ` level-two headings is encouraged — Kaggle builds a table of contents from them.
- Datasets are added via Kaggle "Add Input": search terms are given per task ("code the dream lesson 5", "Code The Dream Eclipses", "Code The Dream Assignment 6"). Students must resolve the input file path (e.g. `/kaggle/input/.../file.csv`) rather than hard-coding a local path.
- `import pandas as pd` (and `import numpy as np`) once near the top; cells share state.
- Submission is the **public Kaggle share link** (Save Version → Share → Public, comments on), pasted into the assignment form.

Check that the link is public with comments enabled, cells run top-to-bottom without errors (Run All), and each task is labeled with a markdown cell.

---

## Task 1: Handling Missing Data — **Objective**

Load the "lesson 5" CSV into `df`, then produce `df1` (dropna) and `df2` (filled, then dropna + reset index + Age as int).

```python
import pandas as pd

df = pd.read_csv("/kaggle/input/.../lesson5.csv")   # resolve the actual input path
print(df)

df1 = df.dropna()
df.info()
df1.info()

df['Name'] = df['Name'].fillna('Unknown')
df['Age'] = df['Age'].fillna(df['Age'].mean())
df['Salary'] = df['Salary'].fillna(df['Salary'].median())
df['Join Date'] = df['Join Date'].fillna('2020-01-01')

df2 = df.dropna().reset_index(drop=True)
df2['Age'] = df2['Age'].astype(int)
print(df2)
```

- `Age` is filled with the **mean**, `Salary` with the **median** — a common mistake is swapping these or using one strategy for both.
- After filling, only `City` still has NaNs, so the final `dropna()` drops just the row(s) with a missing city. `reset_index(drop=True)` is required so the index is clean.
- `astype(int)` must come **after** the mean-fill (you can't cast a column with NaN to int). Expect the mean to be a `.5` value that truncates on cast (e.g. mean 32.5 → 32) — that is fine.
- Watch that they don't `print(df.info())` — `info()` prints directly and returns `None`.

---

## Task 2: Data Transformation (datetime coercion) — **Objective**

Load the "Eclipses" CSV with a **pipe** separator into `df3`, inspect it, then convert `Date` to datetime with `errors='coerce'`.

```python
df3 = pd.read_csv("/kaggle/input/.../eclipses.csv", sep="|")
df3.info()
print(df3.head())

# df3['Date'] = pd.to_datetime(df3['Date'])          # raises on the bad date
df3['Date'] = pd.to_datetime(df3['Date'], errors='coerce')
print(df3.head(20))
```

- The `sep="|"` is essential — without it the whole row loads into one column.
- The point of the task: with `errors='coerce'`, unparseable dates become `NaT` (not a crash). Students should note in a comment/markdown that the invalid date shows as `NaT`.
- Showing the first plain `pd.to_datetime()` raising an error (then fixing it) is the intended learning step; it's fine if they leave it commented out or just describe it.

---

## Task 3: Validating Data Ranges — **Objective**

Force ages outside 18–65 to `np.nan`, then fill with the median.

```python
import numpy as np

df2.loc[(df2['Age'] < 18) | (df2['Age'] > 65), 'Age'] = np.nan
print(df2)

df2['Age'] = df2['Age'].fillna(df2['Age'].median())
print(df2)
```

- Must use `np.nan`, **not** the string `'NaN'` — the assignment calls this out explicitly.
- A `.loc[mask, 'Age']` assignment (or `.where`/`.mask`) is expected; any correct boolean-mask approach is fine.
- Fill uses the **median** of Age.

---

## Task 4: Removing Duplicates & Outliers — **Objective**

Two parts: dedupe `df3`, then handle Age outliers on `df2`.

```python
# Part 1 — duplicates on df3
df3.info()
duplicate_series = df3.duplicated()
print(df3[duplicate_series == True].head(10))
print(duplicate_series.value_counts())
df3 = df3.drop_duplicates()
df3.info()

# Part 2 — outliers on df2 (values > 100 or < 0)
median_age = df2['Age'].median()
df2.loc[(df2['Age'] > 100) | (df2['Age'] < 0), 'Age'] = median_age
print(df2)
```

- `duplicated()` returns a boolean Series; `value_counts()` shows how many `True`/`False`. `drop_duplicates()` keeps the first occurrence by default — that default is acceptable here.
- Part 2 uses **fixed thresholds** (>100 or <0), not a statistical/IQR method, and replaces with the **median**. Given Task 3 already clamped Age to 18–65, there may be no outliers left in `df2` — that's expected; the skill is writing the replacement correctly, not finding a hit.

---

## Task 5: Standardizing Data — **Objective**

Lowercase/trim `Name`, then unify `City` variants.

```python
df['Name'] = df['Name'].str.lower().str.strip()
print(df)

print(df.groupby('City').agg({'Name': 'count'}))   # reveals variants

df['City'] = df['City'].replace({'NYC': 'New York', 'LA': 'Los Angeles'})
print(df)
```

- Chaining `.str.lower().str.strip()` (either order) is the expected idiom.
- The `groupby` is a diagnostic step to spot inconsistent spellings before mapping them. Which variants exist depends on the dataset — accept any reasonable mapping that consolidates the obvious duplicates (`NYC`→`New York`, `LA`→`Los Angeles`). `.replace({...})` or `.map(...)`/`.str.replace(...)` are all fine.

---

## Task 6: Encoding Categorical Variables — **Objective**

```python
df_color = pd.DataFrame({'Color': ['Red', 'Blue', 'Green', 'Blue']})
df_color['Color_Label'] = df_color['Color'].map({'Red': 1, 'Blue': 2, 'Green': 3})
df_encoded = pd.get_dummies(df_color['Color'], prefix='Color')
print(df_color)
print(df_encoded)
```

- `map()` produces a single **ordinal** integer column; `get_dummies()` produces **one-hot** boolean columns (`Color_Red`, `Color_Blue`, `Color_Green`). The exercise is about seeing both encodings side by side.
- In modern pandas the dummy columns are `True`/`False` (boolean) rather than `1`/`0` — that is correct, not a bug.

---

## Task 7: Consolidating Messy Files (Mini Project) — **Objective**

Load 4 CSVs (~400 rows each), concatenate to `df_all` (~1600), fuzzy-fix names/addresses, mode-fix Zip/Phone by group, then dedupe to ~400.

```python
import pandas as pd

paths = ["/kaggle/input/.../file1.csv", ...]           # the 4 input files
df_all = pd.concat([pd.read_csv(p) for p in paths], ignore_index=True)

try:
    from thefuzz import process
except ImportError:
    !pip install thefuzz
    from thefuzz import process

def fix_column(col):
    counts = df_all.value_counts(col)
    good = list(counts[counts > 2].index)
    df_all[col] = df_all[col].map(
        lambda x: x if x in good else process.extractOne(x, good)[0]
    )

fix_column("Name")
fix_column("Address")

def fix_anomaly(group):
    group_na = group.dropna()
    if group_na.empty:
        return group
    mode = group_na.mode()
    if mode.empty:
        return group
    return mode.iloc[0]

df_all["Zip"]   = df_all.groupby(["Name", "Address"])["Zip"].transform(fix_anomaly)
df_all["Phone"] = df_all.groupby(["Name", "Address"])["Phone"].transform(fix_anomaly)

df_all = df_all.drop_duplicates()
df_all.info()
```

- Most of this code is given in the assignment; the graded skills are: concatenating with `ignore_index=True`, installing/importing `thefuzz`, applying the same fix to both `Name` and `Address` (a helper is the DRY win but two copied blocks pass too), and using `groupby(...).transform(...)` to broadcast the per-group mode back to every row.
- Expect roughly **400 unique records** after `drop_duplicates()`. The exact count and remaining null count depend on the data — don't fail a student over a small difference; check that the pipeline runs and the counts are in the right ballpark.
- The idea behind `fix_anomaly`: within a (Name, Address) group, the correct Zip/Phone is the most common non-null value; scattered wrong values get overwritten.

---

## Task 8: Regular Expressions for Validation — **Objective**

These four are worked examples — students run them and observe the output. Reference results:

```python
# 1. Extract: 3 capture groups -> timestamp, level, message
extracted_logs = log_entries.str.extract(r"\[(.*?)\]\s(\w+):\s(.*)")
# columns 0/1/2 = "2023-10-26 10:00:00" / "INFO" / "User logged in", etc.

# 2. Replace bracketed placeholders of any style with "<VALUE>"
standardized_text = text_data.replace(
    [r"\{.*?\}", r"\[.*?\]", r"\(.*?\)", r"\<.*?\>"], "<VALUE>", regex=True)
# "Value is <VALUE>.", "The price is <VALUE>.", "Cost: <VALUE>", "Quantity = <VALUE>"

# 3. filter(regex="_at$") -> ["created_at", "updated_at"] (drops order_id)
time_cols = df.filter(regex="_at$")

# 4. Case-insensitive "ship" match -> keeps the two rows containing "shipped"/"Shipment"
shipped_orders = orders[orders.str.contains("ship", case=False)]
```

- These are demonstrations; correctness is just "did they run and show the output." Reviewers can spot-check that `str.extract` returns three columns, `filter(regex="_at$")` excludes `order_id`, and the `contains("ship", case=False)` filter catches both the lowercase and capitalized matches.

---

## Task 9: Reflection & Validation — **Subjective**

A markdown cell covering the three prompts: most common data issues, which techniques worked best, and how to automate cleaning in a real workflow.

- A strong answer names **specific issues actually seen in this notebook** (missing values, wrong dtypes/unparseable dates, duplicates, out-of-range ages, inconsistent text/city spellings, messy names across files) and ties each to the technique that fixed it (`fillna`/`dropna`, `to_datetime(errors='coerce')`, `drop_duplicates`, range masking, `str` methods, fuzzy matching).
- The automation prompt should mention wrapping steps into **reusable functions / a pipeline**, validating on load, and applying consistent rules to new files — not just "do it again by hand."
- A weak answer is generic ("cleaning is important") with no reference to what they actually did, or lists techniques without connecting them to the problems they solve.

---

## Task 10: Start Your Final Project — **Subjective**

Create a separate Kaggle notebook (e.g. `CTD Python Data Pipeline`), pick one of the four datasets (Global Superstore, TMDB 5000 Movies, Life Expectancy WHO, Seattle Airbnb — or a CIL-approved alternative), and begin cleaning with markdown notes.

- This week only requires a **start**: the dataset loaded with the correct delimiter, an initial quality look (`head()`/`info()`), and at least some cleaning (missing values, duplicates, types, or outliers) documented in markdown cells explaining what was wrong and how it was fixed.
- A good submission shows the student has chosen a dataset and can articulate its quality issues; the full pipeline, feature engineering, aggregation, and 3+ visualizations come in later weeks.
- Submitted as a **second** public Kaggle link in the form. Don't grade against the full capstone rubric yet — just check that the notebook exists, loads data, and shows early, documented cleaning.

---

*Note for reviewers:* The recorded/registration steps (creating a Kaggle account, the "Run All" / session-timeout mechanics) are process guidance, not graded code.
