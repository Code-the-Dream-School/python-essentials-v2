# Week 10 Mentor Summary: Advanced SQL and Database Integration

**Lesson file:** `lessons/10Advanced SQL and Database Integration.md`
**Assignment file:** `assignments/10Advanced SQL and Database Integration.md`

**Learning objective:** Students extend the SQL skills from Week 9. They learn subqueries, multi-table `JOIN`s, aggregation with `GROUP BY` and `HAVING`, indexes, transactions, parameterized queries, window functions, and date functions. They run all of these from Python with the `sqlite3` module.

---

## Lesson Overview

The lesson starts with a setup script that students run once. The script creates `db/company.db` with a `Departments` table and an `Employees` table and fills them with sample data. Students must create the `db` folder first. The script deletes and rebuilds the database each time it runs, so students can run it again to reset the data. The lesson has ten numbered sections. Sections 10.1, 10.4, and 10.7 end with an AI practice prompt.

| Section | What students learn |
|---|---|
| **10.1 Understanding Subqueries** | A subquery is a query inside another query. The example uses a correlated subquery to find the highest-paid employee in each department. Departments with a salary tie return more than one row. |
| **10.2 Complex JOINs** | Students create and fill a `Projects` table. The insert uses subqueries to look up each `department_id` by department name. They then join `Employees` to `Projects` on `department_id`. The section defines `INNER JOIN` and `LEFT JOIN`. |
| **10.3 Aggregation** | `MIN()`, `MAX()`, and `COUNT()` with `GROUP BY`. Every column in the `SELECT` must be aggregated or appear in the `GROUP BY`. A second example joins `Departments` so that the output shows department names instead of ids. |
| **10.4 Aggregation with HAVING** | `HAVING` filters groups after aggregation, and `WHERE` filters rows before aggregation. One example lists departments with an average salary above 70,000. A second example uses a `WITH` clause (a common table expression) and a `LEFT JOIN` to show each manager's name. |
| **10.5 Performance Optimization: Indexing** | `CREATE INDEX` on a column that is often used in `WHERE`, `JOIN`, or `ORDER BY`. The section has one SQL statement and no Python code. |
| **10.6 Transactions and Rollbacks** | `conn.commit()` saves a group of changes, and `conn.rollback()` inside an `except` block discards them after an error. |
| **10.7 Parameterized Queries** | SQL injection is an attack that changes the structure of a query through user input. The section shows an f-string query and a concatenated query that are vulnerable. It then shows the safe form with a `?` placeholder and a tuple of values. |
| **10.8 Window Functions** | `RANK() OVER (PARTITION BY ... ORDER BY ...)` ranks employees by salary within each department. A second example uses `ROW_NUMBER()` with the same partition. `RANK()` gives tied salaries the same rank, and `ROW_NUMBER()` gives every row a unique number. |
| **10.9 Date and Time Functions** | `JULIANDAY('now') - JULIANDAY(hire_date)` calculates each employee's tenure in days. |
| **10.10 Implementing All Techniques in Python** | General tips: test each query on its own, check intermediate results, and use `try` / `except` for database errors. The example script repeats the `HAVING` query from 10.4. |

Most lesson code blocks run on their own after the setup script. The parameterized query snippets in 10.7 are partial examples, so they do not run unless students add a connection and define the variables they use.

---

## Assignment Deliverables

**Setup:** Students work in `python_homework` on an `assignment10` branch. They write all code in one new file, `assignment10/advanced_sql.py`, and mark each task with a comment. The assignment uses `db/lesson.db` and the `sqlcommand.py` tool from Week 9. Students should run `sqlcommand.py` from the `python_homework` folder and run their own script from the `assignment10` folder. For each task, students first make the SQL statement work in `sqlcommand.py`, and then they add it to `advanced_sql.py`.

**Testing:** There is no PyTest file this week. Students test by running `advanced_sql.py` and checking the printed results. Printing raw row tuples is acceptable for every task.

**Coding tasks:** All five tasks are required.

