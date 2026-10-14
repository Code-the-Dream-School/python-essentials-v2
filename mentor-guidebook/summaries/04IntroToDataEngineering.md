# Week 4 Mentor Summary: Intro to Data Engineering

**Lesson file:** `lessons/04IntroToDataEngineering.md`
**Assignment file:** `assignments/04IntroToDataEngineering.md`

**Learning objective:** Students learn to load, preview, and inspect datasets with Pandas. They read data from CSV files, JSON files, and Python dictionaries, and they summarize the structure of a DataFrame.

---

## Lesson Overview

The lesson has three numbered sections. Section 4.1 is long and covers most of the Pandas basics. Each section ends with a short "Check for Understanding" quiz, and sections 4.1 and 4.2 each have an AI practice prompt. Students run all examples in the Python interactive shell from their `python_homework` folder.

| Section | What students learn |
|---|---|
| **4.1 Intro to Pandas: setup and data structures** | Pandas is already installed through `requirements.txt` in `python_homework`, so students only need `import pandas as pd`. The lesson briefly introduces NumPy as the fast numerical library that Pandas is built on. Students learn the two main data structures. A **Series** is one-dimensional and has an index of labels. A **DataFrame** is a two-dimensional table. |
| **4.1 Series and index labels** | Students create a Series with custom index labels. The lesson stresses that index labels can be non-numeric, out of order, and non-unique, and that they are different from row positions. Students access values by label with `[]` and by position with `.iloc[]`, and they multiply a whole Series by a number. |
| **4.1 Creating and combining DataFrames** | Students create a DataFrame from a dictionary of lists, from a list of dictionaries, and from a NumPy array. They combine two DataFrames with `pd.concat(..., ignore_index=True)`. |
| **4.1 Selecting and changing data** | Students use `df.loc[row_label, column]` and `df.iloc[row_position, column_position]`. In both cases the row comes first. They select one or more columns, select rows by position, and filter rows with a condition such as `df[df['Age'] > 23]`. The lesson warns against chained assignment like `df['Age'][1] = 35` and recommends `df.loc[1, 'Age'] = 35`. |
| **4.1 Aggregation and a first look at cleaning** | Students use `mean()` and `nunique()`. The lesson then previews cleaning methods that Week 5 covers in more depth: `pd.to_numeric(errors="coerce")`, `fillna()` with a fixed value or the mean, forward and backward fill, `str.strip()`, `str.upper()`, `str.lower()`, and `pd.to_datetime()`, including `format='mixed'` for mixed date formats. |
| **4.1 Saving DataFrames** | Students save a DataFrame with `to_csv()` and learn the `sep`, `index`, and `header` parameters. The example uses `index=False`. |
| **4.2 Loading Data in Pandas** | Students read files with `pd.read_csv()` and `pd.read_json()`. The `read_csv()` parameters covered are `delimiter` or `sep`, `header`, and `index_col`. The lesson shows the two JSON layouts that Pandas reads directly: a list of row dictionaries and a dictionary of column lists. It also shows how to write a tab-separated file with `sep="\t"`. |
| **4.3 Data Inspection** | Students preview data with `head()` and `tail()`, which show 5 rows by default. They use `info()` to see the row count, column data types, non-null counts, and memory usage. |

The lesson includes two videos on installing and using Pandas. One video uses Jupyter Notebook and the other uses VS Code.

---

## Assignment Deliverables

**Setup:** Students create an `assignment4` branch in `python_homework` and write their code in `assignment4/assignment4.py`. The assignment asks students to use the exact variable names, file names, and column names shown in backticks.

**Testing:** Students run `pytest -v -x assignment4-test.py` from the `assignment4` folder. They should run the tests after each step. According to the hidden rubric, the tests compare the DataFrames against expected values with `.equals()`, so the data values must match exactly.

**Coding tasks:** All four tasks are required. There is no optional work.

