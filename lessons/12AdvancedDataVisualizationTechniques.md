
# **Lesson 12 — Advanced Data Visualization Techniques**

## **Lesson Overview**
**Learning objective:** students will learn how to build advanced, interactive visualizations using Python libraries such as Pandas, Plotly. They will practice visualizing data from DataFrames, creating interactive charts, and developing simple dashboards for real-time data exploration. To prepare for the final project, students will complete this lesson and assignment in a personal GitHub repository rather than in `python_homework`. An optional section introduces Streamlit for dashboard development.

### **Topics:**
1. Plotting with Pandas: Visualizing data directly from DataFrames.
2. Interactive Visualizations: Using Plotly for interactive plotting.
3. Dashboards: Creating dynamic dashboards with Plotly.
4. Advanced Customization: Advanced interactivity, subplots, and real-time updates.
5. Optional lesson on Streamlit.

Note:
For your final project, you will create a dashboard using one of these tools.  Which one you use is up to you.  Check out the optional Streamlit information and assignment if you are interested in Streamlit.

### **Setup**

You are using your own repository, both for the lesson and for the assignment.  If you do the Streamlit portions, these are also to be done within a second new repository.  The steps are these:

1. Create a folder called  `python-assignment12`.  This should **not** be inside of the `python_homework` folder.  Change to this directory.
2. Do a `git init`.
3. Create a `.gitignore` file.  You can copy the one from `python_homework`, but be sure you know why that one says what it does.
4. Create a virtual environment called `.venv`.  See the README.md for `python_homework` if you don't remember how this is done.
5. Create a `requirements.txt` file.  This should include the following packages.  These will also cover the optional streamlit lesson.
    - numpy
    - pandas
    - matplotlib
    - plotly
    - seaborn
    - gunicorn
    - streamlit
    
    If you do the Streamlit assignment, some further setup is needed since a separate repo will be created.  You can specify specific versions of these packages (see the requirements.txt for `python_homework`), but if you don't, you will get the latest version of each of these.
6.  **Important** Activate the virtual environment, with the command:
    ```bash
    source .venv/bin/activate
    ```
    Or, for Windows Git Bash:
    ```bash
    source .venv/Scripts/activate
    ```
    Verify that the virtual environment is active with:
    ```bash
    which python
    ```
    This should return a python location within your python-assignment12 folder.
7. Load the required packages as follows:
    ```bash
    pip install -r requirements.txt
    ```
8. Do `VSCode .`.  Bring up VSCode command palette (Ctrl-Shift-P) and select Python: Select Interpreter.  Select the one with `.venv`.  Close any VSCode terminal sessions and start a new one.  You should see in the command prompt that `.venv` is active.
9. On GitHub, create a new public repository called python-assignment12.  Do not create a README.md or .gitignore or license.  Copy the URL of the repository.  You can use either the HTTL or SSH URL, depending on your preference.  Set the remote for the repository, and push your code.
    ```bash
    git remote add origin <url>
    git add -A
    git commit -m "first commit"
    git push origin main
    ```
10. Create an `assignment12` git branch. Create a folder called `assignment12`.  This is for the exercises prior to the assignment and will also be used for the assignment.

For the following code examples, you create programs in the `assignment12` folder.  Some of this code won't run correctly within the Python interactive shell.  As you do the lesson and assignment, periodically add and commit your changes and push the `assignment12` branch to GitHub.  This is to practice the procedures of a development shop.  When you use these procedures, you can be confident that you won't break something and have to start over.  You can just switch back to the last commit if something breaks.

---

## **11.1 Plotting with Pandas**

### **Overview**
Pandas simplifies data visualization by providing built-in plotting methods for DataFrames and Series. These plots are ideal for quick data exploration and basic visualizations.

### **Key Plot Types:**
- **Line Plot:** Displays trends over time or continuous data.
- **Bar Plot:** Used for comparing categorical data.
- **Histogram:** Shows the distribution of numerical data.

