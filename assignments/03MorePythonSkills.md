# **Lesson 03: More Python Skills**

## **Assignment 3 Objective**

In this assignment, you will get practice in using decorators, list comprehensions, closures, and custom classes.

## **Assignment Instructions**

You create the code for this assignment in your python_homework/assignment3 folder.  Be sure to create an `assignment3` git branch before you start.  As usual, mark the code that completes each task with a comment line.

---

## **Task 1: Writing and Testing a Decorator**

1. Within the assignment3 folder, create a file called `log-decorator.py`.  It should contain the following.
2. Declare a decorator called logger_decorator.  This should log the name of the called function (`func.__name__`), the input parameters of that were passed, and the value the function returns, to a file `./decorator.log`.  (Logging was described in lesson 1, so review this if you need to do so.)  Functions may have positional arguments, keyword arguments, both, or neither.  So for each invocation of a decorated function, the log would have the following. Example format — your exact spacing and wording may vary; what matters is that each invocation logs the function name, the positional parameters (or "none"), the keyword parameters (or "none"), and the return value:
    ```
    function: <the function name>
    positional parameters: <a list of the positional parameters, or "none" if none are passed>
    keyword parameters: <a dict of the keyword parameters, or "none" if none are passed>
    return: <the return value>
    ```
    Here's a setup example on logging (adapt as needed; your logger name may differ):
   
    ```python
    # one time setup
    import logging
    logger = logging.getLogger(__name__ + "_parameter_log")
    logger.setLevel(logging.INFO)
    logger.addHandler(logging.FileHandler("./decorator.log","a"))
    ...
    # To write a log record:
    logger.log(logging.INFO, "this string would be logged")
    ```
3. Declare a function that takes no parameters and returns nothing.  For example, it could print "Hello, World!".  Decorate this function with your decorator.
4. Declare a function that takes a variable number of positional arguments and returns `True`.  Decorate this function with your decorator.
5. Declare a function that takes no positional arguments and a variable number of keyword arguments, and that returns `logger_decorator`.  Decorate this function with your decorator.
6. Within the mainline code, call each of these three functions, passing parameters for the functions that take positional or keyword arguments.  Run the program, and verify that the log file contains the information you want.

---

## **Task 2: A Decorator that Takes an Argument**

1. Within your assignment3 folder, write a script called `type-decorator.py`.
2. Declare a decorator called type_converter.  It has one argument called `type_of_output`, which would be a type, like `str` or `int` or `float`.  It should convert the return from `func` to the corresponding type. Example — this illustrates the conversion; adapt it inside your wrapper:
   ```python
   x = func(*args, **kwargs)
   return type_of_output(x)
   ```
3. Write a function `return_int()` that takes no arguments and returns the integer value 5.  Decorate that function with `type_converter`.  In the decoration, pass `str` as the parameter to `type_converter`.
4. Write a function `return_string()` that takes no arguments and returns the string value "not a number".  Decorate that function with `type_converter`.  In the decoration, pass `int` as the parameter to `type_converter`.  Think: What's going to happen?
5. In the mainline of the program, add the following (use exactly as written — this verifies your decorator):
   ```python
   y = return_int()
   print(type(y).__name__) # This should print "str"
   try:
      y = return_string()
      print("shouldn't get here!")
   except ValueError:
      print("can't convert that string to an integer!") # This is what should happen
   ```

---

## **Task 3: List Comprehensions Practice**

1. Within the assignment3 folder, create a file called `list-comprehensions.py`. Add code that reads the contents of `../csv/employees.csv` into a list of lists using the csv module.
2. Using a list comprehension, create a list of the employee names, first_name + space + last_name.  The list comprehension should iterate through the items in the list read from the csv file.  Print the resulting list.  Skip the item created for the heading of the csv file.
3. Using a list comprehension, create another list from the previous list of names.  This list should include only those names that contain the letter "e".  Print this list.

---

## **Task 4: Closure Practice**

1. Within the assignment3 folder, create a file called `hangman-closure.py`.
2. Declare a function called `make_hangman()` that has one argument called secret_word.  It should also declare an empty array called guesses.  Within the function declare a function called hangman_closure() that takes one argument, which should be a letter.  Within the inner function, each time it is called, the letter should be appended to the guesses array.  Then the word should be printed out, with underscores substituted for the letters that haven't been guessed.  So, if secret_word is "alphabet", and guesses is ["a", "h"], then "a__ha___" should be printed out.  The function should return `True` if all the letters have been guessed, and `False` otherwise.  `make_hangman()` should return `hangman_closure`.
3. Within hangman-closure.py, implement a hangman game that uses make_hangman().  Use the input() function to prompt for the secret word.  Then use the input() function to prompt for each of the guesses, until the full word is guessed.
4. Test your program by playing a few games.

## **Task 5: Extending a Class**

1. Within the assignment3 folder, create a file called `extend-point-to-vector.py`.
2. Create a class called `Point`.  It represents a point in 2d space, with x and y values passed to the `__init__()` method.  It should include methods for equality, string representation, and Euclidian distance to another point.
3. Create a class called `Vector` which is a subclass of `Point` and uses the same `__init__()` method.  Add a method in the vector class which overrides the string representation so `Vector`s print differently than `Point`s.  Override the `+` operator so that it implements vector addition, summing the `x` and `y` values and returning a new `Vector`.
4. Print results which demonstrate all of the classes and methods which have been implemented.

