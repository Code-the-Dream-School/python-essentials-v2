---
marp: true
theme: default
paginate: true
style: |
  section {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-size: 28px;
  }
  h1 {
    color: #1a73e8;
  }
  h2 {
    color: #333;
  }
  code {
    background-color: #f0f0f0;
    padding: 2px 6px;
    border-radius: 4px;
  }
  pre code {
    font-size: 22px;
  }
  .columns {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
  }
---

<!-- _paginate: false -->

# Using These Slides

These slides are written in **Marp** markdown. You can present them two ways:

**In VS Code:**
1. Install the **Marp for VS Code** extension
2. Open this `.md` file
3. Click the Marp preview icon (top right) or use `Marp: Open Preview`
4. To present: `Marp: Export Slide Deck` → HTML or PDF

**In the browser:**
1. Go to [marp.app](https://marp.app/)
2. Paste or upload this file
3. Click **Preview** to present

Feel free to edit these slides to fit your style!

<!-- This first slide is for mentors only — not shown during the session. -->

---

<!-- _paginate: false -->

# Lesson 6: Data Wrangling and Aggregation

**Python Essentials — Code the Dream**

---

## Game Plan for Today

| Section | Time | What We'll Do |
|---------|------|---------------|
| Warm-Up | 5 min | Predict groupby output |
| I Do | 10–15 min | Selection, groupby, pivot tables, merge, apply |
| We Do | 15–20 min | Wrangle football results together |
| You Do | 5–10 min | Filtering and aggregation challenge |
| Wrap-Up | 5 min | Assignment preview & Kaggle intro |

<!-- Mentor note: This is one of the most concept-dense lessons. The I Do moves quickly through six topics — focus on groupby and merge since those are the most assignment-critical. The We Do mirrors Assignment Task 11 (football results). Also: this is the first Kaggle notebook assignment! -->

---

## Warm-Up (5 min)

### What Does This Code Produce?

```python
import pandas as pd

data = {"Team": ["A", "B", "A", "B", "A"],
        "Points": [10, 20, 30, 40, 50]}
df = pd.DataFrame(data)

result = df.groupby("Team")["Points"].mean()
print(result)
```

**Predict the output, then share in the chat.**

<!-- Mentor note: Answer: Team A → 30.0, Team B → 30.0. This reviews groupby basics and sets up the lesson's focus on aggregation. If students struggle, walk through what "group by Team" means — split A's points [10,30,50] and B's points [20,40], then average each group. -->

---

## I Do: Data Selection Review

**Four ways to access data:**

| Method | Selects by | Example |
|--------|-----------|---------|
| `df["col"]` | Column name | `df["Age"]` |
| `df.loc[row, col]` | Labels (inclusive) | `df.loc[0:2, "Name"]` |
| `df.iloc[row, col]` | Position (exclusive) | `df.iloc[:2]` |
| `df[condition]` | Filter | `df[df["Age"] > 25]` |

**Important:** `.loc[0:2]` includes row 2. `.iloc[:2]` does NOT.

```python
# Combine conditions with & (not "and")
df[(df["Age"] > 25) & (df["Salary"] > 50000)]
```

<!-- Mentor note: The loc vs iloc inclusive/exclusive difference trips up many students. Assignment Task 1 practices basic selection. Stress the parentheses around each condition when combining with &. -->

---

## I Do: groupby() — Split, Apply, Combine

**`groupby()` is the most powerful aggregation tool in Pandas:**

```python
# Group by category, sum the values
df.groupby("Category")["Sales"].sum()

# Multiple aggregations at once
df.groupby("Category").agg({
    "Sales": ["sum", "mean", "count"]
})
```

**The pattern:**
1. **Split** the data into groups
2. **Apply** a function to each group
3. **Combine** the results

> Think of it like SQL's `GROUP BY` — if you know SQL, this is the same idea.

<!-- Mentor note: Assignment Task 2 uses groupby with agg. Task 11 uses groupby to find teams with the worst defense. Emphasize that the result of groupby is another DataFrame (or Series). -->

---

## I Do: Pivot Tables

**Pivot tables reshape data for cross-tab reporting:**

```python
pd.pivot_table(
    sales,
    index="Product",       # Row labels
    columns="Region",      # Column headers
    values="Revenue",      # What to aggregate
    aggfunc="sum",         # How to aggregate
    fill_value=0           # Replace NaN
)
```

**Result looks like a spreadsheet summary:**

```
Region      East   West
Product
Doohickey   4803  281024
Gizmo      37500   86006
Widget     13002   24014
```

> Pivot tables = `groupby()` with a nicer layout.

<!-- Mentor note: Assignment Task 3 has students build pivot tables from an ecommerce dataset. The key insight: pivot tables and groupby produce the same information, just formatted differently. -->

---

## I Do: Merging DataFrames

**`merge()` combines DataFrames on shared columns — like SQL JOINs:**

```python
df1 = pd.DataFrame({"ID": [1, 2, 3], "Name": ["A", "B", "C"]})
df2 = pd.DataFrame({"ID": [2, 3, 4], "Score": [85, 92, 88]})

# Inner: only matching rows
pd.merge(df1, df2, on="ID", how="inner")

# Left: keep all from df1
pd.merge(df1, df2, on="ID", how="left")

# Outer: keep everything
pd.merge(df1, df2, on="ID", how="outer")
```

| Join Type | Keeps |
|-----------|-------|
| `inner` | Only matching rows |
| `left` | All left + matching right |
| `right` | All right + matching left |
| `outer` | Everything from both |

<!-- Mentor note: Assignment Task 5 uses outer merge with suffixes. The NaN handling after merge is a key learning moment — students fill missing values and combine overlapping columns with np.where(). -->

---

## I Do: apply() for Complex Logic

**When simple column math isn't enough, use `apply()`:**

```python
def calculate_bonus(row):
    if row["Sales"] > 100000:
        return row["Sales"] * 0.10
    else:
        return row["Sales"] * 0.05

df["Bonus"] = df.apply(calculate_bonus, axis=1)
```

**Key details:**
- `axis=1` means "apply to each **row**"
- The function receives the entire row as a Series
- Access columns with `row["ColumnName"]`

> Use `apply()` when you need to combine multiple columns with if/else logic.

<!-- Mentor note: Assignment Task 4 uses apply() to flag jobs matching specific criteria (entry level, salary >= 70000, requires SQL and Python). This is a very practical use case. -->

---

## I Do: Useful Utilities

**Rename columns:**
```python
df.rename(columns={"Age": "Employee Age"}, inplace=True)
```

**Sort by values:**
```python
df.sort_values(by="Salary", ascending=False)
```

**Reset index after sorting:**
```python
df.reset_index(drop=True, inplace=True)
```

**Transform a column:**
```python
df["Salary"] = df["Salary"] * 1.10  # 10% raise
```

<!-- Mentor note: These are covered in Assignment Tasks 7-9. They're straightforward but students sometimes forget inplace=True or the drop=True in reset_index. Quick tips: avoid inplace=True when possible — assign to a new variable instead. The assignment actually warns against inplace=True in some tasks. -->

---

## We Do: Wrangling Football Results

**The assignment asks you to find the worst defensive teams. Let's build it.**

**Step 1:** Load the data and preview:

```python
import pandas as pd

football = pd.read_csv("results.csv")
print(football.head())
```

```
         date   home_team  away_team  home_score  away_score
0  1872-11-30    Scotland    England           0           0
1  1873-03-08     England   Scotland           4           2
...
```

**Problem:** Each game has a home team and away team. We need results **per team**.

<!-- Mentor note: This directly mirrors Assignment Task 11. Walk through the logic slowly — the "split and recombine" pattern is the key insight. -->

---

## We Do: Reshaping the Data

**Step 2:** Create two views — one for home, one for away:

```python
# Select columns we need
results_1 = football[["home_team", "away_team",
                       "home_score", "away_score"]]

# Home team perspective
results_home = results_1.rename(columns={
    "home_team": "team", "away_team": "opponent",
    "home_score": "points_for", "away_score": "points_against"
})

# Away team perspective
results_away = results_1.rename(columns={
    "away_team": "team", "home_team": "opponent",
    "away_score": "points_for", "home_score": "points_against"
})
```

**Why two DataFrames?** Each game needs to appear twice — once per team.

---

## We Do: Combine and Aggregate

**Step 3:** Stack them and find the worst defenses:

```python
# Combine both perspectives
all_results = pd.concat([results_home, results_away],
                        ignore_index=True)

# Average points allowed per game, per team
points_against = (all_results
    .groupby("team")["points_against"]
    .mean()
    .sort_values(ascending=False))

print(points_against.head(10))
```

**Discussion:** What does a high `points_against` mean?

> These are the teams that allow the most goals per game — the worst defenses.

<!-- Mentor note: This is the full solution for Task 11. The pattern — reshape → concat → groupby → sort — is a core data wrangling workflow. Task 12 asks students to apply similar logic independently to find Tunisia's last 10 games. -->

---

## You Do: Filtering and Aggregation Challenge (5 min)

**Given this DataFrame:**
```python
df = pd.DataFrame({
    "Department": ["Sales", "Eng", "Sales", "Eng", "HR"],
    "Employee": ["Alice", "Bob", "Carol", "Dave", "Eve"],
    "Salary": [60000, 90000, 65000, 85000, 55000]
})
```

**Write code to:**
1. Filter for employees with `Salary > 60000`
2. Group by `Department` and get the `mean` salary
3. Sort the result in descending order

**Bonus:** What's the total salary for the "Eng" department?

<!-- Mentor note: Set a 5-minute timer. Answers: 1) df[df["Salary"] > 60000] 2) df.groupby("Department")["Salary"].mean() 3) .sort_values(ascending=False). Bonus: df[df["Department"]=="Eng"]["Salary"].sum() → 175000. -->

