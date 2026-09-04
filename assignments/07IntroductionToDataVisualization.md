
# **Assignment 7: Introduction to Data Visualization**

Complete this assignment in a Kaggle notebook using a Kaggle dataset.  As usual, mark the section of code that implements each task with a markdown cell.

---

## **Task 1: Data Understanding**

1. In Kaggle, go to the Datasets link on the left side of the page.
2. Search for "Diabetes Health Indicators Dataset".  You should find one from Alex Teboul.
3. In Data Explorer on the right, click on the one that starts with "diabetes_012".
4. We need to understand particular columns.  Click on the pulldown for the 22 columns. Click on "Select All" and then "Apply".  Then select "Column" from the menu.  This explains the meaning of all the columns.  Scroll down through the information about each column.  You see that "Diabetes_012" can have 3 numeric values.  You see that "Age" is broken down into 13 buckets.  You see that "GenHlth" has values from 1 to 5, 5 being worst!  You see that PhysHlth is the number of days in the past month where the person's physical health was not good.  Again, a high number is bad!  We can't understand the plots we make **unless we know what the data means.**

## **Task 2: Creating the Notebook and Loading Data**

1. Create a Kaggle Notebook called "CTD_Assignment_7".  Add the "Diabetes Health Indicators Dataset".  Run the first cell so that you get the filenames.
2. Load the one that has "diabetes_012" in the name into a `diabetes` DataFrame.
3. Print out the first 5 lines of the DataFrame.

## **Task 3: A Histogram For Age Distributions**

1. Use Matplotlib to create a histogram of age distributions in the `diabetes` DataFrame, where the X axis is "Age" and the Y axis is "Count". Give the plot a meaningful title, and give the axes meaningful labels.
2. Show the plot to see if it is as you expect.

## **Task 4: General Health over Time**

1. Create a `health_by_age` DataFrame, using groupby() on the `diabetes` DataFrame.  Group by age.  Aggregate the `GenHlth` column using `mean`.
2. Add a column called "Health" to the resulting DataFrame.  The value of this should be 5 minus the `GenHlth` column.  It is usually more meaningful to have high values mean good and low values mean bad.
3. Sort `health_by_age` by the index.  (You use the sort_index() method.)  The index for this DataFrame is "Age", because of the groupby().
4. Use Matplotlib to create a line plot where the X axis is "Age" (the index) and the Y axis is "Health".  Add a meaningful title and axes labels.
5. Show the plot.

## **Task 5: A Heat Map of All Columns**

1. Create a correlation matrix called `diabetes_corr` for all columns.
2. Use Seaborn to create a heat map from the correlation matrix.  Note: It appears that Seaborn uses some Python deprecated features, so you will see warning messages.  You can ignore these.
3. Show the heat map.

As you will notice, this heat map is hard to read.  You solve that problem in the next task by subsetting the correlation matrix.

## **Task 6: Subset Heat Maps**

Suppose you decide that the data you care about most is in the "Diabetes_012", "HeartDiseaseorAttack", and "GenHlth" columns.

1. Create a subset correlation matrix called `diabetes_corr_subset`, selecting these columns from `diabetes_corr`.
2. Sort this in descending order on the `Diabetes_012` column.
3. Create a heatmap for the first 10 rows and show it.  These are the factors most strongly correlated with diabetes.
4. Create a heatmap for the last 10 rows, with an appropriate title, and show it.  These are the factors that are negatively correlated (or weakly correlated) with diabetes.
5. Sort the matrix again on the "GenHlth" column in descending order, and again show heat maps for the first and last 10 rows.
6. Notice the factors that are most consequential for diabetes and health, according to this dataset.  Add a markdown cell that describes these.

## **Task 7: A Pair Plot: Body Mass Index vs. Age**

