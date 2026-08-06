# Assignment 8 Answer Key: Introduction to Web Scraping

**Mentor note:** This lesson has **no PyTest file** — nothing is auto-graded. You review it by reading the code and the written files. The work uses **Selenium** (plus `webdriver-manager`, `pandas`, and `json`) to scrape data from live web sites and save it to CSV/JSON. Because the targets are live pages, the exact scraped values and the exact class/XPath selectors **will change over time**. Grade the **method** (correct page load, correct find logic, handling multiple authors/missing tags, writing valid CSV/JSON, ethical pacing) rather than matching exact output. Reference code below is one correct approach; students' selectors may differ and still be right if their output looks reasonable.

---

## File Setup

- Work is on an `assignment8` branch, submitted as a PR into `main`.
- All files go in the `assignment8/` folder (it ships with only a `.keep` placeholder).
- Files students create: `get_books.py`, `get_books.csv`, `get_books.json`, `owasp_top_10.py`, `owasp_top_10.csv`, `ethical_scraping.txt`, and `challenges.txt`.
- Each task's code should be marked with a comment (`# Task 3`, etc.).
- Selenium/webdriver work is environment-heavy; do **not** expect to re-run students' scrapers to verify. Confirm the output files exist and look reasonable.
- A second PR (the capstone repo) is submitted separately — see Task 7.

---

## Task 1: Review robots.txt — **Subjective**

Student confirms that scraping the Durham County Library search results does not breach `durhamcountylibrary.org/robots.txt`. There is no code deliverable; this is a compliance check.

- A correct answer shows the student actually looked at the file and understood that `robots.txt` lists paths that are `Disallow`ed for crawlers.
- A weak answer skips this entirely or assumes "any scraping is fine."
- Note: `robots.txt` content is live and can change; do not hold students to an exact quote.

---

## Task 2: Understanding HTML and the DOM — **Subjective**

Student uses browser dev tools to locate the search-result `li` element and note the class/tag values for: the result item, the title, the author link, and the format/year (including the containing `div`). The deliverable is notes in a temporary file that feed Task 3 — not graded output.

- A correct answer identifies the repeating `li` list-item, plus a sensible selector for each of Title, Author, and Format-Year.
- Multiple authors and the nested `div` for format/year are the two "gotchas" the task calls out — good notes acknowledge them.
- **Live-site caveat:** bibliocommons class names change periodically. Judge whether the student found the right *kind* of element, not the literal class string.

---

## Task 3: Write get_books.py to Extract the Data — **Objective (method), live-site dependent**

Loads the Durham library search page, finds every result `li`, and for each builds a dict with keys `Title`, `Author`, `Format-Year`, appends to a `results` list, then builds and prints a DataFrame.

