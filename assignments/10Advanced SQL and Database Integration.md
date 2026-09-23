
# **Assignment 10: Advanced SQL and Database Integration**

---

## **Lesson Overview**

**Learning Objective**:  
Students will deepen their understanding of SQL by learning advanced techniques such as subqueries, complex `JOIN`s, aggregation with functions, and using the `HAVING` clause for conditional filtering.

---

## **Assignment Instructions**

You create the code for this assignment in your python_homework/assignment10 folder.  You may want to have two VSCode terminal sessions.  In one, you have changed directories to `assignment10`.  This is the session where you will run your code.  In the other terminal session, you will run `sqlcommand.py` from the `python_homework` folder.  You need to have the working directory set differently, so that each program will be able to find `db/lesson.db`.  Be sure to create an `assignment10` git branch before you start.  As usual, mark the code that completes each task with a comment line.

### **Preparation and Practice**

You have already experimented with the `sqlcommand.py` program.  Run it again, and practice what you've learned.  You can do SELECT, INSERT, UPDATE and DELETE.  The SELECT statements can have JOIN, GROUP BY, ORDER BY, subqueries, HAVING, etc.  Practice these until you feel familiar with them.

For each of the following tasks, you first use the sqlcommand command line to get the right SQL statement.  Then you add it to your program.

**Help Available!**  

