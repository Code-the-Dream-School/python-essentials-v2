# Assignment 1 Answer Key: Intro to Python

**Mentor note:** This key covers the 10 coding tasks in Lesson 1. All tasks are validated by `assignment1-test.py` (PyTest), so a student whose tests all pass has met the requirements. The reference solutions below show one clean, idiomatic way to solve each task — students may vary in style and still be correct. Focus your review on the points called out under each task.

---

## File Setup

- Work is done in the student's own fork of the [python_homework](https://github.com/Code-the-Dream-School/python_homework) repository.
- Code goes in `assignment1/assignment1.py` (the file is provided but empty).
- All work should be on a branch named `assignment1` (or `lesson1`), submitted as a pull request from that branch into `main`.
- Each task should be marked with a comment (e.g. `# Task 1`) to help the reviewer.
- Tests are run from the `assignment1` folder with `pytest -v -x assignment1-test.py`. All 10 tests should pass.

Check that the PR link works, the branch is correct, tasks are comment-labeled, and all tests pass.

---

## Task 1: Hello — **Objective**

Function takes no arguments and returns (not prints) `Hello!`.

```python
def hello():
    return "Hello!"
```

---

## Task 2: Greet with a Formatted String — **Objective**

Takes a name, returns `Hello, Name!` using an f-string.

```python
def greet(name):
    return f"Hello, {name}!"
```

---

## Task 3: Calculator — **Objective**

Three arguments; third defaults to `"multiply"`. Handles add, subtract, multiply, divide, modulo, int_divide, power. Catches divide-by-zero (`ZeroDivisionError`) and bad operand types (`TypeError`).

```python
def calc(a, b, operation="multiply"):
    try:
        match operation:
            case "add":
                return a + b
            case "subtract":
                return a - b
            case "multiply":
                return a * b
            case "divide":
                return a / b
            case "modulo":
                return a % b
            case "int_divide":
                return a // b
            case "power":
                return a ** b
    except ZeroDivisionError:
        return "You can't divide by 0!"
    except TypeError:
        return "You can't multiply those values!"
```

---

## Task 4: Data Type Conversion — **Objective**

Takes a value and a type name (`float`, `str`, or `int`); returns the converted value. Catches `ValueError` from bad conversions.

```python
def data_type_conversion(value, type_name):
    try:
        match type_name:
            case "int":
                return int(value)
            case "float":
                return float(value)
            case "str":
                return str(value)
    except ValueError:
        return f"You can't convert {value} into a {type_name}."
```

---

## Task 5: Grading System with `*args` — **Objective**

Collects arbitrary scores, averages them, returns a letter grade. Catches bad input.

```python
def grade(*args):
    try:
        average = sum(args) / len(args)
    except TypeError:
        return "Invalid data was provided."
    if average >= 90:
        return "A"
    elif average >= 80:
        return "B"
    elif average >= 70:
        return "C"
    elif average >= 60:
        return "D"
    else:
        return "F"
```

---

## Task 6: For Loop with a Range — **Objective**

Returns a string repeated `count` times, built with a for loop and range (not `string * count`).

```python
def repeat(string, count):
    result = ""
    for i in range(count):
        result += string
    return result
```

---

## Task 7: Student Scores with `**kwargs` — **Objective**

First arg is `"best"` or `"mean"`; keyword args are student=score pairs.

```python
def student_scores(metric, **kwargs):
    if metric == "best":
        return max(kwargs, key=kwargs.get)
    elif metric == "mean":
        return sum(kwargs.values()) / len(kwargs)
```

---

## Task 8: Titleize — **Objective**

Capitalizes a string like a book title. First and last words always capitalized; "little words" (`a`, `on`, `an`, `the`, `of`, `and`, `is`, `in`) stay lowercase in the middle.

```python
def titleize(title):
    little_words = ["a", "on", "an", "the", "of", "and", "is", "in"]
    words = title.split()
    result = []
    for i, word in enumerate(words):
        if i == 0 or i == len(words) - 1:
            result.append(word.capitalize())
        elif word in little_words:
            result.append(word.lower())
        else:
            result.append(word.capitalize())
    return " ".join(result)
```


---

## Task 9: Hangman — **Objective**

Returns the secret with any letter not in the guess string replaced by an underscore.

```python
def hangman(secret, guess):
    result = ""
    for letter in secret:
        if letter in guess:
            result += letter
        else:
            result += "_"
    return result
```

---

## Task 10: Pig Latin — **Objective**

Converts a lowercase, punctuation-free English string to Pig Latin. Vowel-start → add `"ay"`; consonant-start → move leading consonants to the end, then add `"ay"`; `"qu"` moves as a unit.

```python
def pig_latin(text):
    vowels = "aeiou"
    result = []
    for word in text.split():
        if word[0] in vowels:
            result.append(word + "ay")
        else:
            i = 0
            while i < len(word) and word[i] not in vowels:
                # move "qu" together
                if word[i] == "q" and i + 1 < len(word) and word[i + 1] == "u":
                    i += 2
                    break
                i += 1
            result.append(word[i:] + word[:i] + "ay")
    return " ".join(result)
```