---

## Assignment Preview

This week you work in a **Kaggle notebook** for the first time!

| Tasks | What You'll Do |
|-------|---------------|
| 1 | Data selection (columns, slicing, iloc) |
| 2 | Aggregation with groupby + agg |
| 3 | Pivot tables on ecommerce dataset |
| 4 | apply() to flag matching job listings |
| 5 | Merging DataFrames (outer join, fill NaN) |
| 6–10 | Filtering, sorting, renaming, transforms, concat |
| 11–12 | Football results wrangling (we just did this!) |
| 13 | Continue your capstone project |

<!-- Mentor note: Big shift: this is the first Kaggle assignment. Spend a moment explaining the Kaggle notebook workflow — cells, Run All, Add Input for datasets. Students may need help with the Kaggle interface before they can focus on the code. -->

---

## Kaggle Notebook Tips

**First time on Kaggle?** Here's the workflow:

1. Create a notebook → add Markdown + Code cells
2. **Add Input** (upper right) → search for datasets
3. Run the first cell to find file paths (`os.walk`)
4. Code cells share variables — run them in order
5. **Sessions time out** — use "Run All" to restore state

**Important:** If your notebook times out (you walk away), click **Run All** when you come back — your variables will be gone otherwise.

---

## Assignment Tips

**Task 3 (pivot tables):** The `Purchase_Amount` column is a string with a `$` sign. Strip it first:
```python
df["Purchase_Amount"] = df["Purchase_Amount"]
    .str.replace("$", "").astype(float)
```