### **When to Use These Plots:**
- **Line Plots** are typically used for showing data trends over time, such as sales or stock prices over months.
- **Bar Plots** are ideal when you need to compare quantities between different categories, such as the sales of different products or regions.
- **Histograms** are useful for analyzing the distribution of numerical data, identifying patterns, skewness, or the range of values.

Within the `assignment12` folder of your `python-assignment12` directory, create `lesson11_a.py`.  This code uses the DataFrame plot() method, which is part of Pandas, but, to actually display the plot, you also need Matplotlib, to do the `show()`.  Your program should contain the following code:

### **Example Code: Plotting with Pandas**

Create a file called `lesson11_a.py`, with the following content:

```python
import pandas as pd
import matplotlib.pyplot as plt

# Load a dataset
data = {
    "Month": ["Jan", "Feb", "Mar", "Apr", "May", "Jun"],
    "Sales": [100, 150, 200, 250, 300, 350],
    "Expenses": [80, 120, 180, 200, 220, 300]
}
df = pd.DataFrame(data)

# Line Plot
df.plot(x="Month", y=["Sales", "Expenses"], kind="line", title="Sales vs. Expenses")
plt.show()

# Bar Plot
df.plot(x="Month", y="Sales", kind="bar", color="skyblue", title="Monthly Sales")
plt.show()
```

Try this out.  The behavior of Matplotlib is like what you saw in the previous lesson.  A graphic window appears, and the program stops to wait at that point, until you close the window.
---

## **11.2 Interactive Visualizations with Plotly**

### **Overview**
Plotly is a powerful library for creating interactive, highly customizable plots. It allows for hover tooltips, zooming, and dynamic interactions that improve user experience.  The code below uses a sample dataset that is provided as part of Plotly, but in the general case, you would use a Pandas DataFrame loaded from a CSV file or database.  Within the `assignment12` folder, create `lesson11_b.py` with the following code:

### **Example Code: Interactive Scatter Plot**
```python
import plotly.express as px
import plotly.data as pldata


df = pldata.iris(return_type='pandas') # Returns a DataFrame.  plotly.data has a number of sample datasets included.
fig = px.scatter(df, x='sepal_length', y='petal_length', color='species',
                 title="Iris Data, Sepal vs. Petal Length", hover_data=["petal_length"])
fig.write_html("iris.html", auto_open=True)

# Do not try fig.show()!  This sometimes works, but usually it just hangs.
```
Try it out!  The interactive plot comes up in your browser, and you can hover over data points zoom, select, etc.  The HTML file (with lots of embedded JavaScript) can be used in other contexts.exit

### **Key Features of Plotly:**
- **Interactivity:** Hover tooltips, zooming, and panning.
- **Customization:** Wide range of customization options for visual aesthetics and user interaction.
---

## **11.3 Reflection**

### **Differences Between Static and Interactive Visualizations:**
- **Static Visualizations:** Easier to create and quicker to render but lack user interaction.
- **Interactive Visualizations:** Allow users to explore data, zoom, filter, and interact, providing a deeper and more engaging analysis experience.

### **Advantages of Dashboards:**
- Real-time data exploration and updates.
- User interaction with data (e.g., dropdowns, sliders) enables custom insights.
- Efficient presentation of key metrics in a professional setting.

---

# Building Interactive Apps with Streamlit

This portion of the lesson supports the optional assignment on Streamlit.  For the capstone final project, you can use Streamlit.

## Lesson Overview

**Learning objective:** Learn to create interactive web applications for data visualization and analysis using Streamlit.

Topics: 
  * Introduction to Streamlit and its benefits
  * Basic Streamlit components and layout
  * Interactive data visualization with Streamlit
  * Deploying Streamlit applications

## What is Streamlit?

Streamlit is a Python library that makes it easy to create custom web apps for machine learning and data science. It turns data scripts into shareable web apps in minutes, not weeks.

