# Assignment 9 Answer Key: Introduction to Databases and SQL

**Mentor note:** This lesson has **no PyTest file** — everything is verified by reading the student's scripts and, ideally, opening the databases they build. Students write two scripts: `sql_intro.py` (they design and populate a brand-new `magazines.db` from scratch) and `sql_intro_2.py` (a Pandas + SQL summary over the shared `lesson.db`). Because SQL can be phrased many valid ways and each student invents their own sample data, focus on **structure and results**, not exact wording. The reference queries below are one correct form; a student query that returns the right rows is correct even if written differently. All SQL should run inside `try`/`except` blocks (the lesson requires this).

---

## File Setup

- Work is on an `assignment9` branch, submitted as a PR into `main`.
- Files created in `assignment9/`: `sql_intro.py` (Tasks 1–4) and `sql_intro_2.py` (Task 5).
- `sql_intro.py` connects to `../db/magazines.db`; `sql_intro_2.py` reads `../db/lesson.db` and writes `order_summary.csv` into the `assignment9` folder.
- Scripts are meant to be **run from inside the `assignment9` folder** (the `../db/...` paths depend on this).
- `magazines.db` and `order_summary.csv` are generated files and may appear in the PR.
- Task 6 is the web-scraping capstone database step, submitted as a **separate PR** — not assessed here beyond confirming a PR was started (see the note under Task 6).

Check that the PR link works, the branch is correct, both scripts exist, and the queries produce sensible output.

---

## Task 1: Create a New SQLite Database — **Objective**

Connect to `../db/magazines.db` and close the connection, wrapped in `try`/`except`. Running the script should create the file.

```python
import sqlite3

try:
    conn = sqlite3.connect("../db/magazines.db")
    # table creation / inserts go here (Tasks 2 & 3)
    conn.close()
except sqlite3.Error as e:
    print(f"Database error: {e}")
```

- Uses `sqlite3.connect(...)`. Connecting to a file that doesn't exist **creates** it, so simply running the script produces `magazines.db`.
- SQL work is inside a `try` with a matching `except`.

---

## Task 2: Define Database Structure — **Objective**

Design question first: in a one-to-many relationship, **the "many" side holds the foreign key**. So `magazines` has a `publisher_id` foreign key pointing at `publishers`. The `subscriptions` join table has **two** foreign keys — one to `subscribers`, one to `magazines`.

Then create four tables with the right constraints. Student column names may vary; the constraints are what matter.

```python
conn.execute("""
    CREATE TABLE IF NOT EXISTS publishers (
        publisher_id INTEGER PRIMARY KEY,
        name TEXT NOT NULL UNIQUE
    )""")

conn.execute("""
    CREATE TABLE IF NOT EXISTS magazines (
        magazine_id INTEGER PRIMARY KEY,
        name TEXT NOT NULL UNIQUE,
        publisher_id INTEGER NOT NULL,
        FOREIGN KEY (publisher_id) REFERENCES publishers (publisher_id)
    )""")

conn.execute("""
    CREATE TABLE IF NOT EXISTS subscribers (
        subscriber_id INTEGER PRIMARY KEY,
        name TEXT NOT NULL,
        address TEXT NOT NULL
    )""")

conn.execute("""
    CREATE TABLE IF NOT EXISTS subscriptions (
        subscription_id INTEGER PRIMARY KEY,
        subscriber_id INTEGER NOT NULL,
        magazine_id INTEGER NOT NULL,
        expiration_date TEXT NOT NULL,
        FOREIGN KEY (subscriber_id) REFERENCES subscribers (subscriber_id),
        FOREIGN KEY (magazine_id) REFERENCES magazines (magazine_id)
    )""")
```

Key checks:
- `publishers.name` and `magazines.name` are **UNIQUE and NOT NULL**.
- `magazines` holds the foreign key to `publishers` (the FK is on the "many" side).
- `subscribers.name` and `subscribers.address` are **NOT NULL** but **not** unique (duplicate names are allowed).
- `subscriptions` is a proper join table: FKs to both `subscribers` and `magazines`, plus a **NOT NULL** `expiration_date`.

---

## Task 3: Populate Tables with Data — **Objective**

Add `conn.execute("PRAGMA foreign_keys = 1")` right after connecting, then write one insert-function per table and populate each with at least 3 rows. Must `commit`.