**Task 5 (merge):** Use `suffixes=["_left", "_right"]` and `np.where()` to combine Age columns.

**Task 11 (football):** We just built this together — follow the same pattern!

---

## Wrap-Up

**Today we covered:**
- Data selection with `loc[]`, `iloc[]`, and boolean filters
- Aggregation with `groupby()` and `agg()`
- Pivot tables for cross-tab summaries
- Merging DataFrames with inner, outer, left, and right joins
- `apply()` for row-wise logic across multiple columns
- Utilities: rename, sort, reset index, transform

**Quick check:** When would you use `merge()` vs `concat()`?

<!-- Mentor note: Answer: concat stacks DataFrames vertically (same columns, more rows). merge combines DataFrames horizontally on shared keys (new columns). This distinction is important and comes up often. -->

---

## Getting Help This Week

- **Kaggle questions?** Check the Kaggle documentation or ask in #discussion
- The football wrangling (Tasks 11–12) uses everything from this lesson
- **Capstone progress:** This week add wrangling, aggregations, and 2+ engineered features
- Post questions in **#discussion** on Slack
- Book a **1:1 mentor session** for merge or pivot table help

**Next week:** Introduction to Data Visualization — making your data visual!

---

<!-- _paginate: false -->

# Data Wrangled! 🔧

**From raw data to insights — one groupby at a time.**

See you next session!