### Key Features
* Simple Python-first syntax
* Rich set of UI components
* Easy integration with data science libraries
* Quick deployment options

### Installation and Setup

First, let's set up a virtual environment and install Streamlit:

1. Create a project folder named `streamlit_project` in the top level of your `assignment12` folder and change to that folder.

## Basic Streamlit Components
### Text and Data Display

Streamlit provides a variety of methods to render static content such as text, markdown, and code. These elements are useful for building the layout and guiding users through your app.

## Exercise 1: Text and Data Display
- Create a python script app.py
In this exercise, you will add static content to your app by writing the following code into your app.py file:
```python
import streamlit as st  # Importing the Streamlit library

# Basic text elements
st.title("My First Streamlit App")  # Adds a big title at the top of the app
st.header("Section 1")  # Adds a section header — good for breaking content into parts
st.subheader("Header")  # Slightly smaller than header — useful for structure
st.subheader("Subheader")  # Another level down — keeps things organized
st.text("Simple text")  # Displays plain, unformatted text — like a basic message
st.markdown("**Bold** and *italic* text")  # Markdown lets you add simple formatting like bold and italics

# Display data
st.write("Automatic data display")  # Streamlit's flexible method — handles strings, numbers, dataframes, and more
st.code("print('Hello World')", language='python')  # Nicely formats code blocks with syntax highlighting
st.latex(r"\int_{a}^{b} x^2 dx")  # Renders LaTeX math formulas — great for equations

```

Once you've saved and run your app.py file using the command:

```bash 
streamlit run app.py
```
Open your browser to http://localhost:8501 to view your app.

Note: Any time you change a value in one of the input components, go to the browser tab and refresh ,Streamlit  reruns the entire script from top to bottom using the updated values. This means your app always reflects the latest state .
You can refresh the tab manually, or use the “Always rerun” option in the top-right of the Streamlit page for instant updates as you code.

## Exercise 2: Data Input Components
```python
# Text input
st.header("Section 2")  # A new section to group interactive input components
name = st.text_input("Enter your name", "John Doe")  # Simple text field with a default value
description = st.text_area("Description", "Write something...")  # Multi-line text box for longer input

# Numeric input
age = st.number_input("Age", min_value=0, max_value=120, value=25)  # Number picker with min/max range
score = st.slider("Score", 0, 100, 50)  # Slider to pick a number in a range — great for ratings or scores

# Selection widgets
option = st.selectbox("Choose an option", ["A", "B", "C"])  # Dropdown menu — user picks one option
options = st.multiselect("Multiple options", ["X", "Y", "Z"])  # Allows multiple selections at once

# Date and time
date = st.date_input("Select date")  # Calendar-style date picker
time = st.time_input("Select time")  # Clock-style time picker

# Buttons and checkbox
if st.button("Click me"):  # A button that runs code when clicked
    st.write("Button clicked!")  # Responds when the button is pressed
    
if st.checkbox("Show/Hide"):  # Checkbox to toggle something on/off
    st.write("Visible content")  # Displays this text only if the box is checked
```
- you can again now refresh your tab in browser to see the updated output.

Note:Unlike dropdowns or sliders (which always keep a selected value), buttons in Streamlit are "stateless" — they don’t hold their state after being clicked. Instead, Streamlit checks whether the button was pressed during that specific run of the script. That’s why we use an if statement with them.

Also, clicking a button triggers a full rerun of the script, just like other controls.

Note: 📍 Where does st.write() show output?
Streamlit renders output in the order the code runs — so the st.write() here appears right under the button. To control placement more precisely, you can use layout elements like columns or placeholders.


## Exercise 3: Layout and Containers
In this section, you’ll learn how to organize your app using columns, expanders, and a sidebar.

Continue working in the same app.py file you created earlier. You can either:
-Append the new code at the bottom of the file



