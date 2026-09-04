# **Assignment 9: Introduction to Databases and SQL**

For this assignment, you create code in your python_homework/assignment9 folder.


## **Task 1: Create a New SQLite Database**
1. Within your python_homework repository, create an `assignment9` git branch.
2. Make the `assignment9`  folder the working folder.  Within the `assignment9` folder, create a file `sql_intro.py`.
3. Write code to connect to a new SQLite database, `../db/magazines.db` and to close the connection.
4. Execute the script and confirm the database file is created.  Note: All SQL statements should be executed within a `try` block, followed by a corresponding `except` block, because any SQL statement can cause an exception to be raised.

---

## **Task 2: Define Database Structure**
We have publishers that publish magazines.  Each publisher has a unique name, and so does each magazine.  There is a one-to-many relationship between publishers and magazines.  We also have subscribers, and each subscriber has a name and an address.  We have a many-to-many association between subscribers and magazines, because a subscriber may subscribe to several magazines, and a magazine may have many subscribers.  So, we have a join table called subscriptions.  The subscriptions table also stores the expiration_date (a string) for the subscription.  All the names, the address, and the expiration_date must be non-null.  

1. Think for a minute.  There is a one-to-many relationship between publishers and magazines.  Which table has a foreign key? Where does the foreign key point?  How about the subscriptions table: What foreign keys does it have?

2. Add SQL statements to `sql_intro.py` that create the following tables:
   - `publishers`
   - `magazines`
   - `subscribers`
   - `subscriptions`
   Be sure to include the columns you need in each, with the right data types, with UNIQUE and NOT NULL constraints as needed, and with foreign keys as needed.  You can reuse column names if you choose, i.e. you might have a name column for publishers and a name column for magazines.  If you need to start over, you can delete `db/magazines.db`.

3. Open the `db/magazines.db` file in VSCode to confirm that the tables are created.

---

## **Task 3: Populate Tables with Data**
1. Add the following line to sql_intro.py, right after the statement that connects to the database.  Use exactly as written (later tasks depend on this):
   ```
   conn.execute("PRAGMA foreign_keys = 1")
   ```
   This line tells SQLite to make sure the foreign keys are valid.
2. Create functions, one for each of the tables, to add entries.  Include code to handle exceptions as needed, and to ensure that there is no duplication of information.  The subscribers name and address columns don't have unique values -- you might have several subscribers with the same name. But when creating a subscriber, check that you don't already have an entry where *both* the name and the address are the same as for the one you are trying to create.
3. Add code to the main line of your program to populate each of the 4 tables with at least 3 entries.  Don't forget the `commit`!
4. Run the program several times.  View the database to ensure that you are creating the right information, without duplication.

---

## **Task 4: Write SQL Queries**
1. Write a query to retrieve all information from the subscribers table.
2. Write a query to retrieve all magazines sorted by name.
3. Write a query to find magazines for a particular publisher, one of the publishers you created.  This requires a `JOIN`. 
4. Add these queries to your script.  For each, print out all the rows returned by the query.

---

## **Task 5: Read Data into a DataFrame**

You will now use Pandas to create summary data from the `../db/lesson.db` database you populated as part of the lesson.  We want to find out how many times each product has been ordered, and what was the total price paid by product.

1. While still within the `python_homework/assignment9` directory, create a program, `sql_intro_2.py`.
2. Read data into a DataFrame.  The SQL statement should retrieve the line_item_id, quantity, product_id, product_name, and price from a JOIN of the line_items table and the products table. Hint: Your `ON` statement would be `ON line_items.product_id = products.product_id`.
3. Print the first 5 lines of the resulting DataFrame.  Run the program to make sure this much works.
4. Add a column to the DataFrame called "total".  This is the quantity times the price.  (`df['total'] = df['quantity'] * df['price']`.)  Print out the first 5 lines of the DataFrame to make sure this works.
5. Add groupby() code to group by the product_id.  Use an agg() method that specifies 'count' for the line_item_id column, 'sum' for the total column, and 'first' for the 'product_name'.  Print out the first 5 lines of the resulting DataFrame.  Run the program to see if it is correct so far.
6. Sort the DataFrame by the product_name column.
7. Add code to write this DataFrame to a file `order_summary.csv`, which should be written in the `assignment9` directory.  Verify that this file is correct.

As we'll learn in the next lesson, the ordering, grouping, count, and sum operations can be done in SQL, more efficiently than in Pandas.  The key concepts of pandas and SQL overlap very strongly.

---

## **Task 6: Web Scraping Capstone project**
For the web scraping capstone project, you will save your cleaned and transformed data into a **SQLite** database.  Since there are two weeks of database lessons, you have this week and next to complete the database portion of the project.  This is an opportunity to catch up on the data cleaning and transformation parts of the project.  You have learned about importing **CSV** this week, so you have the tools you need to get started.  Import each of the **CSV** files into a separate table in a **SQLite** database. You can also use database tools to do some of the data transformation if you choose to.

### **These are the relevant rubrics for this week and next**
#### **Data Cleaning & Transformation**
- Loads raw data into a **Pandas DataFrame**.
- Cleans missing, duplicate, or malformed entries effectively.
- Applies appropriate transformations, groupings, or filters.
- Shows before/after stages of cleaning or reshaping.
- Saves the clean, transformed data into a **SQLite** database

