# **Lesson 10 — Advanced SQL and Database Integration**

## **Lesson Overview**
**Learning objective:** Students will deepen their understanding of SQL by learning advanced techniques such as subqueries, complex `JOIN`s, aggregation with functions, and using `HAVING` for conditional filtering. This lesson also introduces performance optimization techniques, transactions, parameterized queries, window functions, and more.

**Topics:**
1. Subqueries: Embedding queries within other SQL statements for dynamic filtering or calculations.
2. Complex JOINs: Using INNER and LEFT JOINs across multiple related tables.
3. Aggregation Functions: Using `MIN()`, `MAX()`, `AVG()`, `COUNT()` with `GROUP BY`.
4. Aggregation with HAVING: Filtering grouped results using `HAVING` after aggregation.
5. Performance Optimization: Creating indexes to speed up frequent queries.
6. Transactions and Rollbacks: Ensuring consistency with `BEGIN`, `COMMIT`, and `ROLLBACK`.
7. Parameterized Queries: Preventing SQL injection using safe input handling with placeholders.
8. Window Functions: Applying `RANK()`, `ROW_NUMBER()`, and `OVER()` for row-level analytics.
9. Date and Time Functions: Calculating durations and extracting date components using `JULIANDAY()` and related functions.
10. Python Integration: Writing and executing SQL queries within Python scripts using `sqlite3`.

---

## **Create the database for the lesson examples**

Run this code in your lesson working area to populate a database which will be used to run the examples.

```python
import sqlite3
import os

# Note, you need to create a 'db' directory if it isn't already in your workspace
DB_PATH = "db/company.db"

# Start fresh so results are predictable when re-running this script
if os.path.exists(DB_PATH):
    os.remove(DB_PATH)

conn = sqlite3.connect(DB_PATH)
cur = conn.cursor()
cur.execute("PRAGMA foreign_keys = ON;")

# --- Schema ---
cur.executescript("""
CREATE TABLE Departments (
  department_id   INTEGER PRIMARY KEY,
  department_name TEXT NOT NULL UNIQUE,
  manager_id      INTEGER  -- (left without FK to avoid circular reference)
);
CREATE TABLE Employees (
  employee_id   INTEGER PRIMARY KEY,
  first_name    TEXT NOT NULL,
  last_name     TEXT NOT NULL,
  department_id INTEGER NOT NULL,
  title         TEXT NOT NULL,
  salary        INTEGER NOT NULL,
  hire_date     TEXT DEFAULT (date('now')),
  FOREIGN KEY (department_id) REFERENCES Departments(department_id)
);
""")

# --- Seed data ---
cur.executemany(
    "INSERT INTO Departments(department_id, department_name) VALUES (?, ?);",
    [
        (10, "Engineering"),
        (20, "Sales"),
        (30, "HR"),
        (40, "Finance"),
        (50, "R&D"),
    ],
)

employees = [
    # Engineering (dept 10)
    (1, "Alice", "Nguyen", 10, "Software Engineer",        120000, "2019-05-01"),
    (2, "Bob",   "Smith",  10, "Senior Software Engineer", 135000, "2018-07-15"),
    (3, "Carol", "Zhang",  10, "Staff Engineer",           135000, "2017-03-20"),  # tie
    (4, "David", "Lee",    10, "QA Engineer",               95000, "2021-11-02"),

    # Sales (dept 20)
    (5, "Eve",   "Martinez", 20, "Sales Associate",         90000, "2020-01-10"),
    (6, "Frank", "O'Connor", 20, "Account Executive",      110000, "2016-09-29"),
    (7, "Grace", "Kim",      20, "Sales Manager",          105000, "2015-04-12"),

    # HR (dept 30) -> avg ~68.5k (so it will be filtered out by HAVING > 70000)
    (8, "Heidi", "Brown", 30, "HR Generalist",              65000, "2022-06-03"),
    (9, "Ivan",  "Garcia", 30, "HR Manager",                72000, "2019-08-21"),

    # Finance (dept 40)
    (10, "Judy", "Wilson", 40, "Financial Analyst",        125000, "2017-02-17"),
    (11, "Karl", "Davis",  40, "Finance Director",         130000, "2014-12-09"),

    # R&D (dept 50)
    (12, "Liam", "Patel",  50, "Research Scientist",       150000, "2018-10-31"),
    (13, "Mia",  "Chen",   50, "Principal Scientist",      150000, "2013-05-07"),   # tie
]

cur.executemany(
    """INSERT INTO Employees
       (employee_id, first_name, last_name, department_id, title, salary, hire_date)
       VALUES (?, ?, ?, ?, ?, ?, ?);""",
    employees,
)

# Assign managers by employee_id for each department
cur.executemany(
    "UPDATE Departments SET manager_id = ? WHERE department_id = ?;",
    [
        (2, 10),   # Engineering -> Bob Smith
        (7, 20),   # Sales -> Grace Kim
        (9, 30),   # HR -> Ivan Garcia
        (11, 40),  # Finance -> Karl Davis
        (13, 50),  # R&D -> Mia Chen
    ],
)

conn.commit()
conn.close()
print("company.db created with Departments(department_name, manager_id) and Employees.")
```

