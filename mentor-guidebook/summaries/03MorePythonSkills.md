# Week 3 Mentor Summary: More Python Skills

**Lesson file:** `lessons/03MorePythonSkills.md`
**Assignment file:** `assignments/03MorePythonSkills.md`

**Learning objective:** Students learn object-oriented programming, decorators, list comprehensions, and closures. They see how these patterns help them write cleaner, more modular code, and how frameworks like Dash use decorators.

---

## Lesson Overview

The lesson has four sections. There are no videos or "Check for Understanding" questions this week. Three optional AI practice prompts appear in the lesson.

| Section | What students learn |
|---|---|
| **3.1 Object-oriented programming in Python** | Students define a `Dog` class with `__init__()`, instance variables, and methods, and they learn what `self` means. The section then adds a class variable (`_count`), the single-underscore naming convention, and a class method with `@classmethod` and `cls`. It covers inheritance with a `BigDog` subclass, `super()`, and method overriding. It ends with the `__dict__` attribute and a subclass of `str` that overrides `__new__`. |
| **3.2 Introduction to Decorators** | Students first use built-in decorators: `@classmethod`, `@property`, and `@dataclass`. They learn that functions are first-class objects and can be passed to other functions. They then write their own decorator with an inner `wrapper` function, a timing decorator that uses `*args` and `**kwargs`, and a decorator factory that takes arguments. The section ends with an example of how Dash uses `@app.callback` to register functions, and a small example that registers wrapped functions in a dict. |
| **3.3 Python List Comprehensions** | List comprehensions as a shorter form of a `for` loop with `append()`, including an `if` filter. The section also shows generator expressions, which use parentheses instead of square brackets. |
| **3.4 Python Closures** | A closure is an inner function that keeps access to variables from the outer function. The example is a guessing game made with `make_secret()`. A second version counts bad guesses and uses the `nonlocal` keyword to change an outer variable. |

The lesson ends with a short list of what students learned.

---

## Assignment Deliverables

**Setup:** Students create an `assignment3` branch in their `python_homework` repository and work in the `assignment3` folder. They create six files with exact names: `log-decorator.py`, `type-decorator.py`, `list-comprehensions.py`, `hangman-closure.py`, `extend-point-to-vector.py`, and `tictactoe.py`. Each task should have a comment label.

**Testing:** There is no PyTest file this week. Students test each program by running it and checking the output. Tasks 4 and 6 are interactive games, so students test them by playing several games.

**Coding tasks:** The assignment has six tasks, and all of them are required.

| Task | File | What it does | Concepts practiced |
|---|---|---|---|
| 1 | `log-decorator.py` | A `logger_decorator` writes the function name, the positional parameters, the keyword parameters, and the return value to `./decorator.log` for each call. Students decorate three functions: one with no arguments, one with `*args` that returns `True`, and one with `**kwargs` that returns `logger_decorator`. The log format and logging setup are examples that students may adapt. | Decorators, `*args`, `**kwargs`, `logging` |
| 2 | `type-decorator.py` | A decorator factory `type_converter(type_of_output)` converts a function's return value to the given type. `return_int()` is converted to `str`, and `return_string()` is converted to `int`, which raises a `ValueError`. The mainline verification code is given exactly. | Decorator factories, type conversion, `try`/`except` |
| 3 | `list-comprehensions.py` | Reads `../csv/employees.csv` with the `csv` module. Builds a list of full names with a list comprehension, skipping the header row. Builds a second list of only the names that contain the letter "e". Both lists are printed. | List comprehensions, `csv` |
| 4 | `hangman-closure.py` | `make_hangman(secret_word)` returns an inner function `hangman_closure(letter)`. Each call records the guess, prints the word with underscores for unguessed letters, and returns `True` when the word is complete. Students also write a game loop with `input()`. | Closures |
| 5 | `extend-point-to-vector.py` | A `Point` class with equality, a string representation, and Euclidean distance. A `Vector` subclass reuses the `Point` `__init__()`, prints differently, and supports `+` to return a new `Vector`. | Classes, inheritance, special methods |
| 6 | `tictactoe.py` | A custom `TictactoeException` class and a `Board` class with `__str__()`, `move()`, and `whats_next()`. The assignment provides code for these three methods. Students write the exception class, the `Board` `__init__()`, and an interactive game loop that catches `TictactoeException`. | Custom exceptions, classes, class variables |

**Submission:** Students commit and push the `assignment3` branch, open a pull request into `main` in their own `python_homework` repository, and submit the pull request link. The file `decorator.log` may also appear in the pull request.

**Note:** The assignment uses several tools that the lesson does not teach. These include the special methods `__eq__()`, `__str__()`, and `__add__()`, writing a custom exception class, `logging.getLogger()` with a `FileHandler`, and `func.__name__`. Euclidean distance needs `math.sqrt()` or `** 0.5`. The provided `whats_next()` method also uses `for`/`else`, which students have not seen. Logging was introduced in Week 1 and the `csv` module in Week 2, so students may want to review those lessons.

---

## FAQs and Common Trouble Spots

**1. "My decorated function returns `None`," or "I get `TypeError: 'NoneType' object is not callable`."**
These errors usually mean a `return` statement is missing. The wrapper must return the value from `func(*args, **kwargs)`, and the decorator must return the wrapper. In Task 2, a decorator factory has three levels, and each level must return the next one. Students who write `@type_converter` without parentheses and an argument will also see confusing errors. Ask students to compare their structure with `repeat_with_prefix` in the lesson.

**2. "My log file is empty," or "it has old entries from earlier runs."**
The example logging setup opens `decorator.log` in append mode, so each run adds new entries after the old ones. Students may delete the file between test runs. If the file is empty, check that the logger level is set to `INFO` and that the student calls the decorated functions in the mainline code. A function that is only defined and never called writes nothing to the log.

**3. "My hangman game forgets my earlier guesses."**
The `guesses` list must be created in `make_hangman()`, not inside `hangman_closure()`. If the list is created inside the inner function, it is empty again on every call. Students also sometimes write `return hangman_closure()`, which calls the function. The outer function must return the function itself with no parentheses. Some students ask why `nonlocal` is not needed here. Appending to a list changes the existing list, and `nonlocal` is needed only when the inner function assigns a new value to the outer name.

**4. "My `Vector` prints like a `Point`," or "`+` gives a `TypeError`."**
`Vector` must define its own `__str__()` method to override the one in `Point`. To support the `+` operator, the class must define `__add__(self, other)`, and that method must return a new `Vector` object. Remind students that `Vector` should not define its own `__init__()`. When students define `__eq__()` without `__str__()`, printing an object still shows the default text such as `<__main__.Point object at 0x...>`.

**5. "The game says Cat's Game, but X won on the last move."**
The provided `whats_next()` method checks for a full board before it checks for a winner. If the ninth move completes three in a row, the method returns `(True, "Cat's Game.")` instead of the winner. Students who use the provided code as written will see this behavior. It is not the student's mistake, and it does not affect grading. Students who want to fix it can move the full-board check after the win checks.