| Task | Required result | What it does | Concepts practiced |
|---|---|---|---|
| 1.1 | `task1_data_frame` | Builds a DataFrame from a dictionary of three names, ages, and cities given in the assignment. | `pd.DataFrame()` from a dictionary |
| 1.2 | `task1_with_salary` | Copies the Task 1.1 DataFrame and adds a `Salary` column. | `copy()`, adding a column |
| 1.3 | `task1_older` | Copies `task1_with_salary` and adds 1 to every age. | Operations on a whole column |
| 1.4 | `employees.csv` | Saves `task1_older` to a CSV file without the index. | `to_csv(index=False)` |
| 2.1 | `task2_employees` | Reads `employees.csv` back into a DataFrame. | `pd.read_csv()` |
| 2.2 | `additional_employees.json`, `json_employees` | Students write a JSON file by hand with two more employees, then load it. | JSON structure, `pd.read_json()` |
| 2.3 | `more_employees` | Combines the CSV and JSON DataFrames into five rows with an index of 0 to 4. | `pd.concat()` with `ignore_index=True` |
| 3.1 to 3.4 | `first_three`, `last_two`, `employee_shape` | Stores the first three rows, the last two rows, and the shape of `more_employees`, then prints an `info()` summary. | `head()`, `tail()`, `shape`, `info()` |
| 4.1 | `dirty_data`, `clean_data` | Reads `dirty_data.csv` into `dirty_data` and makes a copy named `clean_data` for all cleaning. | `read_csv()`, `copy()` |
| 4.2 to 4.7 | `clean_data` | Removes duplicate rows, converts `Age` and `Salary` to numbers, replaces the `unknown` and `n/a` placeholders, fills missing `Age` with the mean and missing `Salary` with the median, converts `Hire Date` to datetime, and strips and uppercases `Name` and `Department`. | `pd.to_numeric()`, `replace()`, `fillna()`, `pd.to_datetime(format='mixed')`, `.str` methods |

**Submission:** Students commit and push the `assignment4` branch, open a pull request into `main` in their own `python_homework` repository, and submit the pull request link.

**Note:** The assignment uses a few tools that the lesson does not teach. The `shape` attribute is not covered in the lesson. Removing duplicate rows (Task 4.2) is not covered until Week 5, so students need to find `drop_duplicates()` on their own. The lesson shows JSON examples but does not explain how to create a JSON file by hand.

---

## FAQs and Common Trouble Spots

**1. "My DataFrame looks right, but the test says it is not equal."**
The rubric says the tests compare DataFrames with `.equals()`. This check is strict about values, column names, column order, and data types. A typo in a name, a column in the wrong order, or an integer column that became a float column can all cause a failure. Ask students to print the DataFrame and its `dtypes`, then compare them carefully with the assignment text. Also remind them to use the exact names, ages, and cities from the assignment instead of the names from the lesson examples.

**2. "Changing `task1_older` also changed `task1_with_salary`."**
This happens when a student writes `task1_older = task1_with_salary` instead of calling `.copy()`. Plain assignment creates a second name for the same DataFrame. The same mistake can happen with `clean_data`, which would change `dirty_data`. The `dirty_data` DataFrame must stay raw.

**3. "My CSV file has an extra column," or "the shape is wrong when I read it back."**
Students who forget `index=False` in `to_csv()` save the index as an extra unnamed column. When they read the file back, the DataFrame has 5 columns instead of 4, and its shape is `(3, 5)` instead of `(3, 4)`. Ask students to open `employees.csv` and look at the first line. The assignment asks them to do this in Task 1.4.

**4. "My combined DataFrame has the wrong index," or "`last_two` has the wrong index."**
Without `ignore_index=True`, `pd.concat()` keeps the original index labels, so the combined index is 0, 1, 2, 0, 1. Task 2.3 asks for an index of 0 to 4. In Task 3, `head()` and `tail()` keep the original index labels, so `last_two` should have labels 3 and 4. Students should not reset the index for `first_three` or `last_two`.

**5. "My cleaning steps do not work on `Salary` or `Hire Date`."**
The order of steps matters. The mean and median of a column can only be calculated after the column is numeric, so the conversion steps must come before the fill step. If a student calls `pd.to_numeric()` without `errors="coerce"`, Pandas raises a `ValueError` when it reaches a placeholder such as `unknown`. If `Hire Date` contains several date formats, `pd.to_datetime()` without `format='mixed'` can raise an error, or it can turn many valid dates into `NaT` when `errors="coerce"` is set. The lesson shows this difference in its datetime example. Students may also see warnings about chained assignment if they change a column through `df['col'][i]`. The lesson recommends `df.loc[...]` or assigning a whole column instead.
