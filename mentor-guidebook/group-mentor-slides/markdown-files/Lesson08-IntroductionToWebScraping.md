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

# Lesson 8: Introduction to Web Scraping

**Python Essentials — Code the Dream**

---

## Game Plan for Today

| Section | Time | What We'll Do |
|---------|------|---------------|
| Warm-Up | 5 min | Explore a web page's DOM |
| I Do | 10–15 min | HTML basics, Selenium, CSS selectors, ethics |
| We Do | 15–20 min | Scrape Wikipedia links together |
| You Do | 5–10 min | Find elements on a page |
| Wrap-Up | 5 min | Assignment preview & capstone intro |

<!-- Mentor note: This lesson is very different from the Pandas lessons — it's about pulling data FROM the web. Students who've done any HTML/CSS will have a head start. For those who haven't, the DOM concept needs extra attention. Try to do a live demo with browser dev tools if possible. -->

---

## Warm-Up (5 min)

### Inspect a Web Page

1. Open any website in Chrome
2. Right-click on a heading → **Inspect**
3. Look at the **Elements** tab in Developer Tools

**Find these things:**
- The `<title>` tag
- An `<a>` tag (link) — what's its `href` attribute?
- A `<p>` tag — what text does it contain?

**Share what you found in the chat.**

<!-- Mentor note: This hands-on warm-up gets students into the browser dev tools immediately. If students haven't used Inspect before, walk them through it. This is the #1 skill for web scraping — knowing where the data lives in the DOM. -->

---

## I Do: Why Web Scraping?

There's a lot of data on the internet — but it's trapped in web pages meant for humans.

**Web scraping** extracts that data into a structured format you can analyze.

**Real-world examples:**
- Comparing prices across airline websites
- Collecting research article metadata
- Gathering public meeting schedules
- Building a dataset that doesn't exist as a CSV

> Scraping turns **unstructured web pages** into **structured data** (CSV, JSON, database).

---

## I Do: HTML and the DOM

**HTML** is the structure of every web page. The **DOM** is its tree:

```html
<html>
  <head>
    <title>My Page</title>
  </head>
  <body>
    <h1>Welcome</h1>
    <p>Hello world</p>
    <a href="https://example.com">A link</a>
  </body>
</html>
```

**Each element has:**
- A **tag** (`p`, `h1`, `a`, `div`)
- **Attributes** (`href`, `class`, `id`)
- **Content** (text or child elements)

<!-- Mentor note: If students have done web development, this is review. If not, stress that the DOM is a tree — elements contain other elements. The assignment requires students to navigate this tree to find specific data. -->

---

## I Do: Selenium — The Scraping Tool

**Selenium** drives a real browser — it handles JavaScript, dynamic content, and Ajax:

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.common.by import By

driver = webdriver.Chrome(
    service=Service(ChromeDriverManager().install())
)
driver.get("https://en.wikipedia.org/wiki/Web_scraping")
print(driver.title)
driver.quit()
```

**Why Selenium?** Modern web pages load data with JavaScript. A simple HTTP request won't get the full page — Selenium runs the JavaScript just like a real browser.

<!-- Mentor note: Students often ask "why not just use requests?" The key answer: most modern pages are dynamic. Selenium opens a real browser (or headless browser) that executes JS, making the full page available for scraping. -->

---

## I Do: Finding Elements with CSS Selectors

**CSS selectors** let you find elements in the page:

```python
# By tag name
paragraphs = driver.find_elements(By.CSS_SELECTOR, "p")

# By class
items = driver.find_elements(By.CSS_SELECTOR, ".result-item")

# By ID
header = driver.find_element(By.CSS_SELECTOR, "#main-header")

# By attribute
images = driver.find_elements(By.CSS_SELECTOR, "img[src]")

# Combined
links = driver.find_elements(By.CSS_SELECTOR, "div.content a")
```

| Selector | Matches |
|----------|---------|
| `p` | All `<p>` elements |
| `.classname` | Elements with that class |
| `#id` | Element with that ID |
| `div a` | `<a>` inside `<div>` |

---

## I Do: Getting Data from Elements

**Once you find an element, extract its data:**

```python
# Get the visible text
element.text

# Get an attribute value
element.get_attribute("href")
element.get_attribute("src")
element.get_attribute("class")

# Search within an element (narrow your scope)
parent = driver.find_element(By.CSS_SELECTOR, ".results")
children = parent.find_elements(By.CSS_SELECTOR, "li")
```

