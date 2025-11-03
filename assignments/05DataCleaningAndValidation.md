## Lesson 5 Assignment — Data Cleaning and Validation
### Data Cleaning and Validation with Pandas

### **Objective:**
In this assignment, you will gain hands-on experience cleaning and validating real-world datasets using the Pandas library. You’ll practice identifying and handling missing values, correcting data types, detecting duplicates and outliers, standardizing inconsistent text, encoding categorical variables, and validating data ranges. You’ll also work on a mini-project that simulates cleaning multiple messy files using techniques like fuzzy matching and regular expressions—building a strong foundation for reliable, analysis-ready data.

### **Tasks:**

### **Task 1: Handling Missing Data**

1. **Create a DataFrame using the provided data:**

   - Add another input dataset.  This time, search on "code the dream lesson 5"
   - Create another code block.
   - Resolve the name of the file as usual.
   - Read it into a DataFrame called df.
   - The DataFrame contains columns for 'Name', 'Age', 'Salary', 'Join Date', and 'City', with some missing values.
   - Print the original DataFrame.
  
Here's a summary of what the DataFrame looks like to help you get started (but don't use this code):

```python
data = {
    'Name': ['Alice', 'Bob', None, 'David', 'Eva'],
    'Age': [25, None, 35, 40, 30],
    'Salary': [50000, 60000, None, 80000, 55000],
    'Join Date': ['2020-01-01', None, '2020-03-15', '2020-04-20', None],
    'City': ['New York', 'Los Angeles', 'Chicago', None, 'Miami']
}
df = pd.DataFrame(data)
```

2. **Perform the following operations on new DataFrames:**
     - Create df1 by using `dropna()` on the df DataFrame created above.  Print the `info()` for df and df1 to see how many lines have missing values.
     - **Replace missing values** in df using the `fillna()` method:
     - Replace missing 'Name' values with `'Unknown'`.
     - Replace missing 'Age' values with the **mean** of the 'Age' column.
     - Replace missing 'Salary' values with the **median** of the 'Salary' column.
     - Replace missing 'Join Date' values with `'2020-01-01'`.
     - **Remove rows with missing values** using the `dropna()` method and save the result in df2.  Only the 'City' column should have missing values at this point.  Reset the index.
     - Convert the 'Age' column in df2 to **integer** type using `astype(int)`.
     - Print the updated df2 DataFrame.

### **Task 2: Data Transformation**
1. **Convert Data Types:**
   - Add another input.  This time search for "Code The Dream Eclipses".  This is a list of eclipses that were or will be observed in Arkansas.
   - Load the CSV file into a DataFrame called df3.  **Note:** The separator is "|" for this CSV file.
   - Print df3.info() and the first 5 rows of df3.
   - Attempt to convert the 'Date' column to **datetime** format using `pd.to_datetime()`.  You will see that an error is thrown for an invalid date.
   - Add `errors='coerce` to your `pd.to_datetime()` statement and try the conversion again.
   - Print the first 20 lines of the revised df3.  Examine what is stored for the dates that could not be converted.