| Task | What it does | Concepts practiced |
|---|---|---|
| 1. Complex JOINs with Aggregation | Prints the order id and total price of the first 5 orders. The query joins `orders`, `line_items`, and `products`, then groups, sums, sorts, and limits the results. | Multi-table `JOIN`, `GROUP BY`, `SUM()`, `ORDER BY`, `LIMIT` |
| 2. Understanding Subqueries | Prints each customer's name with the average total price of their orders. A subquery calculates each order's total, and the outer query left-joins `customers` to it. The suggested aliases are only a recommendation. Customers with no orders show `None`. | Subquery in `FROM`, column aliases, `LEFT JOIN`, `AVG()` |
| 3. Insert Transaction Based on Data | Creates one order for the customer "Perez and Sons," entered by employee "Miranda Harris," with 10 of each of the 5 least expensive products. Three `SELECT` statements must look up the ids from the data instead of hard-coding them. All inserts happen in one transaction. A final `SELECT` with a `JOIN` prints the new line items. Two lines are required exactly: `PRAGMA foreign_keys = 1` and `RETURNING order_id` (or `cursor.lastrowid`). | Transactions, `INSERT`, `RETURNING`, foreign keys, `JOIN` |
| 4. Aggregation with HAVING | Prints the id, first name, last name, and order count of each employee with more than 5 orders. The filter must be in `HAVING`. | `JOIN`, `GROUP BY`, `COUNT()`, `HAVING` |
| 5. Web Scraping Capstone database | Students finish the capstone database that they started in Week 9. They load each cleaned CSV file into its own table in a SQLite database. This work goes in the capstone repository as a separate pull request. Reviewers check that the database code exists and runs, and they expect partial progress. | Pandas to SQLite, data cleaning |

**Submission:** Students push the `assignment10` branch and open a pull request into `main` in `python_homework`. They paste that link in the first submission field. They paste the capstone database pull request link in the second submission field.

**Note:** The assignment uses several tools that the lesson does not teach. These include `RETURNING`, `cursor.lastrowid`, `PRAGMA foreign_keys`, a subquery in the `FROM` clause (the lesson only shows subqueries in `WHERE` and `VALUES`), and `LIMIT`. The lesson shows only a short transaction example, so Task 3 is usually the hardest task. The assignment itself says that it combines concepts that the lesson presents only briefly.

---

## FAQs and Common Trouble Spots

**1. "I get `no such table: orders`."**
This is almost always a working directory problem. When `sqlite3.connect()` cannot find the file, it creates a new empty database instead of raising an error. The assignment says to run `advanced_sql.py` from the `assignment10` folder, and to connect to `../db/lesson.db`, because `lesson.db` is in `python_homework/db`. Ask the student to check the folder they run from and look for a stray empty `.db` file.

**2. "My Task 4 query returns every employee. Is that wrong?"**
No. In this dataset, every employee has more than 5 orders, so a 20-row result is correct. The important check is that the filter is in `HAVING` and not in `WHERE`. A `WHERE` clause cannot use `COUNT()`, because `WHERE` runs before the rows are grouped.

**3. "Task 3 fails with a foreign key error, or my order has no line items."**
Check three things. First, the `PRAGMA foreign_keys = 1` line must run right after the connection is opened. Second, the student must use the new `order_id` from `RETURNING order_id` or `cursor.lastrowid` in each line item insert. When `RETURNING` is used, the student must fetch the returned row before running the next statement. Third, the ids must come from the three `SELECT` statements and not from typed-in numbers. Also remind students that `sqlcommand.py` commits each statement separately. They should delete their test order in `sqlcommand.py` before they run the script, as the assignment instructs. If the database gets into a bad state, they can run `load_db.py` again to reset it.

**4. "Task 2 shows `None` for some customers."**
This is expected. The `LEFT JOIN` keeps every customer, and customers with no orders have no totals to average. If a student gets an "ambiguous column name" error, the subquery and the outer table both have a `customer_id` column. The suggested `customer_id_b` alias solves this, but any other alias or a table prefix also works.

**5. "Why should I use `?` placeholders if my query works with an f-string?"**
Both versions may return the same rows, but the f-string version lets input change the SQL statement itself. The lesson's `1 OR 1=1` example shows how a value can return every row in the table. Placeholders send the value separately, so the database always treats it as data. This assignment has no user input, but this is a good moment to build the habit for the capstone.
