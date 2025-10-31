
## Lesson 05 — Data Cleaning and Validation

**Lesson Overview**

**Learning objective:** Students will learn to clean and standardize real-world datasets by handling missing values, duplicates, outliers, and inconsistent formats. They will validate ranges and categories, clean inconsistent formats, and use regular expressions to prepare data for analysis.



### Topics
1. What is Data Cleaning?
2. Handling Missing Data (`isnull`, `dropna`, `fillna`)
3. Data Types & Datetime (`astype`, `to_numeric`, `to_datetime`)
4. Validation (ranges / allowed values)
5. Duplicates (`duplicated`, `drop_duplicates`)
6. Outliers (simple rules; median/IQR concept)
7. Text Standardization & Regex (`lower/strip`, `replace` vs `map`, `str.contains`/`str.extract`/`str.replace`, `filter(regex=...)`)
8. Handling Inconsistent Data (normalize text; brief fuzzy-matching note)
---

## What is Data Cleaning?

Garbage in → garbage out. Cleaning prepares messy, real-world data so analysis is trustworthy.

- **Standardize:** consistent formats (dates, phones, casing).
- **Handle missing values:** drop vs fill (mean/median/constant/ffill/bfill).
- **Remove duplicates:** ensure unique records.
- **Check outliers:** domain-aware; don’t delete real extremes blindly.
- **Validate:** ranges, allowed sets, uniqueness (IDs), and sanity checks.

