# Assignment 7 Answer Key: Introduction to Data Visualization

**Mentor note:** This lesson is done in a **Kaggle notebook**, not the `python_homework` repo, and there is no PyTest file. The output is charts, which can't be auto-graded — you review by looking at the plots and the code that made them. The reference code below shows one correct approach for each task; students may vary in style, color, and figure size and still be fully correct. Focus your review on three things per plot: the **right chart type** for the question, the **right data mapped to the axes**, and the presence of a **title and axis labels**. The dataset is Alex Teboul's "Diabetes Health Indicators Dataset" — specifically the `diabetes_012_health_indicators_BRFSS2015.csv` file (22 columns).

---

## File Setup

- Work is done in a **Kaggle notebook** named `CTD_Assignment_7`, with the "Diabetes Health Indicators Dataset" added as the data source.
- Each task is marked with a **markdown cell** before its code.
- Submission is **two Kaggle sharing links** pasted into the assignment form: one for this homework notebook, one for the capstone project updated with visualizations.
- The notebook must be **saved (Save Version)** and shared as **Public with comments on** — check that the link actually opens for you and shows the run output (the plots should be visible).

Standard imports at the top of the notebook:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

---

## Task 1: Data Understanding — **Subjective (no code)**

This is a reading/exploration task in the Kaggle Data Explorer. There is no code to submit. It matters because the later plots are meaningless without knowing what the columns mean. If the student added a markdown note, a correct one shows they understood the key points:

- `Diabetes_012` has 3 values (0 = no diabetes, 1 = prediabetes, 2 = diabetes).
- `Age` is bucketed into 13 categories (not raw years).
- `GenHlth` runs 1–5 where **5 is worst**, and `PhysHlth` counts bad-health days where **higher is worse**.
- A weak answer treats `Age` as literal years or assumes a high `GenHlth`/`PhysHlth` is good.

---

## Task 2: Creating the Notebook and Loading Data — **Objective**

Load the `diabetes_012` file into a `diabetes` DataFrame and show the first 5 rows.

```python
diabetes = pd.read_csv("/kaggle/input/diabetes-health-indicators-dataset/diabetes_012_health_indicators_BRFSS2015.csv")
diabetes.head()
```

- Must load the file whose name contains **`diabetes_012`** (not the `binary` or `5050` files).
- Uses `.head()` (defaults to 5 rows). The path comes from the filename printed by Kaggle's first cell — exact path text may differ slightly, that's fine.

---

## Task 3: A Histogram for Age Distributions — **Objective**

Matplotlib histogram of the `Age` column, with title and labeled axes.

```python
plt.hist(diabetes["Age"], bins=13, edgecolor="black")
plt.title("Distribution of Respondents by Age Group")
plt.xlabel("Age")
plt.ylabel("Count")
plt.show()
```