## **10.1 Understanding Subqueries**

### **Overview**
Subqueries are nested SQL queries used to perform intermediate calculations or selections before the main query executes.

### **Key Concepts:**
- Use subqueries to fetch results dynamically within another query.
- Common use cases include finding maximum, minimum, or aggregated values.

### **Example:**
This SQL code will find the highest-paid employee in each department using a subquery.
```sql
SELECT department_id, employee_id, salary
FROM Employees AS e
WHERE salary = (
    SELECT MAX(salary)
    FROM Employees
    WHERE department_id = e.department_id
);
```

### **Implementation in Python:**
```python
# Example using SQLite
conn = sqlite3.connect("company.db")
cursor = conn.cursor()

# Execute the query
query = """
SELECT department_id, employee_id, salary
FROM Employees AS e
WHERE salary = (
    SELECT MAX(salary)
    FROM Employees
    WHERE department_id = e.department_id
);
"""
cursor.execute(query)
print(cursor.fetchall())

conn.close()
```

### AI Learning Prompt: Retrieval Practice

Now that you've explored subqueries and how they allow for dynamic filtering, let's reinforce your understanding:

1. Open your preferred AI chatbot (ChatGPT, Claude, Microsoft Copilot, etc.).
2. Explain in your own words what a subquery is and why it is useful for performing intermediate calculations before a main query executes.
3. Provide a real-world example of when you might use a subquery, such as finding the highest-paid employee within a specific department.
4. Ask the AI to give you feedback on your explanation.

> **Example prompt:** "I just learned about SQL subqueries. Here is my understanding of how they work: [your explanation]. I think a good use case is [your example]. Can you tell me what I got right and where I could improve my technical explanation?"

---

## **10.2 Complex JOINs**

### **Overview**
Complex `JOIN`s allow you to retrieve data from multiple related tables.

### **Steps:**
1. Create a `Projects` table and insert sample data.
2. Use a `JOIN` to combine employee and project information through the department field.

### **Key Concepts:**
- `INNER JOIN`: Retrieves records with matching values in both tables.
- `LEFT JOIN`: Retrieves all records from the left table and matching records from the right.

### **Python Example: Create and Populate a Projects Table**
```python
conn = sqlite3.connect("db/company.db")
cursor = conn.cursor()

# create Projects table

creation = """
CREATE TABLE Projects (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    department_id INTEGER NOT NULL,
    FOREIGN KEY (department_id) REFERENCES Departments(department_id)
);
"""
cursor.execute(creation)

# add department_ids
insertion = """
INSERT INTO Projects (name, department_id) VALUES
('Project A', (SELECT department_id FROM Departments WHERE department_name = 'HR')),
('Project B', (SELECT department_id FROM Departments WHERE department_name = 'Engineering')),
('Project C', (SELECT department_id FROM Departments WHERE department_name = 'Finance'));
"""
cursor.execute(insertion)

conn.commit()
conn.close()
```

### **Python Example: Complex JOIN**

```python
# List employees working in departments responsible for a specific project:
conn = sqlite3.connect("db/company.db")
cursor = conn.cursor()

# Execute the query - try it with all the projects
query = """
SELECT (e.first_name || ' ' || e.last_name) AS employee_name,
       p.name AS project_name
FROM Employees AS e
JOIN Projects  AS p ON e.department_id = p.department_id
WHERE p.name = 'Project A';
"""
cursor.execute(query)
print(cursor.fetchall())

conn.close()
```

---

## **10.3 Aggregation**

### **Overview**
Aggregation functions like `MIN()`, `MAX()`, `COUNT()`, and `AVG()` allow you to summarize data across groups.

