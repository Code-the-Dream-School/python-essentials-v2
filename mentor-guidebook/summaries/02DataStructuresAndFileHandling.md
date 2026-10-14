# Week 2 Mentor Summary: Data Structures and File Handling

**Lesson file:** `lessons/02DataStructuresAndFileHandling.md`
**Assignment file:** `assignments/02DataStructuresAndFileHandling.md`

**Learning objective:** Students learn the four core data structures (lists, tuples, dictionaries, and sets). They also learn to read and write text and CSV files, import modules, read keyboard input, use the `os` module, and manage virtual environments.

---

## Lesson Overview

The lesson has six sections. Most sections have a short video and a "Check for Understanding" question, and some sections have an optional AI practice prompt.

| Section | What students learn |
|---|---|
| **2.1 Lists, Tuples, Dictionaries, and Sets** | Students learn the terms *mutable* and *immutable*, with a clay and ceramic statue analogy. The section covers list methods (`append()`, `remove()`, `sort()`), iterating over a list, the built-in `map()` function, lambdas, and list slicing. It then covers tuples, dictionaries and their methods (`keys()`, `values()`, `items()`, `get()`), and sets with union, intersection, and difference. A review table compares the four structures. |
| **2.2 File Handling** | Opening files with `open()` and the `with` statement, reading with `read()`, writing with `"w"` and appending with `"a"`, and wrapping file operations in `try`/`except`. Students learn that `write()` does not add a newline. The section then covers the `csv` module: `csv.reader()`, `csv.writer()` with `newline=''`, and a short mention of `DictReader` and `DictWriter`. |
| **2.3 Introduction to Modules** | The three import styles (`import math`, `from math import sqrt`, and `import pandas as pd`), creating a custom module in a `.py` file, packages and `__init__.py`, the standard library, and installing external libraries with `pip`. |
| **2.4 Keyboard Input** | `input()` always returns a string, so students convert it with `int()` or `float()` and catch `ValueError`. The section ends with a simple calculator example that also catches `ZeroDivisionError`. |
| **2.5 Working with the OS** | Common `os` functions: `getcwd()`, `chdir()`, `listdir()`, `mkdir()`, `rmdir()`, `os.path.exists()`, `isfile()`, `isdir()`, `os.system()`, and `os.environ`. A short addendum introduces `sys.argv` for command-line arguments and mentions `argparse`. |
| **2.6 Virtual Environments** | Why virtual environments are useful, creating one with `venv` or `virtualenv`, activating it on Windows (Git Bash) and Mac/Linux, installing packages, `deactivate`, and `pip freeze > requirements.txt`. Students already set up a virtual environment in Week 1, so this section is mostly review. |

The lesson ends with a reminder to use AI tools for hints and guiding questions, not for complete answers.

---

## Assignment Deliverables

**Setup:** Students create an `assignment2` branch in their `python_homework` repository and work in the `assignment2` folder. They create three files: `diary.py`, `assignment2.py`, and `custom_module.py`. Each task should have a comment label such as `# Task 1`.

**Testing:** Task 1 (`diary.py`) is not checked by tests, so mentors and the reviewer read it by eye. Tasks 2 through 15 are checked by a PyTest file, `assignment2-test.py`. Students run `pytest -v -x assignment2-test.py` from the `assignment2` folder. The test file imports `assignment2.py`, so the module-level code in that file runs during the test. The tests also read several global variables that students must set (`employees`, `employee_id_column`, `minutes1`, `minutes2`, `minutes_set`, and `minutes_list`).

**Coding tasks:** The assignment has fifteen tasks. Tasks 2 through 9 work with `../csv/employees.csv`, and Tasks 12 through 15 work with `../csv/minutes1.csv` and `../csv/minutes2.csv`.