### **Task 3: Validating Data Ranges**
1. **Ensure the 'Age' column contains values within the valid range (18 to 65):**
   - Replace invalid ages (less than 18 or greater than 65) with a NaN value.  (NaN is actually part of numpy: `np.nan` is the value you should use.  Don't use the string 'NaN'!)
   - Print the updated Dataframe.
   - Fill the NaN values with the **median** of the 'Age' column.
   - Print the updated DataFrame.

**Explanation:** Validating data ranges ensures that your data is consistent and suitable for analysis or modeling.

### **Task 4: Removing Duplicates & Outliers**
1. **Identify and remove duplicate records:**
   - Print `df3.info()`.
   - Use the `duplicated()` method to identify duplicate rows in the DataFrame, and save the result in duplicate_series.  This Series has `True` for each duplicate entry.
   - Print `duplicate_series[duplicate_series == True].head(10)` to see the first 10 duplicated entries.
   - Print `duplicate_series.value_counts()` to see how many duplicates you have.
   - Use the `drop_duplicates()` method to remove duplicate rows.
   - Print `info()` for the updated DataFrame.
  
By default, `drop_duplicates()` keeps the first occurrence of each duplicate row. You could use the `keep` parameter to change this behavior, but the default is ok for now.

2. **Identify and replace outliers in the 'Age' column:**
   - For this and the following tasks, use df2 again.
   - Consider outliers as values greater than 100 or less than 0.
   - Replace outliers with the **median** of the 'Age' column.
   - Print the updated DataFrame after handling outliers.

Outliers can also be identified using statistical methods like the Interquartile Range (IQR) or Z-scores -- but we'll just keep it simple for now

### **Task 5: Standardizing Data**
1. **Standardize the 'Name' column:**
   - Convert all names to lowercase and trim any leading or trailing whitespace using `str.lower()` and `str.strip()`.
   - Print the updated DataFrame.

2. **Standardize inconsistent entries in the 'City' column:**
   - Find variations in the City name:
   ```python
   print(df.groupby('City').agg({'Name': 'count'})) # This will show all city names, so you can see variations
   ```
   - Replace variations like 'NYC' with 'New York' and 'LA' with 'Los Angeles'.
   - Print the updated DataFrame.

**Task 6 — Encoding Categorical Variables**

1. Create a new DataFrame with a `Color` column = `['Red','Blue','Green','Blue']`.
2. Apply:
```python
   df["Color_Label"] = df["Color"].map({"Red":1,"Blue":2,"Green":3})
   df_encoded = pd.get_dummies(df["Color"], prefix="Color")
```
3. Display results.

**Task 7 — Consolidating Messy Files (Mini Project)**

1. Add input dataset "Code The Dream Assignment 6" → contains 4 CSV files (400 rows each).
2. Load all 4 into DataFrames and concatenate → `df_all` (~1600 rows).
3. Install and import thefuzz for approximate string matching:
```python
   try:
       from thefuzz import process
   except ImportError:
       !pip install thefuzz
       from thefuzz import process
```

4. Fix spelling errors:
```python
   df_names = df_all.value_counts("Name")
   good_names = list(df_names[df_names > 2].index)
   df_all["Name"] = df_all["Name"].map(
       lambda x: x if x in good_names else process.extractOne(x, good_names)[0]
   )
```
   Repeat for `Address`.

5. Fix Zip and Phone:
```python
   def fix_anomaly(group):
       group_na = group.dropna()
       if group_na.empty:
           return group
       mode = group_na.mode()
       if mode.empty:
           return group
       return mode.iloc[0]

   df_all["Zip"] = df_all.groupby(["Name","Address"])["Zip"].transform(fix_anomaly)
   df_all["Phone"] = df_all.groupby(["Name","Address"])["Phone"].transform(fix_anomaly)
```

6. Drop duplicates → expect ≈ 400 unique records.
7. Print `info()` and remaining nulls.

---

**Task 8 — Regular Expressions for Validation**

1. Extract Log Info:
```python
   log_entries = pd.Series([
       "[2023-10-26 10:00:00] INFO: User logged in",
       "[2023-10-26 10:05:30] WARNING: Invalid input",
       "[2023-10-26 10:10:15] ERROR: Database connection failed"
   ])
   extracted_logs = log_entries.str.extract(r"\[(.*?)\]\s(\w+):\s(.*)")
```

2. Standardize Placeholders:
```python
   text_data = pd.Series([
       "Value is {amount}.",
       "The price is [value].",
       "Cost: (number)",
       "Quantity = <qty>"
   ])
   standardized_text = text_data.replace(
       [r"\{.*?\}", r"\[.*?\]", r"\(.*?\)", r"\<.*?\>"],
       "<VALUE>",
       regex=True,
   )
```

3. Select Columns Ending in `_at`:
```python
   df = pd.DataFrame({
       "order_id":[1,2],
       "created_at":["2021-01-05","2021-01-06"],
       "updated_at":["2021-01-07","2021-01-08"]
   })
   time_cols = df.filter(regex="_at$")
```

4. Find Shipped Orders:
```python
   orders = pd.Series([
       "Order #123 has been shipped on 2021-01-05",
       "Order #124 has been cancelled",
       "Shipment #125 confirmed on 02/06/2021"
   ])
   shipped_orders = orders[orders.str.contains("ship", case=False)]
```

---

**Task 9 — Reflection & Validation**

Create a markdown cell summarizing:
* Most common data issues found
* Which techniques worked best
* How you could automate cleaning in a real workflow

---
### **Task 10: Start Your Final Project**

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


### **Submit the Notebook for Your Assignment**  

📌 **Follow these steps to submit your work:**  

#### **1️⃣ Get a Sharing Link for Your Assignment**  
- On the upper right of the Kaggle page, click on Save Version and save, accepting all defaults.  You can just do a quick save.
- On the upper right, click on Share.  Choose Public, make sure that Allow Comments is on, and copy the public URL to your clipboard.

#### **2️⃣ Submit Your Kaggle Link**  
- Paste the URL into the **assignment submission form**.  

---
