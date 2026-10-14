# Week 6 Mentor Summary: Data Wrangling and Aggregation

**Lesson file:** `lessons/06DataWranglingAndAggregation.md`
**Assignment file:** `assignments/06DataWranglingAndAggregation.md`

**Learning objective:** Students learn to select, summarize, reshape, and combine data in Pandas. They practice filtering, `groupby()` aggregation, pivot tables, merging and joining, column transformations, and `apply()`. They also see categorical encoding and binning with `pd.cut()`.

---

## Lesson Overview

The lesson has ten content sections and a short "Check for Understanding" quiz at the end. Students run the examples in the Python interactive shell from their `python_homework` folder. Several sections have an optional AI practice prompt.

| Section | What students learn |
|---|---|
| **6.1 Pandas Review (Optional)** | A podcast and two linked resources that review Series, DataFrames, and the basic Pandas methods from earlier weeks. |
| **6.2 Data Selection** | Selecting columns, `.loc[]` (by label), and `.iloc[]` (by position). A `.loc[]` slice includes the end label, and an `.iloc[]` slice does not. Students filter rows with boolean conditions and combine conditions with `&` instead of `and`. They also use `.str` methods such as `str.contains()` and `str.upper()`. `.at[]` and `.iat[]` are listed but not demonstrated. |
| **6.3 Data Aggregation** | `groupby()` with `sum()` and `mean()`, and `agg()` with a dictionary of columns and functions. A list of functions produces multi-level column names such as `("Values", "sum")`. A single function name produces a plain column name. |
| **6.4 Pivot Tables** | `pd.pivot_table()` with `index`, `columns`, `values`, `aggfunc`, and `fill_value`, using a sample sales dataset. Students see a two-level index and multi-level columns. |
| **6.5 Merging and Joining** | `pd.merge()` on one key or several keys, and the four `how` options (inner, left, right, outer). Overlapping columns get `_x` and `_y` suffixes by default. `join()` combines DataFrames on their index. |
| **6.6 Data Transformation** | Adding, replacing, and dropping columns. Students transform a column with an operator, a NumPy function such as `numpy.sqrt()`, `map()` with a lambda, and `astype()`. |
| **6.7 Using apply()** | Calculations that combine several columns. Students pass a function to `apply()` with `axis=1`, and the function runs once per row. The example calculates sales commissions under two plans. |
| **6.8 Utility Methods** | `rename()`, `set_index()`, `sort_values()`, and `reset_index(drop=True)`. |
| **6.9 Categorical Encoding** | Label encoding with `map()` and a dictionary, and one-hot encoding with `pd.get_dummies()`. |
| **6.10 Feature Engineering (Binning)** | Using `pd.cut()` to split a numeric column into labeled bins. |

Several examples depend on variables from earlier examples, such as `sales`, `sales_pivot2`, and `joined_df`. Students should run the whole lesson in one shell session.

---

## Assignment Deliverables

**Setup:** This assignment is done entirely in Kaggle, not in the `python_homework` repository. Students create a Kaggle notebook called `CTD_Assignment_6`. Each task starts with a markdown cell, followed by a code cell that has been run so the output is visible.

**Testing:** There is no PyTest file. Reviewers read the notebook cells and check that the shown output matches each task. Values from live Kaggle datasets can vary, so reviewers check structure, not exact numbers.

**Coding tasks:** All 13 tasks are required. Tasks 3 and 4 use separate Kaggle datasets. Students must use the DataFrame and column names given in the tasks, because later tasks depend on them.

