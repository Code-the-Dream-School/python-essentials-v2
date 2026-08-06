# Assignment 3 Answer Key: More Python Skills

**Mentor note:** This lesson covers decorators, list comprehensions, closures, and custom classes. Unlike Lessons 1 and 2, there is **no PyTest file** for Assignment 3 — every task is a standalone script the student runs and verifies by hand. Review each file by reading the code and, where useful, running it. Tasks 4 and 6 are interactive games (they use `input()`), so you can't fully automate them; skim the game loop and confirm the core function/class logic is correct. Reference solutions below show one clean approach — students may vary in style and still be correct.

---

## File Setup

- Work is on an `assignment3` branch, submitted as a PR into `main`.
- All work is in the `assignment3/` folder. Each task is its own file with a specific name:
  - Task 1: `log-decorator.py`
  - Task 2: `type-decorator.py`
  - Task 3: `list-comprehensions.py`
  - Task 4: `hangman-closure.py`
  - Task 5: `extend-point-to-vector.py`
  - Task 6: `tictactoe.py`
- Tasks should be marked with comments. Task 1 also generates an output file, `decorator.log`, which will appear in the PR.
- Tasks 3 reads `../csv/employees.csv`, so `list-comprehensions.py` must be run from inside the `assignment3` folder.

Check that the PR link works, the branch is correct, all six files are present and named correctly, and the code runs without errors.

---

## Task 1: Writing and Testing a Decorator — **Objective**

A `logger_decorator` that logs the function name, positional args (or `"none"`), keyword args (or `"none"`), and return value to `./decorator.log`. Three decorated functions are then called from the mainline.

```python
# one time setup
import logging
logger = logging.getLogger(__name__ + "_parameter_log")
logger.setLevel(logging.INFO)
logger.addHandler(logging.FileHandler("./decorator.log", "a"))

def logger_decorator(func):
    def wrapper(*args, **kwargs):
        result = func(*args, **kwargs)
        logger.log(logging.INFO, f"function: {func.__name__}")
        logger.log(logging.INFO, f"positional parameters: {list(args) if args else 'none'}")
        logger.log(logging.INFO, f"keyword parameters: {kwargs if kwargs else 'none'}")
        logger.log(logging.INFO, f"return: {result}")
        return result
    return wrapper

@logger_decorator
def say_hello():
    print("Hello, World!")

@logger_decorator
def positional_func(*args):
    return True

@logger_decorator
def keyword_func(**kwargs):
    return logger_decorator

say_hello()
positional_func(1, 2, 3)
keyword_func(a=1, b=2)
```

- The wrapper must use `*args, **kwargs` so it works for functions with positional args, keyword args, both, or neither.
- The wrapper must **call the function and capture its return value** (`result = func(...)`) so the return can be logged, and must `return result` so the decorator doesn't break the function.
- The three functions must match the spec: one takes nothing / returns nothing, one takes `*args` / returns `True`, one takes `**kwargs` / returns `logger_decorator`.
- Handling the "none" case for empty args/kwargs is part of the spec — a student who logs an empty `[]` or `{}` instead of `"none"` has missed a detail but is close.

---

## Task 2: A Decorator that Takes an Argument — **Objective**

`type_converter` is a decorator **factory** — it takes a type argument and returns a decorator. The inner wrapper converts the function's return value to that type.

```python
def type_converter(type_of_output):
    def decorator(func):
        def wrapper(*args, **kwargs):
            x = func(*args, **kwargs)
            return type_of_output(x)
        return wrapper
    return decorator

@type_converter(str)
def return_int():
    return 5

@type_converter(int)
def return_string():
    return "not a number"

y = return_int()
print(type(y).__name__)  # "str"
try:
    y = return_string()
    print("shouldn't get here!")
except ValueError:
    print("can't convert that string to an integer!")
```

- Three nested levels are required: `type_converter(type_of_output)` → `decorator(func)` → `wrapper(*args, **kwargs)`. This extra layer (vs. Task 1) is the whole point of the task.
- The "think" question: `int("not a number")` raises `ValueError`, which the mainline `try/except` catches. Confirm the student's output matches the two expected print lines.

---

## Task 3: List Comprehensions Practice — **Objective**

Read the CSV into a list of lists, then build two lists with comprehensions: full names, then names containing `"e"`.

```python
import csv

with open("../csv/employees.csv") as file:
    employees = list(csv.reader(file))

names = [row[1] + " " + row[2] for row in employees[1:]]
print(names)

names_with_e = [name for name in names if "e" in name]
print(names_with_e)
```

- Both lists must be built with **list comprehensions**, not `for`/`append` loops.
- The header row must be skipped — `employees[1:]` (or an equivalent) is the intended way. A student who includes `"first_name last_name"` in the output missed this.
- The first list has 20 names; the second (names containing "e") has 15. Names are `first_name + " " + last_name`.

---

## Task 4: Closure Practice — **Objective**

`make_hangman(secret_word)` holds a `guesses` list in its scope and returns an inner `hangman_closure(letter)`. Each call appends the letter, prints the word with underscores for unguessed letters, and returns `True` when the word is fully guessed.