1. Using the `diabetes` DataFrame, create a pair plot for "BMI" and "Age", with a `hue` of "Diabetes_012".  I find that `palette=['#FF5733', '#33FF57', '#3357FF']` gives a helpful display (this palette is only a suggestion — any clear color mapping is fine).
2. Give the pair plot a descriptive title and show it.
3. The plot is hard to read.  There are too many values for BMI.  Use `qcut()` to divide BMI into 10 quantiles, and add the resulting Series to the `diabetes` DataFrame.  Then plot Age vs. BMI Quantiles, with the same hue as before.
4. Give this pair plot a descriptive title and show it.

## **Task 8: Add visualizations to your capstone project**

Now it's time to use the visualization tools you have learned about in this lesson for your final project.  Use visualizations to help you gain insights about the data and then illustrate those insights in you project.  Add code blocks to create charts and markdown blocks to explain them and how they illustrate details about the data. This adds to the capstone project you have already built in earlier weeks — keep your existing work and build on it.

### **These are the rubrics for data visualization:**

**Visualization Quality**

    * [ ]  Creates multiple (3+) high-quality, informative, and visually appealing visualizations using appropriate libraries (e.g., Matplotlib, Seaborn, Plotly).
    * [ ]  Visualizations are clear, concise, and easy to understand, with appropriate titles, labels, legends, and color schemes.
    * [ ]  Demonstrates strong understanding of design principles.
    * [ ]  Provides clear explanations of the insights conveyed by each visualization.

**Chart Types and Interpretation**

    * [ ]  Uses a diverse range of chart types (e.g., scatter plots, bar charts, histograms, box plots, heatmaps) to provide a comprehensive view of the data.
    * [ ]  Demonstrates a clear understanding of the strengths and weaknesses of each chart type and selects them strategically.
    * [ ]  Provides insightful interpretations of the visualizations, connecting them to the data analysis and the problem domain.

Once you have added visualizations, your Kaggle data pipeline project is complete.  You will submit a Kaggle link this week to show your progress.  **You have until the end of next week (week 8) to finalize and submit your Kaggle data pipeline project — this is its final deadline.**  Next week you will also start your second capstone project, which involves web scraping, a SQL database, and interactive, web-based visualizations.  That web scraping capstone becomes your main focus for the rest of the course, so wrapping up the Kaggle project next week keeps the two from competing at the end of the term.

### **Submit the Notebook for Your Assignment**  

📌 **Follow these steps to submit your work:**  

#### **1️⃣ Get a Sharing Link for Your Assignment**  
- On the upper right of the Kaggle page, click on Save Version and save, accepting all defaults.  You can just do a quick save.
- On the upper right, click on Share.  Choose Public, make sure that Allow Comments is on, and copy the public URL to your clipboard.

#### **2️⃣ Submit Your Kaggle Links**  
You will submit two links this week.  One for the homework assignment and one for your final project updated with visualizations.
- Paste the URLs into the two link submissions fields in the **assignment submission form**.  

---

<details>
<summary>Rubric (for AirHub reviewer and mentors)</summary>

