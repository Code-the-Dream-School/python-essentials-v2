## Lesson 4 Assignment — Intro to Data Engineering
### Data Analysis and Manipulation with Pandas

### **Objective:**
In this assignment, you will explore the basic functionalities of the Pandas library in Python, including creating, manipulating, inspecting, and analyzing data using various DataFrame methods. The goal is to understand how to handle data efficiently and perform essential operations to inspect and analyze datasets.

### **Step 1: Complete the Coding Tasks**  

Homework for this assignment is created within your `python_homework` folder.  Create an `assignment4` branch and change to the `assignment4` folder.  Write your code in `assignment4.py`.  As with the previous lessons, you will run unit tests on the assignment `pytest -v -x assignment4-test.py`. **Use the variable names and file names shown in backticks (`task1_data_frame`, `employees.csv`, etc.) exactly as written — the tests depend on these exact names.**

---

### **Tasks:**

### **Task 1: Introduction to Pandas - Creating and Manipulating DataFrames**
1. **Create a DataFrame from a dictionary:**
   - Use a dictionary containing the following data:
     - `Name`: ['Alice', 'Bob', 'Charlie']
     - `Age`: [25, 30, 35]
     - `City`: ['New York', 'Los Angeles', 'Chicago']
   - Convert the dictionary into a DataFrame using Pandas. **Use these exact names, ages, and cities — the tests compare your DataFrame against these exact values.**
   - Print the DataFrame to verify its creation.
   - save the DataFrame in a variable called `task1_data_frame` and run the tests.

2. **Add a new column:**
   - Make a copy of the dataFrame you created named `task1_with_salary` (use the `copy()` method)
   - Add a column called `Salary` with values `[70000, 80000, 90000]`.
   - Print the new DataFrame and run the tests.

3. **Modify an existing column:**
   - Make a copy of `task1_with_salary` in a variable named `task1_older`
   - Increment the `Age` column by 1 for each entry.
   - Print the modified DataFrame to verify the changes and run the tests.

4. **Save the DataFrame as a CSV file:**
   - Save the `task1_older` DataFrame to a file named `employees.csv` using ```to_csv()```, do not include an index in the csv file.
   - Look at the contents of the CSV file to see how it's formatted.
   - Run the tests.
     

### **Task 2: Loading Data from CSV and JSON**
1. **Read data from a CSV file:**
   - Load the CSV file from Task 1 into a new DataFrame saved to a variable `task2_employees`.
   - Print it and run the tests to verify the contents.

2. **Read data from a JSON file:**
   - Create a JSON file (`additional_employees.json`).  The file adds two new employees.  Eve, who is 28, lives in Miami, and has a salary of 60000, and Frank, who is 40, lives in Seattle, and has a salary of 95000. **Use these exact values, with columns `Name`, `Age`, `City`, `Salary` — the tests compare the combined result exactly.**
   - Load this JSON file into a new DataFrame and assign it to the variable `json_employees`.
   - Print the DataFrame to verify it loaded correctly and run the tests.

3. **Combine DataFrames:**
   - Combine the data from the JSON file into the DataFrame loaded from the CSV file and save it in the variable `more_employees`.  When you combine them, reset the index so the rows are numbered 0–4 (for example, pass `ignore_index=True` to `pd.concat`).
   - Print the combined Dataframe and run the tests.

### **Task 3: Data Inspection - Using Head, Tail, and Info Methods**
1. **Use the `head()` method:**
   - Assign the first three rows of the `more_employees` DataFrame to the variable `first_three`
   - Print the variable and run the tests.

2. **Use the `tail()` method:**
   - Assign the last two rows of the `more_employees` DataFrame to the variable `last_two`
   - Print the variable and run the tests.

3. **Get the `shape` of a DataFrame**
   - Assign the shape of the `more_employees` DataFrame to the variable `employee_shape`
   - Print the variable and run the tests 

4. **Use the `info()` method:**
   - Print a concise summary of the DataFrame using the `info()` method to understand the data types and non-null counts.

### **Task 4: Data Cleaning**

1. Create a DataFrame from `dirty_data.csv` file and assign it to the variable `dirty_data`.
   - Print it and run the tests.
   - Create a copy of the dirty data in the variable `clean_data` (use the `copy()` method).  You will use data cleaning methods to update `clean_data`.

2. Remove any duplicate rows from the DataFrame
   - Print it and run the tests.

3. Convert `Age` to numeric and handle missing values
   - Print it and run the tests.

4. Convert `Salary` to numeric and replace known placeholders (`unknown`, `n/a`) with NaN
   - print it and run the tests.

5. Fill missing numeric values (use `fillna`).  Fill `Age` with the mean and `Salary` with the median
   - Print it and run the tests

6. Convert `Hire Date` to `datetime`.  The dates are stored in more than one format, so use a method that handles mixed formats (for example, `pd.to_datetime(..., format='mixed')`).
   - Print it and run the tests

7. Strip extra whitespace and standardize `Name` and `Department` as uppercase
   - Print it and run the tests


---

### **Step 2: Submit Your Assignment on GitHub**  