### **Task:**
Aggregation: Calculate the minimum and maximum salaries and the number of employees in each department.

### **Python Example:**
```python
conn = sqlite3.connect("db/company.db")
cursor = conn.cursor()

query = """
SELECT department_id, 
       MIN(salary) AS min_salary, 
       MAX(salary) AS max_salary, 
       COUNT(employee_id) AS num_employees
FROM Employees
GROUP BY department_id;
"""
cursor.execute(query)
print(cursor.fetchall())

conn.close()
```

### **Key Notes:**
- Use `GROUP BY` to organize results by department.
- Ensure fields in the `SELECT` clause are either aggregated or part of the `GROUP BY`.

### **Python example which uses Inner Join so department name is used:**

```python
"""
SELECT d.department_name AS department,
       MIN(e.salary)  AS min_salary,
       MAX(e.salary)  AS max_salary,
       COUNT(e.employee_id) AS num_employees
FROM Employees AS e
JOIN Departments AS d
  ON e.department_id = d.department_id
GROUP BY d.department_id, d.department_name
ORDER BY d.department_name;
"""
cursor.execute(query)
print(cursor.fetchall())

conn.close()
```

---

## **10.4 Aggregation with HAVING**

### **Overview**
The `HAVING` clause filters aggregated results after the `GROUP BY` operation.

### **Task:**
List all departments where the average salary exceeds 70,000 and display the department manager.

### **Python Example:**
```python
query = """
SELECT d.department_name, 
       d.manager_id, 
       AVG(e.salary) AS avg_salary
FROM Departments AS d
JOIN Employees AS e ON d.department_id = e.department_id
GROUP BY d.department_id
HAVING AVG(e.salary) > 70000;
"""
cursor.execute(query)
print(cursor.fetchall())

conn.close()
```

### **Example which lists the manager by name**
```python
# This version reports the manager's name instead of id 
query = """
WITH dept_avg AS (
  SELECT e.department_id, AVG(e.salary) AS avg_salary
  FROM Employees AS e
  GROUP BY e.department_id
)
SELECT d.department_name,
       (m.first_name || ' ' || m.last_name) AS manager_name,
       da.avg_salary
FROM dept_avg AS da
JOIN Departments AS d
  ON d.department_id = da.department_id
LEFT JOIN Employees AS m
  ON m.employee_id = d.manager_id
WHERE da.avg_salary > 70000;
"""
cursor.execute(query)
print(cursor.fetchall())

conn.close()
```

### **Key Notes:**
- Use `HAVING` instead of `WHERE` to filter aggregated results.
- Combine `JOIN`s and `GROUP BY` for advanced aggregations.

### AI Learning Prompt: Predict-then-Check

Let's test your understanding of the difference between the `WHERE` clause and the `HAVING` clause. Study this code without running it:

```sql
SELECT department_id, AVG(salary) as avg_salary
FROM employees
GROUP BY department_id
HAVING avg_salary > 70000;
```

Before running it:
1. Predict what this query will return compared to a query that uses `WHERE salary > 70000`.
2. Explain to an AI chatbot why the `HAVING` clause is necessary here instead of `WHERE`.
3. Ask: "Is my understanding of the SQL execution order—specifically why aggregation happens before HAVING—correct?".
4. Run the code and see if your prediction about the filtered groups was right.

> **Example prompt:** "Looking at this SQL code: [paste code]. I predict it will return [your prediction] because the `HAVING` clause filters [your reasoning]. Am I correct? If not, what am I misunderstanding about how `HAVING` filters aggregated data?"

---

## **10.5 Performance Optimization: Indexing**

### **Overview**
SQL queries can sometimes be slow if they involve large tables. Indexes can be created on columns that are frequently used in `WHERE`, `JOIN`, or `ORDER BY` clauses to speed up query performance.

### **SQL Example:**
```sql
CREATE INDEX idx_department ON Employees(department_id);
```

This creates an index on the `department_id` column to speed up queries that filter by department.

---

## **10.6 Transactions and Rollbacks**

### **Overview**
Transactions ensure that multiple database operations are completed successfully before committing them. If an error occurs, you can roll back the changes to keep the database in a consistent state.

### **Example:**
```python
# Start a transaction
conn = sqlite3.connect("company.db")
cursor = conn.cursor()

try:
    cursor.execute("INSERT INTO Employees (name, department_id) VALUES ('John Doe', 2)")
    cursor.execute("INSERT INTO Employees (name, department_id) VALUES ('Jane Smith', 3)")
    conn.commit()  # Commit transaction
except Exception as e:
    conn.rollback()  # Rollback transaction if there's an error
    print("Error:", e)

conn.close()
```

