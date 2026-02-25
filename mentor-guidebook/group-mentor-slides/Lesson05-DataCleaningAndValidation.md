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

# Lesson 5: Data Cleaning and Validation

**Python Essentials — Code the Dream**

---

## Game Plan for Today

| Section | Time | What We'll Do |
|---------|------|---------------|
| Warm-Up | 5 min | Spot the dirty data |
| I Do | 10–15 min | Missing data, types, duplicates, outliers, regex |
| We Do | 15–20 min | Clean a messy DataFrame together |
| You Do | 5–10 min | Validation challenge |
| Wrap-Up | 5 min | Assignment preview & capstone kickoff |

<!-- Mentor note: This is a content-heavy lesson — six distinct cleaning techniques. Move quickly through the I Do and spend the bulk of time on the We Do, where students see the techniques applied in sequence. The assignment mirrors this "pipeline" approach. -->

---

## Warm-Up (5 min)

### Spot the Problems

```python
data = {
    "Name": ["Alice", "  bob ", None, "CHARLIE"],
    "Age": ["25", "thirty", "35", "-5"],
    "Salary": [50000, "n/a", 70000, 80000],
    "Hire Date": ["2023-01-15", "01/20/2023", None, "not a date"]
}
```

**How many data quality issues can you find? List them in the chat.**

<!-- Mentor note: Issues: (1) None in Name, (2) inconsistent casing + whitespace in names, (3) Age is strings not ints, (4) "thirty" is not numeric, (5) "-5" is out of range, (6) "n/a" in Salary, (7) None in Hire Date, (8) "not a date" is invalid, (9) inconsistent date formats. This exercise previews every technique in the lesson. -->

---

## I Do: Why Clean Data?

> **Garbage in → Garbage out.**

Real-world data is messy. Before you can analyze it, you need to:

| Problem | Consequence |
|---------|------------|
| Missing values | Calculations fail or give wrong results |
| Wrong types | `"25" + 1` throws an error |
| Duplicates | Inflated counts and totals |
| Outliers | Skewed averages and charts |
| Inconsistent text | "NYC" ≠ "New York" in a groupby |

**Rule of thumb:** Always keep an untouched copy of raw data before cleaning.

---

## I Do: Handling Missing Data

**Three key methods:**

```python
# Find missing values
df[df.isnull().any(axis=1)]

# Drop rows with ANY missing value
df_clean = df.dropna()

# Fill missing values with specific strategies
df["Age"] = df["Age"].fillna(df["Age"].mean())
df["Name"] = df["Name"].fillna("Unknown")
```

**When to drop vs. fill?**
- **Drop** when few rows are affected and data is plentiful
- **Fill** when you can't afford to lose rows (use mean, median, or a constant)

<!-- Mentor note: The assignment (Task 1) has students practice both approaches. Emphasize that dropna() can be aggressive — if you have 10 columns and most rows have at least one null, you might lose most of your data. -->

---

## I Do: Fixing Data Types

Columns often load as the **wrong type** — especially from CSV:

```python
# Convert string numbers to actual numbers
df["Age"] = pd.to_numeric(df["Age"], errors="coerce")

# Convert strings to integers
df["Age"] = df["Age"].astype(int)

# Parse dates
df["Hire Date"] = pd.to_datetime(df["Hire Date"], errors="coerce")
```

**`errors="coerce"`** turns invalid values into `NaN` instead of crashing.

> Always check `df.dtypes` after loading — if a number column says `object`, it's probably strings.

<!-- Mentor note: This is critical for the assignment. Task 2 has students convert dates with errors="coerce" and see that invalid dates become NaT (Not a Time). Task 4.3 in Assignment 4 also used this pattern. -->

---

## I Do: Removing Duplicates

```python
# See which rows are duplicates
df.duplicated()

# Count duplicates
df.duplicated().sum()

# Remove duplicates (keeps first occurrence)
df_clean = df.drop_duplicates()

# Remove duplicates based on specific columns
df_clean = df.drop_duplicates(subset=["Name", "Age"])
```

**Why duplicates happen:**
- Data merged from multiple sources
- Form submitted twice
- Copy-paste errors

<!-- Mentor note: Assignment Task 4 has students use duplicated() to inspect duplicates before removing them with drop_duplicates(). Stress the inspect-first approach. -->

---

## I Do: Validating Ranges & Handling Outliers

**Replace out-of-range values with NaN:**

```python
import numpy as np

# Ages must be 18–65
df["Age"] = df["Age"].apply(
    lambda x: x if 18 <= x <= 65 else np.nan
)

# Fill the NaNs with the median
df["Age"] = df["Age"].fillna(df["Age"].median())
```

