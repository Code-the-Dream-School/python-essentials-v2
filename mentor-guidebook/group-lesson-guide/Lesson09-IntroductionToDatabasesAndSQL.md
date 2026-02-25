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

# Lesson 9: Introduction to Databases and SQL

**Python Essentials — Code the Dream**

---

## Game Plan for Today

| Section | Time | What We'll Do |
|---------|------|---------------|
| Warm-Up | 5 min | Design a schema on paper |
| I Do | 10–15 min | SQL basics, CREATE, INSERT, SELECT, JOINs |
| We Do | 15–20 min | Build a school database together |
| You Do | 5–10 min | Write SQL queries |
| Wrap-Up | 5 min | Assignment preview & capstone DB goals |

<!-- Mentor note: This lesson covers a LOT of ground — from creating a database to multi-table JOINs. Move quickly through the I Do and spend time on the We Do, where students build the school database step by step. The assignment (magazines database) follows the exact same pattern. -->

---

## Warm-Up (5 min)

### Design a Schema

**A library has books and members. Members can borrow books.**

**Quick questions:**
1. What tables would you need?
2. What columns would each table have?
3. Can a member borrow multiple books? Can a book be borrowed by multiple members?

**Sketch your answer — share in the chat.**

<!-- Mentor note: Answer: Three tables — books, members, borrowings (the join table). Borrowings has foreign keys to both books and members. This is a many-to-many relationship, same pattern as the Students/Courses/Enrollments example in the lesson. Let students discover the need for a join table. -->

---

## I Do: Why SQL?

**SQL databases store structured, related data in tables.**

| Feature | CSV File | SQL Database |
|---------|----------|-------------|
| Updated in real-time? | No — static snapshot | Yes |
| Relationships? | No | Yes — foreign keys |
| Search/filter? | Pandas (in memory) | SQL queries (efficient) |
| Data integrity? | None | Constraints enforce rules |
| Multiple users? | Conflicts | Transactions keep it safe |

**We'll use SQLite** — a file-based database built into Python:

```python
import sqlite3
conn = sqlite3.connect("school.db")
```

> No server needed. The database is just a file.

---

## I Do: Creating Tables

**`CREATE TABLE` defines the structure:**

```sql
CREATE TABLE IF NOT EXISTS Students (
    student_id INTEGER PRIMARY KEY,
    name TEXT NOT NULL UNIQUE,
    age INTEGER,
    major TEXT
);
```

**Key constraints:**

| Constraint | What It Does |
|-----------|-------------|
| `PRIMARY KEY` | Unique identifier, auto-generated |
| `NOT NULL` | Column must have a value |
| `UNIQUE` | No duplicate values allowed |
| `FOREIGN KEY` | Must reference an existing record |

<!-- Mentor note: The assignment asks students to design a magazines database with publishers, magazines, subscribers, and subscriptions. The constraints here mirror what they'll need: names are NOT NULL + UNIQUE, foreign keys link tables. -->

---

## I Do: Relationships and Join Tables

**One-to-many:** One publisher → many magazines

```sql
CREATE TABLE Magazines (
    magazine_id INTEGER PRIMARY KEY,
    name TEXT NOT NULL UNIQUE,
    publisher_id INTEGER,
    FOREIGN KEY (publisher_id) REFERENCES Publishers(publisher_id)
);
```

**Many-to-many:** Students ↔ Courses (need a **join table**)

```sql
CREATE TABLE Enrollments (
    enrollment_id INTEGER PRIMARY KEY,
    student_id INTEGER,
    course_id INTEGER,
    FOREIGN KEY (student_id) REFERENCES Students(student_id),
    FOREIGN KEY (course_id) REFERENCES Courses(course_id)
);
```

> A join table sits in the middle and holds two foreign keys.

---

## I Do: Inserting Data

**`INSERT INTO` adds records:**

```python
cursor.execute(
    "INSERT INTO Students (name, age, major) VALUES (?, ?, ?)",
    ("Alice", 20, "Computer Science")
)
conn.commit()  # Don't forget to commit!
```

**Key points:**
- Use **parameterized queries** (`?`) — never string concatenation
- `PRIMARY KEY` is auto-generated if you don't specify it
- `conn.commit()` makes the changes permanent
- Without commit, changes are **rolled back** when the connection closes

<!-- Mentor note: Parameterized queries prevent SQL injection — stress this. The assignment uses the same pattern: functions that take a cursor and data, then insert with ? placeholders. -->

---

## I Do: Querying Data — SELECT

**`SELECT` retrieves data:**

