# **Assignment 8: Introduction to Web Scraping**

### **Objective**

You learn to harvest data from a live web site, scraping values from the HTML page and storing them in a CSV file.

## **Assignment Instructions**

You create the code for this assignment in the assignment8 folder within your python_homework folder.  Be sure to create an `assignment8` git branch before you start.  As usual, mark the code that completes each task with a comment line.

## **Overview**
This lesson introduces web scraping concepts and practices, including:
1. Understanding HTML and the DOM structure in a live web page.
2. Extracting content using Selenium.
3. Storing data in CSV and JSON files.
4. Ethical considerations in web scraping.

## **Tasks**

## Task 1: Check robots.txt to Ensure Policy Compliance

In this assignment, you collect data from Books to Scrape, a demo bookstore site designed for practicing web scraping. As always, before you scrape a site, you need to check whether a `robots.txt` file exists and review its contents.

1. Try to open <https://books.toscrape.com/robots.txt>.
2. You should find that this URL returns a "Not Found" page — the site has no `robots.txt` file. When a site has no `robots.txt`, that means it places no restrictions on automated access. Combined with the fact that the site explicitly states it is built for scraping practice, we can proceed.
3. For comparison, try opening <https://durhamcounty.bibliocommons.com/robots.txt>. This is a public library catalog that used to allow scraping, but now returns `User-agent: * / Disallow: /`, which means all automated access to all pages is disallowed. Many library sites have added restrictions like this in response to heavy traffic from AI bots. This is a good example of why checking `robots.txt` matters — a site's policy can change at any time.

## Task 2: Understanding HTML and the DOM for Books to Scrape

You are going to find where in the HTML page the data you want is located.

1. Open <https://books.toscrape.com/>.
2. Open your browser developer tools to show the HTML elements. For Chrome, this is `Shift-Ctrl-J` (or `Shift-Cmd-J` on Mac).
3. Find the HTML element for a single book listing. When you select an element in the Chrome developer tools, that part of the web page is highlighted. Start by selecting elements and expanding them until you get to an individual book. Hint: You are looking for an `article` element with a specific class. Note the class value — your program will need it.
4. Within that `article` element, find the element that contains the book's title. Note two things: the tag type, and also look at the `title` attribute of the link inside it. The displayed text on the page may be truncated, but the full title is stored in this attribute. Save these details — your program will need them.
5. Within the same `article` element, find the element that contains the book's price. Note the tag type and its class value. Save it.
6. Within the same `article` element, find the element that indicates the book's star rating. This one is a little different — the rating is not stored as text content. Instead, look at the class attribute of the element. The rating (One, Two, Three, Four, or Five) is part of the class name itself. Note this pattern — your program will need to extract it.

### **Task 3: Write a Program to Extract this Data**

>[!note]
> There are several tools for web scraping in Python. This assignment uses **Selenium**, which controls a real browser. Selenium is especially useful for scraping sites that render content with JavaScript — the browser executes the JavaScript before you extract data, so you see the same page a human visitor would. Other tools like `requests` + `BeautifulSoup` work well for simpler pages that serve static HTML. Books to Scrape is a static site, but we use Selenium here because most real-world sites rely on JavaScript, and learning Selenium prepares you for those.

1. Create a file called `get_books.py`. The program should import from `selenium` and `webdriver_manager`. You also need `pandas` and `json`.
2. Add code to load the Books to Scrape homepage: `https://books.toscrape.com/`.
3. Find all the `article` elements on the page for the book listings. Use the class value you identified in Task 2 step 3. Also use the tag name when you do the find, to make sure you get the right elements.
4. Within your program, create an empty list called `results`. You are going to add `dict` values to this list, one for each book.
5. Main loop: Iterate through the list of `article` elements. For each one:
   - Find the element that contains the title. The displayed text may be truncated, so get the full title from the `title` attribute of the link (the `a` tag inside the heading). Use `get_attribute('title')` for this.
   - Find the element that contains the price, and get its text.
   - Find the element that contains the star rating. Since the rating is encoded in the class name (for example, `star-rating Three`), you need to get the class attribute and extract the rating word from it.
   - Create a `dict` that stores these three values with the keys `Title`, `Price`, and `Rating`. Then append that `dict` to your `results` list.
