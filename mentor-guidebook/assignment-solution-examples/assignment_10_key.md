# Assignment 10 Answer Key: Advanced SQL and Database Integration

**Mentor note:** This lesson has **no PyTest file** — you review by reading the SQL and the Python-DB integration code, and by confirming the printed results match. Students first build each query in the `sqlcommand.py` shell, then embed it in a script called `advanced_sql.py`. The database is SQLite (`db/lesson.db`), built from the CSVs by `load_db.py`. SQL has many valid phrasings, so judge on the **result and the technique used** (correct joins, GROUP BY, subquery, HAVING), not on exact wording. Expected results below were verified by running each query against the actual data.

---

## File Setup

- Work is on an `assignment10` branch in the student's `python_homework` fork, submitted as a PR into `main`.
- All Task 1–4 code lives in one new file: `assignment10/advanced_sql.py`.
- The script opens `db/lesson.db`, runs each statement, prints results, and closes the connection. Because of the relative path to `db/lesson.db`, the script is run **from the `assignment10` folder** (`../db/lesson.db`) or the `python_homework` root depending on how the student wrote the path — check the connect string matches where they run it.
- Each task marked with a comment line (`# Task 1`, etc.).
- Task 5 is the capstone database step, submitted as a **separate PR** whose link goes in the second submission field.
- There is no test file, so a working PR shows the printed output or the student describes it.

**Database schema (for reference):**
- `customers(customer_id, customer_name, contact, street, city, country, postal_code, phone)`
- `employees(employee_id, first_name, last_name, phone)`
- `products(product_id, product_name, price)`
- `orders(order_id, date, customer_id, employee_id)`
- `line_items(line_item_id, order_id, product_id, quantity)`

An order's total price = sum over its line_items of `product.price * line_item.quantity`.

---

## Task 1: Complex JOINs with Aggregation — **Objective**

Total price of each of the first 5 orders. Join `orders` → `line_items` → `products`, group by `order_id`, sum `price * quantity`.

```sql
SELECT o.order_id, SUM(p.price * li.quantity) AS total_price
FROM orders o
JOIN line_items li ON o.order_id = li.order_id
JOIN products p ON li.product_id = p.product_id
GROUP BY o.order_id
ORDER BY o.order_id
LIMIT 5;
```

Reference Python:

```python
import sqlite3

# Task 1
conn = sqlite3.connect("../db/lesson.db")
cursor = conn.cursor()
cursor.execute("""
    SELECT o.order_id, SUM(p.price * li.quantity) AS total_price
    FROM orders o
    JOIN line_items li ON o.order_id = li.order_id
    JOIN products p ON li.product_id = p.product_id
    GROUP BY o.order_id
    ORDER BY o.order_id
    LIMIT 5;
""")
for order_id, total in cursor.fetchall():
    print(order_id, total)
conn.close()
```

- Key checks: two joins (line_items and products), `GROUP BY order_id`, `SUM(price * quantity)`, no subquery needed.
- **Expected result** (order_id, total): `1 → 513.5`, `2 → 38.24`, `3 → 242.85`, `4 → 793.33`, `5 → 271.84`. (Small floating-point tails like `242.85000...` are normal.)

---

## Task 2: Understanding Subqueries — **Objective**

For each customer, the average of their per-order totals. The per-order total is a subquery aliased `total_price`, with the customer id aliased `customer_id_b` to avoid a name collision; the outer query left-joins `customers` to it and averages.

```sql
SELECT c.customer_name, AVG(sub.total_price) AS average_total_price
FROM customers c
LEFT JOIN (
    SELECT o.customer_id AS customer_id_b,
           SUM(p.price * li.quantity) AS total_price
    FROM orders o
    JOIN line_items li ON o.order_id = li.order_id
    JOIN products p ON li.product_id = p.product_id
    GROUP BY o.order_id
) AS sub ON c.customer_id = sub.customer_id_b
GROUP BY c.customer_id
ORDER BY c.customer_id;
```

- Key checks: the subquery produces one row **per order** (total_price) with `customer_id_b`; the outer query does a **LEFT JOIN** and `GROUP BY customer_id`, then `AVG(total_price)`. The LEFT JOIN is what keeps customers who have no orders (they show `None`/NULL).
- **Expected shape:** 100 rows (one per customer). Spot-check values: `Short, Taylor and Brown → 430.15`, `Glover-Hernandez → 493.46`, `Patterson-Smith → None` (a customer with no orders). Averaging happens across that customer's orders, not across all line items.
- Common mistake: averaging `price * quantity` directly (that gives the mean line-item value, not the mean order total) — the per-order grouping in the subquery is the whole point.

---

## Task 3: An Insert Transaction Based on Data — **Objective**

Create a new order for **Perez and Sons**, employee **Miranda Harris**, 10 of each of the 5 cheapest products, all in one transaction, then print the order's line items joined to product names.

Reference Python (this is the important part — the transaction, `RETURNING`, and the foreign-key pragma):