| Task | Function or file | What it does | Concepts practiced |
|---|---|---|---|
| 1 | `diary.py` | Prompts the user in a loop and appends each line to `diary.txt`. The first prompt and later prompts have different text. The loop stops after the line `"done for now"` is written. All of the code is inside a `try` block that catches `Exception` and prints traceback details. | `input()`, append mode, `with`, `try`/`except` |
| 2 | `read_employees` | Reads the employees CSV file and returns a dict with the key `"fields"` (the header row) and the key `"rows"` (all other rows). The result is stored in a global named `employees`. | `csv.reader()`, dictionaries, lists |
| 3 | `column_index` | Returns the position of a given column name in the header row. A global named `employee_id_column` stores the index of `"employee_id"`. | `list.index()` |
| 4 | `first_name` | Returns the first name from a given row number. It uses `column_index` to find the column. | Indexing nested lists |
| 5 | `employee_find` | Returns the rows that match an employee ID. It uses an inner function and `filter()`. The inner function is given exactly. | Nested functions, `filter()`, type conversion |
| 6 | `employee_find_2` | Does the same work as Task 5 with a lambda. The code is given exactly. | Lambdas |
| 7 | `sort_by_last_name` | Sorts the stored rows in place by last name and returns them. | `sort(key=...)` with a lambda |
| 8 | `employee_dict` | Turns one row into a dict of column name to value, without the `employee_id` field. Using `zip()` is optional. | Building dictionaries |
| 9 | `all_employees_dict` | Returns a dict of dicts keyed by employee ID, using `employee_dict` for each value. | Nested dictionaries |
| 10 | `get_this_value` | Returns the value of the environment variable `THISVALUE`. | `os.getenv()`, environment variables |
| 11 | `custom_module.py` and `set_that_secret` | `custom_module.py` is given exactly. `set_that_secret` calls `custom_module.set_secret()` with its argument. | Custom modules, `global` |
| 12 | `read_minutes` | Reads both minutes files into two dicts with `"fields"` and `"rows"`. Each row is stored as a tuple. The function returns both dicts. A helper function is encouraged but not required. | Tuples, returning multiple values, DRY |
| 13 | `create_minutes_set` | Converts the rows of both dicts to sets and returns their union. | Sets, hashability, union |
| 14 | `create_minutes_list` | Converts the set to a list and uses `map()` to turn each date string into a `datetime` object with the format `"%B %d, %Y"`. | `map()`, `datetime.strptime()` |
| 15 | `write_sorted_list` | Sorts the list by date, converts the dates back to strings, and writes `minutes.csv` with the header row first. It returns the converted list. | Sorting, `strftime()`, `csv.writer()` |

The assignment ends with four Check for Understanding questions about sets, lists, and dates. The answers are printed in the assignment, and students do not need to submit written answers.

**Submission:** Students commit and push the `assignment2` branch, open a pull request into `main` in their own `python_homework` repository, and submit the pull request link. The generated files `diary.txt` and `minutes.csv` will appear in the pull request, and that is expected.

**Note:** The assignment uses several tools that the lesson does not teach. These include `filter()`, `list.index()`, the `key=` argument to `sort()`, functions defined inside other functions, `os.getenv()` (the lesson shows `os.environ.get()`), the `global` keyword, returning and unpacking multiple values, hashability, the `traceback` module, and `datetime.strptime()` and `strftime()`. The assignment explains most of these briefly, but students may need extra help with them. Inner functions and closures are taught in Week 3.

---

## FAQs and Common Trouble Spots

**1. "I get `FileNotFoundError` for `../csv/employees.csv`."**
The path is relative to the folder where the student runs the program, not to the folder that holds the script. Students should run both `python assignment2.py` and `pytest` from inside the `assignment2` folder. Ask the student to print `os.getcwd()` to see where Python is looking. Also check that the `csv` folder exists at the top level of their `python_homework` repository.

**2. "The test for Task 10 fails, but my code looks correct."**
The `THISVALUE` environment variable exists only in the terminal session where the student ran `export THISVALUE=ABC`. If the student opens a new terminal or restarts VS Code, the variable is gone, and `os.getenv()` returns `None`. The student should run the `export` command again in the same terminal where they run `pytest`.

**3. "My employee search returns an empty list," or "my sort is in the wrong order."**
The `csv` module returns every value as a string. In Tasks 5 and 6, the employee ID is converted with `int()` before the comparison, because the string `"5"` does not equal the integer `5`. In Task 15, the sort must use the `datetime` objects from Task 14. If students sort by the date strings, the result is in alphabetical order by month name. Also remind students that `sort()` returns `None`, so the function must return the list itself, not the result of `sort()`.

**4. "I get `TypeError: unhashable type: 'list'` in Task 13."**
Set elements must be hashable, and lists are not hashable. This error means the student did not convert each row to a tuple in Task 12. Point them back to the Task 12 instructions. Note that Task 14 describes each element as "a list of strings," but after Task 12 each element is a tuple of strings.

**5. "PyTest fails before any tests run."**
The test file imports `assignment2.py`, so all of the module-level code in that file runs first. If that code raises an exception, or if the student's `except` block exits the program, the tests cannot start. If the student left an `input()` call in `assignment2.py`, PyTest raises an error, because by default it does not allow a test to read from the keyboard. The `input()` code belongs in `diary.py`. Ask the student to run `python assignment2.py` on its own first and fix any errors there.