```python
st.header("Section 3")

# Create two side-by-side columns
col1, col2 = st.columns(2)

with col1:  # Everything under this goes into the left column
    st.header("Column 1")
    st.write("Content for column 1")

with col2:  # Everything under this goes into the right column
    st.header("Column 2")
    st.write("Content for column 2")

# Expandable sections
with st.expander("Click to expand"):
    st.write("Expanded content here")

# Sidebar
st.sidebar.title("Sidebar")
sidebar_option = st.sidebar.selectbox("Select option", ["A", "B", "C"])
```

Streamlit uses the Python with statement to define scoped areas for content. 
```python
with col1:
    st.write("Some content")
```
…it means "put this content inside column 1." Streamlit handles layout placement based on these scopes — it’s a readable way to group content visually.



## Exercise 4: Building a Simple Dashboard

Create a new python script named 'dashboard_app.py'.

```python
import streamlit as st  
import pandas as pd     # Used to work with tabular data
import numpy as np      # Helps generate random numbers
import plotly.express as px  # For interactive charts

# Create sample data — just faking some numbers to simulate a small product dataset
np.random.seed(42)  # Setting a seed so results are consistent every time you run
sample_data = {
    'Product': ['Product A', 'Product B', 'Product C', 'Product D'],
    'Sales': np.random.randint(100, 500, size=4),   # Random sales numbers
    'Profit': np.random.randint(20, 100, size=4)    # Random profit numbers
}
df = pd.DataFrame(sample_data)  # Convert the data into a DataFrame for easy handling

# Sidebar filters — this shows up in the sidebar for user interaction
st.sidebar.header('Filter Options')  # Sidebar title
selected_product = st.sidebar.selectbox('Select Product', df['Product'])  # Dropdown to choose a product

# Filter the data based on the user's selection
filtered_df = df[df['Product'] == selected_product]  # Show only the row that matches the selected product

# Main app content starts here
st.title('Simple Product Dashboard')  # Big title for the dashboard

# Display key numbers using metrics — side-by-side using columns
col1, col2 = st.columns(2)  # Create two columns for layout
with col1:
    st.metric('Sales', f"${filtered_df['Sales'].values[0]:,}")  # Show sales in a pretty format
with col2:
    st.metric('Profit', f"${filtered_df['Profit'].values[0]:,}")  # Show profit similarly

# Add a bar chart comparing all products — gives full context beyond the filter
st.subheader('Sales and Profit Comparison')  # Subheading for the chart
bar_chart = px.bar(df, x='Product', y=['Sales', 'Profit'], barmode='group')  # Grouped bar chart
st.plotly_chart(bar_chart)  # Render the chart in the app
```

in your terminal execute
```bash
streamlit run dashboard_app.py
```

If you completed the Streamlit lesson, add and commit the additional folder and files created.

## Conclusion
 You now know how to:
- Create basic Streamlit apps
- Add input forms, layouts, and sidebars
- Build simple dashboards with metrics and charts

## ** Streamlit**

Streamlit is a way of creating a web based dashboard for data presentation.

Advantages of Streamlit:
- Pretty easy as compared with other methods like Dash, etc.

Disadvantages:
- Not as widely used (though gaining popularity)
- In a production environment, it is not very scalable


## **Summary**

In this lesson, you learned:
1. How to visualize data directly from Pandas DataFrames.
2. How to create interactive visualizations with Plotly.
3. How to build dynamic dashboards using Streamlit.
4. The differences between static and interactive visualizations and their real-world applications.

For more details, explore the [Plotly Documentation](https://plotly.com/python/).

---

### Additional Resources:
1. **Matplotlib Tutorials:** For more detailed Matplotlib tutorials, check out [Matplotlib Tutorials](https://matplotlib.org/stable/tutorials/index.html).
2. **Seaborn Gallery:** Explore different plot examples at the [Seaborn Gallery](https://seaborn.pydata.org/examples/index.html).
3. **Data Visualization in Python:** To explore more about data visualization strategies and best practices, visit [Data Visualization in Python](https://realpython.com/python-data-visualization-using-matplotlib/).

