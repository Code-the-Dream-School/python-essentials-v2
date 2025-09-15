# Assignment 12: Advanced Data Visualization

This assignment is to be created in the `assignment12` folder of your `python-assignment12` directory which is a separate repository. Continue to work in the ``assignment12`` branch.

---


## **Task 1: Plotting with Pandas**
1. Create a file called employee_results.py.
2. Load a DataFrame called employee_results using SQL.  Copy the `db/lesson.db` database from your `python_homework` folder to your `python-assignment12` folder.  Copy the `db` folder and the `lesson.db` file within it.  This can be done using the `cp -r` command.  In your `assignment12` folder, connect to `../db/lesson.db`. You use SQL to join the employees table with the orders table with the line_items table with the products table.  You then group by employee_id, and you SELECT the last_name and revenue, where revenue is the sum of price * quantity.  Ok, that's a lot of SQL to mess with, so here is the statement you need:
   ```SQL
   SELECT last_name, SUM(price * quantity) AS revenue FROM employees e JOIN orders o ON e.employee_id = o.employee_id JOIN line_items l ON o.order_id = l.order_id JOIN products p ON l.product_id = p.product_id GROUP BY e.employee_id;
   ```
3. Use the Pandas plotting functionality to create a bar chart where the x axis is the employee last name and the y axis is the revenue.
4. Give appropriate titles, labels, and colors.
5. Show the plot.

---

## **Task 2: A Line Plot with Pandas**
1. Create a file called cumulative.py.  The boss wants to see how money is rolling in.  You use SQL to access `../db/lesson.db` again.  You create a DataFrame with the order_id and the total_price for each order.  This requires joining several tables, GROUP BY, SUM, etc.
2. Add a "cumulative" column to the DataFrame.  This is an interesting use of apply():
   ```python
   def cumulative(row):
      totals_above = df['total_price'][0:row.name+1]
      return totals_above.sum()

   df['cumulative'] = df.apply(cumulative, axis=1)
   ```
   Because axis=1, apply() calls the cumulative function once per row.  Do you see why this gives cumulative revenue?  One can instead use cumsum() for the cumulative sum:
   ```python
   df['cumulative'] = df['total_price'].cumsum()
   ```
3. Use Pandas plotting to create a line plot of cumulative revenue vs. order_id.
4. Show the Plot.

---

## **Task 3: Interactive Visualizations with Plotly**

1. Load the Plotly wind dataset, via the following:
   ```python
   import plotly.express as px
   import plotly.data as pldata
   df = pldata.wind(return_type='pandas')
   ```
   Print the first and last 10 lines of the DataFrame.
2. Clean the data.  You need to convert the 'strength' column to a float.  Use of str.replace() with regex is one way to do this, followed by type conversion.
3. Create an interactive scatter plot of strength vs. frequency, with colors based on the direction.
4. Save and load the HTML file, as `wind.html`.  Verify that the plot works correctly.

---

## **Task 4: Reflection**
Create a file in the assignment12 folder called reflection.txt, and put in the following thoughts:

1. Reflect on the differences between static and interactive visualizations.
2. Write a short paragraph discussing the advantages of using dashboards for real-time data exploration.
3. Explain how interactive tools like Plotly can improve data communication in professional settings.

## **Task 5: Commit Your Work**

Add and commit all of the files created for the assignment to the `assignment12` branch.

---

# Assignment on Streamlit

You can use Streamlit for your capstone project.

## Overview
This assignment is to be implemented using **Streamlit**.  
You will **import a dataset**, **build a dashboard**, **visualize insights**, **showcase data cleaning**, and **deploy your app** to **Streamlit Community Cloud**.

