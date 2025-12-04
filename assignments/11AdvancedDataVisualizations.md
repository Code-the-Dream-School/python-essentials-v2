# Assignment 11: Advanced Data Visualization

This assignment is to be created in the `assignment11` folder of your `python-assignment11` directory which is a separate repository. Continue to work in the ``assignment11`` branch.

---


## **Task 1: Plotting with Pandas**
1. Create a file called employee_results.py.
2. Load a DataFrame called employee_results using SQL.  Copy the `db/lesson.db` database from your `python_homework` folder to your `python-assignment11` folder.  Copy the `db` folder and the `lesson.db` file within it.  This can be done using the `cp -r` command.  In your `assignment11` folder, connect to `../db/lesson.db`. You use SQL to join the employees table with the orders table with the line_items table with the products table.  You then group by employee_id, and you SELECT the last_name and revenue, where revenue is the sum of price * quantity.  Ok, that's a lot of SQL to mess with, so here is the statement you need:
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
Create a file in the assignment11 folder called reflection.txt, and put in the following thoughts:

1. Reflect on the differences between static and interactive visualizations.
2. Write a short paragraph discussing the advantages of using dashboards for real-time data exploration.
3. Explain how interactive tools like Plotly can improve data communication in professional settings.

## **Task 5: Commit Your Work**

Add and commit all of the files created for the assignment to the `assignment11` branch.

---

## **Task 6: Streamlit dashboard for your web scraping capstone project**

To complete your web scaping capstone project, you will apply the **Streamlit** lesson to create a visualization dashboard.

## Overview 
You will **import the database** which was created last week, **build a dashboard**, **visualize insights**, and **deploy your app** to **Streamlit Community Cloud**.

### **Visualization and dashboard rubrics**
#### **Data Visualization**
- Includes at least three visualizations using **Streamlit**.
- Visuals are relevant, well-labeled, and support the data story.
- User interactions such as dropdowns or sliders are implemented.
- Visualizations respond correctly to user input or filters.

#### **Dashboard / App Functionality**
- Built with **Streamlit** to display data and insights.
- Features clean layout and responsive components.
- Allows users to explore different aspects of the data.
- Provides clear titles, instructions, and descriptions for user guidance.

## **Project Setup**

Review Streamlit in **Lesson 11**.  You will set up a new dashboard in your capstone project repository.

## **Deploy Your App**
1. Push the code to your **Capstone project GitHub repository**:
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

## Submit Your web scraping capstone project

### Required Submissions
- Your **Streamlit Community Cloud app URL** (deployment link), this link is added to a file called ``service_urls.txt`` in the top level of your ``web scraping capstone`` repo.  Be sure to add and commit this file to your repo.
- You will paste your **Capstone GitHub repository URL** link into the second field in the final project submission form.  Submit all of your final changes as a pull request so your reviewer and add comments and approve or request changes.