| Task | What it does | Concepts practiced |
|---|---|---|
| 1 | Creates `df1`, `df2`, and `df3` from sample data (the values may change). Selects one column, two columns, and the first three rows of `df1`. | DataFrame creation, column selection, `iloc` |
| 2 | Groups `df1` by `Age` and shows the mean, sum, and count of `Salary`. | `groupby()`, `agg()` |
| 3 | Loads the "Ecommerce Consumer Behavior" dataset. Converts `Purchase_Amount` from a dollar string to a float, then builds a `buying_patterns` pivot table (sum) and a `demographics` pivot table (count). | Reading a CSV, string cleanup, type conversion, `pivot_table()` |
| 4 | Loads the "AI-Powered Job Recommendations" dataset into `jobs`. Adds a `Check These Out` column with `"Yes"` or `"No"` based on three conditions, then selects the `"Yes"` rows into `my_jobs`. | `apply()` with `axis=1`, boolean filtering |
| 5 | Outer-merges `df1` and `df3` on `Name` with custom suffixes. Fills missing values, builds one `Age` column with a given `np.where()` line, and drops the extra age columns. Then sets `Name` as the index on copies and joins them with an outer join. | `merge()`, `fillna()`, `np.where()`, `set_index()`, `join()` with suffixes |
| 6 | Shows rows of `df1` where `Age` is greater than 30. | Boolean filtering |
| 7 | Sorts `df1` by `Salary` in descending order. | `sort_values()` |
| 8 | Renames two columns of `df1` without `inplace=True`. | `rename()` |
| 9 | Increases every salary in `df1` by 10%. | Column transformation |
| 10 | Adds the rows of `df2` to the end of `df1` with a fresh index. | `pd.concat()` with `ignore_index=True` |
| 11 | Loads international football results. Builds a home-team view and an away-team view with renamed columns, combines them into one `football_results` DataFrame, and finds the teams that concede the most points on average. | Column selection, `rename()`, `pd.concat()`, `groupby()`, sorting a Series |
| 12 | Prints the 10 most recent games for Tunisia from the reshaped `football_results`. Students work out the steps themselves, and sorting is required. | Filtering, sorting by date, combining steps |
| 13 | Continues the final project in the student's capstone notebook. Students filter and select data, do at least two `groupby()` aggregations, create at least two engineered features, and write a markdown summary of what they did and learned. | All of this week's techniques on the student's own dataset |

**Submission:** Students save a version of each notebook and share it as Public with comments allowed. They submit two links: one for `CTD_Assignment_6` and one for the capstone notebook. Task 13 is graded in the capstone notebook.

**Note:** The assignment uses several tools that the lesson does not teach. `pd.concat()` (Tasks 10 and 11) appears in the lesson only as a quiz answer choice. The `lsuffix` and `rsuffix` arguments for `join()` (Task 5) are left for students to find in the documentation. `np.where()` is given as a ready-made line. Reading Kaggle input files, `fillna()`, and `head()` come from earlier weeks. The `regex=False` argument to `str.replace()` is mentioned only in Task 3.

---

## FAQs and Common Trouble Spots

**1. "I cannot sum `Purchase_Amount`," or "the pivot table looks wrong."**
The `Purchase_Amount` column is stored as text with a dollar sign. Students must remove the dollar sign and then convert the result to a float. A common mistake is to remove the dollar sign but forget the conversion, so the column is still text. Another common mistake is to do the conversion but not assign the result back to the column. Ask students to check the column type with `dtypes` or `info()` before they build the pivot table.

**2. "I get a `KeyError` for a column I know exists."**
This usually happens after a rename. If a student renames a column with `inplace=True`, the old name no longer exists in later cells. That is why Task 8 tells students not to use `inplace=True`. The opposite mistake also happens: `rename()` without `inplace=True` returns a new DataFrame, and the original is unchanged unless the student stores the result. Kaggle cells can also run out of order, so a DataFrame may not be in the state the student expects. Running all cells from the top usually clears this up.

**3. "`join()` gives an error about overlapping columns."**
In Task 5, `df1` and `df3` both have an `Age` column. `merge()` adds suffixes automatically, but `join()` does not. Without suffixes, `join()` raises a `ValueError` that says the columns overlap. The task asks students to find the fix in the Pandas documentation for `DataFrame.join()`. Point them to the documentation page instead of giving the arguments directly.

**4. "My `groupby()` output has strange column headers," or "I cannot sort my result."**
When `agg()` gets a list of functions, the result has two rows of column headers. This is expected for Task 2. In Task 11, selecting one column after `groupby()` and calling `mean()` returns a Series, not a DataFrame. A Series is sorted with `sort_values()` and does not need a `by` argument. Students who pass `by=` to a Series method will see an error.

**5. "My Tunisia games are not the most recent ones."**
Task 12 has no step-by-step instructions. Students must start from the reshaped `football_results` from Task 11, where every game appears once per team. They filter on the `team` column, sort by `date` with the newest games first, and then take 10 rows. Students who start from the original file see only home games. Students who skip the sort get the oldest games, because the file is in date order from 1872. The `date` values are text in year-month-day form, so they sort correctly even without conversion.