This assignment is a **Kaggle notebook** (dataset: Alex Teboul's "Diabetes Health Indicators Dataset"), not a `python_homework` PyTest assignment. There are no automated tests — the deliverables are charts, reviewed by looking at the code and the rendered plots. For every plot, check three things: the **right chart type**, the **right data on the axes**, and a **title plus axis labels**. **Be lenient about:** figure size, colors/`palette`, bin counts, exact title/label wording, `cmap`/`annot` choices, and the Kaggle input file path (the reviewer cannot see the filesystem).

### Required Deliverables/Tasks

- **Task 1 — Data Understanding** — Reading/exploration in Kaggle's Data Explorer. No code is required — do not fail this task for having no code. (A written markdown note about the columns is Optional — see below.)
- **Task 2 — Load data** — Load the file whose name contains **`diabetes_012`** (not the `binary` or `5050` files) into a DataFrame named `diabetes`, then show `head()`. `diabetes` → `Use exactly as written (later tasks depend on these names)`. The load path → `Example — adapt to your own layout` (do not fail a differing path).
- **Task 3 — Age histogram** — A **Matplotlib histogram** of the `Age` column, with a title and labeled axes (X = Age, Y = Count). Must be a histogram of the single `Age` column, not a bar chart of an aggregate. Bin count is `Example — adapt to your own layout`.
- **Task 4 — General health by age** — Create `health_by_age` via `groupby("Age")` aggregating `GenHlth` by **mean**; add a `Health` column = **`5 - GenHlth`**; `sort_index()`; then a **line plot** with Age (the index) on X and `Health` on Y, titled and labeled. `health_by_age` and the `Health` column → `Use exactly as written (later tasks depend on these names)`. The `5 - GenHlth` inversion is the graded point.
- **Task 5 — Full correlation heat map** — Create `diabetes_corr` (`diabetes.corr()`) and render it with `sns.heatmap`. `diabetes_corr` → `Use exactly as written (later tasks depend on these names)`. Seaborn deprecation warnings are expected — do not penalize them. The cramped result is intentional. `figsize`, `annot`, `cmap` → `Example — adapt to your own layout`.
- **Task 6 — Subset heat maps** — Create `diabetes_corr_subset` by selecting **`Diabetes_012`, `HeartDiseaseorAttack`, `GenHlth`** from `diabetes_corr`; sort **descending** on `Diabetes_012` and show heatmaps of the first 10 and last 10 rows; then re-sort **descending** on `GenHlth` and again show first-10 and last-10 heatmaps (four total, each titled); plus a **markdown cell (6.6)** naming the strongest factors. `diabetes_corr_subset` and the three column names → `Use exactly as written (later tasks depend on these names)`. The markdown analysis wording and titles → `Example — adapt to your own layout`.
- **Task 7 — Pair plots (BMI vs. Age)** — `sns.pairplot` of `BMI` and `Age` with **`hue="Diabetes_012"`**; then `pd.qcut(diabetes["BMI"], 10)` to bin BMI into 10 quantiles, add it as a column, and re-plot Age vs. the BMI quantiles with the same hue. Both plots titled. `hue="Diabetes_012"` and the `qcut(..., 10)` binning → `Use exactly as written (later tasks depend on these names)`. The `palette` hex values and any quantile column name → `Example — adapt to your own layout` (any clear color mapping is fine).
- **Task 8 — Visualizations in the capstone project** — Add **3 or more** clear, appropriately chosen charts of a **variety of types** (e.g. histogram, bar, scatter, box, heatmap — not three of the same) to the student's own capstone notebook, each with a title, labeled axes, and a legend/color scheme where relevant, plus **markdown explanations** of the insight each chart reveals. This is **cumulative capstone work that builds on prior weeks** — do not fail the student for keeping earlier capstone code, and judge **progress, not perfection** (the Kaggle capstone has until the end of week 8 to be finalized). Everything about the capstone's own data, layout, and chart choices → `Example — adapt to your own layout`.
- **Submission** — Two Kaggle sharing links: one for this homework notebook (`CTD_Assignment_7`), one for the capstone updated with visualizations. Notebook saved and shared **Public with comments on**. Do not fail for exact notebook name or path.

### Optional Deliverables/Tasks

- **Task 1 markdown note** — A written markdown note summarizing what the key columns mean (`Diabetes_012` has 3 values; `Age` is 13 buckets, not raw years; high `GenHlth`/`PhysHlth` is worse). Credit it if present, but **do not fail Task 1 for omitting it** — Task 1 is a reading task with no required code.

There are no stretch/bonus/advanced tasks in this assignment. Values that must not be failed if the student differs: the Task 7 `palette`, all figure sizes, histogram bin counts, `cmap`/`annot` choices, exact chart titles and axis-label wording, and the Kaggle input file path.

</details>
