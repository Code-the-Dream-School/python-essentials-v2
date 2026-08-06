# Assignment 2 Answer Key: Data Structures and File Handling

**Mentor note:** Tasks 2–15 are validated by `assignment2-test.py` (PyTest) — if all tests pass, the functions meet requirements. Task 1 (`diary.py`) is a standalone interactive program that PyTest does **not** check, so review it by reading the code. The "Check for Understanding" section is written-answer and subjective. Reference solutions below show one clean approach; students may vary in style.

**Important setup detail:** The tests read `../csv/employees.csv`, so `assignment2.py` must be run from inside the `assignment2` folder. Task 10 also requires the environment variable `THISVALUE=ABC` to be set in the terminal session, and Task 11 requires a `custom_module.py` file. If a test fails for a student whose code looks correct, check these two things first.

---

## File Setup

- Work is on an `assignment2` branch, submitted as a PR into `main`.
- Files created in `assignment2/`: `diary.py`, `assignment2.py`, and `custom_module.py`.
- Tasks marked with comments (`# Task 1`, etc.).
- Running `pytest -v -x assignment2-test.py` from the `assignment2` folder should pass all 14 test functions.
- Note: `diary.txt` and `minutes.csv` are generated output files and will appear in the PR.

---

## Task 1: Diary — **Objective (not PyTest-tested; read the code)**

Appends user input to `diary.txt` line by line until `"done for now"`, using a `with` block wrapped in a `try`/`except Exception`.

```python
import traceback

try:
    with open("diary.txt", "a") as file:
        prompt = "What happened today? "
        while True:
            line = input(prompt)
            file.write(line + "\n")
            if line == "done for now":
                break
            prompt = "What else? "
except Exception as e:
    trace_back = traceback.extract_tb(e.__traceback__)
    stack_trace = list()
    for trace in trace_back:
        stack_trace.append(f'File : {trace[0]} , Line : {trace[1]}, Func.Name : {trace[2]}, Message : {trace[3]}')
    print(f"Exception type: {type(e).__name__}")
    message = str(e)
    if message:
        print(f"Exception message: {message}")
    print(f"Stack trace: {stack_trace}")
```

---

## Task 2: Read a CSV File — **Objective**

`read_employees()` returns a dict with `"fields"` (header row) and `"rows"` (list of the remaining rows). A module-level line stores the result in a global `employees`.

```python
import csv

def read_employees():
    employees = {}
    rows = []
    try:
        with open("../csv/employees.csv") as file:
            reader = csv.reader(file)
            for i, row in enumerate(reader):
                if i == 0:
                    employees["fields"] = row
                else:
                    rows.append(row)
        employees["rows"] = rows
        return employees
    except Exception as e:
        print(f"Exception type: {type(e).__name__}: {e}")

employees = read_employees()
```

---

## Task 3: Find the Column Index — **Objective**

```python
def column_index(column_name):
    return employees["fields"].index(column_name)

employee_id_column = column_index("employee_id")
```

---

## Task 4: Find the Employee First Name — **Objective**

```python
def first_name(row_number):
    index = column_index("first_name")
    return employees["rows"][row_number][index]
```

- Should call `column_index` rather than hard-coding the column number.

---

## Task 5: Find the Employee (function in a function) — **Objective**

```python
def employee_find(employee_id):
    def employee_match(row):
        return int(row[employee_id_column]) == employee_id
    matches = list(filter(employee_match, employees["rows"]))
    return matches
```

---

## Task 6: Find the Employee with a Lambda — **Objective**

```python
def employee_find_2(employee_id):
    matches = list(filter(lambda row: int(row[employee_id_column]) == employee_id, employees["rows"]))
    return matches
```

- Same result as Task 5 but with an inline `lambda`.

---

## Task 7: Sort the Rows by last_name — **Objective**

```python
def sort_by_last_name():
    last_name_column = column_index("last_name")
    employees["rows"].sort(key=lambda row: row[last_name_column])
    return employees["rows"]
```