For this week, start a pull request with your database code.  You have another week to post the final version since there are two weeks dedicated to databases.  Put the initial version of your pull request in the second submission link to show your progress.

### **Submit Your Assignment on GitHub**  

📌 **Follow these steps to submit your work:**  

#### **1️⃣ Add, Commit, and Push Your Changes**  
- Within your python_homework folder, do a git add and a git commit for the files you have created, so that they are added to the `assignment9` branch.
- Push that branch to GitHub. 

#### **2️⃣ Create a Pull Request**  
- Log on to your GitHub account.
- Open your `python_homework` repository.
- Select your `assignment9` branch.  It should be one or several commits ahead of your main branch.
- Create a pull request.

#### **3️⃣ Submit Your GitHub Links** 
- Your browser now has the link to your pull request.  Copy that link. 
- Paste the URL into the **assignment submission form**
- Paste the URL for your capstone project database pull request into the second submission field. 

---

<details>
<summary>Rubric (for AirHub reviewer and mentors)</summary>

There is **no PyTest file** for this assignment. Grade by reading the two scripts (`sql_intro.py`, `sql_intro_2.py`) and, where possible, the databases they build. SQL can be written many valid ways and each student invents their own sample data — grade **structure and results**, not exact wording. **Paths and working directory are lenient:** the scripts are meant to run from inside `assignment9/` so `../db/...` resolves, but do not fail a student for a differing path, folder layout, or working directory — what matters is that the code connects to the right database and produces the right output. `sql_intro_2.py` reads `../db/lesson.db`, a database the student populated during the lesson — do not fault the student for not creating it.

### Required Deliverables/Tasks

- **Branch & PR** — Work on an `assignment9` branch, submitted as a PR into `main`. `Use exactly as written (later tasks depend on these names)` for the branch name `assignment9`.
- **Task 1 — `sql_intro.py` connects to the database** — Connects to `../db/magazines.db` and closes the connection, with SQL work wrapped in `try`/`except`. Running the script creates the file. `Example — adapt to your own layout` for the exact path/working directory.
- **Task 2 — Four tables with correct constraints** — Creates `publishers`, `magazines`, `subscribers`, `subscriptions`. Required structure: `publishers.name` and `magazines.name` are **UNIQUE and NOT NULL**; `magazines` holds the **foreign key** to `publishers` (FK on the "many" side); `subscribers` name and address are **NOT NULL but not unique** (duplicate names allowed); `subscriptions` is a join table with **FKs to both** `subscribers` and `magazines` plus a **NOT NULL** expiration_date. `Example — adapt to your own layout` for column names — grade the constraints and relationships, not the exact column names.
- **Task 3 — Populate the tables** — `conn.execute("PRAGMA foreign_keys = 1")` present right after connecting (`Use exactly as written (later tasks depend on these names)`); one insert-function per table; **parameterized queries** (`?` placeholders), not string concatenation; at least **3 rows per table**; `conn.commit()` called; re-running creates **no duplicates**. The **subscribers** dedupe is the key check: because name+address are not UNIQUE, the student must **query first** for an identical name+address pair before inserting (a plain UNIQUE constraint there is a misread).
- **Task 4 — Three queries, each printed** — (1) all rows from `subscribers`; (2) all `magazines` sorted by name (`ORDER BY name`); (3) magazines for one publisher via a real **JOIN** of `magazines` and `publishers` on the FK, filtered to one publisher (not two separate queries stitched together in Python). Each result set is iterated and printed. `Example — adapt to your own layout` for the specific SQL wording and which publisher/sample rows appear.
- **Task 5 — `sql_intro_2.py` summary** — Reads a JOIN of `line_items` and `products` from `../db/lesson.db` into a Pandas DataFrame selecting exactly `line_item_id, quantity, product_id, product_name, price`; join `ON line_items.product_id = products.product_id`; adds `df['total'] = df['quantity'] * df['price']`; groups by `product_id` with `agg` applying **`'count'` to line_item_id**, **`'sum'` to total**, **`'first'` to product_name**; sorts by `product_name`; writes `order_summary.csv`. `Use exactly as written (later tasks depend on these names)` for the five selected column names, the join condition, and the three aggregations (dict-form or named-aggregation `agg` syntax both fine). `Example — adapt to your own layout` for the `../db/lesson.db` path, the `order_summary.csv` location, and the exact numeric totals (data-dependent).
- **Task 6 — Web-scraping capstone database step (required this week, progress-only)** — Submitted as a **separate PR** (second submission link). This is the **first of two weeks** on the capstone database, so grade on **progress, not completeness** — confirm the student has started: raw data loaded into a Pandas DataFrame with some cleaning, each cleaned CSV imported into its **own table** in a SQLite database (e.g. `df.to_sql(...)`), and an opened PR. **Do not fail** for unfinished cleaning, missing transformations, or an incomplete final version — that work is due next week. Cumulative: builds on prior-week capstone code; do not fault its presence.
- **Submission** — `assignment9` branch pushed; PR into `main`; the assignment PR link and the capstone-database PR link pasted into the two submission fields.

### Optional Deliverables/Tasks

**None.** Every task is required this week. (Task 6 is cumulative capstone progress but a PR is still expected this week, so it is listed under Required and graded on progress.)

</details>


