# Assignment 11: Final Week — Web Scraping Capstone Dashboard

Congratulations — you've made it to the final week of the course! This week has a **single focus**: finish and submit your **web scraping capstone** by building an interactive **Streamlit** dashboard on top of the data you have already scraped, cleaned, and stored in a SQLite database over the past several weeks.

Your Kaggle data pipeline capstone was already finalized and submitted back in **week 8**, so this week you can put all of your energy into the dashboard. There is one short, **optional** Plotly warm-up below if you'd like to ease into interactive visualization before building the dashboard — but the dashboard is the only required deliverable.

As always, reach out to a mentor if you need help this week. You're almost there!

---

## **Warm-Up: Interactive Visualizations with Plotly** (Optional)

This task is **optional**. Plotly is introduced in this week's lesson, and a quick exercise with it is a nice primer for the interactive dashboard work — but skip it if you'd rather go straight to your capstone. You can do this in a Kaggle notebook or a single local script; no separate repository or pull request is required.

1. Load the Plotly wind dataset, via the following (use exactly as written — this is the dataset-loading boilerplate):
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

## **Complete Your Web Scraping Capstone Project**

To complete your web scraping capstone project, you will apply this week's **Streamlit** lesson to create a visualization dashboard on top of the database you built in weeks 9 and 10.

### Overview
You will **import the database** you created earlier in the course, **build a dashboard**, **visualize insights**, and (optionally) **deploy your app** to **Streamlit Community Cloud**.

### **This week's focus areas**
#### **Data Visualization**
- Includes at least three visualizations using **Plotly, Streamlit, or Dash**.
- Visuals are relevant, well-labeled, and support the data story.
- User interactions such as dropdowns or sliders are implemented.
- Visualizations respond correctly to user input or filters.

#### **Dashboard / App Functionality**
- Built with **Streamlit** or **Dash** to display data and insights.
- Features clean layout and responsive components.
- Allows users to explore different aspects of the data.
- Provides clear titles, instructions, and descriptions for user guidance.

### **Project Setup**

Review Streamlit in **Lesson 11**.  You will set up a new dashboard in your capstone project repository, reading from the SQLite database you created in the database weeks.

### **Deploy Your App (Optional)**

Deployment to Streamlit Community Cloud is **optional** this term. A dashboard that runs correctly on your local machine with `streamlit run` fully meets the requirements — if you run out of time, a working local app is enough. If you'd like to deploy it so others can see it live, follow these steps:

1. Push the code to your **Capstone project GitHub repository**.
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
3. Verify your app loads successfully and is publicly accessible.

## Submit Your Web Scraping Capstone Project

### Required Submissions
- You will paste your **Capstone GitHub repository URL** into the final project submission form.  Submit all of your final changes as a pull request so your reviewer can add comments and approve or request changes.
- **If you deployed to Streamlit Community Cloud (optional):** add your **Streamlit Community Cloud app URL** to a file called `service_urls.txt` in the top level of your web scraping capstone repo, and add and commit that file. (Skip this if you are running your app locally.)

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

### **Submit Your Capstone on GitHub**

📌 **Follow these steps to submit your work:**

#### **1️⃣ Add, Commit, and Push Your Changes**
- Within your web scraping capstone repository, do a git add and a git commit for the dashboard files you have created.
- If you deployed, make sure your `service_urls.txt` file contains your `streamlit.io` app URL and is committed.
- Push your branch to GitHub.

#### **2️⃣ Create a Pull Request**
- Log on to your GitHub account.
- Open your web scraping capstone repository.
- Select your working branch.  It should be one or several commits ahead of your main branch.
- Create a pull request.

#### **3️⃣ Submit Your GitHub Link**
- Copy the link to your pull request.
- Paste the URL into the **final project submission form**.

---

### **Optional: Record a Short Project Presentation**

Recording a presentation is **optional this term** and is **not graded** — it will have no bearing on your graduation. We still encourage it: it's good practice to talk about what you built, because you'll be asked to describe your projects in job interviews. If you have the time, please record a 3–5 minute presentation that demonstrates and describes the workings of your web scraping dashboard project.