```python
conn.execute("PRAGMA foreign_keys = 1")

def add_publisher(name):
    try:
        conn.execute("INSERT INTO publishers (name) VALUES (?)", (name,))
    except sqlite3.IntegrityError:
        pass  # already exists (UNIQUE) — skip silently

def add_subscriber(name, address):
    # name+address aren't UNIQUE, so check for a duplicate pair first
    cur = conn.execute(
        "SELECT 1 FROM subscribers WHERE name = ? AND address = ?", (name, address))
    if cur.fetchone() is None:
        conn.execute(
            "INSERT INTO subscribers (name, address) VALUES (?, ?)", (name, address))

# ... add_magazine(name, publisher_id) and add_subscription(...) similarly ...

add_publisher("Conde Nast")
# ... at least 3 rows per table ...
conn.commit()
```

Key checks:
- `PRAGMA foreign_keys = 1` is present (so bad FKs raise an error).
- Duplicate handling: UNIQUE columns rely on catching `IntegrityError` (or `INSERT OR IGNORE`); the **subscribers** table is the special case — since name/address aren't unique, the student must **query first** to avoid inserting an identical name+address pair.
- Uses **parameterized queries** (`?` placeholders), not string concatenation.
- `conn.commit()` is called, and re-running the script does **not** create duplicates.
- At least 3 rows in each table.

*Note for mentors:* the "check before insert" on subscribers is the one genuinely tricky part — a student who just relies on a UNIQUE constraint there has misread the requirement, because duplicate names with different addresses must be allowed.

---

## Task 4: Write SQL Queries — **Objective**

Three queries, each printed. Data values depend on the student's own inserts, so verify the **shape/intent**, not specific rows.

```python
# 1. All subscribers
rows = conn.execute("SELECT * FROM subscribers").fetchall()

# 2. All magazines sorted by name
rows = conn.execute("SELECT * FROM magazines ORDER BY name").fetchall()

# 3. Magazines for a particular publisher (requires a JOIN)
rows = conn.execute("""
    SELECT magazines.name
    FROM magazines
    JOIN publishers ON magazines.publisher_id = publishers.publisher_id
    WHERE publishers.name = ?
""", ("Conde Nast",)).fetchall()
```

Key checks:
- Query 2 sorts with `ORDER BY name`.
- Query 3 is a real **JOIN** between `magazines` and `publishers` on the FK, filtered to one publisher — not two separate queries stitched together in Python.
- Each result set is iterated and printed.

---

## Task 5: Read Data into a DataFrame — **Objective**

In `sql_intro_2.py`, read a JOIN of `line_items` and `products` from `../db/lesson.db` into a Pandas DataFrame, add a `total` column, group and aggregate by product, sort by name, and write `order_summary.csv`.

```python
import sqlite3
import pandas as pd

conn = sqlite3.connect("../db/lesson.db")

query = """
    SELECT line_item_id, quantity, line_items.product_id, product_name, price
    FROM line_items
    JOIN products ON line_items.product_id = products.product_id
"""
df = pd.read_sql_query(query, conn)
print(df.head())

df['total'] = df['quantity'] * df['price']
print(df.head())

grouped = df.groupby('product_id').agg(
    line_item_id=('line_item_id', 'count'),
    total=('total', 'sum'),
    product_name=('product_name', 'first'),
)
grouped = grouped.sort_values('product_name')
print(grouped.head())

grouped.to_csv("order_summary.csv")
conn.close()
```

Key checks:
- The SQL JOIN is `ON line_items.product_id = products.product_id` and selects exactly the five columns named in the assignment.
- `df['total'] = df['quantity'] * df['price']`.
- `groupby('product_id')` with `agg` applying **`'count'` to line_item_id**, **`'sum'` to total**, **`'first'` to product_name**. (Equivalent `agg` syntaxes — dict form or named-aggregation form — are all fine.)
- Sorted by `product_name` before writing.
- Writes `order_summary.csv` into the `assignment9` folder.

*Verification (run against the class CSVs loaded into sqlite):* the JOIN returns 1,092 rows (a handful of line_items reference product_ids that aren't in `products`, so those drop out — expected). After grouping and sorting by name, the first product is **Ball** (14 orders, total ≈ 1385.80) and other early rows include **Bike** and **Chair**. Exact totals aren't the point; a summary with one row per product, a count column, and a summed total column is what mentors should see.

---

## Task 6: Web Scraping Capstone — Database Step — **Subjective (progress check only)**

This is the first of two weeks on the capstone database work and is submitted as a **separate PR**. Not graded on completeness this week — just confirm the student has started. A reasonable in-progress submission shows:

- Raw data loaded into a Pandas DataFrame and some cleaning/deduplication of missing or malformed rows.
- Each cleaned CSV imported into its **own table** in a SQLite database (e.g. via `df.to_sql(...)`).
- A pull request opened with the initial database code, even if transformations aren't finished.

A weak submission has no SQLite database at all, or dumps everything into one undifferentiated table with no cleaning.

*(Any recorded-video or form-submission steps are not assessed.)*
