# Lesson Plan 01 – Introduction to Python

## About This Week
This week introduces students to **Python** and its role in software development and data work.  
Students learn how to set up Python, understand variables and data types, and begin using basic control flow and functions.  
By the end of the week, they should be able to:  
- Run Python locally in VS Code  
- Use variables, conditionals, and loops  
- Write and call simple functions  

The assignment reinforces these skills through short exercises that build from syntax to problem-solving.

---

## Explore Session (60 minutes)

**Purpose:** Reinforce Python fundamentals and show how to write and run code before students begin the assignment.  
**Materials:** Mentor slides (Explore section), VS Code demo, or Replit alternative.

### Segment 1 – Warm-Up (5 min)
- Ask: “Who’s coded in Python before—or in any other language?”  
- Quick chat/poll: “What do you think Python is used for most—web, data, or AI?”  
*Mentor Tip: Python syntax feels simple, but indentation matters a lot. Model good habits early.*

---

### Segment 2 – I Do (10 min)
Mentor demonstrates basic setup and variables:

    # Bash commands
    python3 --version
    python3 -m venv .venv
    source .venv/bin/activate
    code .

Then open a new `.py` file:

    name = "Jazmine"
    age = 28
    height = 5.8

    print("Hello, my name is", name)

Explain:
- Python uses indentation instead of braces (`{}`).
- Strings, integers, floats, and booleans are core data types.
- `print()` is how we output to the console.

**CFU Questions**
- “What would happen if we forgot the quotation marks?”
- “What’s the difference between a string and a number here?”

*Mentor Tip: Students often forget `()` in `print()`—show that error and how to fix it.*

---

### Segment 3 – We Do (20 min)
Work together on simple examples:

    is_student = True
    balance = 1000.75
    print(type(balance))

Then try conversions:

    num_str = "42"
    num_int = int(num_str)
    print(num_int)

Discuss:
- Type conversion (`int()`, `float()`, `str()`, `bool()`)
- Implicit vs explicit conversion
- The `type()` function

**CFU Questions**
- “What happens if we try to add a string and an integer?”
- “When might we want to convert a number to a string?”

---

### Segment 4 – You Do (20 min)
Students practice individually or in chat:

> “Write a short script that stores your name, age, and whether you’re a student.  
> Then print a sentence using all three pieces of data.”

Mentor walks through one or two submissions and models debugging.

---

### Segment 5 – Wrap-Up (5 min)
- Summarize: variables → types → print → type conversion.  
- Ask: “What’s one difference you noticed between Python and JavaScript?”  
- Preview: Next session, we’ll explore conditionals, loops, and functions.

---

## Apply Session (60 minutes)

**Purpose:** Support students as they practice control flow and functions in Python.  
**Materials:** Mentor slides (Apply section), assignment file, code editor.

### Segment 1 – Quick Recap (5 min)
Ask:  
- “What’s one thing you learned about Python syntax that surprised you?”  
Address common errors (e.g., missing colons or indentation errors).

---

### Segment 2 – Guided Coding (25 min)
Mentor demos simple logic and loops:

    age = 20
    if age >= 18:
        print("You're an adult!")
    elif age >= 13:
        print("You're a teenager.")
    else:
        print("You're a child.")

Then a loop:

    for i in range(3):
        print("Loop iteration:", i)

Explain:
- `if`, `elif`, `else` structure  
- The importance of indentation  
- `range()` and `for` loops  

**CFU Questions**
- “What’s the difference between `for` and `while` loops?”  
- “When would we use `break` or `continue`?”  

*Mentor Tip: Students will often forget indentation inside loops; show how to read the error message.*

---

### Segment 3 – Functions Practice (20 min)
Introduce functions:

    def greet(name="stranger"):
        print("Hello,", name + "!")

Then extend:

    def add(a, b):
        return a + b

Discuss `return` vs `print`, and demonstrate `*args` and `**kwargs` briefly:

    def add_numbers(*args):
        return sum(args)

**CFU Questions**
- “What’s the difference between printing a value and returning one?”
- “What happens if we call a function without required parameters?”

---

### Segment 4 – Wrap-Up and Reflection (10 min)
Discussion or chat:

> “What’s one bug you fixed today?”  
> “What’s one thing you’ll try before the next session?”

Remind students:
- Assignments are due before Monday.  
- Always run your file early to test often and fix small errors first.  
- Share questions with code snippets and error messages in Slack.

*Mentor Tip: Celebrate the moment students see their first successful output—it builds confidence fast.*

---

### Notes for Mentors
- Keep pacing steady; alternate between demo, discussion, and coding.
- Show common beginner errors (indentation, missing `:`, wrong types).
- Encourage experimentation—Python’s REPL makes testing easy.
- Reinforce that debugging is part of learning, not failure.
