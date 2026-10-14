# Week 9 Mentor Summary: Introduction to Databases and SQL

**Lesson file:** `lessons/09Introduction to Databases and SQL.md`
**Assignment file:** `assignments/09Introduction to Databases and SQL.md`

**Learning objective:** Students learn the basics of relational databases with Python's built-in `sqlite3` module. They define tables with constraints and foreign keys, insert and query data with SQL, model many-to-many associations with a join table, and load query results into a pandas DataFrame.

---

## Lesson Overview

The lesson has twelve sections. Windows students install `pyreadline3` for the `sqlcommand.py` program, and Mac and Linux students do not need it. Students build two lesson scripts, `school_a.py` and `school_b.py`, that create and populate a small school database. Several sections include a "Check for Understanding" question or an AI practice prompt.

| Section | What students learn |
|---|---|
| **9.1 What SQL Is, and Why it is Used** | Tables, schemas, and SQLite datatypes. Students read an external introduction to primary keys and foreign keys. The lesson then explains one-to-one, one-to-many, and many-to-many associations, join tables, transactions (using a bank transfer as the example), and constraints such as `NOT NULL`, `UNIQUE`, and `FOREIGN KEY`. SQLite does not enforce foreign keys unless they are turned on. |
| **9.2 Creating a New SQLite Database** | `sqlite3.connect()` creates the database file if it does not exist. The connection is used in a `with` block, and the database is stored in `../db/` so that it is not pushed to GitHub. The `with` block does not close the connection, so the example calls `conn.close()` afterward. |
| **9.3 Defining the Database Structure** | `CREATE TABLE IF NOT EXISTS` for `Students`, `Courses`, and `Enrollments`, with `INTEGER PRIMARY KEY`, `NOT NULL UNIQUE`, and `FOREIGN KEY ... REFERENCES`. Statements run with `cursor.execute()`. |
| **9.4 Populating Tables with Data** | `INSERT INTO`, multi-row inserts, `conn.commit()`, and `PRAGMA foreign_keys = 1`. Running the script twice raises `sqlite3.IntegrityError` because names are unique. The fix uses helper functions with `try` / `except` and **parameterized statements** with `?` placeholders. Students install the SQLite Viewer extension for VS Code. |
| **9.5 Writing SQL Queries** | `SELECT`, `WHERE`, `AND`, and `ORDER BY`. Students use `cursor.fetchall()` and learn that each row is a tuple. |
| **9.6 Adding Entries with Foreign Keys** | An `enroll_student()` function looks up the student id and the course id, then inserts the enrollment. Students learn to check for an existing enrollment before inserting, because a `UNIQUE` constraint on one column cannot prevent duplicate pairs. |
| **9.7 More Complicated Queries** | Comparison operators, `LIKE` with `%`, `JOIN ... ON`, table aliases with and without `AS`, `LEFT JOIN`, and a three-table join through `Enrollments`. `RIGHT JOIN` and `FULL JOIN` are mentioned briefly. |
| **9.8 The UPDATE Statement** | `UPDATE ... SET ... WHERE`, and the warning that an `UPDATE` without `WHERE` changes every row. |
| **9.9 The DELETE Statement** | `DELETE FROM ... WHERE`, and the warning that a `DELETE` without `WHERE` removes every row. |
| **9.10 SQL Query Practice** | Students run `load_db.py` from the `python_homework` folder to build `db/lesson.db` (employees, customers, orders, products, and line items). They then practice SQL in the `sqlcommand.py` prompt, where each statement ends with a semicolon. |
| **9.11 SQL from Pandas** | `pd.read_sql_query()` loads the result of a `SELECT` with several joins into a DataFrame. `df.to_sql()` writes a DataFrame (for example, one read from a CSV file) into a table. The lesson explains the `if_exists` options and warns that `"replace"` drops the existing table and its constraints. |
| **9.12 Optional: More Practice** | The SQLBolt tutorial. It is optional but strongly recommended. |

Aggregation and subqueries are covered in the next lesson.

---

## Assignment Deliverables

**Setup:** Students create an `assignment9` branch in `python_homework` and work in the `assignment9` folder. Scripts use the relative paths `../db/magazines.db` and `../db/lesson.db`, so students must run them from inside `assignment9`. The assignment asks students to put every SQL statement inside a `try` / `except` block.

