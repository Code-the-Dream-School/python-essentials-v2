# Week 8 Mentor Summary: Introduction to Web Scraping

**Lesson file:** `lessons/08IntroductionToWebScraping.md`
**Assignment file:** `assignments/08IntroductionToWebScraping.md`

**Learning objective:** Students learn how web pages are structured as HTML and the DOM. They use Selenium and WebDriver Manager to load live pages and extract data with CSS selectors and XPath. They also learn to scrape ethically by reading `robots.txt` and pausing between requests.

---

## Lesson Overview

The lesson has eight sections. It explains why scraping is useful with examples such as comparing airline prices and collecting public meeting schedules. The final section has four AI practice prompts.

| Section | What students learn |
|---|---|
| **8.1 Basics of HTML and DOM** | The DOM is a tree of elements. Each element has a tag, attributes such as `class` and `href`, and content. Students open the browser developer tools on the Wikipedia "Web scraping" page and inspect the title, paragraphs, headers, and links. The lesson presents this inspection as the first step of every scraping program. |
| **8.2 Selenium and WebDriver Manager** | Students install `selenium` and `webdriver-manager`, create a Chrome driver, and load a page with `driver.get()`. The lesson explains that Selenium runs a real browser engine because modern pages use JavaScript to build their content. Students learn headless mode, CSS selectors (by tag, class, attribute, and combined), `find_element()` and `find_elements()`, `.text`, `get_attribute()`, `driver.title`, and `driver.quit()`. A short example wraps `driver.get()` in `try` / `except` / `finally`. |
| **8.3 Ethical Web Scraping** | `robots.txt` tells crawlers which parts of a site they may access. Students view Wikipedia's `robots.txt` in a browser and with Selenium (`driver.page_source`). |
| **8.4 Scraping Structured Data with XPath** | Some elements have no useful class or id. Students find the "See also" heading by id, move to its parent with the XPath `..`, and then move to the next sibling `div` with `following-sibling::div`. The links are collected into a list of dicts. |
| **8.5 Managing Requests and Handling Errors** | `driver.get()` can raise an exception, but Selenium does not raise one for HTTP errors such as 404. Students can check `driver.title` instead. Students use `time.sleep()` to pause between page requests. A retry function tries `driver.get()` up to three times, catches `TimeoutException` and `WebDriverException`, waits longer after each failure, and then gives up with a message. The lesson asks students to keep retries few and slow so that they do not overload the site. |
| **8.6 Saving Scraped Data** | Students save the list of links with `csv.writer` and with `json.dump()`. A short paragraph asks students to think about a relational data model before they store scraped data in SQL. |
| **8.7 Frailty in Web Scraping** | Scraping programs break when a site changes its layout. Starting from known text on the page can make a scraper more robust. |
| **8.8 AI Prompts** | Retrieval practice on why Selenium is needed and on `robots.txt`, a predict-then-check exercise on a CSS selector, and a scaffold-removal prompt for XPath problems. |

---

## Assignment Deliverables

**Setup:** Students create an `assignment8` branch in `python_homework` and write their code in the `assignment8` folder. Each task should have a comment label such as `# Task 3`. Students need `selenium`, `webdriver-manager`, and `pandas` installed.

**Testing:** There is no PyTest file. The programs scrape live sites, so selectors and scraped values change over time. The rubric tells reviewers to grade the method and not the exact output or selectors.

**Tasks:**