This lesson combines concepts that have been presented only briefly.  If you get stuck, 1:1 mentors are available to answer your questions.  Appointments are available in the [1:1 Mentor Table](https://airtable.com/appoSRJMlXH9KvE6w/shrQinGb1phZYwdiL)

---

### **Task 1: Complex JOINs with Aggregation**

1. **Problem Statement**:  
   Find the total price of each of the first 5 orders.  There are several steps.  You need to join the orders table with the line_items table and the products table.  You need to GROUP BY the order_id.  You need to select the order_id and the SUM of the product price times the line_item quantity.  Then, you ORDER BY order_id and LIMIT 5.  You don't need a subquery. Print out the order_id and the total price for each of the rows returned.

2. **Deliverable**: 
   - Within the python_homework folder, create an `assignment10` branch.  Change to the `assignment10` folder.
   - Get the SQL statement working in sqlcommand.
   - Within the `assignment10` folder, create `advanced_sql.py`. This should open the database, issue the SQL statement, print out the result, and close the database.
   - test your program.

---

### **Task 2: Understanding Subqueries**

1. **Problem Statement**:  
   For each customer, find the average price of their orders.  This can be done with a subquery. You compute the price of each order as in Task 1, but you return the customer_id and the total_price.  That's the subquery. You need to return the total price using `AS total_price`, and you need to return the customer_id with `AS customer_id_b`, for reasons that will be clear in a moment.  In your main statement, you left join the customer table with the results of the subquery, using `ON customer_id = customer_id_b`.  You aliased the customer_id column in the subquery so that the column names wouldn't collide.  Then group by customer_id -- this `GROUP BY` comes *after* the subquery -- and get the average of the total price of the customer orders.  Return the customer name and the average_total_price.

2. **Deliverable**:  
   - Again, get the SQL statement working in sqlcommand.
   - Add code to `advanced_sql.py` to print out the result.

---

### **Task 3: An Insert Transaction Based on Data**

1. **Problem Statement**:  
   You want to create a new order for the customer named Perez and Sons.  The employee creating the order is Miranda Harris.  The customer wants 10 of each of the 5 least expensive products.  You first need to do a SELECT statement to retrieve the customer_id, another to retrieve the product_ids of the 5 least expensive products, and another to retrieve the employee_id.  Then, you create the order record and the 5 line_item records comprising the order.  You have to use the customer_id, employee_id, and product_id values you obtained from the SELECT statements. You have to use the order_id for the order record you created in the line_items records. The inserts must occur within the scope of one transaction. Then, using a SELECT with a JOIN, print out the list of line_item_ids for the order along with the quantity and product name for each.

   You want to make sure that the foreign keys in the INSERT statements are valid.  So, add this line to your script, right after the database connection.  Use exactly as written — this exact line is required for foreign-key checks:
   ```
   conn.execute("PRAGMA foreign_keys = 1")
   ```

   In general, when creating a record, you don't want to specify the primary key.  So leave that column name off your insert statements.  SQLite will assign a unique primary key for you.  But, you need the order_id for the order record you insert to be able to insert line_item records for that order.  You can have this value returned by adding the following clause to the INSERT statement for the order (use exactly as written, or use `cursor.lastrowid` to get the new order_id instead):
   ```
   RETURNING order_id
   ```

2. **Deliverable**:   
   - Get this working in sqlcommand.  (Note that sqlcommand does not provide a way to begin and end transactions, so for sqlcommand, the creation of the order and line_item records are separate transactions.)
   - Use sqlcommand to delete the line_items records for the order you created.  (This is one delete statement.)  Delete also the order record you created.
   - Add statements for the complete transaction and the subsequent SELECT statement into `advanced_sql.py`, and to print out the result of the SELECT.
   - Test your program.

---

### **Task 4: Aggregation with HAVING**

1. **Problem Statement**:  
   Find all employees associated with more than 5 orders.  You want the first_name, the last_name, and the count of orders.  You need to do a `JOIN` on the employees and orders tables, and then use GROUP BY, COUNT, and HAVING.

2. **Deliverable**:  
   - Get it working in sqlcommand.
   - Add code `advanced_sql.py` to print out the employee_id, first_name, last_name, and an order count for each of the employees with more than 5 orders.
   - Test your program.

---

### **Task 5: Web Scraping Capstone Project**
For the web scraping capstone project, you will save your cleaned and transformed data into a **SQLite** database.  This is the second week covering databases.  This week you finalize the capstone database you started in Assignment 9 — keep and build on that Assignment 9 database work rather than starting over.  Import each of the **CSV** files into a separate table in a **SQLite** database.  You can also use database tools to do some of the data transformation if you choose to.

### **This week's focus areas for the database portion of the project**
#### **Data Cleaning & Transformation**
- Loads raw data into a **Pandas DataFrame**.
- Cleans missing, duplicate, or malformed entries effectively.
- Applies appropriate transformations, groupings, or filters.
- Shows before/after stages of cleaning or reshaping.
- Saves the clean, transformed data into a **SQLite** database

This week, you will submit a pull request with the database code for your project. Put the link for your pull request in the second submission field. You have until the final week of class to refine your project.


### **Submit Your Assignment on GitHub**  

📌 **Follow these steps to submit your work:**  

#### **1️⃣ Add, Commit, and Push Your Changes**  
- Within your python_homework folder, do a git add and a git commit for the files you have created, so that they are added to the `assignment10` branch.
- Push that branch to GitHub. 

#### **2️⃣ Create a Pull Request**  
- Log on to your GitHub account.
- Open your `python_homework` repository.
- Select your `assignment10` branch.  It should be one or several commits ahead of your main branch.
- Create a pull request.

#### **3️⃣ Submit Your GitHub Links**
- Your browser now has the link to your pull request.  Copy that link. 
- Paste the URL into the **assignment submission form**. 
- Paste the URL for your capstone project database pull request into the second submission field. 

---

<details>
<summary>Rubric (for AirHub reviewer and mentors)</summary>

This is a SQLite-database assignment with **no test file** — grade by reading the SQL/Python and confirming the printed results and technique. SQL has many valid phrasings; judge on the **result and the technique used**, not exact wording. **Paths and working directory are lenient:** do not fail on folder location, working directory, or the exact database path (`db/lesson.db` vs `../db/lesson.db`) — confirm the connect string matches where the student runs the script. `sqlcommand.py` and `db/lesson.db` are course-provided from earlier lessons — do not fault the student for not creating them. Floating-point tails and `ROUND(..., 2)` are both fine.

### Required Deliverables/Tasks

- **Branch and folder setup** — Work on an `assignment10` branch, code in the `assignment10` folder, submitted as a PR into `main`, each task marked with a comment. `Example — adapt to your own layout` for folder location and working directory.
- **`advanced_sql.py`** — one new file in `assignment10/` holding Tasks 1–4, opening `db/lesson.db`, running each statement, printing results, and closing the connection. `Example — adapt to your own layout` on the file path.
- **Task 1 — Complex JOINs with Aggregation** — Print the order_id and total price of each of the first 5 orders: join `orders` → `line_items` → `products`, `GROUP BY order_id`, `SUM(price * quantity)`, `ORDER BY order_id`, `LIMIT 5`; no subquery needed. `Example — adapt to your own layout`: accept any equivalent SQL phrasing/join order. Expected totals (small floating-point tails are normal): 1→513.5, 2→38.24, 3→242.85, 4→793.33, 5→271.84.
- **Task 2 — Understanding Subqueries** — Print each customer name and the average of their per-order totals. A subquery produces one row per order (total price) with the customer id; the outer query LEFT JOINs `customers` to it, groups by customer, and averages. `Example — adapt to your own layout`: the alias names the prose suggests (`AS total_price`, `AS customer_id_b`, `ON customer_id = customer_id_b`) are the author's way to make the join work — do not fail a correct result that used different alias names; the LEFT JOIN + per-order grouping + AVG is the point (customers with no orders show None/NULL). ~100 rows.
- **Task 3 — Insert Transaction Based on Data** — Create one new order for customer "Perez and Sons", employee "Miranda Harris", 10 of each of the 5 cheapest products, all in **one transaction**, then print each line item's line_item_id, quantity, and product name (SELECT + JOIN). Three SELECTs must pull customer_id, employee_id, and the 5 product_ids **from the data** (not hard-coded). `Use exactly as written`: `conn.execute("PRAGMA foreign_keys = 1")` right after connecting, and `RETURNING order_id` on the order INSERT to capture the new id (`cursor.lastrowid` is an accepted alternative). The new order_id varies — do not check for a specific value. Expected: Perez and Sons = customer_id 16, Miranda Harris = employee_id 7, 5 cheapest = Bike, Shirt, Salad, Unbranded Wooden Keyboard, Practical Tuna; final SELECT prints 5 rows, quantity 10 each.
- **Task 4 — Aggregation with HAVING** — Print employee_id, first_name, last_name, and order count for employees with more than 5 orders: JOIN `employees`+`orders`, `GROUP BY` employee, `COUNT`, filter in **HAVING** (not WHERE). `Example — adapt to your own layout` on SQL phrasing. In this dataset all 20 employees qualify (counts 8–18) — a 20-row result is correct; do not fail it.
- **Task 5 — Web-scraping capstone database step (required this week, cumulative)** — Load the capstone's cleaned CSV files into a **SQLite** database, one table per CSV, submitted as a **separate PR** (link in the second submission field). This is the **second** database week (continuing Assignment 9) — graded against the project rubric (loads into a Pandas DataFrame, cleans missing/duplicate/malformed data, applies transformations/groupings/filters, shows before/after, writes to SQLite). Since the project runs until the final week, **check that the database code exists and runs; do not fail for unfinished refinement.** Cumulative — do not fault building on prior-week capstone code.
- **Submission** — `assignment10` branch pushed, PR into `main`, the assignment PR link in the main field and the capstone-database PR link in the second field.

### Optional Deliverables/Tasks

**None.** Tasks 1–5 are all required this week. (Task 5 is cumulative across Assignments 9–10 and refined until the final week, but a PR with database code is still required this week — treat it as required, expect partial progress, and do not fail it for being unfinished.)

</details>

### **Resources**
- [SQLite Documentation](https://www.sqlite.org/docs.html)
- [Python `sqlite3` Library Documentation](https://docs.python.org/3/library/sqlite3.html)