```python
# Task 3
conn = sqlite3.connect("../db/lesson.db")
conn.execute("PRAGMA foreign_keys = 1")
cursor = conn.cursor()

customer_id = cursor.execute(
    "SELECT customer_id FROM customers WHERE customer_name = 'Perez and Sons'"
).fetchone()[0]
employee_id = cursor.execute(
    "SELECT employee_id FROM employees WHERE first_name = 'Miranda' AND last_name = 'Harris'"
).fetchone()[0]
product_ids = cursor.execute(
    "SELECT product_id FROM products ORDER BY price ASC LIMIT 5"
).fetchall()

try:
    order_id = cursor.execute(
        "INSERT INTO orders (customer_id, employee_id, date) VALUES (?, ?, ?) RETURNING order_id",
        (customer_id, employee_id, "2024-01-01"),
    ).fetchone()[0]
    for (pid,) in product_ids:
        cursor.execute(
            "INSERT INTO line_items (order_id, product_id, quantity) VALUES (?, ?, ?)",
            (order_id, pid, 10),
        )
    conn.commit()
except Exception as e:
    conn.rollback()
    print("Transaction failed:", e)

cursor.execute("""
    SELECT li.line_item_id, li.quantity, p.product_name
    FROM line_items li
    JOIN products p ON li.product_id = p.product_id
    WHERE li.order_id = ?
""", (order_id,))
for row in cursor.fetchall():
    print(row)
conn.close()
```

- Key checks:
  - Three SELECTs pull the `customer_id`, `employee_id`, and the 5 product_ids **from the data** (not hard-coded numbers).
  - The order INSERT omits the primary key and uses `RETURNING order_id` to capture the new id; that id is reused in all 5 line-item inserts.
  - All inserts are in **one transaction** — a single `commit()` at the end (and ideally a `rollback()` on error). `PRAGMA foreign_keys = 1` is set right after connecting.
- **Expected values:** Perez and Sons = `customer_id 16`; Miranda Harris = `employee_id 7`; 5 cheapest products = Bike (0.12), Shirt (0.23), Salad (0.53), Unbranded Wooden Keyboard (0.83), Practical Tuna (1.00). The final SELECT prints 5 rows, each with quantity 10 and one of those product names.
- Note: `RETURNING` needs SQLite 3.35+ (fine on current Python builds). A student who instead used `cursor.lastrowid` to get the order id is also correct.
- The new `order_id` will vary (it's the next available id, e.g. 101), so don't check for a specific value there.

---

## Task 4: Aggregation with HAVING — **Objective**

Employees associated with more than 5 orders: join `employees` and `orders`, group by employee, count orders, filter with `HAVING`.

```sql
SELECT e.employee_id, e.first_name, e.last_name, COUNT(o.order_id) AS order_count
FROM employees e
JOIN orders o ON e.employee_id = o.employee_id
GROUP BY e.employee_id
HAVING COUNT(o.order_id) > 5
ORDER BY order_count DESC;
```

- Key checks: `JOIN` on employee_id, `GROUP BY` employee, `COUNT(...)`, and the filter in **HAVING** (not WHERE — the count is an aggregate). Prints employee_id, first_name, last_name, and the count.
- **Expected result:** in this dataset **all 20 employees** qualify (order counts range from 8 to 18), so the query returns 20 rows. That's fine — the point is demonstrating HAVING, and here nothing happens to fall below the threshold. Top rows: Matthew Meyers (18), Logan Lopez (16), Gregory Pittman (16). A student can confirm HAVING works by raising the threshold (e.g. `> 15`) and seeing fewer rows.

---

## Task 5: Web Scraping Capstone — Database Step — **Subjective**

Load the capstone's cleaned CSV files into a SQLite database, one table per CSV. Submitted as a separate PR (link in the second submission field). This is graded against the project rubric, not a fixed answer.

- A strong submission: loads raw data into a **Pandas DataFrame**, cleans it (missing/duplicate/malformed rows handled), applies transformations/groupings/filters, shows **before/after** of the cleaning, and writes the result to a **SQLite** database (each CSV → its own table, e.g. via `df.to_sql(...)` as in `load_db.py`).
- A weak submission dumps CSVs into tables with no cleaning step, shows no before/after evidence, or skips the DataFrame stage entirely.
- Since the project runs until the final week, partial progress is expected here — check that the database code exists and runs, not that the whole capstone is finished.

---

## General review notes

- **This is SQLite-specific.** `PRAGMA foreign_keys`, `RETURNING`, and `AUTOINCREMENT`-style implicit primary keys behave differently in Postgres/MySQL. If a student targeted another engine, the join/aggregation logic still applies but the transaction syntax will differ.
- Accept any correct join order and any equivalent aggregate phrasing — verify by the printed result.
- Floating-point totals may show long decimal tails; that is not an error. Rounding with `ROUND(..., 2)` is a fine optional touch.
