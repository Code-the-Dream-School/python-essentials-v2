# Week 5 Mentor Summary: Data Cleaning and Validation

**Lesson file:** `lessons/05DataCleaningAndValidation.md`
**Assignment file:** `assignments/05DataCleaningAndValidation.md`

**Learning objective:** Students learn to clean and standardize real-world datasets. They handle missing values, duplicates, outliers, and inconsistent formats, validate value ranges and allowed values, and use regular expressions to prepare data for analysis.

---

## Lesson Overview

The lesson opens with a short introduction to data cleaning and the idea of "garbage in, garbage out." It recommends keeping an untouched raw copy of the data before cleaning. The lesson then has seven content sections and ends with a Check for Understanding quiz in section 5.8. There are three AI practice prompts. Students run the examples in the Python interactive shell.

| Section | What students learn |
|---|---|
| **5.1 Handling Missing Data** | Students find rows with missing values with `df.isnull().any(axis=1)`, remove them with `dropna()`, and replace them with `fillna()`. The example fills each column with a different value by passing a dictionary to `fillna()`. The lesson warns that `dropna()` can remove a lot of data. A second example explains that `isna()` is an alias for `isnull()` and `notna()` is its opposite. It counts missing values per column with `df.isna().sum()` and keeps rows with a value with `notna()`. |
| **5.2 Data Types & Datetime** | Students convert text numbers with `astype(int)` and parse dates with `pd.to_datetime()`. They check the result with `df.dtypes`. |
| **5.3 Validating Data Ranges and Allowed Values** | Students replace ages outside 18 to 65 with a missing value by using `apply()` with a `lambda`. They then fill the missing values with the median. They also find values outside an allowed list with `~df['Location'].isin(allowed)` and keep only the valid rows. |
| **5.4 Duplicates** | Students remove fully duplicated rows with `drop_duplicates()` and remove rows with a repeated value in one column with `drop_duplicates(subset='Name')`. The lesson mentions `duplicated()` in one sentence but does not show an example. |
| **5.5 Handling Outliers** | Students replace ages above 100 or below 0 with the median. The lesson uses fixed thresholds and does not teach a statistical method. |
| **5.6 Text Standardization & Regex** | Students normalize text with `str.lower()` and `str.strip()`. They learn the difference between `map()` and `replace()`: `map()` turns unmapped values into `NaN`, and `replace()` keeps them. A phone number example passes a named function to `map()`. A regex example shows `str.replace()`, `str.extract()`, `str.contains()`, and `df.filter(regex=...)`, plus a tip about non-greedy patterns like `.*?`. |
| **5.7 Handling Inconsistent Data** | Students combine lowercase conversion, stripping, and `replace()` to unify city names. Fuzzy matching is named as a technique but is not demonstrated. |

---

## Assignment Deliverables