| Task | Deliverable | What it does | Concepts practiced |
|---|---|---|---|
| 1 | No file | Students read the `robots.txt` file for the library catalog site (`durhamcounty.bibliocommons.com`) and confirm that the scraping is allowed. | Ethical scraping |
| 2 | Notes in a temporary file | Students use developer tools on a library search page for "learning spanish." They record the tag and class of each search result `li`, and the elements that hold the title, the authors, and the format and year. | DOM inspection, CSS classes |
| 3 | `get_books.py` | Loads the search page, finds all result `li` elements, and builds a `dict` for each book with the keys `Title`, `Author`, and `Format-Year`. Multiple authors are joined with `;`. The dicts go into a list named `results`, which becomes a printed DataFrame. Paging through all results is optional, but a pause between pages is expected if a student attempts it. | `find_elements()`, nested searches, `.text`, `join()`, lists of dicts, pandas |
| 4 | `get_books.csv`, `get_books.json` | Writes the DataFrame to CSV and the `results` list to JSON. | `to_csv()`, `json.dump()` |
| 5 | `ethical_scraping.txt` | Answers questions about Wikipedia's `robots.txt` (restricted sections and user-agent rules) and gives 2–3 sentences on its purpose. | Ethical scraping |
| 6 | `owasp_top_10.py`, `owasp_top_10.csv`, `challenges.txt` | Uses XPath to collect the title and link of each of the OWASP Top 10 risks, prints the list, and writes it to CSV. `challenges.txt` is a short reflection on difficulties. | XPath, `get_attribute('href')`, CSV output |
| 7 | Separate capstone repository | Students create a new GitHub repository with a README for the web scraping capstone. This week they scrape their chosen site with Selenium, save raw data to CSV, and begin cleaning it in pandas. The SQLite and Streamlit parts are graded in later weeks. | Project setup, Selenium, pandas cleaning |

**Kaggle capstone:** This week is the final deadline for the Kaggle data pipeline capstone. Students polish the notebook, save a version, make it public with comments allowed, and submit the link.

**Submission:** Students open a pull request from `assignment8` into `main` in `python_homework`. The submission form has three fields: the assignment pull request, the capstone pull request, and the public Kaggle notebook link.

**Note:** Task 6 says "You will need XPath," but the lesson only teaches the `..` parent step and the `following-sibling::` axis. Students may need help writing other XPath expressions. The assignment also uses `df.to_csv()`, which the lesson does not teach (the lesson uses `csv.writer`). Task 7 lists "pagination" and "user-agent headers" as scraping challenges, and the lesson does not cover either one.

---

## FAQs and Common Trouble Spots

**1. "Chrome opens and closes, or I get an error before the page loads."**
The first run of `ChromeDriverManager().install()` downloads a driver, so it needs a network connection and can be slow. Check that Chrome is installed and that `selenium` and `webdriver-manager` are installed in the active virtual environment. If a program crashes before `driver.quit()`, a Chrome process may stay open. Encourage students to put `driver.quit()` in a `finally` block, as the lesson shows.

**2. "`find_element()` crashes with `NoSuchElementException`."**
`find_element()` raises an exception when nothing matches. It does not return `None`, so a check such as `if element:` does not protect against a missing element. The lesson shows two safer patterns: `find_elements()` with a `len()` check, and `try` / `except NoSuchElementException`. `find_elements()` returns an empty list when nothing matches, which is often easier inside a loop. Also check the timing. JavaScript may still be building the page when the code runs, so a short `sleep()` after `driver.get()` can help students see whether the problem is timing or a wrong selector.

**3. "My selector returns zero results," or "it returns the wrong elements."**
Students often copy a class attribute that contains several class names separated by spaces. In a CSS selector, each class needs its own dot, for example `li.class-one.class-two`. A space in a selector means "a descendant of," so a selector with spaces in the wrong place finds nothing. Ask students to print `len()` of the result list after each find, as the assignment hint suggests.

**4. "Some books have no author, or several authors."**
Inside the loop, students should call `find_elements()` on the current `li` element, not on `driver`. A search on `driver` returns elements from the whole page. A book with no author returns an empty list, and `";".join()` on an empty list gives an empty string. This is acceptable output.

**5. "My scraped values or class names do not match my classmate's."**
The library and OWASP pages are live and can change. The rubric says reviewers should not fail a submission because its selectors or values differ from a reference. If a student's scraper stopped working, this is a good moment to discuss the frailty topic in section 8.7.