**Testing:** There is no PyTest file. Reviewers read the two scripts and grade their structure and results. Each student creates their own sample data, so exact rows and totals will differ.

**Tasks:**

| Task | File | What it does | Concepts practiced |
|---|---|---|---|
| 1 | `sql_intro.py` | Connects to `../db/magazines.db`, which creates the file, and closes the connection. | `sqlite3.connect()`, `try` / `except` |
| 2 | `sql_intro.py` | Creates `publishers`, `magazines`, `subscribers`, and `subscriptions`. Publisher and magazine names are unique and required. Subscriber name and address are required but not unique. `magazines` has a foreign key to `publishers`, and `subscriptions` is a join table with two foreign keys and a required `expiration_date`. | Schema design, constraints, one-to-many and many-to-many associations |
| 3 | `sql_intro.py` | Turns on foreign keys right after connecting. One function per table adds rows with parameterized queries. Each table gets at least 3 rows, and running the script again must not create duplicates. Subscribers must be checked with a query for an identical name and address before inserting. | `INSERT`, `?` placeholders, `IntegrityError`, duplicate checks, `commit()` |
| 4 | `sql_intro.py` | Prints the rows of three queries: all subscribers, all magazines sorted by name, and the magazines of one publisher using a `JOIN`. | `SELECT`, `ORDER BY`, `JOIN`, `fetchall()` |
| 5 | `sql_intro_2.py`, `order_summary.csv` | Reads a join of `line_items` and `products` from `lesson.db` into a DataFrame with five named columns. It adds a `total` column, groups by `product_id` with `count`, `sum`, and `first` aggregations, sorts by `product_name`, and writes the result to CSV. | `pd.read_sql_query()`, `groupby()`, `agg()`, `sort_values()`, `to_csv()` |
| 6 | Capstone repository | Students continue cleaning their scraped data and import each cleaned CSV into its own table in a SQLite database. This week is graded on progress only. | pandas cleaning, loading data into SQLite |

**Submission:** Students open a pull request from `assignment9` into `main` in `python_homework`. They submit that link and the link to their capstone database pull request.

**Note:** Task 5 uses `groupby()` with `agg()` and the `'first'` aggregation, which the lesson does not teach. Students must remember these from earlier pandas weeks.

---

## FAQs and Common Trouble Spots

**1. "`sqlite3.OperationalError: unable to open database file`."**
SQLite creates the database file but not the folder. The path `../db/` must point to an existing `db` folder in `python_homework`. This error usually means the student is running the script from the wrong folder. Ask them to `cd` into `assignment9` first, or to check that the `db` folder exists.

**2. "I ran my script again and now I have duplicate rows," or "I get `IntegrityError`."**
A `UNIQUE` constraint raises `IntegrityError` on the second run, so students need `try` / `except` around those inserts. Subscribers and subscriptions have no single unique column, so a constraint cannot prevent duplicates there. Students must run a `SELECT` first and insert only when no matching row exists. The lesson shows this pattern for enrollments in section 9.6. If the data becomes messy, students can delete `db/magazines.db` and run the script again.

**3. "My inserts do not show up in the SQLite Viewer."**
Check that the script calls `conn.commit()` and that the student is viewing the same database file the script writes to. The viewer may also need to be closed and opened again to show changes, as the lesson notes. The `with` block commits when it ends without an exception, but explicit `commit()` calls are still good practice. The `with` block does not close the connection.

**4. "My foreign key insert works even though the id does not exist."**
SQLite ignores foreign keys unless `PRAGMA foreign_keys = 1` runs on that connection. The setting does not persist in the database file, so it must run each time the script connects. Students should place it right after the connect statement, as Task 3 requires.

**5. "My `JOIN` returns too many rows, or no rows."**
Students often join on the wrong columns, such as matching a primary key to another primary key. The `ON` clause should match a foreign key to the primary key it refers to, for example a magazine's publisher id to the publisher's id. When two tables both have a `name` column, students must qualify it (for example `magazines.name`) or use aliases. Ask students to start with a join that has no `WHERE` clause, print the rows, and then add the filter.