## **Task 6: More on Classes**

1. Within the assignment3 folder, create a file called `tictactoe.py`.
2. Within this file, declare a class called TictactoeException.  This should inherit from the Exception class.  Add an `__init__` method that stores an instance variable called `message` and then calls the `__init__` method of the superclass.  This is a common way of creating a new type of exception.
3. Declare also a class called Board.  This should have an `__init__` function that only has the `self` argument.  It creates a list of lists, 3x3, all containing " " as a value.  This is stored in the variable self.board_array.  Create instance variables self.turn, which is initialized to "X".  The Board class should have a class variable called valid_moves, with the value. Use exactly as written — the provided `move()` finds moves by their position in this list:
```python
   valid_moves=["upper left", "upper center", "upper right", "middle left", "center", "middle right", "lower left", "lower center", "lower right"]
```
4. Add a `__str__()` method.  This converts the board into a displayable string.  You want it to show the current state of the game. The rows to be displayed are separated by newlines ("\n") and you also want some "|" and "-" characters.  Once you have created this method, you can display the board by doing a `print(board)`.
5. Add a move() method.  This has two arguments, `self` and `move_string`.  The following strings are valid in TicTacToe: "upper left", "upper center", "upper right", "middle left", "center", "middle right", "lower left", "lower center", and "lower right".  When a string is passed, the move() method will check if it is one of these, and if not it will raise a TictactoeException with the message "That's not a valid move.".  Then the move() method will check to see if the space is taken.  If so, it will raise an exception with the message "That spot is taken."  If neither is the case, the move is valid, the corresponding entry in board_array is updated with X or O, and the turn value is changed from X to O or from O to X.
6. Add a whats_next() method.  This will see if the game is over.  If there are 3 X's or 3 O's in a row, it returns a tuple, where the first value is True and the second value is either "X wins!" or "O wins!".  If the board is full but no one has won, it returns a tuple where the first value is True and the second value is "Cat's Game.".  Otherwise, it returns a tuple where the first value is False and the second value is either "X's turn." or "O's turn.".
7. Implement the game within the mainline code of `tictactoe.py`.  At the start of the game, an instance of the board class is created, and then the methods of the board class are used to progress through the game.  Use the `input()` function to prompt for each move, indicating whose turn it is.  Note that you need to call board.move() within a try block, with an except block for TictactoeException.  Give appropriate information to the user.
8. Test your program by playing a few games.

On assembling this program, the assignment author found that it was too time consuming to write some of the methods.  So, here are some pieces to reuse.  Please make sure you understand them. Use as written, or adapt if you prefer.
```python
    def __str__(self):
        lines=[]
        lines.append(f" {self.board_array[0][0]} | {self.board_array[0][1]} | {self.board_array[0][2]} \n")
        lines.append("-----------\n")
        lines.append(f" {self.board_array[1][0]} | {self.board_array[1][1]} | {self.board_array[1][2]} \n")
        lines.append("-----------\n")
        lines.append(f" {self.board_array[2][0]} | {self.board_array[2][1]} | {self.board_array[2][2]} \n")
        return "".join(lines)
    
    def move(self, move_string):
        if not move_string in Board.valid_moves:
            raise TictactoeException("That's not a valid move.")
        move_index = Board.valid_moves.index(move_string)
        row = move_index // 3 # row
        column = move_index % 3 #column
        if self.board_array[row][column] != " ":
            raise TictactoeException("That spot is taken.")
        self.board_array[row][column] = self.turn
        if self.turn == "X":
            self.turn = "O"
        else:
            self.turn = "X"
    
    def whats_next(self):
        cat = True
        for i in range(3):
            for j in range(3):
                if self.board_array[i][j] == " ":
                    cat = False
                else:
                    continue
                break
            else:
                continue
            break
        if (cat):
            return (True, "Cat's Game.")
        win = False
        for i in range(3): # check rows
            if self.board_array[i][0] != " ":
                if self.board_array[i][0] == self.board_array[i][1] and self.board_array[i][1] == self.board_array[i][2]:
                    win = True
                    break
        if not win:
            for i in range(3): # check columns
                if self.board_array[0][i] != " ":
                    if self.board_array[0][i] == self.board_array[1][i] and self.board_array[1][i] == self.board_array[2][i]:
                        win = True
                        break
        if not win:
            if self.board_array[1][1] != " ": # check diagonals
                if self.board_array[0][0] ==  self.board_array[1][1] and self.board_array[2][2] == self.board_array[1][1]:
                    win = True
                if self.board_array[0][2] ==  self.board_array[1][1] and self.board_array[2][0] == self.board_array[1][1]:
                    win = True
        if not win:
            if self.turn == "X": 
                return (False, "X's turn.")
            else:
                return (False, "O's turn.")
        else:
            if self.turn == "O":
                return (True, "X wins!")
            else:
                return (True, "O wins!")
```

