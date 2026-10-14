# Week 7 Mentor Summary: Introduction to Data Visualization

**Lesson file:** `lessons/07IntroductionToDataVisualization.md`
**Assignment file:** `assignments/07IntroductionToDataVisualization.md`

**Learning objective:** Students learn to create and customize basic and statistical charts with Matplotlib and Seaborn. They learn to choose a chart type that fits the data and to use charts to explain what the data shows.

---

## Lesson Overview

The lesson has four sections. Students install `matplotlib` and `seaborn` with pip and run the examples in the Python interactive shell. Each `plt.show()` call opens a separate window, and the shell waits until the student closes it. Several sections have an optional AI practice prompt.

| Section | What students learn |
|---|---|
| **7.1 Introduction to Matplotlib** | Line plots with `plt.plot()`, bar plots with `plt.bar()`, and histograms with `plt.hist()`. The section explains when to use each type, and how to add a title and axis labels. It also introduces `np.random.randn()` and the normal (Gaussian) distribution. Students should not save chart images inside `python_homework`. |
| **7.2 Customizing Plots** | Line style, markers, colors, line width, bar width and edge color, font size and weight, grids, and legends. |
| **7.3 Introduction to Seaborn** | Correlation values between -1.0 and 1.0, a correlation heatmap with `sns.heatmap()`, and a pair plot with `sns.pairplot()` colored by a `hue` column. The examples use the built-in Titanic dataset, loaded with `sns.load_dataset()`. Students learn that the diagonal of a pair plot shows the distribution of one variable. |
| **7.4 Advanced Customization** | Several plots in one figure with `plt.subplots()` and `plt.tight_layout()`, Seaborn color palettes with `sns.set_palette()`, and annotations. `plt.annotate()` uses `xy`, `xytext`, and `arrowprops` to point an arrow at the highest revenue value, and `plt.text()` adds a plain text label. |

---

## Assignment Deliverables

**Setup:** This assignment is done in Kaggle, not in the `python_homework` repository. Students create a notebook called `CTD_Assignment_7` and add Alex Teboul's "Diabetes Health Indicators Dataset." Each task starts with a markdown cell.

**Testing:** There is no PyTest file. Reviewers look at the code and the rendered charts. For each chart, they check the chart type, the data on each axis, and the title and axis labels. Colors, figure size, bin counts, and exact wording are flexible.

**Tasks:** Tasks 1 through 8 are required. A markdown note about the columns in Task 1 is optional.

| Task | What it does | Concepts practiced |
|---|---|---|
| 1 | Students read the column descriptions in Kaggle's Data Explorer. They learn that `Age` is 13 buckets, not years, and that high `GenHlth` and `PhysHlth` values are bad. No code is required. | Understanding the data before plotting |
| 2 | Loads the `diabetes_012` file (not the `binary` or `5050` file) into a DataFrame named `diabetes` and shows the first 5 rows. | Reading a CSV in Kaggle |
| 3 | Draws a Matplotlib histogram of the `Age` column with a title and axis labels. | `plt.hist()`, labels |
| 4 | Builds `health_by_age` by grouping on `Age` and taking the mean of `GenHlth`. Adds a `Health` column equal to 5 minus `GenHlth`, sorts by the index, and draws a line plot of `Health` by age. | `groupby()`, new columns, `sort_index()`, `plt.plot()` |
| 5 | Builds a correlation matrix named `diabetes_corr` for all columns and draws it as a Seaborn heatmap. The result is hard to read on purpose. | `corr()`, `sns.heatmap()` |
| 6 | Selects three columns from `diabetes_corr` into `diabetes_corr_subset`. Sorts it twice, by `Diabetes_012` and then by `GenHlth`, and draws heatmaps of the first 10 and last 10 rows each time (four heatmaps). Adds a markdown cell about the strongest factors. | Column selection, `sort_values()`, `head()` and `tail()`, interpretation |
| 7 | Draws a pair plot of `BMI` and `Age` with `hue="Diabetes_012"`. Then bins BMI into 10 quantiles with `qcut()`, adds the result as a column, and draws a second pair plot. Both plots need titles. | `sns.pairplot()`, `hue`, `pd.qcut()` |
| 8 | Adds at least three charts of different types to the capstone notebook, each with labels and a markdown explanation of the insight it shows. | Choosing chart types, explaining insights |

**Submission:** Students save a version of each notebook and share it as Public with comments allowed. They submit two links: one for `CTD_Assignment_7` and one for the capstone notebook with the new charts. The Kaggle capstone project has a final deadline at the end of week 8.

**Note:** The assignment uses several tools that the lesson does not teach. `groupby()` comes from week 6. `sort_index()`, `pd.qcut()`, and selecting the first or last rows of a correlation matrix are new. The lesson shows `pd.cut()` only in week 6, and `qcut()` is not explained. The lesson also does not show how to put one title on a whole pair plot.

---

## FAQs and Common Trouble Spots

**1. "Two of my charts are drawn on top of each other."**
In a Kaggle notebook, charts appear inside the notebook, and no separate window opens. If a student draws two charts in one cell without calling `plt.show()` between them, Matplotlib may draw both on the same axes. Ask students to call `plt.show()` after each chart, or to put each chart in its own cell.

**2. "My pair plot title is in the wrong place."**
A pair plot is a grid of several small plots. Calling `plt.title()` after `sns.pairplot()` puts the title above only one small plot, usually the bottom-right one. The student needs a title for the whole figure. Point them to the object that `sns.pairplot()` returns and to the Matplotlib `suptitle()` method.

**3. "My BMI quantile pair plot gives an error or shows only one variable."**
`pd.qcut()` returns category values that are intervals, such as `(24.0, 26.0]`. Seaborn pair plots work only with numeric columns. If the student lists the new column in `vars`, Seaborn raises a `TypeError` that says the variable is categorical. If the student does not list it, Seaborn leaves the column out without an error. Students need numeric bin numbers instead of interval labels. Point them to the `labels` parameter in the `pd.qcut()` documentation.

**4. "My heatmap is unreadable," or "I do not know which rows to show."**
The full 22-column heatmap in Task 5 is supposed to be hard to read. In Task 6, the subset keeps all 22 rows but only three columns. Students sort the subset in descending order and then show the first 10 rows and the last 10 rows. The top row of the first heatmap is the sort column itself, with a correlation of 1.0. Students sometimes select rows instead of columns, or forget to sort before taking the first and last rows.

**5. "My charts look fine, but I am not sure what they mean."**
Remind students of Task 1. `Age` is a bucket number from 1 to 13, not an age in years. A high `GenHlth` value means poor health, which is why Task 4 creates `Health` as 5 minus `GenHlth`. In the Task 6 markdown cell and in the capstone charts, students should explain what each chart shows in plain words. The capstone needs at least three charts of different types, each with an explanation.
