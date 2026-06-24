---
marp: true
theme: default
paginate: true
style: |
  section {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-size: 28px;
  }
  h1 {
    color: #1a73e8;
  }
  h2 {
    color: #333;
  }
  code {
    background-color: #f0f0f0;
    padding: 2px 6px;
    border-radius: 4px;
  }
  pre code {
    font-size: 22px;
  }
  .columns {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
  }
---

<!-- _paginate: false -->

# Using These Slides

These slides are written in **Marp** markdown. You can present them two ways:

**In VS Code:**
1. Install the **Marp for VS Code** extension
2. Open this `.md` file
3. Click the Marp preview icon (top right) or use `Marp: Open Preview`
4. To present: `Marp: Export Slide Deck` → HTML or PDF

**In the browser:**
1. Go to [marp.app](https://marp.app/)
2. Paste or upload this file
3. Click **Preview** to present

Feel free to edit these slides to fit your style!

<!-- This first slide is for mentors only — not shown during the session. -->

---

<!-- _paginate: false -->

# Lesson 1: Introduction to Python

**Python Essentials — Code the Dream**

![bg right:35% 80%](https://www.python.org/static/community_logos/python-logo-generic.svg)

---

## Game Plan for Today

| Section | Time | What We'll Do |
|---------|------|---------------|
| Warm-Up | 5 min | Introductions & icebreaker |
| I Do | 10–15 min | Python basics overview |
| We Do | 15–20 min | Build a function together |
| You Do | 5–10 min | Solo practice exercise |
| Wrap-Up | 5 min | Assignment preview & Q&A |

---

## Warm-Up (5 min)

### Let's Get to Know Each Other!

Go around and share:

1. **Your name**
2. **Where you're joining from**
3. **One thing you're excited to build or learn with Python**

<!-- Mentor note: This is the first session — spend a moment making everyone feel welcome. Encourage cameras on if people are comfortable. If the group is large, use the Zoom chat instead. -->

---

## I Do: Why Python?

- One of the most popular languages in the world
- Used in **data science**, **web development**, **automation**, and more
- Clean, readable syntax — designed to be beginner-friendly
- Huge ecosystem of libraries (Pandas, Matplotlib, Selenium...)

> This class focuses on **data engineering** — by the end, you'll build a full data pipeline!

<!-- Mentor note: Keep this brief. The goal is to get students excited, not to lecture about Python's history. -->

---

## I Do: Python vs. Other Languages

```python
# Python
name = "Jazmine"
print(f"Hello, {name}!")
```

```javascript
// JavaScript
let name = "Jazmine";
console.log(`Hello, ${name}!`);
```

**Key differences:**
- No `let`/`const`/`var` — just assign directly
- No semicolons or curly braces
- Indentation defines code blocks (not `{}`)

<!-- Mentor note: Many students may have JS experience. This quick comparison helps them anchor new syntax to something familiar. -->

---

## I Do: Variables & Data Types

```python
name = "Jazmine"       # str (string)
age = 28               # int (integer)
height = 5.8           # float (decimal)
is_student = True      # bool (boolean)
```

Check a variable's type:

```python
print(type(age))  # <class 'int'>
```

**What do you think `type("42")` returns?**

<!-- Mentor note: Pause here. Let students answer in chat or unmute. The answer is <class 'str'> — use this to transition into type conversion. -->

---

## I Do: Type Conversion

Sometimes you need to convert between types:

```python
num_str = "42"
num_int = int(num_str)    # 42 (now an integer)
num_float = float(num_int) # 42.0 (now a float)
back_to_str = str(num_int) # "42" (back to string)
```

**Why does this matter?**

```python
age = 30
# This causes a TypeError:
message = "I am " + age + " years old."
```

```python
# Fix it with str() or an f-string:
message = f"I am {age} years old."
```

---

## I Do: Control Flow

```python
age = 20

if age >= 18:
    print("You're an adult!")
elif age >= 13:
    print("You're a teenager.")
else:
    print("You're a child.")
```

**Notice:**
- Colon `:` before each indented block
- `elif` instead of `else if`
- 4-space indentation is standard

**Quick poll: What prints if `age = 16`?**

<!-- Mentor note: Use a Zoom poll or ask in chat. Answer: "You're a teenager." -->

---

## I Do: Loops

```python
# For loop — iterate over a range
for i in range(3):
    print(i)  # 0, 1, 2

# While loop — repeat while a condition is True
count = 0
while count < 3:
    print(count)
    count += 1
```

**Key detail:** `range(3)` gives `0, 1, 2` — it stops *before* 3.

<!-- Mentor note: This is a common stumbling block. Emphasize that range() is exclusive of the stop value. -->

---

## I Do: Functions

```python
def greet(name):
    return f"Hello, {name}!"

message = greet("Luis")
print(message)  # Hello, Luis!
```

**Key ideas:**
- `def` keyword defines a function
- `return` sends a value back to the caller
- `return` ≠ `print` — this is a common early mistake!

<!-- Mentor note: The distinction between return and print is critical for the assignment. Make sure students understand that PyTest checks return values, not printed output. -->

---

## I Do: Error Handling

```python
try:
    result = 10 / 0
except ZeroDivisionError:
    print("You can't divide by zero!")
```

- `try` block: code that *might* fail
- `except` block: what to do *if* it fails
- Without this, your program crashes!

**The assignment will ask you to catch specific errors.**

<!-- Mentor note: Students will need try/except for Tasks 3, 4, and 5 of the assignment. Reassure them that the lesson covers the patterns they'll need. -->

---

## We Do: Let's Build a Calculator Together

**Goal:** Write a `calc` function that takes two numbers and an operation.

Let's start simple. **What should our function signature look like?**

```python
def calc(a, b, operation="multiply"):
```

**What operations should we support?**

<!-- Mentor note: Ask students to suggest operations. Write them on screen: add, subtract, multiply, divide, modulo, int_divide, power. This maps directly to assignment Task 3. -->

---

## We Do: Building the Calculator

```python
def calc(a, b, operation="multiply"):
    if operation == "add":
        return a + b
    elif operation == "subtract":
        return a - b
    elif operation == "multiply":
        return a * b
    elif operation == "divide":
        return a / b
```

**Let's test it:**

```python
print(calc(10, 5, "add"))       # 15
print(calc(10, 5))              # 50 (default)
```

**What happens if we call `calc(10, 0, "divide")`?**

<!-- Mentor note: Let students predict. Then run it to show the ZeroDivisionError. This sets up the next slide. -->

---

## We Do: Adding Error Handling

```python
def calc(a, b, operation="multiply"):
    try:
        if operation == "add":
            return a + b
        elif operation == "subtract":
            return a - b
        elif operation == "multiply":
            return a * b
        elif operation == "divide":
            return a / b
    except ZeroDivisionError:
        return "You can't divide by 0!"
```

**What about `calc("hello", "world", "multiply")`?**

<!-- Mentor note: Run this to show the TypeError. Ask: "What exception should we catch?" Answer: TypeError. Have a student suggest where to add the except clause. -->

---

## We Do: Handling TypeError

```python
def calc(a, b, operation="multiply"):
    try:
        if operation == "add":
            return a + b
        # ... other operations ...
    except ZeroDivisionError:
        return "You can't divide by 0!"
    except TypeError:
        return "You can't multiply those values!"
```

**Nice! We just built something very close to Task 3.**

What other operations does the assignment ask for?
- `modulo`, `int_divide`, `power`

**Can someone suggest the code for one of these?**

<!-- Mentor note: Invite a student to write or dictate the code for modulo (a % b), int_divide (a // b), or power (a ** b). -->

---

## You Do: Mini Challenge (5 min)

**Write a `greet` function** that:
1. Takes a `name` parameter
2. Returns `"Hello, {name}!"` using an f-string

**Then try this:**
Write a `data_type_conversion` function that:
1. Takes a `value` and a `type_name` (like `"int"`, `"float"`, or `"str"`)
2. Returns the converted value

**Stuck?** Think about: `int()`, `float()`, `str()`

<!-- Mentor note: Set a 5-minute timer. Let students work in silence — don't jump in too early. After time's up, ask if anyone wants to share their solution or ask a question. These map to Tasks 2 and 4. -->

---

## Assignment Preview

This week's assignment has **10 tasks** — here's the structure:

| Tasks | Concepts |
|-------|----------|
| 1–2 | Functions, f-strings, `return` |
| 3 | Calculator with error handling |
| 4 | Type conversion with error handling |
| 5 | Grading system using `*args` |
| 6 | String repetition with `for` loop |
| 7 | Student scores using `**kwargs` |
| 8–10 | String manipulation challenges |

<!-- Mentor note: Reassure students that the tasks build in difficulty. Tasks 1–4 are directly previewed in today's session. -->

---

## Assignment Tips

**Use PyTest to check your work as you go:**
```bash
pytest -v -x assignment1-test.py
```

- `-v` shows which tests pass
- `-x` stops at the first failure (so you can fix one at a time)

**Workflow:**
1. Write the function
2. Test it with a `print()` statement
3. Run PyTest
4. Fix and repeat!

**Remember:** Functions must `return` values, not just `print` them.

---

## Wrap-Up

**Today we covered:**
- Variables, data types, and type conversion
- Control flow (`if`/`elif`/`else`, loops)
- Functions and `return` values
- Error handling with `try`/`except`

**Quick check:** On a scale of 1–5 in the chat, how confident do you feel about writing a Python function?

<!-- Mentor note: Use the responses to gauge comfort. If most students are 1–2, offer extra encouragement and remind them about office hours and Slack. -->

---

## Getting Help This Week

- Post questions in the **#discussion** Slack channel
- Book a **1:1 mentor session** if you get stuck
- **Start early** — the assignment has 10 tasks and they build on each other
- Read the **PyTest output carefully** — it tells you exactly what didn't match

**Next week:** Data Structures & File Handling (lists, tuples, dicts, and reading files!)

---

<!-- _paginate: false -->

# You've Got This! 🐍

**Go build something great this week.**

See you next session!