6. Create a DataFrame from this list of dicts. Print the DataFrame.

**Hint:** You can add small print statements at each step to check whether that part works. For example, when you find all the `article` elements, print the length of the list to verify you get 20. Then implement just the title extraction and print it. Build the program incrementally.

**For Further Thought (Optional):** The homepage shows 20 books, but the site has 1,000 books across 50 pages. How would you get all the books from all pages? Look at how the "next" button works and what the URL pattern looks like for each page. **Optional:** Change your program to page through all the results. However — you need to make sure your program pauses between pages. Fast scraping, where many requests are sent in short order, is an abuse of the privilege, even on a site that welcomes scraping.

### **Task 4: Write out the Data**
Modify your program to do the following:

1. Write the DataFrame to a file called `get_books.csv`, within the assignment8 folder. Examine the file to see if it looks right.
2. Write the `results` list out to a file called `get_books.json`, also within the assignment8 folder. You should write it out in JSON format. Examine the file to see if it looks right.

### **Task 5: Ethical Web Scraping**

**Goal**:  
Understand the importance of ethical web scraping and `robots.txt` files.

1. Access the `robots.txt` file for Wikipedia: [Wikipedia Robots.txt](https://en.wikipedia.org/robots.txt).
2. Analyze the file and answer the following questions.  Put your answers in a file called `ethical_scraping.txt` in your python_homework/assignment8 directory
   - Which sections of the website are restricted for crawling?
   - Are there specific rules for certain user agents?
3. Reflect on why websites use `robots.txt` and write 2–3 sentences explaining its purpose and how it promotes ethical scraping.  Put these in `ethical_scraping.txt` in your python_homework/assignment8 directory.


---

### **Task 6: Scraping Structured Data**

**Goal**:  
Extract a web page section and store the information.

1. Use your browser developer tools to view this page: <https://owasp.org/www-project-top-ten/>.  You are going to extract the top 10 security risks reported there.  Figure out how you will find them.

2. Within your python_homework/assignment8 directory, write a script called `owasp_top_10.py`.  Use selenium to read this page.

3. Find each of the top 10 vulnerabilities.  Hint: You will need XPath.  For each of the top 10 vulnerabilities, keep the vulnerability title and the href link in a dict.  Accumulate these dict objects in a list.

4. Print out the list to make sure you have the right data.  Then, add code to the program to write it to a file called `owasp_top_10.csv`.  Verify that this file appears correct.

5. Create a file, `challenges.txt`, also within your assignment 8 directory.  In this file, describe any challenges you faced in completing this assignment and how you resolved them.

---

### **Task 7: Start your web scraping capstone project**

Now you're ready to get started on your second capstone project.  In this project, you will use web scraping to create your dataset, save it in a sqlite database and then create interactive, web-based visualizations to find and illustrate insights gained from the scraped data.  We recommend using the [Weather Around The World](https://www.timeanddate.com/weather/) web site, however you can use another site with CIL approval if there's a different topic you would like to explore.  Note that web scraping friendliness varies significantly and also depends on how often the structure and content changes.

For this capstone project, you will create **four programs** that retrieve data from the **Weather Around The World** website (or another approved site) and display the results in an interactive dashboard. The project will involve **Selenium** for web scraping, data cleaning, transforming the data into a structured format, storing it in a SQLite database, querying it via command line, and presenting it using **Streamlit** for interactive visualizations.

This week, you will scrape data from the web site, clean it and save it to a csv file.

#### **Set up capstone project GitHub repository**

You will create a new, separate GitHub repository for your web scraping capstone project, outside of all your other repos.  You can call it anything you want.  

1. Create a new empty repository in your GitHub account.  Please include a README which you will use to explain how to set up and run the project.
2. Clone the repo to your local computer

#### **Create your web scraping code**

1. Examine the web site and decide what you would like to explore.
2. Create a branch to implement your web scraping code
3. Submit a pull request

#### **Web scraping capstone project rubrics**

<details>

<summary>This is the complete web scraping capstone project rubric</summary>

## **Web Scraping and Dashboard Project**

### **Web Scraping**
- Uses **Selenium** to retrieve data from the web.
- Handles common scraping challenges like missing tags, pagination, and user-agent headers.
- Saves raw data as a **CSV**.
- Avoids scraping duplication or redundant requests.

### **Data Cleaning & Transformation**
- Loads raw data into a **Pandas DataFrame**.
- Cleans missing, duplicate, or malformed entries effectively.
- Applies appropriate transformations, groupings, or filters.
- Shows before/after stages of cleaning or reshaping.
- Saves the clean, transformed data into a **Sqlite** database

### **Data Visualization**
- Includes at least three visualizations using **Streamlit**.
- Visuals are relevant, well-labeled, and support the data story.
- User interactions such as dropdowns or sliders are implemented.
- Visualizations respond correctly to user input or filters.

### **Dashboard / App Functionality**
- Built with **Streamlit** to display data and insights.
- Features clean layout and responsive components.
- Allows users to explore different aspects of the data.
- Provides clear titles, instructions, and descriptions for user guidance.

### **Code Quality & Documentation**
- Code is well-organized and split into logical sections or functions.
- Inline comments or markdown cells explain major steps or choices.
- All dependencies are listed and environment setup is reproducible.
- Comments or markdown cells explain logic.
- **README.md** includes summary, setup steps, and a screenshot.

---
</details>
<br>

This week you will perform web scraping, data cleaning, and save the raw data to a **CSV** file.

##### **Web Scraping**
- Uses **Selenium** to retrieve data from the web.
- Handles common scraping challenges like missing tags, pagination, and user-agent headers.
- Saves raw data as a **CSV**.
- Avoids scraping duplication or redundant requests.

##### **Data Cleaning & Transformation**
- Loads raw data into a **Pandas DataFrame**.
- Cleans missing, duplicate, or malformed entries effectively.
- Applies appropriate transformations, groupings, or filters.
- Shows before/after stages of cleaning or reshaping.


This week, you will submit your pull request to show progress on webscraping and data cleaning on your capstone project.  You have until the final week of class to refine your project.

---

📌 **Follow these steps to submit your work:**  

## **Submit Your GitHub Links**

First, commit and push your work and open your pull requests:
- **Main assignment:** within your `python_homework` folder, `git add` and `git commit` the files you created on the `assignment8` branch, push the branch to GitHub, and open a pull request into `main`.
- **Web scraping capstone:** push your capstone branch and open a pull request in that separate repository.

Then submit your links:
- Paste your assignment pull request **URL** into the first submission field on the **assignment submission form**.
- Paste your capstone project pull request **URL** into the second submission field on the **assignment submission form**.
- Paste the public link to your **finalized Kaggle data pipeline capstone** into the Kaggle submission field on the **assignment submission form** (see the section below).

---

## **Finalize and Submit Your Kaggle Data Pipeline Capstone**

This is the **final deadline** for your first capstone project — the Kaggle data pipeline you have been building since week 5.  You completed the core of it in week 7 by adding your visualizations, so this week is about polishing and submitting it, not new building.  Getting it in now means the rest of the course is dedicated to your web scraping capstone, so the two projects don't compete at the end of the term.

Before you submit, check your Kaggle notebook against the full rubric:

<details>
<summary>Kaggle Data Pipeline Capstone Rubric</summary>

## Kaggle Project Rubric

* **General Code Quality**

    * [ ]  Code demonstrates a strong understanding of Python basics.
    * [ ]  Code is well organized and documented with comments.
    * [ ]  Functions are used to structure and organize the code.

* **File Handling and Data Loading**

    * [ ]  Data is loaded from appropriate file formats (CSV, JSON, etc.) using Pandas.
    * [ ]  File paths and loading procedures are clearly defined and handled robustly.
    * [ ]  Demonstrates effective use of Pandas `read_csv()`, `read_json()`, or similar functions.
    * [ ]  Uses `head()`, `tail()`, and `info()` effectively to preview and inspect the data.

* **Data Wrangling and Transformation**

    * [ ]  Demonstrates proficiency in using Pandas for data selection, filtering, and transformation.
    * [ ]  Implements advanced data manipulation techniques, including indexing, slicing, and data type conversion.
    * [ ]  Handles missing data effectively using `dropna()` or `fillna()` with appropriate strategies.
    * [ ]  Identifies and removes duplicate records if necessary using Pandas.
    * [ ]  Code is efficient, well-documented, and follows Pandas best practices.
    * [ ]  At least two extracted features

* **Data Aggregation**

    * [ ]  Uses Pandas `groupby()` function effectively to aggregate data and gain insights.
    * [ ]  Applies a variety of aggregation functions (e.g., `sum()`, `mean()`, `count()`, `min()`, `max()`) to analyze grouped data.
    * [ ]  Clearly presents and interprets the results of data aggregation.
    * [ ]  At least 2 aggregations

* **Visualization Quality**

    * [ ]  Creates multiple (3+) high-quality, informative, and visually appealing visualizations using appropriate libraries (e.g., Matplotlib, Seaborn, Plotly).
    * [ ]  Visualizations are clear, concise, and easy to understand, with appropriate titles, labels, legends, and color schemes.
    * [ ]  Demonstrates strong understanding of design principles.
    * [ ]  Provides clear explanations of the insights conveyed by each visualization.

* **Chart Types and Interpretation**

    * [ ]  Uses a diverse range of chart types (e.g., scatter plots, bar charts, histograms, box plots, heatmaps) to provide a comprehensive view of the data.
    * [ ]  Demonstrates a clear understanding of the strengths and weaknesses of each chart type and selects them strategically.
    * [ ]  Provides insightful interpretations of the visualizations, connecting them to the data analysis and the problem domain.

* **Dataset and Feature Understanding**

    * [ ]  Uses a dataset that is appropriate for the analysis.
    * [ ]  Demonstrates a clear understanding of the dataset's characteristics, limitations, and potential biases.
    * [ ]  Selects and uses a sufficient number of relevant features to support a meaningful analysis.

* **Conclusions and Insights**

    * [ ]  Provides a clear, concise, and insightful summary of the project's key findings and conclusions.
    * [ ]  Connects the findings to the original problem or question and discusses their implications.
    * [ ]  Identifies potential limitations of the analysis.
    * [ ]  Demonstrates a strong understanding of the data's story and effectively communicates it.
    * [ ]  At least 3 conclusions supported by charts and text.

* **Reproducibility**

    * [ ]  Provides a well-organized and clearly documented notebook or script that allows others to easily reproduce the entire analysis.
    * [ ]  All dependencies are clearly specified.

</details>
<br>

**To submit:** on the upper right of the Kaggle page, click **Save Version** and save (a quick save is fine).  Then click **Share**, choose **Public**, make sure **Allow Comments** is on, and copy the public URL.  Paste that URL into the Kaggle submission field on this week's **assignment submission form**.

---

## **Resources**

- [Selenium Documentation](https://www.selenium.dev/documentation/webdriver/)
- [MDN Web Docs: Understanding the DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)
- [OWASP robots.txt](https://owasp.org/robots.txt)

---  

<details>
<summary>Rubric (for AirHub reviewer and mentors)</summary>

This assignment scrapes **live web sites** with Selenium and has **no automated tests**. Grade the **method**, not exact output. The exact scraped values (titles, prices, ratings, vulnerability names/links) and the exact CSS class names / XPath selectors **change over time** — `Example — adapt to your own layout`: do NOT fail a submission because its selectors or scraped values differ from any reference. The reviewer also cannot see the student's filesystem — do NOT fail a student because you cannot confirm a file sits in a particular folder. Tasks 1–6 are submitted as an `assignment8` branch PR into `main` in `python_homework`; Task 7 and the Kaggle finalization are **separate repos/notebooks** submitted via their own links (see the last two Required items).

### Required Deliverables/Tasks

- **Task 1 — robots.txt review (Books to Scrape)** — A compliance check; no file deliverable. Done = the student shows they checked for `books.toscrape.com/robots.txt`, understood that the file does not exist (meaning no restrictions), and noted the contrast with a site like the Durham County Library that disallows all access. Do not require exact wording.
- **Task 2 — DOM/selector notes** — Notes (in a student-chosen temporary file) identifying the repeating `article` element (with class `product_pod`) and sensible selectors for Title (the `a` tag's `title` attribute inside `h3`), Price (a `p` with the price class), and Rating (the star-rating class pattern). Feeds Task 3; judge whether the right *kind* of element was found, not the literal class string. `Example — adapt to your own layout` for the class strings.
- **Task 3 — `get_books.py`** — Loads the Books to Scrape homepage, finds all `article` elements for book listings, and for each builds a `dict` appended to a list named `results`, then builds and prints a pandas DataFrame. Dict keys are `Title`, `Price`, `Rating`. The title should be the full title (from the `title` attribute), not the truncated display text. Use exactly as written (later tasks depend on these names) for the dict keys and the `results` list. Must import from `selenium` and `webdriver_manager` (and `pandas`; `json` is used in Task 4). `Example — adapt to your own layout` for the CSS/class selectors and the scraped values.
- **Task 4 — `get_books.csv` and `get_books.json`** — `get_books.py` writes the DataFrame to `get_books.csv` (header + one row per book) and the `results` list to `get_books.json` as valid JSON (list of objects). Use exactly as written (later tasks depend on these names) for the filenames. An index column from `to_csv` is a minor nit, not a failure; `df.to_json(...)` is an acceptable alternative to `json.dump`. The CSV should be comma-separated (the default for `to_csv()` and the csv module). A different separator is a minor miss, not a failure.
- **Task 5 — `ethical_scraping.txt`** — Reads Wikipedia's `robots.txt` and answers: which sections are restricted (some `Disallow`ed paths named), whether there are user-agent-specific rules (yes), plus 2–3 sentences on why sites use `robots.txt`. Use exactly as written for the filename. Subjective otherwise; do not require exact paths (live file). All Task 5 files go in `assignment8/`.
- **Task 6 — `owasp_top_10.py` and `owasp_top_10.csv`** — Uses Selenium with **XPath** (`By.XPATH`) to collect the 10 vulnerability titles and their `href` links from the OWASP Top Ten page, accumulates them as dicts in a list, prints the list, and writes `owasp_top_10.csv` (title + link). Use exactly as written (later tasks depend on these names) for the filenames and the use of XPath. `Example — adapt to your own layout` for the exact XPath and the scraped titles/links.
- **Task 6 — `challenges.txt`** — A short, honest reflection on difficulties and how they were resolved. Any genuine reflection passes; empty/missing fails. Use exactly as written for the filename.
- **Submission (assignment)** — Work on an `assignment8` branch, submitted as a PR into `main`, each task's code marked with a comment (e.g. `# Task 3`). `Example — adapt to your own layout`: file placement inside `assignment8/` cannot be verified by the reviewer — do not fail on it.
- **Task 7 — Web-scraping capstone (this week's scope only)** — A **new, standalone** GitHub repo (not inside `python_homework`) with a README, cloned locally, plus a branch and PR. This week's required scope is **only**: Selenium scraping of the chosen site with raw data saved to a **CSV**, raw data loaded into a pandas DataFrame and cleaned (missing/duplicate/malformed), and a README describing setup. `Example — adapt to your own layout` for the site choice (Weather Around The World or a CIL-approved alternative) and all layout/selectors. **Do NOT grade the full capstone rubric (SQLite, 3+ Streamlit visualizations, dashboard) this week** — those are later-week deliverables. Submitted as a separate PR link in the second submission field, so review it via that link (not the `assignment8` PR).
- **Kaggle data-pipeline capstone (final submission)** — The **final deadline** for the first capstone (built since week 5, core completed week 7) — this week is polishing and submitting, not new building. Deliverable is the finalized public Kaggle notebook link, submitted in its own field. Do not grade it as part of the `assignment8` code.

### Optional Deliverables/Tasks

- **Task 3 — "For Further Thought" pagination** — Marked **Optional** in the text. Paging through all 50 pages of book listings to collect all 1,000 books. **Do not fail a student for omitting this.** If a student attempted it, confirm they added a pause between page requests (e.g. `time.sleep`) — scraping without a delay is the abuse the lesson warns against. `Example — adapt to your own layout` for any pagination approach and pacing value.

</details>
