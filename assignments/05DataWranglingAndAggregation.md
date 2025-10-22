## Lesson 5 Assignment — Data Wrangling and Manipulation
### Advanced Data Wrangling and Aggregation with Pandas

### **Objective:**
The purpose of this assignment is to deepen your understanding of data wrangling and aggregation using the Pandas library in Python. You will work with sample DataFrames to perform various operations such as filtering, handling missing values, merging, sorting, transforming, pivot tables and apply().

Note: Tasks 3–4 are mini-labs (independent datasets) and pair together; These use independent Kaggle datasets; do them together, then resume Task 5.
### **Setup**

The assignments up to this one have required you to create `.py` files and to submit them by creating pull requests for your python_homework repository.  For this assignment, you create a Jupyter notebook file.  Jupyter notebooks are a way to do data presentation and analysis, using Python code.  A notebook is comprised of a sequence of cells, which come in two kinds: Markdown cells, for putting in the text you want to show, and code cells, where you put your Python code.

With a little setup, you can create Jupyter notebooks in VSCode, and submit them to GitHub.  However, GitHub is not a friendly environment for collaboration on notebooks.  Your reviewer wants to see the notebooks, to run them, and to give you comments in context.  For that purpose, we use [https://kaggle.com].  That site also has an interesting collection of data sets you can use for practice in data analysis and presentation.  Connect to the site now and register, so that you have an account.

### **Tasks:**

### **Task 1: Data Selection**
1. **On the Kaggle site, click on the plus button in the upper left, and create a notebook called CTD_Assignment_5.**  It comes up with a code cell already present.  Leave that one alone, and click on the plus markdown button to add a cell that says "Task 1". You do not have to use markdown formatting directives, however if you do choose to use formatting, it's worth noting that level two headings starting with '## ' are automatically added to the table of contents.  This is how you convey information about your code to your reviewer, Jupyter notebook style.  After adding a markdown cell, click on the plus code button to add another cell.  You add the code for this task to the cell.  As you complete each of the following tasks, run the cell to make sure your code works.  You run the cell by clicking on the arrow at the top left of the cell.

**Note:** The various code cells in a Jupyter notebook are all part of the same program, so you have access to the variables and functions of one cell from each of the ones that follow.  You only need to import Pandas once, for example.  However, Kaggle sessions **time out** if you go to the kitchen for a sandwich or something.  When they time out, your variables go away.  So, if you then run cell 2, which is dependent on something in cell 1, you'll get an error.  To correct this, click on the Run All button at the top of your Kaggle notebook screen, and the entirety of the program runs in the order the cells appear.

2. **Create DataFrames `df1`, `df2`, and `df3` using the provided sample data(feel free to change the values):**
   - `df1` contains names, ages, and salaries of five employees.
   - `df2` contains names, ages, and salaries of five other employees.
   - Display each DataFrame.
   
```
data1 = {
    'Name': ['Alice', 'Bob', 'Charlie', 'David', 'Eva'],
    'Age': [25, 30, 35, 40, 30],
    'Salary': [50000, 60000, 70000, 80000, 55000]
}
```

```
data2 = {
    'Name': ['Frank', 'Grace', 'Helen', 'Ian', 'Jack'],
    'Age': [28, 33, 35, 29, 40],
    'Salary': [52000, 58000, 72000, 61000, 85000]
}

data3 = {
    'Name': ['Frank', 'Helen', 'Ian', 'Hima', 'Chaka'],
    'Age': [17, 93, 12, 57, 106],
    'Favorite Color': ['blue', 'pink', 'burgundy', 'red', 'turquoise']
}
```
Print the resulting dataframes. 

3. **Perform the following selection operations on `df1`:**
   - Select the 'Name' column, and print the result.
   - Select both 'Name' and 'Salary' columns, and print the result.
   - Slice the first three rows using integer-based indexing (`iloc`), and print the result.

### **Task 2: Data Aggregation**

Again, you create a markdown cell to describe this task, and a code cell containing the code for the task.  Do this throughout, whenever you are doing your homework in Jupyter notebooks.

1. **Group `df1` by 'Age' and aggregate the 'Salary' column:**
   - Calculate the mean, sum, and count of the salary for each age group.
   - Display the aggregated results.

### **Task 3: Practice Pivot Tables**

1. In the upper right of your notebook page click on "Add Input" and then "Datasets".  Search on "Ecommerce Consumer Behavior".  You should find a dataset from Salahuddin Ahmed.  Click on the plus button to add this one to your notebook.

2. Resolve the file you need to read, by running the first cell in your notebook. Then read the file into a DataFrame called `ecommerce`.  Print out the first 5 rows, so that you know what the data looks like.

3. In the following step, you will want to sum the Purchase_Amount values.  Unfortunately, this column is stored as a string.  Replace the column with one that converts the string to a float.  You will need to take the dollar sign off before conversion.

4. Create a "buying_patterns" pivot table from the data.  The index should be the 'Purchase_Category'.  The columns should be 'Gender' and 'Income_Level'.  The value you sum should be 'Purchase_Amount'.  Print out the resulting DataFrame.

5. Create a "demographics" pivot table from the ecommerce DataFrame.  You want two levels of index, on 'Income_Level' and 'Education_Level'.  For columns you want 'Marital_Status'.  You want to count the 'Customer_ID' values.  Print out the resulting DataFrame.

### **Task 4: Practice apply()**

1. Add another input.  This time, you search for "AI-Powered Job Recommendations".  You should find a dataset from Samay Ashar.  

2. Create a new code cell.  Add code to resolve the name of the file you need to read.  For this part, you use the job recommendations file.  Read it into a DataFrame called "jobs".  Print out the first 10 lines, so that you know what the column names are.

3. Use the `apply()` method to create an additional column in the jobs DataFrame called 'Check These Out'.  Give that a value of "Yes" if the job is entry level, has a salary greater than or equal to 70000, and requires both SQL and Python.  Give the column a value of "No" otherwise.

4. Create a DataFrame called "my_jobs".  Select the rows from "jobs" that have a 'Check These Out' column value of 'Yes'.  Print out the first 10 lines.

### **Task 5: Merging and Joining DataFrames**
1. **Merge `df1` and `df3` into `df_1_3_merged` on the 'Name' column:**
   - Use an outer merge to combine the two DataFrames and handle any missing data.
   - Use the suffixes `_left` and `_right` to differentiate columns from each DataFrame. (You specify `suffixes=['_left','_right']` on the call to merge.)
   - Display the result with print(). When you do, you will see some annoying warning messages that contain words like "Runtime Warning: invalid value encountered ...".  This is because of the NaN values in the frame.  You could suppress these warnings by 
      ```python
      np.warnings.filterwarnings('ignore')
      ```
      But, don't do this yet.  You might see some other warnings if you make a mistake, and you don't want to miss them.  Just ignore these particular warnings.
   - We see that the 'Salary' column has `NaN` values.  Transform this column to replace `NaN` with the starting salary of 15000.  Hint: fillna() can be used on a Series.
   - Also, transform the 'Favorite Color' column to replace `NaN` values with 'yellow'.
   - Display the result.
   - We have `NaN` for some of the entries in the 'Age_left' column.  For other rows, the `NaN` value is in the 'Age_right' column.  We want to create a new `Age` column, using the values from 'Age_left' if they are not `NaN`, and from 'Age_right' otherwise.  We'll use np.where().  As this is a new technique, the answer is as follows:
       ```python
       df_1_3_merged['Age'] = np.where(df_1_3_merged['Age_left'].notna(),df_1_3_merged['Age_left'], df_1_3_merged['Age_right'])
       # This works like a ternary expression.  If the expression passed to np.where is True, you get the left value, otherwise the right value.
       ```
   - Display the result.
   - Drop the 'Age_left' and 'Age_right' columns and display the result.

2. **Use the Join Method:**
   - Create new DataFrames df1_b and df3_b from df1 and df3.  In these new DataFrames, set 'Name' as the index.
   - Join the DataFrames with outer join logic and display the result.  Do not use `inplace=True`.  Unlike the merge method, the join method does not provide default suffixes if there are overlapping columns.  Check the online documentation to find out how to specify them.

### **Task 6: Filtering Rows Based on Conditions**
1. **Filter rows in `df1` where 'Age' is greater than 30:**
   - Display the filtered rows.

### **Task 7: Sorting Data**
1. **Sort `df1` by the 'Salary' column in descending order:**
   - Display the sorted DataFrame.

### **Task 8: Renaming Columns**
1. **Rename columns in `df1`:**
   - Rename 'Age' to 'Employee Age' and 'Salary' to 'Employee Salary'.  Do not use `inplace=True`, because then you wouldn't be able to do Task 9.
   - Display the DataFrame with the renamed columns.

### **Task 9: Data Transformation**
1. **Apply a transformation to the 'Salary' column in `df1`:**
   - Increase the salary by 10% for each employee.
   - Display the updated DataFrame.

### **Task 10: Concatenating DataFrames**
1. **Concatenate `df1` and `df2` to add the rows of `df2` to the end of `df1`**
   - Use `ignore_index=True` to reset the index.
   - Display the result.

### **Task 11: Data Wrangling a Kaggle Dataset**

Kaggle has some nice datasets you can use in exercises.  These are `csv` files.  We are going to do some data wrangling on one of those provided files. For this task, we are going to find the international football teams that are especially bad on defense.
- On the upper right of your notebook, click on 'Add Input'.  Click on the 'Datasets' button.  Then do a search on 'international football results'.  You should see one from Mart Jürisoo.  Click on the plus sign next to that one.  That adds the dataset to your notebook, so that you can read the CSV files.
- Create a markdown cell for Task 11, and then create a new code cell for the following steps.
- You need to find the available CSV file path names.  The first cell in your notebook, the one it started out with, has the following code:
    ```python
    import pandas as pd
    import os
    for dirname, _, filenames in os.walk('/kaggle/input'):
        for filename in filenames:
            print(os.path.join(dirname, filename))
    ```
   Click on this cell to make it active, and run the cell.  This will list, among others, the path `/kaggle/input/international-football-results-from-1872-to-2017/results.csv`.  This is the one you want.  Read it into a DataFrame called football_results.

- Print the first 5 lines of this file.
- All the entries have a home team and an away team.  This is kind of clumsy for us, because we want results for each team whether they were home or away.  So, we'll create a new DataFrame that organizes the in that way.  First, create a DataFrame called results_1.  You select the following columns from football_results: 'home_team','away_team','home_score','away_score',  and 'date'.  Print out the first 5 lines.
- Next, create a DataFrame called results_2 from results_1.  You rename the column for 'home_team' to 'team', for 'away_team' to 'opponent', for 'home_score' to 'points_for', and for 'away_score' to 'points_against'.  Do not use `inplace=True`.  This dataset gives all the entries for the home teams.  Print out the first 5 lines.
- Next, create a DataFrame called results_3 from results_1.  This also renames the columns, but now the rename is: 'away_team' becomes 'team', 'home_team' becomes 'opponent', 'away_score' becomes 'points_for', and 'home_score' becomes 'points_against'.  This dataset gives all the entries for the away teams.  Print out the first 5 lines.
- Concatenate the results, resetting the index.  Store the result in football_results.  Print out the first 5 lines.  Now we have all the entries per team.
- Do a `groupby()` on 'team'.  Get the mean() of the 'points_against' column.  Store the result (it is a Series) in the variable points_against.
- Sort points_against so the values are descending.  Print out the first 10 lines.  These are the teams that are very bad on defense.

### **Task 12: More Data Wrangling for Football Results**

This time, you'll have to figure out the steps.  Starting with the football_results DataFrame you created in Task 11, print out the most recent 10 games for Tunisia.  Remember to sort these so that you get the right games.  Avoid use of "in_place=True", as you may get annoying warnings.  It is often better to create a new DataFrame and store the result.

### **Task 13: Start Your Final Project**

Now that you have worked with pandas and gotten started with data cleaning and aggregation, it's time to start your final project.  Throughout the course, you'll apply the techniques you have learned incrementally to your project.  For this week, you will create a Kaggle notebook for you data pipeline final project.  The notebook name is up to you but you might use something like **CTD Python Data Pipeline**.

<details>
<summary>Here are the details on the Kaggle Data Pipeline Capstone Project</summary>

# **Capstone Project - Kaggle Data Pipeline**

Select one of the following datasets and perform cleaning, aggregation, analysis and visualization, illustrating at least 3 insights gleaned from the data.  Optionally, students can propose an alternative dataset and set of metrics to their CIL by the end of week 13.  The projects are intentionally open-ended in that it is up to the student to decide what they learn from the data, and how it is documented and visualized.

## **Global Superstore**

The [Global Superstore](https://www.kaggle.com/datasets/anandaramg/global-superstore/data) is a collection of sales data from a large retailer.  The goal is to find, document and visualize at least three business insights from the data.

1. **Data Conversion and Cleaning**

Load the data into a ``DataFrame`` using the appropriate delimiter.  Evaluate data quality, looking for issues such as duplicates, null values, and mixed formats.  If these are found, decide whether they signify an issue which needs to be corrected.  Some potential cleaning tasks include:

* Convert dates to ``datetime``
* Strip whitespace
* Unify abbreviations such as state names
* Convert numbers to ``float``

Document the data cleaning performed in a Markdown block

2. **Feature Engineering**

Create at least three additional columns which can be used to derive insights from the data.  For example:

* Columns for just ``year`` and ``month`
* Derive ``Gross Margin`` from ``Profit`` and ``Sales``
* Discretize discounts into buckets such as ``none``, ``low``, ``medium``, ``high``

3. **Aggregation**

Perform at least three aggregations to help drive insights.  For example:

* top/bottom 10 states by total profit
* Category ``Gross Margin``
* ``Customer Lifetime Value`` (``CLV``) - Total profit each customer has generated across all orders

4. **Analyze, Document, and Visualize**

Create at least three plots and associated metrics to illustrate the insights found in the data.
Include ``Markdown`` sections which explain the graphs and analysis.

## **TMDB 5000 Movie Dataset**

The [TMDB 5000 Movie Dataset](https://www.kaggle.com/datasets/tmdb/tmdb-movie-metadata?select=tmdb_5000_movies.csv) is a collection of movies with a variety of data about production along with ratings.  The goal is to find the factors which influence critical and financial success.

1. **Data Conversion and Cleaning**

Load the data into a ``DataFrame``.  Evaluate data quality, looking for issues such as duplicates, null values, composite data which could be expanded into additional columns, and mixed formats.  If these are found, decide whether they signify an issue which needs to be corrected.  Some potential cleaning tasks include:

* Convert dates to ``datetime``, year vs date consistency
* Handle null values if necessary
* Numeric conversion e.g. ``revenue`` to ``float``
* Expand/convert ``json`` formated columns, e'g' ``genres``, ``keywords``, ``production companies``
* This is an explanation of the [format](https://www.kaggle.com/code/sohier/tmdb-format-introduction)

Document the data cleaning performed in a Markdown block

2. **Feature Engineering**

Create at least three additional columns which can be used to derive insights from the data.  For example:

* Can be satisfied by expanding ``json`` columns
* Derive ``Gross Margin`` from ``Profit`` and ``Sales``
* Discretize discounts into buckets such as ``none``, ``low``, ``medium``, ``high``

3. **Aggregation**

Perform at least three aggregations to help drive insights.  For example:

* top/bottom 10 directors/actors/studios by rating(average vote)/revenue/budget/profit
* return on investment profit vs budget
* Discretize budgets

4. **Analyze, Document, and Visualize**

Create at least three plots and associated metrics to illustrate the insights found in the data.
Include ``Markdown`` sections which explain the graphs and analysis.  Examples:

* Best ``genres`` for a given success metric
* Impact of particular actors and directors on success metrics
* Genre popularity derived from vote count

## **Life Expectancy (WHO)**

The [Life Expectancy (WHO)](https://www.kaggle.com/datasets/kumarajarshi/life-expectancy-who/data) is a collection of data by country and year with a variety of factors which may impact health.  The goal is to find, document and visualize at least three insights into the factors which influence longevity.

1. **Data Conversion and Cleaning**

Load the data into a ``DataFrame``.  Evaluate data quality, looking for issues such as duplicates, null values, and mixed formats.  If these are found, decide whether they signify an issue which needs to be corrected.  Some potential cleaning tasks include:

* Missing values as blanks or zeros
* Conversion to numeric types
* Unify country names, some appear in different forms
* Outliers

Document the data cleaning performed in a Markdown block

2. **Feature Engineering**

Create at least three additional columns which can be used to derive insights from the data.  For example:

* Adult vs infant mortality
* Discretize factors such as GDP
* Combined vaccination rate

3. **Aggregation**

Perform at least three aggregations to help drive insights.  For example:

* Global life-expectancy trend
* Trends for developing vs developed
* Life expectancy deciles (10 buckets) vs various driving factors

4. **Analyze, Document, and Visualize**

Create at least three plots and associated metrics to illustrate the insights found in the data.
Include ``Markdown`` sections which explain the graphs and analysis.

## **Seattle Airbnb Open Data (2016)**

The [Seattle Airbnb Open Data (2016)](https://www.kaggle.com/datasets/airbnb/seattle/data?select=listings.csv) is a collection of data on Airbnb listing in Seattle.  The goal is to find, document and visualize at least three insights into the factors which influence listing success, or conclusion which can be drawn about Seattle such as desirability or affluence for particular neighborhoods.

1. **Data Conversion and Cleaning**

Load the data into a ``DataFrame``.  Evaluate data quality, looking for issues such as duplicates, null values, and mixed formats.  If these are found, decide whether they signify an issue which needs to be corrected.  Some potential cleaning tasks include:

* Columns with a large percentage of nulls
* Numeric formatting and conversion
* Conversion of ``json``
* Text with html tags

Document the data cleaning performed in a Markdown block

2. **Feature Engineering**

Create at least three additional columns which can be used to derive insights from the data.  For example:

* Derived from ``json`` columns
* revenue per month
* Simplified room type
* Availability ratio

3. **Aggregation**

Perform at least three aggregations to help drive insights.  For example:

* Median price by neighbourhood & simplified room type
* Review score vs. amenity count
* Monthly availability
* 10 most/least affluent neighborhoods and associated profitability

4. **Analyze, Document, and Visualize**

Create at least three plots and associated metrics to illustrate the insights found in the data.
Include ``Markdown`` sections which explain the graphs and analysis.

</details>

<details>
<summary>This is the Kaggle Final Project Grading Rubric</summary>
 
 # Kaggle Project Rubric

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
    * [ ]  At least three extracted features

* **Data Aggregation**

    * [ ]  Uses Pandas `groupby()` function effectively to aggregate data and gain insights.
    * [ ]  Applies a variety of aggregation functions (e.g., `sum()`, `mean()`, `count()`, `min()`, `max()`) to analyze grouped data.
    * [ ]  Clearly presents and interprets the results of data aggregation.
    * [ ]  At least 3 aggregations

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

### **This week's capstone project goals**
For this week, you are just getting started.  Create your Kaggle notebook and then look at the four datasets and decide which one you would like to work with.  You can also select a different dataset which meets rubric criteria with CIL approval.  Review the data in your chosen data set and add one or more cells to your notebook to clean up any issues.  Add a markdown cell to describe what you needed to fix.

### **Submit the Notebooks for Your Assignment**  

📌 **Follow these steps to submit your work:**  

#### **1️⃣ Get a Sharing Link for Your Assignment**  
- On the upper right of the Kaggle page, click on Save Version and save, accepting all defaults.  You can just do a quick save.
- On the upper right, click on Share.  Choose Public, make sure that Allow Comments is on, and copy the public URL to your clipboard.

#### **2️⃣ Submit Your Kaggle Links**  
- Paste the URL for this week's assignment into the **assignment submission form**.
- Paste the URL for your capstone project notebook into the second field in the **assignment submission form**.

---