**Follow these steps to submit your work:**  

#### **1️⃣ Add, Commit, and Push Your Changes**  
- Within your python_homework folder, do a git add and a git commit for the files you have created, so that they are added to the `assignment4` branch.
- Push that branch to GitHub. 

#### **2️⃣ Create a Pull Request**  
- Log on to your GitHub account.
- Open your `python_homework` repository.
- Select your `assignment4` branch.  It should be one or several commits ahead of your main branch.
- Create a pull request.

#### **3️⃣ Submit Your GitHub Link**  
- Your browser now has the link to your pull request.  Copy that link. 
- Paste the URL into the **assignment submission form**.  

---

<details>
<summary>Rubric (for AirHub reviewer and mentors)</summary>

### Required Deliverables/Tasks

All four tasks are required; there is no optional work in this assignment. Work is on an `assignment4` branch; code in `assignment4/assignment4.py`, run from inside the `assignment4` folder. Hold the line on the exact variable names, file names, column names, and the specified data values (the tests compare them directly with `.equals()`). Be lenient about coding style, folder location / working directory, and the JSON file's internal orientation. Do not fail on printed / `info()` output.

- **Setup / submission** — `assignment4` branch; PR into `main`; PR link submitted. `Example — adapt to your own layout`: the working folder and branch name must NOT be failed for differing — the reviewer cannot see the filesystem.
- **Task 1.1 — `task1_data_frame`** — A DataFrame built from the dictionary with `Name` = Alice, Bob, Charlie; `Age` = 25, 30, 35; `City` = New York, Los Angeles, Chicago. `Use exactly as written (later tasks depend on these names)` — the variable name and these values are compared with `.equals()`.
- **Task 1.2 — `task1_with_salary`** — A `.copy()` of `task1_data_frame` with a `Salary` column `[70000, 80000, 90000]`. `Use exactly as written` for the variable name, column name, and values. Must be an independent copy (not a reference).
- **Task 1.3 — `task1_older`** — A `.copy()` of `task1_with_salary` with `Age` incremented by 1. `Use exactly as written` for the variable name. Must be an independent copy so earlier DataFrames are not mutated.
- **Task 1.4 — `employees.csv`** — Written from `task1_older` via `to_csv(..., index=False)`. `Use exactly as written` for the filename and no-index requirement (re-read shape must be `(3, 4)`).
- **Task 2.1 — `task2_employees`** — `employees.csv` read back into a DataFrame. `Use exactly as written` for the variable name.
- **Task 2.2 — `additional_employees.json` + `json_employees`** — A hand-created JSON file adding Eve (28, Miami, 60000) and Frank (40, Seattle, 95000), loaded into `json_employees` with columns `Name, Age, City, Salary`. `Use exactly as written` for the values and column order — compared with `.equals()`. `Example — adapt to your own layout` for the JSON orientation itself (any orientation that yields the correct frame is fine).
- **Task 2.3 — `more_employees`** — The combination of `task2_employees` and `json_employees`, shape `(5, 4)`, with the index reset to run 0–4 (`ignore_index=True`). `Use exactly as written` for the variable name.
- **Task 3.1 — `first_three`** — First three rows of `more_employees` (`head(3)`), keeping the original index. `Use exactly as written` for the variable name.
- **Task 3.2 — `last_two`** — Last two rows of `more_employees` (`tail(2)`), keeping the original index (rows 3, 4). `Use exactly as written` for the variable name.
- **Task 3.3 — `employee_shape`** — The shape tuple of `more_employees`, i.e. `(5, 4)`. `Use exactly as written` for the variable name.
- **Task 3.4 — `info()` call** — Print a summary of `more_employees` via `info()`. No test asserts its output; do not fail on the printed content — only that the call is present.
- **Task 4.1 — `dirty_data` + `clean_data`** — Read `dirty_data.csv` raw into `dirty_data` (do NOT clean this one), then make a `.copy()` named `clean_data` for all cleaning. `Use exactly as written` for both variable names and the filename.
- **Task 4.2 — Remove duplicates** — Drop duplicate rows from `clean_data` (leaves 6 rows from the original 7).
- **Task 4.3 — `Age` to numeric** — Convert `Age` to numeric, coercing bad values to NaN.
- **Task 4.4 — `Salary` to numeric** — Replace placeholders `unknown` and `n/a` with NaN, then convert `Salary` to numeric. `Use exactly as written` for the placeholder strings.
- **Task 4.5 — Fill missing values** — `fillna` `Age` with its mean and `Salary` with its median; no nulls should remain in either column.
- **Task 4.6 — `Hire Date` to datetime** — Convert `Hire Date` to datetime with no `NaT` values; the source mixes date formats, so `format='mixed'` (or an equivalent per-row parse) is needed.
- **Task 4.7 — Standardize text** — Strip whitespace and uppercase both `Name` and `Department`.

### Optional Deliverables/Tasks

**None.** This assignment contains no optional, stretch, bonus, or advanced work — every task is required, and the answer key marks all four tasks as Objective.

</details>
