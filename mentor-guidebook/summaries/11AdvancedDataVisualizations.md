# Week 11 Mentor Summary: Advanced Data Visualization Techniques

**Lesson file:** `lessons/11AdvancedDataVisualizations.md`
**Assignment file:** `assignments/11AdvancedDataVisualizations.md`

**Learning objective:** Students learn to build interactive charts with Plotly and simple dashboards with Streamlit. They use these skills to build the Streamlit dashboard for their web scraping capstone, which is the final deliverable of the course.

---

## Lesson Overview

The lesson starts with a setup section. Students create a new practice repository called `python-assignment11` outside of `python_homework`. They run `git init`, add a `.gitignore`, create a `.venv`, and write a `requirements.txt` that lists numpy, pandas, matplotlib, plotly, seaborn, gunicorn, and streamlit. They install the packages, push the repository to a new public GitHub repository, and create an `assignment11` branch and folder. This repository is only for practice and is not graded.

| Section | What students learn |
|---|---|
| **11.1 Plotting with Pandas** | The DataFrame `plot()` method makes line and bar charts, and `plt.show()` from Matplotlib displays them. The section explains when to use line plots, bar plots, and histograms. Each chart window pauses the program until the student closes it. |
| **11.2 Interactive Visualizations with Plotly** | `plotly.express.scatter()` builds an interactive scatter plot from the iris sample dataset. `fig.write_html(..., auto_open=True)` saves the chart and opens it in the browser. The lesson tells students not to use `fig.show()`, because it often hangs. |
| **11.3 Building Interactive Apps with Streamlit** | Students run apps with `streamlit run app.py` and view them at `http://localhost:8501`. Exercise 1 covers text elements such as `st.title()`, `st.markdown()`, `st.code()`, and `st.latex()`. Exercise 2 covers input widgets: text inputs, number inputs, sliders, select boxes, date and time pickers, buttons, and checkboxes. Exercise 3 covers layout with `st.columns()`, `st.expander()`, and `st.sidebar`. Exercise 4 builds `dashboard_app.py`, which has a sidebar filter, two `st.metric()` values, and a Plotly bar chart shown with `st.plotly_chart()`. |
| **11.4 Reflection** | The differences between static and interactive charts, and the advantages of dashboards. |

Two Streamlit ideas are central to the lesson. First, Streamlit runs the whole script again from top to bottom every time a user changes a widget. Second, `st.button()` returns `True` only during the run right after a click, so a button does not keep its state. The lesson includes an AI practice prompt about each of these ideas.

The lesson ends by stating that this is the final week of class.

---

## Assignment Deliverables

**Setup:** Students add the dashboard to their existing web scraping capstone repository. The dashboard reads from the SQLite database that they built in Weeks 9 and 10. The practice repository from the lesson is not part of the submission.

**Testing:** There are no automated tests. Students confirm that the app runs locally with `streamlit run` and that the charts respond to the controls.

**Tasks:** The dashboard is the only required deliverable this week.

| Task | Status | What it does | Concepts practiced |
|---|---|---|---|
| Plotly warm-up | Optional, ungraded | Loads the Plotly wind dataset, prints the first and last 10 rows, and converts the `strength` column to a float. It then builds a scatter plot of strength against frequency, colored by direction, and saves it as an HTML file. | `head()` and `tail()`, `str.replace()` with a regular expression, type conversion, `px.scatter()`, `write_html()` |
| Streamlit dashboard | Required | Reads the capstone database and shows at least three visualizations. It has at least one interactive control, such as a dropdown, slider, or filter, that the charts respond to. It has clear titles, labels, and instructions in a clean layout. Charts can use Plotly, Streamlit, or Dash. | Reading SQLite into Pandas, Streamlit widgets and layout, Plotly charts |
| Finalized capstone repository | Required | The repository contains the earlier scraping code, raw data in CSV or JSON format, the cleaning code, and the dashboard. Selenium is only an example, so other scraping libraries are acceptable. | Project organization |
| `README.md` | Required | Includes a project summary, setup steps, and a screenshot of the dashboard. | Documentation |
| Deploy to Streamlit Community Cloud | Optional | If students deploy, they put the app URL in `service_urls.txt` at the top level of the repository. This filename is required exactly. | Deployment |
| Presentation video | Optional, not graded | A 3 to 5 minute recording that demonstrates the dashboard, uploaded to YouTube as an unlisted video. | Presenting technical work |

**Submission:** Students commit and push the dashboard work, open a pull request in the capstone repository, and paste the link into the final project submission form. If they record a video, they add the YouTube link to the same form.

**Note:** The assignment asks for several things that the lesson does not teach. The lesson never shows how to read a SQLite database inside a Streamlit app or how to deploy. In Exercise 4, the sidebar filter changes only the metrics, and the bar chart always shows all products. Students must work out how to make their charts respond to a widget. The warm-up uses `str.replace()` with a regular expression, which the lesson does not cover this week. Students also need a `requirements.txt` in the capstone repository for the setup steps and for deployment.

---

## FAQs and Common Trouble Spots

**1. "My dashboard says the table does not exist," or "the dashboard is empty."**
This is usually a database path problem. `sqlite3.connect()` creates a new empty database when it cannot find the file. Streamlit resolves a relative path from the folder where the student runs `streamlit run`, which may differ from the folder that holds the script. Ask the student to run the app from the repository root and use a path relative to that folder. For a deployed app, the database file must also be committed to the repository.

**2. "`streamlit` is not found," or "`ModuleNotFoundError: No module named 'plotly'`."**
The virtual environment is usually not active, or VS Code is using a different interpreter. Check that the prompt shows `(.venv)` and that `which python` points inside the project folder. The lesson asks students to type `VSCode .` to open the editor, but the standard command is `code .`. The capstone repository needs its own environment and its own `requirements.txt`.

**3. "My button message disappears," or "my app resets when I click something."**
Streamlit runs the whole script again after every interaction. A button returns `True` only during the run right after the click, so any message under `if st.button(...)` disappears on the next interaction. Refreshing the browser tab also resets all widgets to their default values. For content that should stay visible, students can use a checkbox, a select box, or `st.session_state`. The lesson does not cover `st.session_state`.

**4. "How do I convert the `strength` column in the warm-up?"**
The values are ranges such as `0-1` and `5-6`, and one value is `6+`. These strings cannot be converted with `float()` directly. Students must decide how to turn each range into one number, for example by keeping one end of the range, and then remove the extra characters with `str.replace()`. The assignment does not say which number to keep, so any reasonable choice is fine. The warm-up is optional and ungraded, so students who are short on time can skip it.

**5. "Do I have to deploy my app?"**
No. Deployment is optional this term. A dashboard that runs correctly on the student's machine with `streamlit run` meets the requirements. If a student does deploy, the most common problems are a missing `requirements.txt`, a database file that was not committed, and a wrong "Main file path." Students who deploy must also add `service_urls.txt` with the app URL.
