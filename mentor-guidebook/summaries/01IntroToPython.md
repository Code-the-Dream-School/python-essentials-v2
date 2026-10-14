# Week 1 Mentor Summary: Introduction to Python

**Lesson file:** `lessons/01IntroToPython.md`
**Assignment file:** `assignments/01IntroToPython.md`

**Learning objective:** Students learn the basics of Python: variables, data types, operators, control flow, and functions. They also practice debugging and basic error handling.

---

## Lesson Overview

The lesson has nine sections. Each section ends with a short "Check for Understanding" question, and several sections have an optional video and an optional AI practice prompt.

| Section | What students learn |
|---|---|
| **1.1 Setting up your environment** | Students install Python 3 and pip, create a working folder for lesson code, create and activate a virtual environment (`.venv`), and set up VS Code with the Python extension. Windows students should use Git Bash and install Python natively, not in WSL. |
| **1.2 Python Basics** | Variables and naming conventions, the four basic data types (`int`, `float`, `str`, `bool`), and `type()`. Students learn explicit conversion with `int()`, `float()`, `str()`, and `bool()`, and the difference between implicit and explicit conversion. |
| **1.3 Operators** | Arithmetic operators (including `//`, `%`, and `**`), comparison operators, and logical operators (`and`, `or`, `not`). |
| **1.4 Block Structure and Indentation** | Python uses indentation and a colon to define blocks. The standard is 4 spaces, and students must not mix tabs and spaces. |
| **1.5 Control Flow** | `if` / `elif` / `else`, nested conditionals, `for` loops with lists and `range()`, `while` loops, `break`, and `continue`. A short note covers splitting long lines with `\` and triple-quoted strings. |
| **1.6 Functions** | Defining and calling functions, parameters and arguments, return values (and the implicit `None`), default parameters, `*args`, `**kwargs`, and the correct order of parameter types. The section ends with variable scope: local variables, reading global variables, and why assigning to a name inside a function creates a new local variable. |
| **1.7 Basic Debugging** | Debugging with `print()` statements and with the `logging` module and its levels (DEBUG, INFO, WARNING, ERROR). The VS Code debugger is mentioned as optional further reading. |
| **1.8 Error Handling** | `try`, `except`, `else`, `finally`, catching specific exceptions such as `ZeroDivisionError`, and raising exceptions with `raise`. |
| **1.9 String Operations** | String methods (students explore them with VS Code autocomplete) and f-strings, including format specifiers like `{cost:.2f}`. |

The lesson ends with an introduction to the **final project**, which students build incrementally during the course. It has two parts:

1. A Kaggle Notebook data pipeline (cleaning, aggregation, analysis, and visualization) using one of four curated datasets, or another dataset with CIL approval.
2. A public GitHub repository that scrapes weather data, stores it in a SQL database, and presents it in a Streamlit dashboard.

Students do not start work on the final project this week.

---

## Assignment Deliverables

**Setup:** Students follow the README in the [`python_homework`](https://github.com/Code-the-Dream-School/python_homework) repository to create their own copy. They create an `assignment1` branch and write all code in `assignment1/assignment1.py`. Each task should have a comment label such as `# Task 1`.

**Testing:** The assignment includes a PyTest file, `assignment1-test.py`. Students run `pytest -v -x assignment1-test.py` from the `assignment1` folder and work through the functions in order until all 10 tests pass.

**Coding tasks:** Students write ten functions. The tests check exact function names and exact return values.

| Task | Function | What it does | Concepts practiced |
|---|---|---|---|
| 1 | `hello` | Returns a fixed greeting. | Returning instead of printing |
| 2 | `greet` | Returns a greeting that includes the name argument. | f-strings |
| 3 | `calc` | Performs one of seven named math operations. The operation defaults to `"multiply"`. Returns specific messages for division by zero and for invalid operand types. | Default parameters, conditionals or `match`/`case`, `try`/`except` |
| 4 | `data_type_conversion` | Converts a value to `float`, `str`, or `int`. Returns a formatted error message if the conversion fails. | Type conversion, error handling, f-strings |
| 5 | `grade` | Averages any number of scores and returns a letter grade. Returns an error message for invalid input. | `*args`, `sum()`, `len()`, conditionals |
| 6 | `repeat` | Repeats a string a given number of times. Students must use a `for` loop with `range()`, not `string * count`. | `for` loops, `range()` |
| 7 | `student_scores` | Takes `"best"` or `"mean"` plus student scores as keyword arguments. Returns the top student's name or the average score. | `**kwargs`, dictionary iteration |
| 8 | `titleize` | Capitalizes a string like a book title, with a list of small words that stay lowercase in the middle. | `split()`, `join()`, `capitalize()`, `enumerate()`, `in` |
| 9 | `hangman` | Returns the secret word with unguessed letters replaced by `_`. | String iteration, string immutability, concatenation |
| 10 | `pig_latin` | Converts a lowercase sentence to Pig Latin, with a special rule for `qu`. | String manipulation, loops, conditionals |

**Submission:** Students commit and push the `assignment1` branch, open a pull request into `main` in their own `python_homework` repository, and submit the pull request link.

**Note:** The assignment introduces several tools that the lesson does not cover. These include `match`/`case`, `enumerate()`, `kwargs.items()`, `split()` and `join()`, list indexing such as `words[-1]`, and string immutability. The assignment explains each one briefly, but students may need extra help with them.

---

## FAQs and Common Trouble Spots

**1. "My function prints the right answer, but the test still fails."**
The tests check what a function *returns*, not what it prints. Students who are new to programming often use `print()` inside the function and forget `return`. If the function has no `return` statement, it returns `None`. Students should call their functions inside a `print()` in the main part of the file to check the return value. The test output also shows the expected value next to the actual value, so ask students to read that comparison carefully.

**2. "`pytest` (or `python`) is not found," or "my package is not installed."**
This is usually a virtual environment problem. Check that the terminal prompt shows `(.venv)`. Students must activate the environment again in every new terminal session. Windows students should use Git Bash. If `python` hangs in Git Bash, the lesson gives a `winpty` alias as the fix. Also check that the student is running `pytest` from the `assignment1` folder.

**3. "Which exception am I supposed to catch?"**
Tasks 3, 4, and 5 ask students to discover the exception type themselves. Encourage them to trigger the error on purpose in the Python interactive shell and read the exception name. Possible exceptions include `ZeroDivisionError`, `TypeError`, and `ValueError`. Students sometimes catch the wrong exception, or they put the `try` block around only part of the code. Some students use a bare `except:` or `except Exception:`. This passes the tests, but it is a good moment to discuss why catching specific exceptions is better practice.

**4. "My `titleize` or `pig_latin` function works for most words but fails one test."**
These two tasks have edge cases that students often miss. In `titleize`, the first and last words are always capitalized, even when they are small words like "the" or "of." Students also need to make sure the small words are lowercase. In `pig_latin`, a word can start with more than one consonant, and `qu` moves as a single unit, even when it comes after another consonant. Ask students to test with several example words of their own before they run PyTest.

**5. "My pull request does not look right," or "I do not know which branch to use."**
For many students, this is their first branch-and-pull-request workflow. Common problems include committing to `main` instead of `assignment1`, forgetting to push the branch, and opening the pull request against the wrong repository. The pull request should go from `assignment1` into `main` in the student's *own* `python_homework` repository. The submitted link should point to the pull request, not to the repository or a single file.