Optional: [3-min intro video](https://www.youtube.com/watch?v=WpX2F2BS3Qc)  
> Tip: always **keep an untouched raw copy** before cleaning.

## 5.1 Handling Missing Data

### Overview

Missing data is common in datasets and can significantly affect the outcome of analyses if not handled properly. In Pandas, there are methods to identify and handle missing data, either by removing rows or by filling in the missing values. Correctly managing missing data is crucial for ensuring accurate results.

### Key Methods

- `isnull()`: Find the rows where data is missing.
- `dropna()`: Removes rows or columns with missing data.
- `fillna()`: Replaces missing values with specified values.

### Why Handle Missing Data?

- Ensures accurate calculations and visualizations.
- Prevents runtime errors during analysis.
- Allows consistent dataset formatting.

### Example: Using `isnull()`, `dropna()` and `fillna()`

**For this and all examples below, you should run the code within the Python interactive shell of your python_homework VSCode terminal.**

```python
import pandas as pd

# Sample DataFrame with missing values
data = {'Name': ['Alice', 'Bob', None, 'David'],
        'Age': [24, 27, 22, None],
        'Score': [85, None, 88, 76]}
df = pd.DataFrame(data)

# Find rows with missing data
df_missing = df[df.isnull().any(axis=1)]
print(df_missing)
# Remove rows with missing data
df_dropped = df.dropna()
print(df_dropped)

# Replace missing data with default values
df_filled = df.fillna({'Age': 0, 'Score': df['Score'].mean()})
print(df_filled)
```

**Explanation:**
`df.isnull().any(axis=1)` finds the rows that have null or NaN values.  The `axis=1` is needed to specify rows.

`dropna()` removes any row that contains a `None` (missing) value. This can remove quite a lot of data, especially if you have a lot of columns.

`fillna()` is used to replace missing values. In this case, the `Age` column's missing values are replaced with 0, and the `Score` column's missing values are filled with the mean of the existing scores. This can cause issues if the values you are replacing become outliers.

## 5.2 Data Types & Datetime

### Overview
Get columns into the right types so everything else works (math, comparisons, joins, time ops).

### Key Tasks
- Convert text numbers to numeric with `astype` or `pd.to_numeric`.
- Parse dates with `pd.to_datetime` (use `errors='coerce'` to flag bad values).

### Why this matters

- Prevents errors due to mismatched data types.
- Simplifies operations such as comparisons and calculations.
- Ensures uniformity in dataset representation.
- Improves model performance in machine learning tasks.

### Example: Converting Data Types and Reformatting Dates

```python
import pandas as pd

# Sample DataFrame with mixed data types
data = {'Name': ['Alice', 'Bob', 'Charlie'],
        'Age': ['24', '27', '22'],
        'JoinDate': ['2023-01-15', '2022-12-20', '2023-03-01']}
df = pd.DataFrame(data)

# Convert 'Age' column to integers
df['Age'] = df['Age'].astype(int)

# Convert 'JoinDate' column to datetime
df['JoinDate'] = pd.to_datetime(df['JoinDate'])

print(df.dtypes)  # Verify data types
print(df)
```
---
## **5.3 Validating Data Ranges**

### **Overview**
Validating data ranges ensures all values fall within acceptable limits, avoiding invalid or erroneous data.

### **Example Task:**
- Ensure ages are between 18 and 65. Replace invalid values with `NaN` and fill them with the median.

### **Code Example:**
```python
# Replace invalid ages with NaN
df['Age'] = df['Age'].apply(lambda x: x if 18 <= x <= 65 else None)

# Fill missing values with median
df['Age'] = df['Age'].fillna(df['Age'].median())

print("DataFrame after validating age ranges:")
print(df)
```

### **Explanation:**
- Any age outside the range of 18 to 65 is replaced with `None` (NaN).
- Missing values in the `Age` column are filled with the median value of the column.

---


## 5.4 Duplicates

### Overview

Duplicate records can inflate metrics and skew results. Removing duplicates ensures that each record is unique, which improves the reliability of analyses. Pandas provides the `drop_duplicates()` method to identify and remove duplicate rows.

### Key Method

- `drop_duplicates()`: Removes duplicate rows based on one or more columns.

### Why Remove Duplicates?

- Prevents redundant information in analysis.
- Improves data quality and storage efficiency.
- Ensures unique data for accurate insights.

### Example: Using `drop_duplicates()`

```python
import pandas as pd

# Sample DataFrame with duplicates
data = {'Name': ['Alice', 'Bob', 'Alice', 'David'],
        'Age': [24, 27, 24, 32],
        'Score': [85, 92, 85, 76]}
df = pd.DataFrame(data)

# Identify and remove duplicates
df_cleaned = df.drop_duplicates()
print(df_cleaned)

# Remove duplicates based on 'Name' column
df_cleaned_by_name = df.drop_duplicates(subset='Name')
print(df_cleaned_by_name)
```
Use `duplicated()` to see which rows are duplicates.

**Explanation:**

`drop_duplicates()` removes rows where the entire record is a duplicate of another.
`drop_duplicates(subset='Name')` removes rows where the `Name` column is duplicated, keeping only the first occurrence of each name.

---

## **5.5 Handling Outliers**

### **Overview**
Outliers are extreme values that deviate significantly from other observations and can bias statistical calculations.

### **Common Approach:**
- Replace outliers with statistical measures like the median.

### **Code Example:**

(You don't need to run this one, as the DataFrame is not provided.)

```python
# Replace outliers in 'Age' (e.g., Age > 100 or Age < 0)
df['Age'] = df['Age'].apply(lambda x: df['Age'].median() if x > 100 or x < 0 else x)

print("DataFrame after handling outliers:")
print(df)
```

### **Explanation:**
- Outliers in the `Age` column that are greater than 100 or less than 0 are replaced by the median value of the `Age` column.

---
## 5.6 Text Standardization & Regex

### Overview
Real data has typos, inconsistent casing, and extra symbols. Standardize first, then analyze.

### Basic normalization
- Lowercase & trim: `df["Name"] = df["Name"].str.lower().str.strip()`
- Prefer `replace` over `map` when you don’t want unmapped values to become `NaN`.

### Example: map vs replace (Location)

You can use the Series map() method to change items in a column.

```python
import pandas as pd

# Sample DataFrame

data = {'Name': ['Alice', 'Bob', 'Charlie'],
        'Location': ['LA', 'LA', 'NY'],
        'JoinDate': ['2023-01-15', '2022-12-20', '2023-03-01']}
df = pd.DataFrame(data)

# Convert 'Location' abbreviations into full names

df['Location'] = df['Location'].map({'LA': 'Los Angeles', 'NY': "New York"})
print(df)
```

The problem with the code above is that if the value in the 'Location' column is not either 'LA' or 'NY', it is converted to `NaN`.  Suppose you want to preserve the existing value in this case. You'd use the replace() method instead:

```python
df['Location'] = df['Location'].replace({'LA': 'Los Angeles', 'NY': "New York"})
```
### Example: normalize phone numbers
Here is another case.  Suppose we have a list of people's phone numbers, but they are not in standard format.  We can attempt to clean this up in this way:

```python
import pandas as pd
data = {'Name': ['Tom', 'Dick', 'Harry', 'Mary'], 'Phone': [3212347890, '(212)555-8888', '752-9103','8659134568']}
df = pd.DataFrame(data)
df['Correct Phone'] = df['Phone'].astype(str)

def fix_phone(phone):
    if phone.isnumeric():
        out_string = phone
    else:
        out_string = ''
        for c in phone:
            if c in '0123456789':
                out_string += c
    if len(out_string) == 10:
        return out_string
    return None
    
df['Correct Phone'] = df['Correct Phone'].map(fix_phone)
print(df)
```
In the code above, the 'Phone' column is preserved, in case there is useful information, but the 'Correct Phone' column is created, with a best effort at reformatting the phone numbers.  Note that the logic did not fit in a lambda, so a function was declared to pass to map().


```python

import pandas as pd

data = {'Name': ['Alice', 'Bob', 'Charlie'],
	'Age': [20, 22, 43]}

df = pd.DataFrame(data)

# Increase the age by 1 as a new year has passed
df['Age'] = df['Age'] + 1
print(df)
```


### Practical regex with Pandas
```python
import pandas as pd

# Remove non-digits (e.g., clean phone strings)
s = pd.Series(["(123) 456-7890", "+1-555-123-4567", "555.456.7890"])
clean = s.str.replace(r"\D", "", regex=True)

# Remove HTML tags
html = pd.Series(["<p>Hello</p>", "<div>Hi <b>there</b></div>"])
text_only = html.str.replace(r"<.*?>", "", regex=True)

# Extract with capture groups (domain from email)
emails = pd.Series(["john.doe@example.com","user@my-domain.org"])
domains = emails.str.extract(r"@(\w[\w\.-]+)")

# Contains (case-insensitive)
orders = pd.Series(["Order #10 shipped", "Canceled order", "Shipment #22"])
shipped = orders[orders.str.contains(r"ship", case=False, regex=True)]

# Select columns by name pattern
df = pd.DataFrame({"created_at":[1], "updated_at":[2], "note":[3]})
time_cols = df.filter(regex=r"_at$")
```

Tip: Greedy patterns like `.*` can over-match. Use `.*?` (non-greedy) when needed.

## 5.7 Handling Inconsistent Data

### **Overview**
Inconsistent data can result from typos, different formats, or various naming conventions. Handling inconsistencies ensures uniformity in the dataset.

### **Key Techniques:**
- **Fuzzy Matching**: Identifying and standardizing similar but non-exact values.
- **Regex (Regular Expressions)**: Using patterns to extract or replace inconsistent data.

### **Why Handle Inconsistent Data?**
- Improves the quality of data for analysis.
- Helps identify patterns across otherwise unmatchable data points.

### **Code Example:**
```python

# Sample DataFrame with inconsistent data
data = {'City': ['New York', 'new york', 'San Francisco', 'San fran']}
df = pd.DataFrame(data)

# Standardize text data (convert to lowercase and strip spaces)
df['City'] = df['City'].str.lower().str.strip()

# Use Regex to replace shorthand names
df['City'] = df['City'].replace({'san fran': 'san francisco'})

print("DataFrame with Inconsistent Data Handled:")
print(df)
```

### **Explanation:**
- **String standardization**: Converts all entries in the `City` column to lowercase and removes extra spaces.
- **Regex**: Matches and replaces shorthand for cities with their full names.

---

## 5.8 Quick Knowledge Check

Which method removes rows with missing values?

A) `fillna()`
B) `dropna()`
C) `remove_missing()`
D) `replace_na()`

<details>
<summary>Answer</summary>
Answer: B
</details>

How do you convert a column of strings to integers?

A) `pd.to_int(column)`
B) `df['column'].convert(int)`
C) `df['column'].astype(int)`
D) `df['column'].to_int()`

<details>

<summary>Answer</summary>

**Answer**: C

</details>


Which method is used to remove duplicate rows in Pandas?

A) `remove_duplicates()`
B) `drop_duplicates()`
C) `find_duplicates()`
D) `remove_redundant()`

<details>

<summary>Answer</summary>

**Answer**: B

</details>

**Summary**

In this lesson, you learned how to:

* Handle missing data with `isna/notna`, `dropna`, and `fillna`.
* Convert data types and parse datetimes.
* Validate ranges / allowed values (e.g., age 18–65).
* Identify and remove duplicates with `drop_duplicates`.
* Flag/handle outliers with simple rules.
* Standardize text and apply regex (`replace`, `contains`, `extract`, `filter(regex)`).
* Normalize inconsistent values (and when to consider fuzzy matching).

By applying these techniques, you can clean and validate your datasets for accurate and effective analysis. For further exploration, refer to the Pandas Documentation and Python's official documentation.