```python
def make_hangman(secret_word):
    guesses = []

    def hangman_closure(letter):
        guesses.append(letter)
        display = ""
        for ch in secret_word:
            if ch in guesses:
                display += ch
            else:
                display += "_"
        print(display)
        return "_" not in display

    return hangman_closure


secret = input("Enter the secret word: ")
guess_letter = make_hangman(secret)
solved = False
while not solved:
    letter = input("Guess a letter: ")
    solved = guess_letter(letter)
print("You guessed it!")
```

- This must be a **closure**: `guesses` lives in the enclosing `make_hangman` scope and persists across calls to the inner function. A student who makes `guesses` a global, or passes it in as an argument, hasn't demonstrated the closure.
- `make_hangman` returns the inner **function object** (`hangman_closure`), not the result of calling it.
- The return is `True` only when no underscores remain.
- Note: the assignment's example ("alphabet" with guesses `["a", "h"]` → `"a__ha__"`) has a typo — "alphabet" is 8 letters, so the correct display is `"a__ha___"` (three trailing underscores). Don't dock a student whose output has the extra underscore; theirs is right.

---

## Task 5: Extending a Class — **Objective**

A `Point` class with equality, string representation, and Euclidean distance. A `Vector` subclass reuses `__init__`, overrides `__str__`, and overloads `+` for vector addition.

```python
import math

class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __eq__(self, other):
        return self.x == other.x and self.y == other.y

    def __str__(self):
        return f"Point({self.x}, {self.y})"

    def distance(self, other):
        return math.sqrt((self.x - other.x) ** 2 + (self.y - other.y) ** 2)


class Vector(Point):
    def __str__(self):
        return f"Vector({self.x}, {self.y})"

    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)


p1 = Point(0, 0)
p2 = Point(3, 4)
print(p1)                 # Point(0, 0)
print(p1 == Point(0, 0))  # True
print(p1.distance(p2))    # 5.0

v1 = Vector(1, 2)
v2 = Vector(3, 4)
print(v1)                 # Vector(1, 2)
print(v1 + v2)            # Vector(4, 6)
```

- `Vector` must **inherit** from `Point` (`class Vector(Point):`) and **not** redefine `__init__` — reusing the parent's is the point of the task.
- Equality is via `__eq__`, string via `__str__` (or `__repr__`), distance via the Euclidean formula.
- `+` must be overloaded via `__add__` and must return a **new `Vector`** (not mutate in place, not return a `Point`).
- The mainline must actually print results that exercise each method for both classes.

---

## Task 6: More on Classes (Tic-Tac-Toe) — **Objective**

A custom `TictactoeException`, plus a `Board` class with `__str__`, `move()`, and `whats_next()`, driven by an interactive game loop. The assignment **provides** working versions of `__str__`, `move()`, and `whats_next()` for reuse, so most students will submit those largely intact. Focus your review on the exception class, the `__init__`, and the game loop.

```python
# Task 6
class TictactoeException(Exception):
    def __init__(self, message):
        self.message = message
        super().__init__(self.message)


class Board:
    valid_moves = ["upper left", "upper center", "upper right",
                   "middle left", "center", "middle right",
                   "lower left", "lower center", "lower right"]

    def __init__(self):
        self.board_array = [[" ", " ", " "], [" ", " ", " "], [" ", " ", " "]]
        self.turn = "X"

    # __str__, move, and whats_next are the reuse code provided in the assignment
```

The provided `move()` and `whats_next()` (see the assignment) are correct as given; running them confirms:
- An invalid move string raises `TictactoeException("That's not a valid move.")`.
- A move onto an occupied square raises `TictactoeException("That spot is taken.")`.
- A valid move fills the square, then flips `self.turn` between `"X"` and `"O"`.
- `whats_next()` returns a `(bool, message)` tuple: a completed row/column/diagonal ends the game, a full board is `"Cat's Game."`, otherwise it reports whose turn it is.

A reasonable game loop:

```python
board = Board()
game_over = False
while not game_over:
    print(board)
    move_string = input(f"{board.turn}'s move: ")
    try:
        board.move(move_string)
    except TictactoeException as e:
        print(e.message)
        continue
    game_over, message = board.whats_next()
    print(message)
print(board)
```

Key checks:
- `TictactoeException` inherits from `Exception`, stores `self.message`, and calls `super().__init__()`. This is the main thing students write from scratch here.
- `board.move()` is called inside a `try` block with an `except TictactoeException`, and the game gives the user feedback (and lets them retry) rather than crashing.
- The loop uses `whats_next()` to decide when to stop and whose turn it is.

Commentary for mentors: the assignment prose says `whats_next()` should return messages like `"X has won"` / `"O has won"`, but the **provided reuse code** actually returns `"X wins!"` / `"O wins!"` and `"Cat's Game."`. Either wording is fine — most students will use the provided strings. Also, the prose mentions `move()` updating a `last_move` variable, but the provided `move()` doesn't need it and `whats_next()` doesn't use it; its absence is not a defect.

---

## Summary for mentors

All six tasks are **objective** (coding). There is no automated test suite, so verify by reading each file and, for the non-interactive ones (Tasks 1, 2, 3, 5), running them. The two most common places students stumble: the extra nesting level in Task 2's decorator factory, and making Task 4 a genuine closure rather than leaning on a global or an extra argument.