**For outliers (e.g., Age > 100 or < 0):**

```python
df["Age"] = df["Age"].apply(
    lambda x: df["Age"].median() if x > 100 or x < 0 else x
)
```

> Domain knowledge matters — an age of 95 might be valid; a salary of -$500 probably isn't.

---

## I Do: Text Standardization

Inconsistent text breaks grouping and matching:

```python
# Lowercase and strip whitespace
df["Name"] = df["Name"].str.lower().str.strip()

# Replace abbreviations
df["City"] = df["City"].replace({
    "NYC": "New York",
    "LA": "Los Angeles"
})
```

**`map()` vs `replace()`:**

| Method | Unmapped values become... |
|--------|--------------------------|
| `map()` | `NaN` (dangerous!) |
| `replace()` | Unchanged (safer) |

> Use `replace()` when you only want to fix specific values.

<!-- Mentor note: This map vs replace distinction trips up students. The assignment (Task 5) uses replace for city names. Stress: map is for complete transformations, replace is for targeted fixes. -->

---

## I Do: Regex Basics for Cleaning

**Regular expressions** help clean messy text patterns:

```python
# Remove non-digit characters from phone numbers
df["Phone"] = df["Phone"].str.replace(r"\D", "", regex=True)

# Extract domain from email
df["Domain"] = df["Email"].str.extract(r"@(\w[\w\.-]+)")

# Find rows containing "ship" (case-insensitive)
shipped = df[df["Status"].str.contains("ship", case=False)]
```

**Common patterns:**
| Pattern | Matches |
|---------|---------|
| `\d` | Any digit |
| `\D` | Any non-digit |
| `\w` | Word character (letter/digit/_) |
| `.*?` | Any characters (non-greedy) |

<!-- Mentor note: Assignment Task 8 covers regex in depth. For this session, just give students the mental model: regex = pattern matching for strings. They'll practice it hands-on in the assignment. -->

---

## We Do: Let's Clean a Messy DataFrame

**Here's our dirty data:**

```python
import pandas as pd
import numpy as np

data = {
    "Name": ["  Alice ", "BOB", None, "charlie", "Alice"],
    "Age": [25, "unknown", 35, -5, 25],
    "Salary": [50000, 60000, "n/a", 80000, 50000],
    "City": ["NYC", "LA", "Chicago", "NYC", "NYC"]
}
df = pd.DataFrame(data)
print(df)
```

**How many issues can we spot?**

<!-- Mentor note: Issues: whitespace in "  Alice ", inconsistent casing (BOB, charlie), None name, "unknown" age, -5 age, "n/a" salary, duplicate row (Alice), city abbreviations. Walk through each step with students. -->

---

## We Do: Step 1 — Fix Text

```python
# Standardize names: strip whitespace, lowercase
df["Name"] = df["Name"].str.strip().str.lower()

# Fill missing names
df["Name"] = df["Name"].fillna("unknown")

# Standardize city abbreviations
df["City"] = df["City"].replace({
    "NYC": "New York",
    "LA": "Los Angeles"
})
print(df)
```

**Check:** Are all names lowercase and trimmed now?

<!-- Mentor note: Point out the order matters — we strip/lowercase BEFORE filling, because fillna("unknown") is already lowercase. This mirrors Assignment Task 5. -->

---

## We Do: Step 2 — Fix Types and Missing Values

```python
# Convert Age to numeric (invalid → NaN)
df["Age"] = pd.to_numeric(df["Age"], errors="coerce")

# Convert Salary (replace placeholder first)
df["Salary"] = df["Salary"].replace("n/a", np.nan)
df["Salary"] = pd.to_numeric(df["Salary"], errors="coerce")

# Fill missing values
df["Age"] = df["Age"].fillna(df["Age"].median())
df["Salary"] = df["Salary"].fillna(df["Salary"].median())

print(df.dtypes)
print(df)
```

**Why replace "n/a" before `to_numeric()`?**

> `to_numeric` with `errors="coerce"` would also catch it, but explicit replacement is clearer and self-documenting.

---

## We Do: Step 3 — Validate and Deduplicate

```python
# Validate age range (must be 0–120)
df["Age"] = df["Age"].apply(
    lambda x: x if 0 <= x <= 120 else np.nan
)
df["Age"] = df["Age"].fillna(df["Age"].median())

# Remove duplicates
print(f"Before: {len(df)} rows")
df = df.drop_duplicates()
print(f"After: {len(df)} rows")

print(df)
```

**Discussion:** Why did we validate *after* converting types?

> You can't check `if 0 <= x <= 120` on a string — the column must be numeric first.

<!-- Mentor note: This ordering (text → types → validation → dedup) is a common cleaning pipeline pattern. The assignment follows a similar order. Emphasize that order matters! -->