The rubric details for the web scraping capstone are below. Be sure your project meets the requirements specified.

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

<h4>4. The file title will be the default video title.  You can change this to include your name and "Python Essentials Final Project Presentation"</h4>

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

### Submitting your recording (optional)
If you record a presentation, include the YouTube link to it in your Assignment Submission Form for this week.

### See below for the rubric for your web scraping capstone:

<details>
<summary>Web Scraping Capstone Rubric</summary>
* **Web Scraping**

    * [ ] Uses appropriate libraries (e.g., Selenium) to retrieve data from the web
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
</br>

Use this [link for the final project submission form](https://airtable.com/appoSRJMlXH9KvE6w/shrBpqHbS6wgInoF9?prefill_Lessons=Python+100+v2%3A+Lesson+11+-+Advanced+Data+Visualization+Techniques&prefill_Student+Class+Records=Mary+Alice+Moore+%28Python-essentials-26.2%29) for your web scraping capstone and optional presentation video.

What happens next?
Once you've submitted your final capstone (and your optional video), your reviewer will check your work and either declare it needing revisions or being successful, as they have with all your other assignments.  Watch your email for those results.

---

<details>
<summary>Rubric (for AirHub reviewer and mentors)</summary>

This is the final-week capstone submission. The **new required work this week is the Streamlit dashboard**; the scraping and cleaning were built in prior weeks and are carried into the final submission — **do not fail this week's submission for not containing brand-new scraping or cleaning code.** There are no automated tests. Grade by reading the dashboard code and confirming it runs. **Be lenient about:** the database path/filename, the app filename (e.g. `streamlit_app.py`), and all directory structure — the reviewer cannot see the filesystem.

### Required Deliverables/Tasks

- **Streamlit dashboard (this week's new required deliverable)** — A Streamlit app, added to the student's existing web-scraping capstone repo, that reads from the SQLite database they built in weeks 9–10. It must include **at least three visualizations**, **at least one interactive control** (dropdown, slider, or filter) that the visualizations respond to, and **clear titles, labels, and instructions** in a clean layout. It must run locally with `streamlit run`. `Example — adapt to your own layout` for the database path/name and the app filename — do not fail on a differing path or name. Visualization library: the official rubric allows **Plotly, Streamlit, or Dash** — do NOT fail a student for using Plotly or Dash charts inside the app.
- **Web-scraping capstone repo, finalized** — The repo contains the prior-week scraping code, raw data saved in a structured format (`.csv`/`.json`), and data cleaning/transformation, now joined by the dashboard. The scraping library "Selenium" is named only as an example → do NOT fail a valid alternative such as BeautifulSoup, requests, or Scrapy. `Example — adapt to your own layout` for all paths and directory structure.
- **`README.md`** — Includes a project summary, setup steps, and a screenshot of the dashboard.
- **Submission** — Commit and push the dashboard work, open a **pull request** in the capstone repo, and paste the repo/PR URL into the final project submission form. `Use exactly as written` for the filename `service_urls.txt` **only if** the student deployed (see Optional).

### Optional Deliverables/Tasks

**Do not fail a student for omitting any of these.**

- **Plotly warm-up (Optional, ungraded)** — Marked "(Optional)" and "This task is optional." Per the course rubric this warm-up is **ungraded — do not grade or fail it at all.** If attempted: load the Plotly wind dataset, convert the `strength` column to float, build an interactive scatter (strength vs. frequency, colored by direction), and save/load an HTML file. The dataset-load snippet is `Use exactly as written` (boilerplate); the filename `wind.html` is `Example — adapt to your own layout`.
- **Deploy to Streamlit Community Cloud (Optional)** — Marked "(Optional)"; a dashboard that runs locally with `streamlit run` fully meets the requirements. Do NOT fail a project that is not deployed. If deployed, the student adds their app URL to a file named `service_urls.txt` at the top level of the capstone repo — `Use exactly as written` for that filename, but only when deployment was attempted.
- **Presentation video (Optional, not graded)** — Marked "optional this term" and "not graded." If recorded, the YouTube link goes in the submission form. Do NOT fail for its absence.

</details>