**Pattern:**
1. Find a container element
2. Search *within* it for specific children
3. Extract text or attributes

> Always narrow your search — find the container first, then search inside it.

---

## I Do: Ethical Web Scraping

**Before scraping ANY site, check `robots.txt`:**

```
https://example.com/robots.txt
```

**`robots.txt` tells you:**
- Which pages you're allowed to scrape
- Which pages are off-limits
- How frequently you should make requests

**Other ethical guidelines:**
- **Add delays** between requests (`time.sleep(2)`)
- **Don't overload** the server with rapid requests
- **Respect** the site's terms of service
- **Handle errors gracefully** with try/except

<!-- Mentor note: Assignment Task 1 has students check robots.txt for the Durham County Library site. Task 5 has them analyze Wikipedia's robots.txt. Stress that ethical scraping is not optional — it's a professional responsibility. -->

---

## I Do: Handling Errors and Headless Mode

**Always wrap `driver.get()` in try/except:**

```python
try:
    driver.get("https://example.com")
    # ... extract data ...
except Exception as e:
    print(f"Error: {type(e).__name__}: {e}")
finally:
    driver.quit()
```

**Run without a visible browser window (headless):**

```python
options = webdriver.ChromeOptions()
options.add_argument("--headless")

driver = webdriver.Chrome(
    service=Service(ChromeDriverManager().install()),
    options=options
)
```

> Use headless mode in scripts. Use the visible browser while debugging.

---

## I Do: XPath for Tricky Navigation

**When CSS selectors aren't enough, use XPath:**

```python
# Go up to the parent element
parent = element.find_element(By.XPATH, "..")

# Find the next sibling div
next_div = parent.find_element(
    By.XPATH, "following-sibling::div"
)
```

**When to use XPath:**
- You need to go **up** the tree (parent)
- You need a **sibling** element (no direct CSS path)
- The element has no useful class or ID

> CSS selectors cover 90% of cases. XPath handles the tricky 10%.

<!-- Mentor note: Assignment Task 6 (OWASP Top 10) requires XPath. The lesson covers the Wikipedia "See also" example which uses the same parent → sibling pattern. Don't spend too much time here — just make sure students know XPath exists for when CSS selectors fail. -->

---

## We Do: Scrape Wikipedia Links

**Let's extract all links from the "See also" section together.**

**Step 1:** Set up and load the page:

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.common.by import By

driver = webdriver.Chrome(
    service=Service(ChromeDriverManager().install())
)
driver.get("https://en.wikipedia.org/wiki/Web_scraping")
```

**Step 2:** Find our starting point:

```python
see_also = driver.find_element(
    By.CSS_SELECTOR, '[id="See_also"]'
)
```

<!-- Mentor note: Walk through this step by step. Show the dev tools to find the "See_also" ID. Explain why we start with a known anchor point and navigate from there. -->

---

## We Do: Navigate and Extract

**Step 3:** Use XPath to find the links container:

```python
parent = see_also.find_element(By.XPATH, "..")
links_div = parent.find_element(
    By.XPATH, "following-sibling::div"
)
```

**Step 4:** Extract all links:

```python
links = []
for a in links_div.find_elements(By.CSS_SELECTOR, "a"):
    name = a.text.strip()
    url = a.get_attribute("href")
    if name and url:
        links.append({"name": name, "url": url})
        print(f"{name}: {url}")

driver.quit()
```

**Discussion:** Why do we check `if name and url`?

> Some link elements might be empty or have no href — always validate.

---

## We Do: Save the Scraped Data

**Step 5:** Save to CSV and JSON:

```python
import csv
import json

# Save to CSV
with open("scraped_links.csv", "w", newline="") as f:
    writer = csv.writer(f)
    writer.writerow(["Name", "URL"])
    for link in links:
        writer.writerow([link["name"], link["url"]])

# Save to JSON
with open("scraped_links.json", "w") as f:
    json.dump({"links": links}, f, indent=4)