**Setup:** This assignment is different from Weeks 1 to 4. Students work in a Jupyter notebook on [Kaggle](https://www.kaggle.com) instead of a `.py` file in `python_homework`. They register for a Kaggle account and create a notebook named `CTD_Assignment_5`. Each task should start with a Markdown cell, followed by a code cell. Students load datasets through Kaggle's input feature by searching for the dataset names given in the assignment.

**Testing:** There is no PyTest file. The reviewer reads the notebook and may run it with "Run All." Row counts depend on the dataset version, so the rubric treats them as approximate.

**Tasks:** Tasks 1 to 10 are required. The full capstone rubric and the option to choose an alternative dataset are optional this week.

| Task | Topic | What students do | Concepts practiced |
|---|---|---|---|
| 1 | Handling missing data | Load the "code the dream lesson 5" dataset into `df`. Create `df1` with `dropna()` and compare `info()` for both. Fill each column of `df` with a specific value or statistic. Then drop the remaining rows with missing `City` values into `df2`, reset the index, and convert `Age` to integers. | `dropna()`, `fillna()`, mean, median, `reset_index()`, `astype(int)` |
| 2 | Data transformation | Load the eclipse dataset into `df3` with a pipe character as the separator. Try to convert `Date` with `pd.to_datetime()`, see the error, and then use `errors='coerce'`. | `sep`, `pd.to_datetime()`, `NaT` |
| 3 | Validating ranges | Replace ages outside 18 to 65 with `np.nan`, then fill them with the median. | Range validation, `np.nan` |
| 4 | Duplicates and outliers | Use `duplicated()` and `value_counts()` on `df3` to find duplicate rows, then remove them. On `df2`, replace ages above 100 or below 0 with the median. | `duplicated()`, `drop_duplicates()`, outlier rules |
| 5 | Standardizing data | Lowercase and strip `Name`. Use a `groupby()` diagnostic to find variants in `City`, then replace variants such as `NYC` and `LA`. | `.str` methods, `replace()` |
| 6 | Encoding categories | Create `df_color` and run the given code for label encoding and one-hot encoding. | `map()`, `pd.get_dummies()` |
| 7 | Consolidating messy files | Load four CSV files and combine them into `df_all`. Use the given `thefuzz` code to fix misspelled values in `Name` and `Address`, use the given `fix_anomaly` function to fix `Zip` and `Phone`, and remove duplicates. The result should be about 400 records. | `concat()`, fuzzy matching, `groupby().transform()`, `mode()` |
| 8 | Regular expressions | Run four given regex examples and show their output. Students do not need to change them. | `str.extract()`, `replace(regex=True)`, `filter(regex=...)`, `str.contains()` |
| 9 | Reflection | Write a Markdown cell about the most common data issues, the techniques that worked best, and how to automate cleaning. | Written reflection |
| 10 | Start the final project | Create a separate Kaggle notebook, choose one of the four capstone datasets, load it, and document some first cleaning steps in Markdown cells. | Applying the week's techniques to new data |

**Submission:** Students save a version of each notebook, share it as Public with comments allowed, and paste the links into the submission form. The `CTD_Assignment_5` link goes in the first field and the capstone notebook link goes in the second field.

**Note:** The assignment uses many tools that the lesson does not teach. These include Jupyter notebooks, `np.nan`, `value_counts()`, `groupby()`, `transform()`, `mode()`, `pd.get_dummies()`, and the `thefuzz` library. The lesson names fuzzy matching but does not show it. The assignment itself explains how to add a Kaggle input and copy its file path. `reset_index()` and the `sep` parameter come from Week 4. Most of the unfamiliar tools appear in code that the assignment provides, but students may still need help understanding what that code does.

---

## FAQs and Common Trouble Spots

**1. "I do not know how to load the data," or "my file is not found."**
The assignment explains how to add a Kaggle input just before Task 1, step 2. Students open the Input section of the right sidebar, choose "Add Input," and search for the dataset. The files appear under `/kaggle/input/<dataset-folder>/`, and students can copy a file path from the sidebar. Kaggle's labels change over time, so some students may still need help finding these controls. The default first code cell in a new Kaggle notebook lists the files in the input folder. Paths differ between students, and the rubric says reviewers should not fail a student for the path string.

**2. "My cell worked earlier, but now I get a `NameError`."**
Kaggle sessions time out when they are idle, and all variables are lost. Later cells depend on variables from earlier cells, such as `df`, `df2`, and `df3`. The fix is to click **Run All** so that the cells run in order. Students should also check that they did not run cells out of order while they were editing.

**3. "Which DataFrame am I supposed to use?"**
The assignment switches between DataFrames. Task 1 creates `df`, `df1`, and `df2`. Task 2 and the first part of Task 4 use the eclipse data in `df3`. Task 3 uses `df2` from Task 1. The second part of Task 4 and Task 5 use `df2`. Also remind students that methods such as `drop_duplicates()` and `fillna()` return a new DataFrame. If the student does not assign the result back to a variable, the next `info()` call shows the old data.

**4. "`astype(int)` fails," or "`print(df.info())` prints `None`."**
`astype(int)` raises an error if the column still contains missing values. In Task 1, `Age` must be filled before students convert it. `info()` prints its summary directly and returns `None`, so wrapping it in `print()` shows an extra `None` line. In Task 3, students must use `np.nan` instead of the string `'NaN'`. The string is ordinary text, so `fillna()` and `median()` do not treat it as a missing value.

**5. "My Task 7 results do not match the expected counts," or "`thefuzz` will not import."**
The assignment's code installs `thefuzz` with `!pip install` if the import fails. This command works in a notebook cell but not in a `.py` file. Students must fix both `Name` and `Address` before they fix `Zip` and `Phone`, because `fix_anomaly` groups rows by those two columns. The expected counts of about 1600 rows and about 400 records are approximate. The rubric says reviewers should not fail students for small differences. In Task 6, `pd.get_dummies()` returns `True` and `False` values in current versions of Pandas, and this output is correct.