### Requirements
- Download the dataset from here: [Retail sales dataset](https://www.kaggle.com/datasets/mohammadtalib786/retail-sales-dataset)
- Import a dataset that has already been cleaned and prepared
- Explain what cleaning and preparation was done
- Visualize key insights through interactive dashboards
- Deploy your app using Streamlit Community Cloud

## Task 1: Project Setup

Go through Lesson 12 for Streamlit setup.

## Task 2: Build Your Streamlit Dashboard

### Required Components

1. **Title and Description**
   - Use `st.title()` and `st.markdown()` to introduce your dashboard

2. **Dataset Overview**
   - Import your cleaned dataset using Pandas.
   - Display the dataset using `st.dataframe()` or `st.write()`
   - Optionally, summarize with `.describe()` or key statistics

3. **Data Cleaning Summary**
   - Briefly describe what cleaning steps you performed
   - Optional: Show comparison table (raw vs cleaned)

4. **Visualizations**
   - Include at least two interactive charts
   - Examples: bar chart, pie chart, line chart, scatter plot, histogram
   - Use Streamlit's built-in charts or libraries like Plotly/Matplotlib


## Task 3: Deploy Your App
1. Create a **GitHub repository** and push your code:
   - Log in to your GitHub account and create a new repository.(https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository)
   - Copy the repository URL.
   - Link your local project folder to the GitHub repository:
     ```bash
     git remote add origin <repository-url>
     ```
   - Add and commit your changes:
     ```bash
     git add .
     git commit -m "Initial commit"
     ```
   - Push your code to the GitHub repository:
     ```bash
     git branch -M main
     git push -u origin main
     ```
2. Deploy to **Streamlit Community Cloud**:
   - Visit [Streamlit Cloud](https://streamlit.io/cloud) and log in with your Streamlit account. If you don't have an account, create one using your email or GitHub credentials.

   - Click on the **"New App"** button to start the deployment process.

   - In the **"Select a repository"** section:
      - Connect your GitHub account if you haven't already.
      - Choose the repository where your Streamlit app code is stored.

   - In the **"Branch"** dropdown, select the branch containing your code (usually `main`).

   - In the **"Main file path"** field, specify the path to your Streamlit app file (e.g., `streamlit_app.py`).

   - Click **"Deploy"** to start the deployment process.

   - Wait for the deployment to complete. Once done, you will see a URL where your app is hosted.

   - Test your app by visiting the provided URL to ensure everything works as expected.

   - If you need to make updates to your app, push the changes to your GitHub repository. Streamlit Cloud will automatically redeploy your app with the latest changes.
3. Verify your app loads successfully and is publicly accessible

## Task 4: Submit Your Assignment

### Required Submissions
- Your **Streamlit Community Cloud app URL** (deployment link), this link is added to the ``service_urls.txt`` in the ``assignment12` folder in the `python-assignment12` repo.
- Your **GitHub repository URL** link is also added to the `service_urls.txt`
- Add and commit the `service_urls.txt` file in the `assignment12` branch.

### Resources
- [Streamlit Cheat Sheet](https://cheat-sheet.streamlit.app/)

### Example Submissions
1. Canada Dashboard:
   - App: https://canada.streamlit.app/
   - Code: https://github.com/parker84/canada-dashboard

2. Dashboard v2:
   - App: https://dash-board.streamlit.app/
   - Code: https://github.com/dataprofessor/dashboard-v2

---

### **Submit Your Assignment on GitHub**  

📌 **Follow these steps to submit your work:**  

#### **1️⃣ Add, Commit, and Push Your Changes** 
- Create a file called `service_urls.txt` in the assignment12 folder.  In it, paste the URL for your Render.com service.  If you did the Streamlit assignment, make sure the Streamlit github repository url and streamlit.io service url are added to the `service_urls.txt` file as well.
- Within your `python-assignment12` folder, do a git add and a git commit for the files you have created, so that they are added to the `assignment12` branch.
- Push that branch to GitHub. 

#### **2️⃣ Create a Pull Request**  
- Log on to your GitHub account.
- Open your `python-assignment12` repository.
- Select your `assignment12` branch.  It should be one or several commits ahead of your main branch.
- Create a pull request.

#### **3️⃣ Submit Your GitHub Link**  
- Your browser now has the link to your pull request.  Copy that link. 
- Paste the URL into the **assignment submission form**.
- if you did the optional Streamlit assignment, make sure that repository link and the url for the streamlit.io service are included in the `service_urls.txt` file. 

To summarize, a pull request for the `assignment12` branch in your new `python-assignment12` repository is pasted into the link submission field in the **assignment submission form**. For the streamlit assignment, the link for the `streamlit-assignment` repository and the url for the `streamlit.io` service are included in the `service_urls.txt` file.

---