```sql
-- All students
SELECT * FROM Students;

-- Specific columns
SELECT name, age FROM Students;

-- With a filter
SELECT * FROM Students WHERE age > 21;

-- Sorted
SELECT * FROM Students ORDER BY name;

-- Combined conditions
SELECT * FROM Students WHERE age > 20 AND major = 'Biology';
```

**In Python:**
```python
cursor.execute("SELECT * FROM Students WHERE name = ?", ("Alice",))
results = cursor.fetchall()
for row in results:
    print(row)  # Each row is a tuple
```

---

## I Do: JOINs — Combining Tables

**JOINs combine related tables:**

```sql
SELECT s.name, c.course_name
FROM Students s
JOIN Enrollments e ON s.student_id = e.student_id
JOIN Courses c ON e.course_id = c.course_id;
```

| Join Type | What It Returns |
|-----------|----------------|
| `JOIN` (inner) | Only matching rows |
| `LEFT JOIN` | All left rows + matches from right |
| `RIGHT JOIN` | All right rows + matches from left |
| `FULL JOIN` | Everything from both tables |

> `LEFT JOIN` is useful when you want ALL records from one table even if there's no match in the other.

<!-- Mentor note: The assignment (Task 4) asks students to JOIN publishers and magazines to find magazines for a specific publisher. The compound JOIN through a join table (like Enrollments) is the pattern they'll use for subscriptions. -->

---

## I Do: UPDATE and DELETE

**Modify existing records:**
```sql
-- Update a student's name and age
UPDATE Students SET name = 'Charles', age = 20
WHERE name = 'Charlie';

-- Raise all salaries by 10%
UPDATE products SET price = price * 1.1;
```

**Remove records:**
```sql
-- Delete products under $1
DELETE FROM products WHERE price < 1.0;
```

**Warning:** Without a `WHERE` clause:
- `UPDATE` changes **every** row
- `DELETE` removes **every** row

<!-- Mentor note: Students don't need UPDATE/DELETE heavily for the assignment, but they should understand them conceptually. The sqlcommand.py tool in the lesson lets them practice these interactively. -->

---

## I Do: SQL from Pandas

**Load SQL query results directly into a DataFrame:**

```python
import pandas as pd
import sqlite3

with sqlite3.connect("lesson.db") as conn:
    sql = """
    SELECT c.customer_name, p.product_name, li.quantity
    FROM customers c
    JOIN orders o ON c.customer_id = o.customer_id
    JOIN line_items li ON o.order_id = li.order_id
    JOIN products p ON li.product_id = p.product_id
    """
    df = pd.read_sql_query(sql, conn)
    print(df.head())
```

> **This is the bridge between SQL and everything you've learned about Pandas.** Query with SQL, analyze with Pandas.

<!-- Mentor note: Assignment Task 5 does exactly this — students query the lesson.db database, load into a DataFrame, add a computed column, groupby, and save to CSV. This shows how SQL and Pandas complement each other. -->

---

## We Do: Build the School Database

**Let's build the Students/Courses/Enrollments schema together.**

**Step 1:** Connect and create tables:

```python
import sqlite3

with sqlite3.connect("school.db") as conn:
    conn.execute("PRAGMA foreign_keys = 1")
    cursor = conn.cursor()

    cursor.execute("""
    CREATE TABLE IF NOT EXISTS Students (
        student_id INTEGER PRIMARY KEY,
        name TEXT NOT NULL UNIQUE,
        age INTEGER,
        major TEXT
    )""")

    cursor.execute("""
    CREATE TABLE IF NOT EXISTS Courses (
        course_id INTEGER PRIMARY KEY,
        course_name TEXT NOT NULL UNIQUE,
        instructor_name TEXT
    )""")
```

**What constraint prevents duplicate student names?**

---

## We Do: Insert Data Safely

**Step 2:** Create a reusable function with error handling:

```python
def add_student(cursor, name, age, major):
    try:
        cursor.execute(
            "INSERT INTO Students (name, age, major) VALUES (?,?,?)",
            (name, age, major)
        )
    except sqlite3.IntegrityError:
        print(f"{name} is already in the database.")

add_student(cursor, "Alice", 20, "Computer Science")
add_student(cursor, "Bob", 22, "History")
add_student(cursor, "Charlie", 19, "Biology")
conn.commit()
```

**Why do we catch `IntegrityError`?**

> The UNIQUE constraint on `name` raises this error if we try to add a duplicate.

<!-- Mentor note: This function pattern is exactly what the assignment (Task 3) needs — students must create functions for adding publishers, magazines, subscribers, and subscriptions, each with appropriate error handling. -->

---

## We Do: Enroll Students in Courses

**Step 3:** Create enrollments using foreign keys:

