# Lesson 1 Rubric – Introduction to Python

## Overview
This first Python assignment builds on the same spirit as the JavaScript Lesson 1: it’s about verifying foundational skills rather than perfection.  
Students should demonstrate the ability to write valid Python syntax, define and call functions, and handle core concepts such as loops, conditionals, and error handling.  

Mentors should **check for functional correctness, clarity, and effort**.  
As long as the student’s code runs and passes PyTest (or mostly does, with minor syntax issues), the assignment can be marked complete.

---

## Completion Criteria

Mark the assignment **complete** if the student:

- [ ] Forked/cloned the `python_homework` repository and submitted a pull request from `assignment1` → `main`.
- [ ] Created a file named `assignment1.py` in the correct location and implemented **all 10 functions**.
- [ ] Demonstrated an understanding of:
  - Function definitions and `return` statements  
  - String formatting and type conversion  
  - Conditionals (`if`, `elif`, `else`)  
  - Loops and ranges  
  - Error handling with `try` / `except`
  - Use of `*args` and `**kwargs`
- [ ] All functions **return** values (not just print them).
- [ ] Code runs without syntax errors and passes most or all PyTest checks.
- [ ] The student commits and pushes their work properly to GitHub.

---

## Concept-by-Concept Rubric

| Task | Concept | Expected Output / Criteria | Common Issues |
|------|----------|-----------------------------|----------------|
| **1** | `hello()` | Returns `"Hello!"` | Forgetting to use `return` instead of `print` |
| **2** | `greet(name)` | Returns formatted string `"Hello, Name!"` | Missing f-string or capitalization mismatch |
| **3** | `calc(a, b, operation="multiply")` | Handles all listed operations correctly and returns correct values; catches `ZeroDivisionError` and `TypeError` with appropriate messages | Not handling divide-by-zero; forgetting default argument |
| **4** | `data_type_conversion(value, type_name)` | Returns correct type or formatted error message | Not catching `ValueError`; incorrect f-string formatting |
| **5** | `grade(*args)` | Returns correct letter grade based on average; handles invalid data | Missing error handling for non-numeric inputs |
| **6** | `repeat(string, count)` | Uses a `for` loop with `range` to repeat a string | Using `*` operator instead of loop |
| **7** | `student_scores(mode, **kwargs)` | Returns either best student name (`"best"`) or average score (`"mean"`) | Not looping through `kwargs.items()` properly |
| **8** | `titleize(string)` | Returns correctly title-cased string with lowercase "little words" | Forgetting to capitalize first/last word; case sensitivity |
| **9** | `hangman(secret, guess)` | Returns string revealing guessed letters, underscores for others | String mutation errors; misunderstanding of concatenation |
| **10** | `pig_latin(string)` | Converts words correctly following vowel/consonant rules | Not handling "qu" case; splitting/joining incorrectly |

---

## Mentor Notes

- **Debugging and Testing:** Encourage students to run `pytest -v -x assignment1-test.py` after each function. Partial progress is fine—passing even half of the tests shows strong engagement.
- **Encourage Readability:** Well-named functions and consistent indentation matter more than perfection.
- **Be Kind to Beginners:** This assignment introduces both Python syntax and a new GitHub workflow. If a student submits working code but hasn’t fully passed PyTest, give detailed feedback rather than withholding completion.
- **Stretch Goals (Optional):**
  - Clean handling of edge cases
  - Consistent use of formatted strings
  - Thoughtful commenting and clear organization

---

## Mentor Scoring (Optional)

| Category | Description | Points |
|-----------|--------------|--------|
| **Code Correctness** | Functions work and pass tests | 4 |
| **Syntax & Structure** | Proper indentation, naming, use of `return` | 2 |
| **Error Handling** | Uses `try/except` appropriately | 2 |
| **GitHub Submission** | Proper branch, PR link, commit message | 1 |
| **Effort & Completeness** | Code attempts all 10 tasks | 1 |
| **Total** |  | **/10** |

---

### Mentor Checklist Summary
Before marking the assignment complete, confirm that:
- [ ] Student’s PR includes all 10 functions in `assignment1.py`
- [ ] Code runs without major syntax errors
- [ ] PyTest passes or mostly passes
- [ ] Student has demonstrated comprehension of function structure and Python basics
- [ ] You’ve provided at least one short encouraging comment in your feedback

---

### Example Mentor Feedback
> Great work completing your first Python assignment! You defined all required functions and handled most of the logic cleanly.  
> Be sure to check how your code handles divide-by-zero cases in `calc`, and try formatting your greeting string with an f-string next time.  
> Keep up the excellent work — this was a strong start!