```

> The scraping pattern: **load → find → extract → save**.

<!-- Mentor note: Assignment Tasks 3-4 follow this exact pattern — scrape Durham Library search results, save to CSV and JSON. Task 6 scrapes OWASP Top 10 to CSV. The pattern is always the same. -->

---

## We Do: The Scraping Workflow

```
1. Check robots.txt          → Am I allowed?
2. Open dev tools             → Where is the data?
3. Initialize Selenium        → Set up the driver
4. Load the page              → driver.get(url)
5. Find container elements    → CSS selectors
6. Navigate if needed         → XPath for tricky cases
7. Extract text/attributes    → .text, .get_attribute()
8. Store in a list of dicts   → Structured data
9. Save to CSV/JSON           → Persistent storage
10. Close the driver          → driver.quit()
```

> Follow this workflow for every scraping task.

---

## You Do: Find Elements Challenge (5 min)

**Open the browser dev tools on any website and find:**

1. The page `<title>` — what does it say?
2. An element with a **class** attribute — what's the class?
3. A link (`<a>`) — what's the `href`?
4. An image (`<img>`) — what's the `src`?

**Write the CSS selector you'd use to find each one.**

<!-- Mentor note: Set a 5-minute timer. This is a dev tools exercise, not a coding exercise. The goal is to get comfortable navigating the DOM and writing selectors. If students finish early, challenge them to find a nested element (e.g., a link inside a specific div). -->

---

## Assignment Preview

| Task | What You'll Do |
|------|---------------|
| 1 | Check `robots.txt` for the Durham Library site |
| 2 | Use dev tools to find title, author, format elements |
| 3 | Write `get_books.py` — scrape search results |
| 4 | Save results to CSV and JSON |
| 5 | Analyze Wikipedia's `robots.txt` |
| 6 | Scrape OWASP Top 10 vulnerabilities (uses XPath) |
| 7 | **Start your web scraping capstone project!** |

<!-- Mentor note: The Durham Library scraping (Tasks 2-4) is the main assignment. Task 6 (OWASP) is a second scraping exercise that requires XPath. Task 7 starts the second capstone project — a web scraping + SQL + Streamlit dashboard. -->

---

## Assignment Tips

**Task 2 (DOM exploration):** Use Chrome dev tools. Click the arrow next to a `div` to expand it. Keep drilling down until you find the `li` element for a single search result.

**Task 3 (get_books.py):** Build incrementally:
1. First, just get the page and print the title
2. Then find all `li` elements and print the count
3. Then extract just the title from each `li`
4. Then add author and format

**Task 6 (OWASP):** You'll need XPath to navigate from the heading to the links.

---

## Capstone Project #2 Kickoff

Starting this week, you're building a **Web Scraping Dashboard**:

| Week | What You Build |
|------|---------------|
| 8 (this week) | Scrape data + clean + save to CSV |
| 9 | Store in SQLite database |
| 10 | Advanced SQL queries |
| 11 | Streamlit interactive dashboard |

**Recommended site:** Weather Around The World (timeanddate.com)

> Create a new GitHub repo for this project — it's separate from `python_homework`.

<!-- Mentor note: This is the second capstone. Students need to create a new repo, examine the weather site, and start scraping this week. Emphasize that the project builds incrementally over 4 weeks. -->

---

## Web Scraping Fragility

**Warning:** Scraping programs are **fragile**.

- Websites change their HTML structure without notice
- Your selectors may stop working overnight
- Different pages on the same site may have different layouts

**Best practices:**
- Use broad selectors when possible
- Add error handling for missing elements
- Log errors so you know when something breaks
- Don't rely on deeply nested selectors

---

## Wrap-Up

**Today we covered:**
- HTML structure and the DOM tree
- Selenium + WebDriver for browser automation
- CSS selectors and XPath for finding elements
- Extracting text and attributes from elements
- Ethical scraping: `robots.txt`, delays, error handling
- Saving scraped data to CSV and JSON

**Quick check:** Before scraping any website, what's the first thing you should do?

<!-- Mentor note: Answer: Check robots.txt. This reinforces the ethical scraping habit. -->

---

## Getting Help This Week

- **Browser dev tools** are your best friend — practice with Inspect
- Build your scraper **one step at a time** — don't try to do everything at once
- Selenium docs: [selenium.dev/documentation/webdriver](https://www.selenium.dev/documentation/webdriver/)
- Post questions in **#discussion** on Slack
- Book a **1:1 mentor session** for help with selectors or XPath

**Next week:** Introduction to Databases and SQL — storing your data properly!

---

<!-- _paginate: false -->

# Time to Scrape! 🕸️

**The web is full of data — now you know how to get it.**

See you next session!