---

## We Do: The Cleaning Pipeline

Here's the pattern we just followed:

```
Raw Data
  ↓  1. Standardize text (strip, lowercase)
  ↓  2. Fill missing text values
  ↓  3. Convert types (to_numeric, to_datetime)
  ↓  4. Fill missing numeric values (mean/median)
  ↓  5. Validate ranges
  ↓  6. Remove duplicates
Clean Data ✓
```

> This pipeline works for most datasets. The assignment follows this exact order.

<!-- Mentor note: Draw attention to this pipeline — it's the conceptual framework for the whole assignment. Students should think of cleaning as a sequence of steps, not random fixes. -->

---

## You Do: Validation Challenge (5 min)

**Given this DataFrame:**

```python
df = pd.DataFrame({
    "Product": ["Widget", "Gadget", "Widget", "Doohickey"],
    "Price": [10.99, -5.00, 10.99, 1500.00],
    "Rating": [4.5, 3.2, 4.5, 6.1]
})
```

**Write code to:**
1. Remove duplicate rows
2. Replace negative prices with `NaN`, then fill with the median
3. Replace ratings > 5.0 with `NaN`, then fill with the median

<!-- Mentor note: Set a 5-minute timer. Answer: 1) df = df.drop_duplicates() 2) df["Price"] = df["Price"].apply(lambda x: x if x >= 0 else np.nan); df["Price"] = df["Price"].fillna(df["Price"].median()) 3) Same pattern for Rating with > 5.0 threshold. -->

---

## Assignment Preview

This week's assignment is on **Kaggle** and covers 10 tasks:

| Tasks | Focus |
|-------|-------|
| 1 | Handling missing data (dropna, fillna) |
| 2 | Data type conversion (dates with errors) |
| 3 | Validating age ranges |
| 4 | Removing duplicates & outliers |
| 5 | Standardizing text (names, cities) |
| 6 | Encoding categorical variables |
| 7 | Mini-project: consolidating 4 messy CSV files |
| 8 | Regex for validation and extraction |
| 9 | Reflection on techniques used |
| 10 | **Start your capstone project!** |

<!-- Mentor note: Task 7 (messy files) is the most involved — it uses fuzzy matching (thefuzz library). Task 10 is the capstone kickoff where students pick a dataset. Make sure to mention both. -->

---

## Assignment Tips

**Tasks 1–5:** Follow the cleaning pipeline from our We Do — same order, same techniques.

**Task 6 (encoding):** Two approaches to learn:
- `map()` for label encoding (Red → 1, Blue → 2)
- `pd.get_dummies()` for one-hot encoding (separate column per category)

**Task 7 (messy files):** The `thefuzz` library handles approximate string matching. Don't worry about understanding every line — focus on the *pattern*.

**Task 10 (capstone):** Pick your dataset this week and start exploring it. Just get it loaded and run `info()`.

---

## Capstone Project Kickoff

Starting this week, you'll build a **data pipeline project** on Kaggle.

**Dataset options:**
- Global Superstore (retail sales)
- TMDB 5000 Movies (film data)
- Life Expectancy (WHO health data)
- Seattle Airbnb (listing data)

**This week's goal:** Create your Kaggle notebook, pick a dataset, load it, and start cleaning.

> You'll add to this project each week as you learn new techniques.

<!-- Mentor note: Students sometimes feel overwhelmed by the capstone. Reassure them: this week is JUST pick a dataset + load + basic cleaning. They'll build incrementally. Encourage them to choose a dataset they find personally interesting. -->

---

## Wrap-Up

**Today we covered:**
- Handling missing data (`dropna`, `fillna`)
- Fixing data types (`to_numeric`, `to_datetime`, `astype`)
- Validating ranges and handling outliers
- Removing duplicates (`drop_duplicates`)
- Standardizing text and intro to regex
- The cleaning pipeline pattern

**Quick check:** What's the first thing you should do after loading a new dataset?

<!-- Mentor note: Answer: df.info() — to check types, nulls, and shape. Reinforce this habit. -->

---

## Getting Help This Week

- Follow the **cleaning pipeline** order — it prevents cascading errors
- Use the **Python interactive shell** to test cleaning steps one at a time
- **Task 7** is a mini-project — budget extra time for it
- Post questions in **#discussion** on Slack
- Book a **1:1 mentor session** for help with regex or the capstone

**Next week:** Data Wrangling and Aggregation — groupby, pivot tables, and merging!

---

<!-- _paginate: false -->

# Clean Data, Clear Insights! 🧹

**Garbage in, garbage out — but clean data in, insights out.**

See you next session!