### Resources
- [Streamlit Cheat Sheet](https://cheat-sheet.streamlit.app/)

### Example **Streamlit** Submissions
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
- Create a file called `service_urls.txt` in the assignment11 folder. Once you complete the Streamlit assignment, make sure the streamlit.io service url are added to the `service_urls.txt` file.
- Within your `python-assignment11` folder, do a git add and a git commit for the files you have created, so that they are added to the `assignment11` branch.
- Push that branch to GitHub. 

#### **2️⃣ Create a Pull Request**  
- Log on to your GitHub account.
- Open your `python-assignment11` repository.
- Select your `assignment11` branch.  It should be one or several commits ahead of your main branch.
- Create a pull request.

#### **3️⃣ Submit Your GitHub Link**  
- Your browser now has the link to your pull request.  Copy that link. 
- Paste the URL into the **assignment submission form**.


To summarize, a pull request for the `assignment11` branch in your new `python-assignment11` repository is pasted into the link submission field in the **assignment submission form**. For the streamlit assignment, the link for the url for the `streamlit.io` service are included in the `service_urls.txt` file.

---
### **Submit Your Final Projects**

Congratulations — you've made it to the final week of the course! This week, your goals are:
1. **Finish and submit your final projects**.
2. **Record a short project presentation** to share with the rest of the class.

As always, reach out to a mentor if you need help this week. You're almost there!

📌 **Follow these steps to submit both of your final projects:**

### Project Demo Details

It is good practice to talk about what you have learned and know about project as you will be asked to talk about your experiences in job interviews.  This is why we ask all students to give a short demo of their final projects.  This demo is NOT graded and your performance in the demo will have no bearing on your graduation.  Please record a 3-5 minute presentation that touches on all of the following:
 - [ ] demonstrate the data visualizations you created with your Kaggle project
 - [ ] demonstrate and describe the workings of your Local Data Scraping project done with Dash or Streamlit

Rubric details for the Kaggle Project and Scraping Project can be seen below.  Be sure both your projects meet the requirements specified for each.

### How to record your presentation
You can record your presentation in any of these three ways: 
 1. Logging into your personal Zoom account and record your personal meeting where only you are in attendance and you are screensharing your work ([this is a link to a video on how to do this](https://www.youtube.com/watch?v=njwbjFYCbGU))
 2. Use a screen recording program already on your machine
    * [Mac users can use this link to watch a how-to video](https://www.youtube.com/watch?v=w9Byefp51tY)
    * [Windows users can use this link to watch a how-to video](https://www.youtube.com/watch?v=PJB7pM5bvNI)
 3. Use an online option such as loom ([link to a how to video on loom here](https://www.youtube.com/watch?v=oAdLPbfXcQo)).

### How to upload your video so it can be shared
<details>
<summary>Click here to expand detailed instructions on how to upload your recording</summary>
<br>
<h4>1. Make sure you're logged in to youtube.</h4>
 <p>If you don't have a youtube account, <a href="https://support.google.com/youtube/answer/161805?hl=en&co=GENIE.Platform%3DDesktop">create one by following these instructions</a>.</p>
 <p>You will know you're logged in if you have an initial/icon/other in the top right corner (where the M in the brown circle is on this screenshot):</p>
 
 ![User logged in to youtube account](https://github.com/Code-the-Dream-School/intro-to-programming-2025/blob/d2f9b35d7206eeb0af24f85a8e8e5d97d43cbfad/images/Screenshot%202025-01-27%20at%204.01.20%E2%80%AFPM.png?raw=true)

 <h4>2. Click `+ Create` in the top right and select `Upload video`</h4>
 
 ![Create menu expanded](https://github.com/Code-the-Dream-School/intro-to-programming-2025/blob/d2f9b35d7206eeb0af24f85a8e8e5d97d43cbfad/images/Screenshot%202025-01-27%20at%204.01.27%E2%80%AFPM.png?raw=true)

 <h4>3. In the Upload videos window that appears, click the black `Select files` button</h4>
 <p>You'll need to select the file of your recording you have saved on your computer.</p>
 
 ![Upload videos modal](https://github.com/Code-the-Dream-School/intro-to-programming-2025/blob/d2f9b35d7206eeb0af24f85a8e8e5d97d43cbfad/images/Screenshot%202025-01-27%20at%204.01.35%E2%80%AFPM.png?raw=true)

<h4>4. The fle title will be the default video title.  You can change this to include your name and "Python Essentials Final Projects Presentation"</h4>

![Edit Video Details title](https://github.com/Code-the-Dream-School/intro-to-programming-2025/blob/d2f9b35d7206eeb0af24f85a8e8e5d97d43cbfad/images/Screenshot%202025-01-27%20at%204.02.17%E2%80%AFPM.png?raw=true)

<h4>5. Scroll down under the title; select "No, it's not made for kids" and click on the `Show more` gray button to make further setting changes</h4>

![Adjust video settings](https://github.com/Code-the-Dream-School/intro-to-programming-2025/blob/d2f9b35d7206eeb0af24f85a8e8e5d97d43cbfad/images/Screenshot%202025-01-27%20at%204.02.30%E2%80%AFPM.png?raw=true)

<h4>6. You'll want to be sure the following options for some of the sections that appear after click `Show more` are set to the following:</h4>

 - [ ] Altered content: select "No" since you have not used AI to alter reality in your video
 
 - [ ] Recording date and location: please select the date you made your final project recording
      
 - [ ] Shorts remixing: select "Don't allow remixing"
       
 - [ ] Comments and ratings: if you would like classmates to leave comments, leave comments "On", otherwise you can turn them off by selecting "Off".  _NOTE: we do not and cannot monitor comments.  Please report anything concerning to Code the Dream but have screenshots if needed._  You can also UNcheck the "Show how many viewers like this video if you wish.
       
 - [ ] Click the black `Next` button in the bottom right to proceed from the Details section of the upload through the Video elements and Checks portion.
       
 - [ ] Once you are on the Visibility section of the upload, select "Unlisted" as seen here

![Select Unlisted](https://github.com/Code-the-Dream-School/intro-to-programming-2025/blob/d2f9b35d7206eeb0af24f85a8e8e5d97d43cbfad/images/Screenshot%202025-01-27%20at%204.04.52%E2%80%AFPM.png?raw=true)

 - [ ] Lastly, click `Save` and copy your video link as seen here

![Save and copy](https://github.com/Code-the-Dream-School/intro-to-programming-2025/blob/d2f9b35d7206eeb0af24f85a8e8e5d97d43cbfad/images/Screenshot%202025-01-27%20at%204.05.09%E2%80%AFPM.png?raw=true)
 
</details>

### Submitting your recording
**Once you have recorded your presentation, please include the youtube link to your presentation in your Assignment Submission Form for this week.  Your assignment submission form this week will have you share the link to your Kaggle Project, the link to your repo that holds your Local Data Scraping Project, and a place for you to share your video recording link.**

### See below for rubrics for your final projects:

<details>
<summary>Scraping Project Rubric</summary>
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
</details>

<details>
<summary>Kaggle Final Project Rubric</summary>
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
</br>

Use this [link for the final project submission form](https://airtable.com/appoSRJMlXH9KvE6w/shrthD4fozy4UI21I?prefill_Lessons=Python%20100%20v1:%20Lesson%2015%20-%20Project%20Completion%20and%20Presentations) for your Kaggle, Web scraping, and presentation video.

What happens next?
Once you've submitted your finals and your video, your reviewer will check your work on your final projects and either declare them needing revisions or being successful, as they have with all your other assignments.  Watch you email for those results.  

To view your and your classmates' final project presentations, you can visit your class gallery here: [Jamaican Boa Python Essentails Class - Video Demo Gallery](https://airtable.com/appoSRJMlXH9KvE6w/shrqml5HWXby1FzEz/tblTvxLLUlqtgtfbq)