```python
def enroll_student(cursor, student_name, course_name):
    cursor.execute(
        "SELECT student_id FROM Students WHERE name = ?",
        (student_name,))
    student = cursor.fetchall()

    cursor.execute(
        "SELECT course_id FROM Courses WHERE course_name = ?",
        (course_name,))
    course = cursor.fetchall()

    if student and course:
        cursor.execute(
            "INSERT INTO Enrollments (student_id, course_id) VALUES (?,?)",
            (student[0][0], course[0][0]))

enroll_student(cursor, "Alice", "Math 101")
conn.commit()
```

**Key insight:** We look up the IDs first, then use them as foreign keys.

---

## We Do: Query with JOINs

**Step 4:** Find which students are in which courses:

```python
cursor.execute("""
    SELECT s.name, c.course_name
    FROM Students s
    JOIN Enrollments e ON s.student_id = e.student_id
    JOIN Courses c ON e.course_id = c.course_id
""")

for row in cursor.fetchall():
    print(f"{row[0]} is enrolled in {row[1]}")
```

**Output:**
```
Alice is enrolled in Math 101
Alice is enrolled in Chemistry 101
Bob is enrolled in English 101
...
```

**Discussion:** Why do we need TWO JOINs here?

> Because Students and Courses aren't directly linked — Enrollments sits in the middle.

---

## You Do: SQL Query Challenge (5 min)

**Using the school database we just built, write SQL for:**

1. Find all students majoring in "Biology"
   *Hint:* `SELECT * FROM Students WHERE ...`

2. Find all courses taught by "Dr. Smith"
   *Hint:* `SELECT * FROM Courses WHERE ...`

3. Count how many students are enrolled in each course
   *Hint:* `SELECT c.course_name, COUNT(*) FROM ... GROUP BY ...`

<!-- Mentor note: Set a 5-minute timer. Answers: 1) SELECT * FROM Students WHERE major = 'Biology'; 2) SELECT * FROM Courses WHERE instructor_name = 'Dr. Smith'; 3) SELECT c.course_name, COUNT(*) FROM Enrollments e JOIN Courses c ON e.course_id = c.course_id GROUP BY c.course_name; The GROUP BY in #3 previews Lesson 10 concepts. -->

---

## Assignment Preview

| Task | What You'll Do |
|------|---------------|
| 1 | Create a SQLite database (`magazines.db`) |
| 2 | Design schema: publishers, magazines, subscribers, subscriptions |
| 3 | Write functions to populate all 4 tables |
| 4 | Write SQL queries with JOINs |
| 5 | Load `lesson.db` into Pandas, groupby, save CSV |
| 6 | Store capstone scraping data in SQLite |

<!-- Mentor note: The magazines schema is a great test — it has a one-to-many (publishers → magazines) and a many-to-many (subscribers ↔ magazines via subscriptions). Students need to figure out where the foreign keys go. Task 5 bridges SQL and Pandas — query SQL, analyze in Pandas. -->

---

## Assignment Tips

**Task 2 (schema design):** Think about relationships first:
- Publishers → Magazines: one-to-many (foreign key in `magazines`)
- Subscribers ↔ Magazines: many-to-many (join table = `subscriptions`)

**Task 3 (populating):** Watch for duplicates! The subscriber check is trickier — same name AND same address means duplicate.

**Task 5 (Pandas + SQL):** Use `pd.read_sql_query()` to load, then it's regular Pandas (groupby, agg, sort, to_csv).

**If you mess up the database:** Just delete `magazines.db` and re-run your script.

---

## Wrap-Up

**Today we covered:**
- Why SQL databases vs flat files
- Creating tables with constraints (PRIMARY KEY, NOT NULL, UNIQUE, FOREIGN KEY)
- One-to-many and many-to-many relationships (join tables)
- INSERT with parameterized queries
- SELECT with WHERE, ORDER BY, and JOINs
- UPDATE and DELETE
- Loading SQL results into Pandas DataFrames

**Quick check:** What's the difference between a primary key and a foreign key?

<!-- Mentor note: Answer: A primary key uniquely identifies a record in its own table. A foreign key points to a primary key in another table, creating a relationship between tables. -->

---

## Getting Help This Week

- Install the **SQLite Viewer** VS Code extension to inspect your database
- Use `sqlcommand.py` from the lesson to practice queries interactively
- If your database gets messy, **delete the `.db` file** and re-run
- SQLBolt tutorial (optional but recommended): [sqlbolt.com](https://sqlbolt.com/)
- Post questions in **#discussion** on Slack

**Next week:** Advanced SQL — subqueries, aggregation, window functions, and more!

---

<!-- _paginate: false -->

# Data, Meet Database! 🗄️

**Structured storage for structured insights.**

See you next session!
