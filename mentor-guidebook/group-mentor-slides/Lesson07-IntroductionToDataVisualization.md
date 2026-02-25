---
marp: true
theme: default
paginate: true
style: |
  section {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    font-size: 28px;
  }
  h1 {
    color: #1a73e8;
  }
  h2 {
    color: #333;
  }
  code {
    background-color: #f0f0f0;
    padding: 2px 6px;
    border-radius: 4px;
  }
  pre code {
    font-size: 22px;
  }
  .columns {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
  }
---

<!-- _paginate: false -->

# Using These Slides

These slides are written in **Marp** markdown. You can present them two ways:

**In VS Code:**
1. Install the **Marp for VS Code** extension
2. Open this `.md` file
3. Click the Marp preview icon (top right) or use `Marp: Open Preview`
4. To present: `Marp: Export Slide Deck` → HTML or PDF

**In the browser:**
1. Go to [marp.app](https://marp.app/)
2. Paste or upload this file
3. Click **Preview** to present

Feel free to edit these slides to fit your style!

<!-- This first slide is for mentors only — not shown during the session. -->

---

<!-- _paginate: false -->

# Lesson 7: Introduction to Data Visualization

**Python Essentials — Code the Dream**

---

## Game Plan for Today

| Section | Time | What We'll Do |
|---------|------|---------------|
| Warm-Up | 5 min | Match the chart to the question |
| I Do | 10–15 min | Matplotlib basics, Seaborn, customization |
| We Do | 15–20 min | Build visualizations from a dataset together |
| You Do | 5–10 min | Create a chart from a prompt |
| Wrap-Up | 5 min | Assignment preview & capstone viz goals |

<!-- Mentor note: This lesson is more visual than previous ones — try to show examples on screen as you go. If possible, run the code live in a Kaggle notebook so students see the plots appear. The assignment uses the Diabetes Health Indicators dataset. -->

---

## Warm-Up (5 min)

### Match the Chart to the Question

| Question | Chart Type |
|----------|-----------|
| How has revenue changed month to month? | ??? |
| Which region has the highest sales? | ??? |
| What's the distribution of customer ages? | ??? |
| Which features are correlated? | ??? |

**Options:** Line Plot, Bar Plot, Histogram, Heatmap

**Share your matches in the chat.**

<!-- Mentor note: Answers: (1) Line Plot — trends over time, (2) Bar Plot — comparing categories, (3) Histogram — distribution of values, (4) Heatmap — correlation matrix. This exercise builds the mental model of "question → chart type" which is the key skill for the assignment. -->

---

## I Do: Why Visualize?

> A good chart can tell a story that a table of numbers can't.

**Visualization helps you:**
- **Spot patterns** (trends, clusters, outliers)
- **Compare** categories and groups
- **Communicate** findings to non-technical audiences

**Two main libraries:**

| Library | Strengths |
|---------|-----------|
| **Matplotlib** | Full control, basic plots, highly customizable |
| **Seaborn** | Statistical plots, nicer defaults, works with DataFrames |

```python
import matplotlib.pyplot as plt
import seaborn as sns
```

---

## I Do: Matplotlib — Line Plot

**Line plots show trends over continuous data:**

```python
import matplotlib.pyplot as plt

months = ["Jan", "Feb", "Mar", "Apr", "May", "Jun"]
revenue = [1000, 1200, 1500, 1700, 1600, 1800]

plt.plot(months, revenue, marker="o", color="blue")
plt.title("Monthly Revenue")
plt.xlabel("Month")
plt.ylabel("Revenue ($)")
plt.show()
```

**Key elements:**
- `marker="o"` adds dots at data points
- Always add `title`, `xlabel`, `ylabel`

> Every plot needs a title and axis labels — unlabeled charts are useless.

---

## I Do: Matplotlib — Bar Plot & Histogram

**Bar plot — comparing categories:**
```python
regions = ["North", "South", "East", "West"]
sales = [500, 700, 600, 800]

plt.bar(regions, sales, color=["skyblue", "salmon",
                               "lightgreen", "gold"])
plt.title("Sales by Region")
plt.xlabel("Region")
plt.ylabel("Sales ($)")
plt.show()
```

**Histogram — distribution of values:**
```python
import numpy as np

data = np.random.randn(1000)
plt.hist(data, bins=30, color="purple", alpha=0.7)
plt.title("Distribution of Random Data")
plt.xlabel("Value")
plt.ylabel("Frequency")
plt.show()
```

<!-- Mentor note: alpha controls transparency (0 = invisible, 1 = solid). bins controls how many bars the histogram has. The assignment uses histograms for age distributions in the diabetes dataset. -->

---

## I Do: Customizing Plots

**Make plots more informative:**

```python
plt.plot(months, revenue, marker="o", linestyle="--",
         color="red", linewidth=2)
plt.title("Monthly Revenue", fontsize=14, fontweight="bold")
plt.xlabel("Month", fontsize=12)
plt.ylabel("Revenue ($)", fontsize=12)
plt.grid(color="gray", linestyle="--", linewidth=0.5)
plt.legend(["Revenue"], loc="upper left")
plt.show()
```

**Common customizations:**

| Parameter | What It Does |
|-----------|-------------|
| `linestyle` | `"-"`, `"--"`, `":"`, `"-."` |
| `linewidth` | Thickness of the line |
| `fontsize` | Size of text labels |
| `grid()` | Background gridlines |
| `legend()` | Label for each data series |

---

## I Do: Seaborn — Heatmaps

**Heatmaps visualize correlations between columns:**

```python
import seaborn as sns

# Compute correlations
corr = df.corr(numeric_only=True)

# Plot as heatmap
plt.figure(figsize=(10, 6))
sns.heatmap(corr, annot=True, cmap="coolwarm", fmt=".2f")
plt.title("Correlation Heatmap")
plt.show()
```

**Reading a heatmap:**
- Values range from **-1.0** to **1.0**
- **Positive** (warm/red): both go up together
- **Negative** (cool/blue): one goes up, the other goes down
- **Near 0**: no relationship

<!-- Mentor note: The assignment (Tasks 5–6) has students build heatmaps from the diabetes dataset and subset them to find the strongest correlators. Stress that students need to understand what the columns MEAN before interpreting correlations. -->

---

## I Do: Seaborn — Pair Plots

**Pair plots show relationships between multiple variables:**

```python
sns.pairplot(df, vars=["age", "fare"],
             hue="survived", palette="Set2")
plt.show()
```

**What you see:**
- **Diagonal:** Distribution of each variable (KDE plot)
- **Off-diagonal:** Scatter plots between variable pairs
- **Colors:** Groups defined by `hue`

> Pair plots are great for exploratory analysis — they show you which variables relate to each other at a glance.

<!-- Mentor note: Assignment Task 7 creates pair plots for BMI vs Age colored by diabetes status. Students also learn qcut() to bucket continuous data for cleaner visualizations. -->

---

## I Do: Subplots — Multiple Charts in One Figure

```python
fig, axes = plt.subplots(1, 2, figsize=(12, 5))

# Left: bar plot
axes[0].bar(["A", "B", "C"], [10, 20, 15])
axes[0].set_title("Bar Plot")

# Right: histogram
axes[1].hist(np.random.randn(500), bins=20, color="teal")
axes[1].set_title("Histogram")

plt.tight_layout()
plt.show()
```

**Key details:**
- `plt.subplots(rows, cols)` creates a grid
- Access each subplot with `axes[index]`
- Use `set_title()` (not `plt.title()`) on each subplot
- `tight_layout()` prevents overlapping labels

---

## We Do: Visualizing the Titanic Dataset

**Let's build some charts together using Seaborn's built-in data:**

```python
import seaborn as sns
import matplotlib.pyplot as plt

titanic = sns.load_dataset("titanic")
print(titanic.head())
print(titanic.columns.tolist())
```

**Discussion:** What columns do we have to work with?

> Key columns: `survived`, `pclass`, `age`, `fare`, `sex`, `alone`

<!-- Mentor note: Use the Titanic dataset because it's built into Seaborn (no file loading needed) and has interesting relationships. The assignment uses a different dataset (diabetes) but the techniques are identical. -->

---

## We Do: Histogram — Age Distribution

```python
plt.hist(titanic["age"].dropna(), bins=25,
         color="steelblue", edgecolor="black")
plt.title("Age Distribution of Titanic Passengers")
plt.xlabel("Age")
plt.ylabel("Count")
plt.show()
```

**Discussion:** What does this tell us?
- Where is the peak?
- Are there many children? Elderly passengers?

> `.dropna()` removes NaN values — without it, the histogram may error.

<!-- Mentor note: The assignment (Task 3) asks students to create a similar histogram for the diabetes dataset's Age column. Stress that histograms show distributions — the shape tells a story (normal, skewed, bimodal, etc.). -->

---

## We Do: Heatmap — What Correlates with Survival?

```python
corr = titanic.corr(numeric_only=True)

plt.figure(figsize=(10, 6))
sns.heatmap(corr, annot=True, cmap="coolwarm", fmt=".2f")
plt.title("Titanic Correlation Heatmap")
plt.show()
```

**Look for strong correlations with `survived`:**

**Discussion:** Which factors most affected survival?

<!-- Mentor note: Key findings: fare has a positive correlation with survival (richer passengers survived more). pclass has a negative correlation (1st class = 1, so lower number = higher survival). adult_male has a strong negative correlation (women and children first). Let students discover these. Assignment Tasks 5–6 do the same analysis on the diabetes dataset. -->

---

## We Do: Subsetting a Heatmap

**Full heatmaps with many columns are hard to read. Subset them:**

```python
# Focus on what correlates with survival
subset = corr[["survived"]].sort_values(
    by="survived", ascending=False)

plt.figure(figsize=(4, 6))
sns.heatmap(subset, annot=True, cmap="coolwarm", fmt=".2f")
plt.title("Factors Correlated with Survival")
plt.show()
```

**This single-column heatmap makes the story clear:**
- Top = strongest positive correlation
- Bottom = strongest negative correlation

<!-- Mentor note: This is the technique for Assignment Task 6 — students subset the correlation matrix and sort to find the top/bottom 10 factors for diabetes and general health. Much more readable than a full 22x22 heatmap. -->

---

## We Do: Choosing the Right Chart

| Question You're Asking | Best Chart |
|------------------------|-----------|
| How is a value distributed? | **Histogram** |
| How does something change over time? | **Line plot** |
| How do categories compare? | **Bar plot** |
| What's correlated with what? | **Heatmap** |
| How do multiple variables relate? | **Pair plot** |
| How does a group differ from another? | **Box plot** |

> The chart type should match the **question**, not the data.

---

## You Do: Create a Chart (5 min)

**Using the Titanic data (or any DataFrame you have):**

Pick ONE of these and write the code:

1. A **bar plot** showing average fare by passenger class
   *Hint:* `titanic.groupby("pclass")["fare"].mean()`

2. A **histogram** of fares paid
   *Hint:* `plt.hist(titanic["fare"], bins=30)`

3. A **line plot** of average age by passenger class
   *Hint:* `titanic.groupby("pclass")["age"].mean()`

**Don't forget:** title, xlabel, ylabel!

<!-- Mentor note: Set a 5-minute timer. Any of the three is fine — the goal is to practice the pattern: get data → choose chart → add labels → show. Walk around and help students who get stuck on syntax. -->

---

## Assignment Preview

This week's Kaggle notebook uses the **Diabetes Health Indicators** dataset:

| Task | What You'll Do |
|------|---------------|
| 1–2 | Load data and understand column meanings |
| 3 | Histogram of age distributions |
| 4 | Line plot: health by age group |
| 5 | Full correlation heatmap |
| 6 | Subset heatmaps: top/bottom correlators |
| 7 | Pair plot: BMI vs Age by diabetes status |
| 8 | **Add visualizations to your capstone!** |

<!-- Mentor note: Task 1 is critical — students must read the column descriptions to understand what the numbers mean BEFORE making charts. Emphasize this. A chart without understanding the data tells a wrong story. Task 8 completes their Kaggle data pipeline capstone! -->

---

## Assignment Tips

**Task 4 (line plot):** Health scores are 1–5 where 5 = worst. Create a "Health" column = `5 - GenHlth` so higher = better.

**Task 6 (subset heatmaps):** Sort the correlation matrix by the column you care about, then plot just the top 10 and bottom 10 rows.

**Task 7 (pair plot):** BMI has too many unique values. Use `pd.qcut()` to bucket it into 10 quantiles for a cleaner chart.

**Task 8 (capstone):** Add 3+ charts to your capstone notebook. This completes your data pipeline project!

---

## Capstone Visualization Goals

Your **Kaggle Data Pipeline** capstone should now include:

- Data loading and inspection (Lesson 4)
- Data cleaning and validation (Lesson 5)
- Wrangling, aggregation, and features (Lesson 6)
- **3+ visualizations with explanations** (this week!)

> After this week, your first capstone project is feature-complete. You can keep refining it until the end of the course.

<!-- Mentor note: This is a milestone — after this assignment, the Kaggle data pipeline capstone is essentially complete. Next week starts the second half of the course (web scraping, SQL, Streamlit). Celebrate the progress! -->

---

## Wrap-Up

**Today we covered:**
- Matplotlib: line plots, bar plots, histograms
- Customization: titles, labels, grids, legends, colors
- Seaborn: heatmaps and pair plots
- Subplots for multiple charts in one figure
- Choosing the right chart for the right question

**Quick check:** If you want to see how two variables relate to each other, which chart type would you use?

<!-- Mentor note: Answer: scatter plot (from a pair plot) or heatmap for correlation. Both are valid — scatter shows individual data points, heatmap shows the statistical relationship. -->

---

## Getting Help This Week

- **Understand the data first** — read the column descriptions before plotting
- Experiment in a Kaggle notebook — you can see charts inline
- Matplotlib docs: [matplotlib.org/stable/gallery](https://matplotlib.org/stable/gallery/index.html)
- Seaborn docs: [seaborn.pydata.org/examples](https://seaborn.pydata.org/examples/index.html)
- Post questions in **#discussion** on Slack
- Book a **1:1 mentor session** for help with your capstone charts

**Next week:** Introduction to Web Scraping — pulling data from the web!

---

<!-- _paginate: false -->

# Let Your Data Speak! 📈

**A good chart tells a story that a spreadsheet never could.**

See you next session!