- Uses `list.sort(key=...)` with a lambda. `.sort()` sorts in place, so the function returns `employees["rows"]`.
- After sorting, the first last name is `"Bowman"`.

---

## Task 8: Create a dict for an Employee — **Objective**

Maps column headers to row values, **skipping** `employee_id`.

```python
def employee_dict(row):
    result = {}
    for i, field in enumerate(employees["fields"]):
        if field != "employee_id":
            result[field] = row[i]
    return result
```

---

## Task 9: A dict of dicts — **Objective**

```python
def all_employees_dict():
    result = {}
    for row in employees["rows"]:
        result[row[employee_id_column]] = employee_dict(row)
    return result
```

---

## Task 10: Use the os Module — **Objective**

```python
import os

def get_this_value():
    return os.getenv("THISVALUE")
```

---

## Task 11: Creating Your Own Module — **Objective**

`custom_module.py`:
```python
secret = "shazam!"

def set_secret(new_secret):
    global secret
    secret = new_secret
```

In `assignment2.py`:
```python
import custom_module

def set_that_secret(new_secret):
    custom_module.set_secret(new_secret)
```

---

## Task 12: Read minutes1.csv and minutes2.csv — **Objective**

Returns two dicts; rows stored as **tuples** (needed for the set in Task 13). Look for a DRY helper function.

```python
def read_minutes():
    def read_file(filename):
        d = {}
        rows = []
        with open(filename) as file:
            reader = csv.reader(file)
            for i, row in enumerate(reader):
                if i == 0:
                    d["fields"] = row
                else:
                    rows.append(tuple(row))
        d["rows"] = rows
        return d
    minutes1 = read_file("../csv/minutes1.csv")
    minutes2 = read_file("../csv/minutes2.csv")
    return minutes1, minutes2

minutes1, minutes2 = read_minutes()
```

---

## Task 13: Create minutes_set — **Objective**

```python
def create_minutes_set():
    set1 = set(minutes1["rows"])
    set2 = set(minutes2["rows"])
    return set1.union(set2)

minutes_set = create_minutes_set()
```

---

## Task 14: Convert to datetime — **Objective**

```python
from datetime import datetime

def create_minutes_list():
    minutes_list = list(minutes_set)
    minutes_list = list(map(lambda x: (x[0], datetime.strptime(x[1], "%B %d, %Y")), minutes_list))
    return minutes_list

minutes_list = create_minutes_list()
```

- Uses `map()` with a lambda; each element becomes a tuple of `(name, datetime object)`.
- The date format string is `"%B %d, %Y"` (e.g. "October 31, 1992").

---

## Task 15: Write Out Sorted List — **Objective**

```python
def write_sorted_list():
    minutes_list.sort(key=lambda x: x[1])
    converted = list(map(lambda x: (x[0], datetime.strftime(x[1], "%B %d, %Y")), minutes_list))
    with open("./minutes.csv", "w", newline="") as file:
        writer = csv.writer(file)
        writer.writerow(minutes1["fields"])
        writer.writerows(converted)
    return converted
```


---

## Check for Understanding — **Subjective**

Four short written questions. The assignment provides model answers; use them as the rubric. A correct response should convey:

1. **Why a set if you end up with a list?** — The source lists had duplicates; converting to a set removes them (sets hold only unique values).
2. **Why convert the set back to a list?** — Sets are unordered and don't support `sort()`/`map()` the way the task needs; a list does, and preserves order.
3. **Why convert date strings to datetime objects?** — So sorting is chronological. As strings they'd sort alphabetically, putting "April 1, 2023" before "September 2, 1980".
4. **Why convert back to strings before writing the CSV?** — datetime objects print in a less friendly format; converting back restores the original readable date style.

- A weak answer misses that the **set is about deduplication** and the **datetime is about correct chronological sorting** — these two ideas are the heart of the exercise.