- Correct chart type is a **histogram** (`plt.hist`), plotting the single `Age` column — not a bar chart of some aggregate.
- X axis is Age, Y axis is Count, and both are labeled with a title present. Bin count is flexible (13 matches the 13 age buckets but isn't required).

---

## Task 4: General Health over Time — **Objective**

Group by age, aggregate `GenHlth` by mean, add an inverted `Health` column, sort by index, then line-plot.

```python
health_by_age = diabetes.groupby("Age").agg({"GenHlth": "mean"})
health_by_age["Health"] = 5 - health_by_age["GenHlth"]
health_by_age = health_by_age.sort_index()

plt.plot(health_by_age.index, health_by_age["Health"])
plt.title("Average General Health by Age Group")
plt.xlabel("Age")
plt.ylabel("Health (higher is better)")
plt.show()
```

- Uses `groupby("Age")` with a **mean** aggregation of `GenHlth`.
- `Health` column is `5 - GenHlth` (the inversion so high = good). This is the conceptual point of the task.
- Sorted with `sort_index()` and drawn as a **line plot** with Age (the index) on X and `Health` on Y, labeled and titled.

---

## Task 5: A Heat Map of All Columns — **Objective**

Correlation matrix of every column, shown as a Seaborn heatmap.

```python
diabetes_corr = diabetes.corr()

plt.figure(figsize=(14, 12))
sns.heatmap(diabetes_corr, annot=True, cmap="coolwarm", fmt=".2f")
plt.title("Correlation Heat Map of All Columns")
plt.show()
```

- `diabetes_corr` must be a **correlation matrix** (`diabetes.corr()`), passed to `sns.heatmap`.
- Deprecation warnings from Seaborn are expected and can be ignored (the assignment says so).
- The result is intentionally cramped/hard to read — that's the point, and Task 6 fixes it. `annot`/`cmap` choices are optional polish.

---

## Task 6: Subset Heat Maps — **Objective**

Subset the correlation matrix to three columns, sort, and show first/last 10 rows as heatmaps.

```python
diabetes_corr_subset = diabetes_corr[["Diabetes_012", "HeartDiseaseorAttack", "GenHlth"]]

# Sorted on Diabetes_012
diabetes_corr_subset = diabetes_corr_subset.sort_values("Diabetes_012", ascending=False)

sns.heatmap(diabetes_corr_subset.head(10), annot=True, cmap="coolwarm")
plt.title("Top 10 Factors Correlated with Diabetes")
plt.show()

sns.heatmap(diabetes_corr_subset.tail(10), annot=True, cmap="coolwarm")
plt.title("Factors Negatively / Weakly Correlated with Diabetes")
plt.show()

# Re-sort on GenHlth
diabetes_corr_subset = diabetes_corr_subset.sort_values("GenHlth", ascending=False)

sns.heatmap(diabetes_corr_subset.head(10), annot=True, cmap="coolwarm")
plt.title("Top 10 Factors Correlated with Poor General Health")
plt.show()

sns.heatmap(diabetes_corr_subset.tail(10), annot=True, cmap="coolwarm")
plt.title("Factors Weakly / Negatively Correlated with Poor General Health")
plt.show()
```

- Subset selects the three named columns from `diabetes_corr` (a DataFrame with those columns, all rows).
- Sorted **descending** with `sort_values(...)` first on `Diabetes_012`, later re-sorted on `GenHlth`.
- Four (or more) heatmaps total: first 10 and last 10 rows for each sort, each with a title.
- Look for a **markdown cell** (Task 6.6) naming the strongest factors. A good answer notes that `GenHlth`, `HighBP`, `BMI`, `HighChol`, `Age`, `PhysHlth`, and `DiffWalk` correlate positively with diabetes, while items like `PhysActivity`, `Education`, and `Income` correlate negatively. Exact wording varies; the student should show they read the plots.

---

## Task 7: A Pair Plot — BMI vs. Age — **Objective**

Seaborn pair plot of `BMI` and `Age` colored by `Diabetes_012`, then a cleaner version using `qcut` BMI quantiles.

```python
sns.pairplot(diabetes, vars=["BMI", "Age"], hue="Diabetes_012",
             palette=['#FF5733', '#33FF57', '#3357FF'])
plt.suptitle("BMI vs. Age by Diabetes Status", y=1.02)
plt.show()

# Bucket BMI into 10 quantiles
diabetes["BMI_Quantiles"] = pd.qcut(diabetes["BMI"], 10, duplicates="drop")

sns.pairplot(diabetes, vars=["Age", "BMI_Quantiles"], hue="Diabetes_012",
             palette=['#FF5733', '#33FF57', '#3357FF'])
plt.suptitle("Age vs. BMI Quantiles by Diabetes Status", y=1.02)
plt.show()
```

- Correct chart is `sns.pairplot` with **`hue="Diabetes_012"`** so the three diabetes classes are colored separately.
- Second plot uses **`pd.qcut(diabetes["BMI"], 10)`** to bin BMI into 10 quantiles, adds that Series as a column, and re-plots. (`duplicates="drop"` is a reasonable safeguard if quantile edges collide; not required.)
- Both plots have a descriptive title. The palette is a suggestion from the assignment — any clear color mapping is fine.

---

## Task 8: Add Visualizations to Your Capstone Project — **Subjective**

Open-ended: the student adds 3+ visualizations to their own capstone notebook. This is graded against the assignment's rubric. A strong submission has:

- **3 or more** clear, appropriately-chosen charts using a **variety of chart types** (e.g. histogram, bar, scatter, box, heatmap) — not three versions of the same chart.
- Every chart has a **title, labeled axes, and a legend/color scheme** where relevant, and the chart type actually fits the question being asked.
- **Markdown explanations** stating the insight each chart reveals and connecting it back to the project's data/problem.
- A weak submission uses one repeated chart type, leaves axes/titles off, or shows plots with no written interpretation of what they mean.

Note: this is a capstone-progress task, so it may be incomplete — students have until the last week of class to refine it. Judge progress, not perfection.
