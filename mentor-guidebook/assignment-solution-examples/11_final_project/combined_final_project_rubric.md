# Final Project Grading Rubric (Week 11)

> **What changed:** Week 11 now grades a **single deliverable — the Web Scraping Dashboard capstone.**
> - The old standalone **Tasks 1–5** (Pandas plots, Plotly, reflection, commit) have been removed. Only an **optional, ungraded** Plotly warm-up remains — do not grade it.
> - The **Kaggle Data Pipeline** capstone is now finalized and graded at the **end of week 8**, not this week. Use `kaggle_rubric.md` when reviewing week-8 submissions.

## Web Scraping Dashboard Capstone

* **Web Scraping**

    * [ ] Uses appropriate libraries (Selenium) to retrieve data from the web
    * [ ] Handles common scraping challenges like missing tags, pagination, and user-agent headers
    * [ ] Saves raw data in a structured format such as .csv or .json
    * [ ] Avoids scraping duplication or redundant requests

* **Data Cleaning & Transformation**

    * [ ] Loads raw data into a Pandas DataFrame or equivalent structure
    * [ ] Cleans missing, duplicate, or malformed entries effectively
    * [ ] Applies appropriate transformations, groupings, or filters
    * [ ] Shows before/after stages of cleaning or reshaping

* **Data Visualization**

    * [ ] Includes at least three visualizations using Plotly, Streamlit, or Dash
    * [ ] Visuals are relevant, well-labeled, and support the data story
    * [ ] User interactions such as dropdowns or sliders are implemented
    * [ ] Visualizations respond correctly to user input or filters

* **Dashboard / App Functionality**

    * [ ] Built with Streamlit or Dash to display data and insights
    * [ ] Features clean layout and responsive components
    * [ ] Allows users to explore different aspects of the data
    * [ ] Provides clear titles, instructions, and descriptions for user guidance

* **Code Quality & Documentation**

    * [ ] Code is well-organized and split into logical sections or functions
    * [ ] Inline comments or markdown cells explain major steps or choices
    * [ ] All dependencies are listed and environment setup is reproducible
    * [ ] Comments or markdown cells explain logic
    * [ ] `README.md` includes summary, setup steps, and a screenshot

> **Note on deployment:** Streamlit Community Cloud deployment is **optional** this term. A dashboard that runs correctly with `streamlit run` locally fully satisfies the "Dashboard / App Functionality" criteria. Do not penalize a project that is not deployed to the cloud.