---

## **10.7 Parameterized Queries to Prevent SQL Injection**

### **Overview**
SQL injection can be prevented by using parameterized queries, ensuring that user input is treated safely.

### **Example:**
```python
cursor.execute("SELECT * FROM Employees WHERE department_id = ?;", (department_id,))
```

This ensures that `department_id` is treated as a parameter and not part of the SQL statement itself.  If any part of the SQL statement comes from the end user or other untrusted source, always put that part in a parameter so that it can be sanitized to strip out rogue SQL.  Don't do it like this:

```python
cursor.execute(f"SELECT * FROM Employees WHERE department_id = {department_id};")
# or, equally bad:
cursor.execute("SELECT * FROM Employees WHERE department_id = " + department_id + ";")
```

### AI Learning Prompt: Retrieval Practice

Security is a critical part of database integration. Let's practice explaining SQL Injection and how to prevent it:

1. Open your AI chatbot.
2. Explain what a parameterized query is and why using placeholders (like ?) is safer than using f-strings or string concatenation to build a query.
3. Ask the AI: "Can you explain a scenario where failing to use a parameterized query could allow a 'rogue SQL' statement to harm a database?".

> **Example prompt:** "I am learning about preventing SQL injection in Python. I think parameterized queries work by [your explanation]. Is this correct? Also, can you show me a simple example of 'unsafe' code versus 'safe' code using placeholders?"

---

## **10.8 Window Functions**

### **Overview**
SQL window functions allow for advanced analysis over a specified range of rows. For example, calculating the rank of employees within a department based on salary.

### **Python Example:**
```python
conn = sqlite3.connect("db/company.db")
cursor = conn.cursor()

query = """
SELECT (e.first_name || ' ' || e.last_name) AS employee_name,
       e.salary,
       e.department_id,
       RANK() OVER (PARTITION BY e.department_id ORDER BY e.salary DESC) AS rank
FROM Employees AS e;
"""
cursor.execute(query)
print(cursor.fetchall())

conn.close()
```

---

## **10.9 Date and Time Functions**

### **Overview**
SQL provides functions for manipulating and querying date and time data, which are useful when working with time-based analysis.

### **Python Example:**
```python
conn = sqlite3.connect("db/company.db")
cursor = conn.cursor()

query = """
SELECT (e.first_name || ' ' || e.last_name) AS employee_name,
       e.hire_date,
       ROUND(JULIANDAY('now') - JULIANDAY(e.hire_date), 2) AS tenure_in_days
FROM Employees AS e;
"""
cursor.execute(query)
print(cursor.fetchall())

conn.close()
```

---

## **10.10 Implementing All Techniques in Python**

### **Implementation Tips**

1. **Test Queries Separately:**
   - Write and test each query independently in your script or database interface before integrating into Python.

2. **Iterative Debugging:**
   - Verify intermediate outputs (e.g., after `JOIN` or subquery execution).

3. **Error Handling in Python:**
   - Use `try-except` blocks to handle database connection errors.

### **Example Python Script:**
```python
conn = sqlite3.connect("db/company.db")
cursor = conn.cursor()

# Aggregation with HAVING
query = """
SELECT d.department_name, 
       d.manager_id, 
       AVG(e.salary) AS avg_salary
FROM Departments AS d
JOIN Employees AS e ON d.department_id = e.department_id
GROUP BY d.department_id
HAVING AVG(e.salary) > 70000;
"""

# Execute and fetch results
cursor.execute(query)
results = cursor.fetchall()
print(results)

conn.close()
```

---

## **Summary**

In this lesson, you’ve learned:
1. How to use subqueries to dynamically fetch values for other queries.
2. How to use complex `JOIN`s to integrate data from multiple tables.
3. How to use aggregation functions to summarize data across groups.
4. How to apply `HAVING` for conditional filtering on aggregated data.
5. How to optimize performance with indexes.
6. How to use transactions and rollbacks to ensure data consistency.
7. How to prevent SQL injection with parameterized queries.
8. How to use window functions for advanced analytics.
9. How to handle date and time data for time-based analysis.

For further exploration, refer to the [SQLite Documentation](https://www.sqlite.org/docs.html) and Python's `sqlite3` library documentation.