---


### **Submit Your Assignment on GitHub**  

📌 **Follow these steps to submit your work:**  

#### **1️⃣ Add, Commit, and Push Your Changes**  
- Within your python_homework folder, do a git add and a git commit for the files you have created, so that they are added to the `assignment3` branch.
- Push that branch to GitHub. 

#### **2️⃣ Create a Pull Request**  
- Log on to your GitHub account.
- Open your `python_homework` repository.
- Select your `assignment3` branch.  It should be one or several commits ahead of your main branch.
- Create a pull request.

#### **3️⃣ Submit Your GitHub Link**  
- Your browser now has the link to your pull request.  Copy that link. 
- Paste the URL into the **assignment submission form**. 

---

<details>
<summary>Rubric (for AirHub reviewer and mentors)</summary>

### Required Deliverables/Tasks

Work is on an `assignment3` git branch; all files live in the `assignment3/` folder; each task is marked with a comment. Six files must be present, named exactly: `log-decorator.py`, `type-decorator.py`, `list-comprehensions.py`, `hangman-closure.py`, `extend-point-to-vector.py`, `tictactoe.py`. `Use exactly as written (later tasks depend on these names)` for the file names. Be lenient about the folder's absolute location and the working directory — the reviewer cannot see the filesystem. There is no PyTest for this assignment; grade against the requirements stated below.

- **Task 1 — `log-decorator.py`** — A `logger_decorator` that wraps a function and logs to `./decorator.log`: the function name (`func.__name__`), the positional parameters (or `"none"`), the keyword parameters (or `"none"`), and the return value. Plus three decorated functions — one takes nothing/returns nothing, one takes `*args`/returns `True`, one takes `**kwargs`/returns `logger_decorator` — all called from the mainline. Running the file produces `decorator.log`. The log-format block and the logging-setup block are `Example — adapt to your own layout`: exact spacing and wording may vary, and logging an empty `[]`/`{}` instead of `"none"` is a minor miss, not a failure.
- **Task 2 — `type-decorator.py`** — A decorator *factory* `type_converter(type_of_output)` with three nested levels (`type_converter` → inner decorator → `wrapper`) that converts the wrapped function's return to the given type. `return_int()` decorated to convert to `str`; `return_string()` decorated to convert to `int`. The mainline verification block is `Use exactly as written (later tasks depend on these names)` — it must catch the `ValueError` from `int("not a number")`. The decorator is named `type_converter`; a student who instead writes `type_decorator` should not be failed on the name alone.
- **Task 3 — `list-comprehensions.py`** — Read `../csv/employees.csv` into a list of lists (csv module), then build two lists using **list comprehensions** (not `for`/`append`): full names as `first_name + " " + last_name` with the header row skipped, then only the names containing `"e"`. Both lists are printed. Expected: 20 names in the first list, 15 in the second. The path `../csv/employees.csv` is `Example — adapt to your own layout` for grading purposes: do NOT fail on working-directory or path-resolution differences — the reviewer cannot verify the filesystem.
- **Task 4 — `hangman-closure.py`** — `make_hangman(secret_word)` holds a `guesses` list in its enclosing scope and returns the inner `hangman_closure(letter)` **function object** (not the result of calling it). Each call appends the letter, prints the word with underscores for unguessed letters, and returns `True` only when no underscores remain. Plus a playable game loop using `input()`. Must be a genuine closure — a global or an extra passed-in argument does not satisfy it. For `secret_word = "alphabet"` with guesses `["a", "h"]`, the correct output is `"a__ha___"` (three trailing underscores).
- **Task 5 — `extend-point-to-vector.py`** — A `Point` class taking `x, y` in `__init__`, with equality (`__eq__`), string representation (`__str__` or `__repr__`), and Euclidean `distance` to another point. A `Vector(Point)` subclass that reuses the parent `__init__` (does not redefine it), overrides `__str__` so Vectors print differently, and overloads `+` via `__add__` to return a **new `Vector`**. The mainline prints results exercising each method for both classes.
- **Task 6 — `tictactoe.py`** — A `TictactoeException(Exception)` that stores `self.message` and calls `super().__init__()` (the main from-scratch piece). A `Board` class with the class variable `valid_moves` (`Use exactly as written (later tasks depend on these names)` — the provided `move()` indexes moves by position in this list), an `__init__` building a 3×3 `self.board_array` of `" "` and `self.turn = "X"`, plus `__str__`, `move()`, and `whats_next()`, and an interactive game loop that calls `board.move()` inside a `try`/`except TictactoeException` and gives the user feedback. The provided reuse block (`__str__`, `move()`, `whats_next()`) is `Use exactly as written` and most students will submit it largely intact. Accept the result strings from the provided reuse code (`"X wins!"`, `"O wins!"`, `"Cat's Game."`, `"X's turn."`, `"O's turn."`). A missing/unused `last_move` is not a defect.
- **Submission** — The `assignment3` branch is pushed to GitHub, a pull request into `main` is opened, and the PR link is submitted.

### Optional Deliverables/Tasks

**None.** The assignment contains no optional, stretch, bonus, or advanced work — every task and the submission steps are required.

</details>