Reference approach (class names are illustrative — students' selectors will vary):

```python
# Task 3
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.by import By
from webdriver_manager.chrome import ChromeDriverManager
import pandas as pd
import json

url = ("https://durhamcounty.bibliocommons.com/v2/"
       "search?query=learning%20spanish&searchType=smart")

options = Options()
options.add_argument("--headless=new")
driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()),
                          options=options)
driver.get(url)

# Find all search-result list items by tag + class
items = driver.find_elements(By.CSS_SELECTOR, "li.cp-search-result-item")

results = []
for item in items:
    title = item.find_element(By.CSS_SELECTOR, ".cp-title").text

    # Handle one OR many authors, joined with ";"
    authors = item.find_elements(By.CSS_SELECTOR, ".cp-author-link")
    author = ";".join(a.text for a in authors)

    # Format/year lives in a span inside a specific div
    fmt_div = item.find_element(By.CSS_SELECTOR, ".cp-format-indicator")
    format_year = fmt_div.find_element(By.TAG_NAME, "span").text

    results.append({
        "Title": title,
        "Author": author,
        "Format-Year": format_year,
    })

df = pd.DataFrame(results)
print(df)

driver.quit()
```

- Imports from `selenium` and `webdriver_manager` are required; `pandas` and `json` should be imported (json is used in Task 4).
- `find_elements` (plural) returns the list of results; the loop uses `find_element` (singular) inside each item — check they didn't mix these up.
- **Multiple authors** must be joined with a semicolon `;`. A student who only grabs the first author has missed a stated requirement.
- Format/year requires drilling into the `div` and then its `span` — a common failure point.
- `driver.quit()` (or `close()`) should appear so the browser process is released.
- Do **not** grade on exact titles/authors — those depend on live search results.
- **Optional pagination** ("For Further Thought") is not required. If a student did it, confirm they added a pause (e.g. `time.sleep`) between page requests — scraping without a delay is the abuse the lesson warns against.

---

## Task 4: Write Out the Data — **Objective**

Adds CSV and JSON output to `get_books.py`.

```python
# Task 4
df.to_csv("get_books.csv", index=False)

with open("get_books.json", "w") as f:
    json.dump(results, f, indent=4)
```

- `get_books.csv` should have a header row (`Title,Author,Format-Year`) and one row per book. `index=False` avoids a junk index column (minor — not disqualifying).
- `get_books.json` should be valid JSON — a list of objects. Using `json.dump(results, ...)` on the list of dicts is the clean approach; `df.to_json(...)` is also acceptable.
- Both files must land inside `assignment8/`. Confirm they exist in the PR.

---

## Task 5: Ethical Web Scraping — **Subjective**

Student reads Wikipedia's `robots.txt` and writes answers in `ethical_scraping.txt`.

A correct `ethical_scraping.txt` should:
- Name some **restricted/`Disallow`ed** sections (Wikipedia disallows paths like `/w/`, various `Special:` and non-article namespaces, edit/history query URLs).
- Note that there **are user-agent-specific rules** — `robots.txt` uses `User-agent:` blocks, and some bots are singled out (certain crawlers are blocked entirely, others get general rules).
- Include 2–3 sentences on **why** sites use `robots.txt`: to tell crawlers what they may/may not access, to protect server load and non-content pages, and to set expectations for polite/ethical scraping.

A weak answer just says "some parts are blocked" without specifics, or misses that `robots.txt` is advisory guidance for ethical behavior rather than a hard technical lock.

**Live-site caveat:** Wikipedia's `robots.txt` changes; don't require exact path matches.

---

## Task 6: Scraping Structured Data (OWASP Top 10) — **Objective (method), live-site dependent**

`owasp_top_10.py` uses Selenium + XPath to pull the 10 vulnerability titles and their href links from the OWASP Top Ten project page, collects them as dicts in a list, prints it, and writes `owasp_top_10.csv`.

Reference approach (XPath will vary with the page):

```python
# Task 6
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.common.by import By
from webdriver_manager.chrome import ChromeDriverManager
import pandas as pd

options = Options()
options.add_argument("--headless=new")
driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()),
                          options=options)
driver.get("https://owasp.org/www-project-top-ten/")

# The Top 10 render as links whose text starts "A01:2021" ... "A10:2021"
links = driver.find_elements(
    By.XPATH, "//a[contains(text(), ':2021')]"
)

results = []
for link in links:
    results.append({
        "Title": link.text,
        "Link": link.get_attribute("href"),
    })

print(results)

df = pd.DataFrame(results)
df.to_csv("owasp_top_10.csv", index=False)

driver.quit()
```

- **XPath is required** by the task — check they used `By.XPATH` rather than only CSS.
- Each dict must hold both the **title** and the **href link** (`get_attribute("href")`, not `.text`, gets the URL).
- The list should end up with the 10 items. If the XPath is loose it may grab extras — a correct solution filters/slices to the actual Top 10.
- `owasp_top_10.csv` must exist in `assignment8/` with title + link columns.
- `challenges.txt` (subjective): a short honest reflection on difficulties (finding the right XPath, headless setup, driver install). Any genuine reflection passes; an empty or missing file does not.

---

## Task 7: Web Scraping Capstone Kickoff — **Subjective**

Separate deliverable: a **new, standalone GitHub repo** (not inside python_homework) with a README, cloned locally, plus a branch and PR containing this week's scraping code. This week only requires **scraping + data cleaning saved to CSV** — the SQLite/Streamlit pieces come later.

Evaluate this week's PR for:
- **Selenium** used to scrape the chosen site (Weather Around The World, or a CIL-approved alternative), with raw data saved to a **CSV**.
- Handling of real-world scraping issues — missing tags, pagination, and **no redundant/duplicate requests** (polite pacing).
- Raw data loaded into a **Pandas DataFrame** and cleaned (missing/duplicate/malformed entries), ideally showing a before/after.
- A **README** describing setup and how to run.

A weak submission scrapes but skips cleaning, hammers the site without pauses, or puts the capstone inside the homework repo instead of a fresh one. The full rubric (SQLite, 3+ Streamlit visualizations, dashboard polish) is for later weeks — don't grade those now.

---

## Overall review checklist

- Branch `assignment8`, PR into `main`, code comment-labeled by task.
- Files present in `assignment8/`: `get_books.py`, `get_books.csv`, `get_books.json`, `owasp_top_10.py`, `owasp_top_10.csv`, `ethical_scraping.txt`, `challenges.txt`.
- Method over exact values: correct page load, correct find logic, multi-author handling, XPath usage, valid CSV/JSON, and evidence of ethical pacing.
- Separate capstone repo PR link submitted in the second submission field.
```
