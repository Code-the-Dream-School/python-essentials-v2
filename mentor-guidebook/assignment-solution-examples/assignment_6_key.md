# Assignment 6 Answer Key: Data Wrangling and Aggregation

**Mentor note:** This lesson is done entirely in a **Kaggle notebook**, not in the `python_homework` repo, so there is no PyTest file. Students submit two public Kaggle links: one for this assignment (`CTD_Assignment_6`) and one for their capstone (Task 13). Review by reading the notebook cells and confirming the output shown under each cell matches what the task asks for.

Tasks 1, 2, and 5–10 use small hard-coded sample DataFrames, so their outputs are fully deterministic — the expected results are given below. Tasks 3, 4, 11, and 12 use external Kaggle datasets that change over time and are not in this repo, so those give reference code and describe what a correct result looks like rather than exact numbers.

Reference code shows one clean approach; students may vary in style (e.g. `.agg()` vs. named calls) and still be correct.

---

## File Setup

- Work is a single Kaggle notebook named **`CTD_Assignment_6`** (created from Kaggle's "+" / New Notebook button).
- Each task should have a **markdown cell** labeling it (e.g. "Task 1") followed by one or more **code cells**. Encourage students to run every cell so output is visible in the saved version.
- Tasks 3, 4, and 11 require adding Kaggle **datasets as inputs** (Ecommerce Consumer Behavior, AI-Powered Job Recommendations, and international football results). The starter cell that walks `/kaggle/input` is used to find file paths.
- Submission: **Save Version** (quick save), then **Share → Public** with comments on, and paste the public URL into the submission form. The capstone notebook URL goes in the second field.
- Common review check: the saved version must actually show cell outputs. A notebook with no run output is hard to assess.

---

## Task 1: Data Selection — **Objective**

Build `df1`, `df2`, `df3` from the provided dicts, then select from `df1`.

```python
import pandas as pd

df1 = pd.DataFrame({'Name':['Alice','Bob','Charlie','David','Eva'],
                    'Age':[25,30,35,40,30],
                    'Salary':[50000,60000,70000,80000,55000]})
df2 = pd.DataFrame({'Name':['Frank','Grace','Helen','Ian','Jack'],
                    'Age':[28,33,35,29,40],
                    'Salary':[52000,58000,72000,61000,85000]})
df3 = pd.DataFrame({'Name':['Frank','Helen','Ian','Hima','Chaka'],
                    'Age':[17,93,12,57,106],
                    'Favorite Color':['blue','pink','burgundy','red','turquoise']})

print(df1['Name'])              # single column (a Series)
print(df1[['Name','Salary']])   # two columns (a DataFrame)
print(df1.iloc[0:3])            # first three rows by integer position
```

- Students were told they may change the values — if they did, later Objective outputs will differ. Judge the **operations**, not the literal numbers, when they customized the data.
- `df1[['Name','Salary']]` needs **double brackets** (a list of columns). Single brackets with two names is an error.
- The slice must use `iloc` (integer-based), not `loc`.

---

## Task 2: Data Aggregation — **Objective**

Group `df1` by `Age`, aggregate `Salary` with mean, sum, and count.

```python
print(df1.groupby('Age')['Salary'].agg(['mean','sum','count']))
```

Expected output (with the provided data):

```
        mean     sum  count
Age
25   50000.0   50000      1
30   57500.0  115000      2
35   70000.0   70000      1
40   80000.0   80000      1
```

- Age 30 is the only group with two people (Bob 60000, Eva 55000) → mean 57500, count 2. That row is the tell that grouping worked.

---

## Task 3: Practice Pivot Tables — **Objective (external dataset — verify structure)**

Uses the "Ecommerce Consumer Behavior" dataset (Salahuddin Ahmed). Not in this repo, so exact numbers can't be given; check the code and shape.

Convert `Purchase_Amount` from a `$`-string to float, then build two pivot tables.

```python
ecommerce = pd.read_csv('/kaggle/input/.../<file>.csv')
print(ecommerce.head())

ecommerce['Purchase_Amount'] = ecommerce['Purchase_Amount'].str.replace('$','').astype(float)

buying_patterns = ecommerce.pivot_table(index='Purchase_Category',
                                        columns=['Gender','Income_Level'],
                                        values='Purchase_Amount',
                                        aggfunc='sum')
print(buying_patterns)

demographics = ecommerce.pivot_table(index=['Income_Level','Education_Level'],
                                     columns='Marital_Status',
                                     values='Customer_ID',
                                     aggfunc='count')
print(demographics)
```

- The `$` strip must happen **before** `astype(float)`; `str.replace('$','')` (or `.str.replace('$','',regex=False)`) is the key step. Forgetting it raises a conversion error.
- `buying_patterns`: index = `Purchase_Category`, columns = the (`Gender`, `Income_Level`) MultiIndex, values summed.
- `demographics`: two-level index (`Income_Level`, `Education_Level`), columns = `Marital_Status`, counting `Customer_ID`.
- Common miss: using the default `aggfunc='mean'` on the count pivot instead of `'count'`.

---

## Task 4: Practice apply() — **Objective (external dataset — verify logic)**

Uses "AI-Powered Job Recommendations" (Samay Ashar). Column names vary, so students must read `jobs.head(10)` first and match names.

```python
jobs = pd.read_csv('/kaggle/input/.../<file>.csv')
print(jobs.head(10))

def check(row):
    if (row['Job Level'] == 'Entry' and row['Salary'] >= 70000
            and 'SQL' in row['Required Skills'] and 'Python' in row['Required Skills']):
        return "Yes"
    return "No"

jobs['Check These Out'] = jobs.apply(check, axis=1)

my_jobs = jobs[jobs['Check These Out'] == 'Yes']
print(my_jobs.head(10))
```

- `apply()` must use **`axis=1`** so the function receives a whole row. Without it, the function gets a column and the logic breaks.
- All four conditions combined with `and`: entry level, salary ≥ 70000, and both `SQL` and `Python` in the skills field. Exact column names depend on the dataset.
- `my_jobs` is a boolean-mask filter on the new column — should contain only `"Yes"` rows.

---

## Task 5: Merging and Joining DataFrames — **Objective**

### Part 1 — outer merge of `df1` and `df3` on `Name`

```python
import numpy as np

df_1_3_merged = pd.merge(df1, df3, on='Name', how='outer', suffixes=['_left','_right'])
df_1_3_merged['Salary'] = df_1_3_merged['Salary'].fillna(15000)
df_1_3_merged['Favorite Color'] = df_1_3_merged['Favorite Color'].fillna('yellow')
df_1_3_merged['Age'] = np.where(df_1_3_merged['Age_left'].notna(),
                                df_1_3_merged['Age_left'],
                                df_1_3_merged['Age_right'])
df_1_3_merged = df_1_3_merged.drop(columns=['Age_left','Age_right'])
print(df_1_3_merged)
```

Expected final DataFrame (with the provided data):

```
      Name   Salary Favorite Color    Age
0    Alice  50000.0         yellow   25.0
1      Bob  60000.0         yellow   30.0
2    Chaka  15000.0      turquoise  106.0
3  Charlie  70000.0         yellow   35.0
4    David  80000.0         yellow   40.0
5      Eva  55000.0         yellow   30.0
6    Frank  15000.0           blue   17.0
7    Helen  15000.0           pink   93.0
8     Hima  15000.0            red   57.0
9      Ian  15000.0       burgundy   12.0
```

- Outer merge yields **10 rows** — only names in both frames (Frank, Helen, Ian) could have shared, but here `df1` and `df3` share no names, so every row has NaN on one side. That's expected and is the whole point of the `np.where` step.
- Only `df1`/`df3` people who lacked a salary get `15000`; `Favorite Color` NaN → `yellow`.
- `np.where(cond, a, b)` acts like a ternary: keep `Age_left` where it's not NaN, else `Age_right`. Then both `_left`/`_right` age columns are dropped.

### Part 2 — join method

```python
df1_b = df1.set_index('Name')
df3_b = df3.set_index('Name')
print(df1_b.join(df3_b, how='outer', lsuffix='_left', rsuffix='_right'))
```

- `join` aligns on the **index**, so `Name` must be set as index on both first.
- `join` gives no default suffixes for the overlapping `Age` column — the student must supply `lsuffix`/`rsuffix` or the call errors. This is the point the assignment is testing.

---

## Task 6: Filtering Rows — **Objective**

```python
print(df1[df1['Age'] > 30])
```

Expected: Charlie (35) and David (40) only.

---

## Task 7: Sorting Data — **Objective**

```python
print(df1.sort_values('Salary', ascending=False))
```

Expected order: David (80000), Charlie (70000), Bob (60000), Eva (55000), Alice (50000).

- Must use `sort_values`, not `sort_index`. `ascending=False` for descending.

---

## Task 8: Renaming Columns — **Objective**

```python
df1_renamed = df1.rename(columns={'Age':'Employee Age', 'Salary':'Employee Salary'})
print(df1_renamed)
```

- Do **not** use `inplace=True` (assignment says so, because Task 9 needs the original `df1`). A dict of `old:new` is passed to `columns=`.

---

## Task 9: Data Transformation — **Objective**

Increase every salary by 10%.

```python
df1_raised = df1.copy()
df1_raised['Salary'] = df1_raised['Salary'] * 1.1
print(df1_raised)
```

Expected salaries: 55000, 66000, 77000, 88000, 60500.

- Vectorized `* 1.1` on the column is expected (an `apply`/lambda also works). Watch that they didn't overwrite the original `df1` if later tasks reuse it.

---

## Task 10: Concatenating DataFrames — **Objective**

```python
print(pd.concat([df1, df2], ignore_index=True))
```

Expected: 10 rows, Alice→Eva then Frank→Jack, index reset 0–9.

- `ignore_index=True` is required so the index runs 0–9 instead of repeating 0–4.

---

## Task 11: Data Wrangling a Kaggle Dataset (football) — **Objective (external dataset — verify structure)**

Uses "international football results from 1872 to 2017" (Mart Jürisoo), file `results.csv`. Reshape home/away into one row-per-team-per-game frame, then find worst defenses.

```python
football_results = pd.read_csv('/kaggle/input/.../results.csv')
print(football_results.head())

results_1 = football_results[['home_team','away_team','home_score','away_score','date']]

results_2 = results_1.rename(columns={'home_team':'team','away_team':'opponent',
                                      'home_score':'points_for','away_score':'points_against'})
results_3 = results_1.rename(columns={'away_team':'team','home_team':'opponent',
                                      'away_score':'points_for','home_score':'points_against'})

football_results = pd.concat([results_2, results_3], ignore_index=True)
print(football_results.head())

points_against = football_results.groupby('team')['points_against'].mean()
points_against = points_against.sort_values(ascending=False)
print(points_against.head(10))
```

- The reshape is the crux: `results_2` = home perspective, `results_3` = away perspective (note how the rename **swaps** for/against and team/opponent). Concatenating gives every team both its home and away games.
- After the concat, `football_results` is **reassigned** to the combined frame — Task 12 depends on this reshaped version.
- `points_against` is a **Series** (mean per team), sorted descending. The top 10 are the worst-defense teams. Exact teams depend on the current dataset, but they tend to be small/amateur national sides with very high average goals conceded.

---

## Task 12: Most Recent 10 Games for Tunisia — **Objective (external dataset — verify logic)**

Students figure out the steps themselves, using the reshaped `football_results` from Task 11.

```python
tunisia = football_results[football_results['team'] == 'Tunisia']
tunisia = tunisia.sort_values('date', ascending=False)
print(tunisia.head(10))
```

- Two required moves: **filter** to `team == 'Tunisia'`, then **sort by date descending** and take the first 10. Sorting is essential — the assignment explicitly warns "sort these so you get the right games."
- If `date` sorts oddly, converting with `pd.to_datetime` first is a valid, more robust approach.
- Should avoid `inplace=True` (store in a new DataFrame), as the assignment notes.

---

## Task 13: Continue Your Final Project (Week 6 Progress) — **Subjective**

Work happens in the student's **capstone** notebook (submitted as the second link), applying this week's techniques to their own cleaned dataset.

A strong submission includes:
- **Wrangling:** filtering with `loc`/boolean masks, column selection, and at least one string operation, on their real dataset.
- **Aggregation:** at least two `groupby` summaries using functions like `sum`/`mean`/`count`/`agg`, with the results actually shown.
- **Feature engineering:** at least two new columns via `apply()`, `map()`, arithmetic, or `pd.cut()` bins.
- **A markdown cell** explaining what transformations were done, why, and what the aggregations revealed.

A weak submission copies the lesson's sample DataFrames instead of using their own data, adds fewer than two aggregations or engineered features, or omits the written reflection connecting the operations to insights. Interpretation of results — not just running the methods — is what the rubric rewards.
