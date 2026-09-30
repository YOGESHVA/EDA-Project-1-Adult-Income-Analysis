# EDA Project 1 – Adult Income Dataset Analysis

## 📌 Project Overview

This project performs **Exploratory Data Analysis (EDA)** on the `adult.csv` dataset using Python.

The project covers:

- Loading data with Pandas
- Dataset inspection
- Data cleaning
- Missing-value handling
- Duplicate removal
- Inconsistent text cleaning
- Data filtering and selection
- Statistical analysis
- Correlation analysis
- Outlier detection
- IQR method
- Z-score method
- Z-score trimming
- Winsorization
- Automated visualization using AutoViz
- Data visualization using Matplotlib

---

## 🛠️ Technologies Used

- Python
- Pandas
- NumPy / SciPy
- Matplotlib
- AutoViz

---

## 📂 Dataset

**Dataset:** `adult.csv`

The code loads the dataset using:

```python
import pandas as pd

df = pd.read_csv('/content/adult.csv')

The project works with columns such as:

Age
Education
EducationNum
Occupation
Workclass
Income
Hours per Week
Gender
Marital Status
Relationship
Race
Native Country
Final Weight
1. 📥 Load the Dataset
import pandas as pd

df = pd.read_csv('/content/adult.csv')

The dataset is loaded into a Pandas DataFrame named df.

2. 🔍 Dataset Inspection
First 5 rows
print(df.head(5))

Displays the first five records.

Last 5 rows
print(df.tail(5))

Displays the last five records.

Dataset information
print(df.info())

Shows:

Number of rows
Number of columns
Column names
Data types
Non-null values
Statistical summary
print(df.describe())

Provides statistical information for numerical columns.

Shape
print(df.shape)

Returns the number of rows and columns.

Data types
print(df.dtypes)

Displays the data type of each column.

Column names
print(df.columns)

Displays all column names.

3. 🧹 Missing-Value Analysis
print(df.isna().sum())

Checks the number of missing values in each column.

The project also converts ? values into missing values:

df = df.replace(' ?', None)

Then missing values are filled for selected categorical columns:

df['Workclass'] = df['Workclass'].fillna('Unknow')
df['Occupation'] = df['Occupation'].fillna('Unknow')
df['Native Country'] = df['Native Country'].fillna('Unknow')
4. ♻️ Duplicate Removal
df = df.drop_duplicates()

print(df.duplicated().sum())

This removes duplicate records and checks whether duplicates remain.

5. 🧽 Data Consistency Cleaning

The project standardizes text values using:

df['Workclass'] = df['Workclass'].str.strip().str.title()
df['Education'] = df['Education'].str.strip().str.title()
df['Marital Status'] = df['Marital Status'].str.strip().str.title()
df['Occupation'] = df['Occupation'].str.strip().str.title()
df['Relationship'] = df['Relationship'].str.strip().str.title()
df['Race'] = df['Race'].str.strip().str.title()
df['Gender'] = df['Gender'].str.strip().str.title()
Why?

str.strip() removes unnecessary spaces.

str.title() standardizes capitalization.

6. 🔎 Data Selection and Filtering
Select one column
print(df['Age'])
Select multiple columns
print(df[['Age', 'Education', 'Occupation', 'Income']])
Select rows using iloc
print(df.iloc[0:10])
Select rows and columns using loc
print(df.loc[10:20, ['Age', 'Education']])
Age greater than 50
print(df[df['Age'] > 50])
Income greater than 50K
print(df[df['Income'] == '>50K'])
Multiple conditions
print(df[
    (df['Age'] > 40) &
    (df['Hours per Week'] > 40)
])
Select multiple education categories
print(
    df[df['Education'].isin([' Bachelors', 'Masters'])]
)
7. 📊 Statistical Analysis
Mean, Median, Minimum and Maximum
print(
    df['Age'].agg([
        'mean',
        'median',
        'min',
        'max'
    ])
)
Standard deviation
print(
    df[['Age', 'Hours per Week']].agg(['std'])
)
Complete numerical summary
print(
    df.select_dtypes(include='number').agg([
        'count',
        'mean',
        'median',
        'min',
        'max',
        'std'
    ])
)
8. 🔢 Unique Values and Frequencies
Number of unique education values
print(df['Education'].nunique())
Frequency of education categories
print(df['Education'].value_counts())
Number of people in each income category
print(df['Income'].value_counts())
Percentage of income categories
print(
    df['Income'].value_counts(normalize=True) * 100
)
9. 📈 Conditional Statistical Analysis
Hours per week for people earning >50K
print(
    df[
        df['Income'] == '>50K'
    ]['Hours per Week'].agg([
        'count',
        'mean',
        'median',
        'min',
        'max',
        'std'
    ])
)
Age statistics for Bachelors education
print(
    df[
        df['Education'] == ' Bachelors'
    ]['Age'].agg([
        'count',
        'mean',
        'median',
        'min',
        'max',
        'std'
    ])
)
10. 🔗 Correlation Analysis
Correlation between numerical columns
print(df.corr(numeric_only=True))
Correlation between Age and Hours per Week
print(
    df[['Age', 'Hours per Week']].corr()
)

Correlation helps identify the statistical relationship between numerical variables.

11. 🏆 Mode Analysis

The project finds the most common education and occupation values.

print(
    "Education Mode:",
    df['Education'].mode()[0]
)

print(
    "Occupation Mode:",
    df['Occupation'].mode()[0]
)
12. 🔄 Missing Value Filling
Backward Fill
df['Age'] = df['Age'].bfill()

Backward fill uses a following available value.

Forward Fill
df['Age'] = df['Age'].ffill()

Forward fill uses the previous available value.

Interpolation
df['Age'] = df['Age'].interpolate()

Interpolation estimates missing numerical values based on surrounding values.

13. 🚨 Outlier Detection

An outlier is a value that is unusually far away from the normal range of the data.

The project uses multiple techniques.

13.1 IQR Method
Q1 = df['Age'].quantile(0.25)
Q3 = df['Age'].quantile(0.75)

IQR = Q3 - Q1

lower_bond = Q1 - 1.5 * IQR
upper_bond = Q3 + 1.5 * IQR
Identify IQR outliers
iqr_outliers = df[
    (df['Age'] < lower_bond) |
    (df['Age'] > upper_bond)
]

print("IQR Outliers:", len(iqr_outliers))
Formula
IQR = Q3 - Q1

Lower Bound = Q1 - 1.5 × IQR

Upper Bound = Q3 + 1.5 × IQR
14. 📐 Z-Score Method

The project uses SciPy's zscore():

from scipy.stats import zscore

df['Age_zscore'] = zscore(df['Age'])

Outliers are identified using:

z_outliers = df[
    df['Age_zscore'].abs() > 3
]

print("Z-Score Outliers:", len(z_outliers))
15. ✂️ Z-Score Trimming

Rows with an absolute Z-score greater than 3 are removed:

df_trimmed = df[
    df['Age_zscore'].abs() <= 3
]

print(df_trimmed)
16. 📏 Winsorization

The project also applies Winsorization:

from scipy.stats.mstats import winsorize

df['Age_winsorized'] = winsorize(
    df['Age'],
    limits=[0, 0.3]
)

Winsorization limits extreme values instead of simply deleting the rows.

17. 🎓 EducationNum Outlier Analysis

The project also performs outlier detection on EducationNum.

IQR
Q1 = df['EducationNum'].quantile(0.25)
Q3 = df['EducationNum'].quantile(0.75)

IQR = Q3 - Q1

lower_bond = Q1 - 1.5 * IQR
upper_bond = Q3 + 1.5 * IQR
Z-score
df['EducationNum_zscore'] = zscore(
    df['EducationNum']
)

z_outliers = df[
    df['EducationNum_zscore'].abs() > 2
]

print(
    "Z-Score Outliers:",
    len(z_outliers)
)
Trimming
df_trimmed = df[
    df['EducationNum_zscore'].abs() <= 2
]
Winsorization
df['EducationNum_winsorized'] = winsorize(
    df['EducationNum'],
    limits=[0, 0.4]
)
18. 🤖 Automated EDA with AutoViz

The project uses AutoViz for automated visualization.

from autoviz import AutoViz_Class

data = pd.read_csv('adult.csv')

AV = AutoViz_Class()

AV.AutoViz(
    filename='adult.csv',
    sep=',',
    depVar='Income',
    dfte=data
)

AutoViz can automatically generate visualizations from the dataset.

19. 📊 Matplotlib Visualization

The project uses Matplotlib for manual visualization.

import matplotlib.pyplot as plt
Bar Chart – Education vs Age
x = df['Education']
y = df['Age']

plt.bar(x, y)

plt.title('Education VS Age')
plt.xlabel('Education')
plt.ylabel('Age')

plt.show()

A bar chart is used to compare values across categories.

Scatter Plot – EducationNum vs Hours per Week
EducationNum = df['EducationNum']
Hours_per_week = df['Hours per Week']

plt.scatter(
    EducationNum,
    Hours_per_week
)

plt.title(
    'EducationNum Vs Hours_per_week'
)

plt.xlabel('EducationNum')
plt.ylabel('Hours_per_week')

plt.show()

A scatter plot is used to visualize the relationship between two numerical variables.

Pie Chart – Age-wise Gender Visualization
x = df['EducationNum']
y = df['Gender']

plt.pie(
    x,
    labels=y,
    autopct='%1.1f%%'
)

plt.title('Age wise Gender')

plt.show()

autopct displays percentages on the pie chart.

Stem Plot
Final_Weight = df['Final Weight']

plt.stem(Final_Weight)

plt.title('Final Weight')
plt.xlabel('Final Weight')

plt.show()

A stem plot displays individual data values using stems.

Line Plot
x = df['EducationNum']
y = df['Gender']

plt.plot(
    x,
    y,
    marker='o'
)

plt.title('Age wise Gender')

plt.show()

A line plot displays values connected by lines.

Box Plot
Age = df['Age']

plt.boxplot(Age)

plt.title('Age Individual Data')
plt.xlabel('Age')

plt.show()

A box plot is useful for understanding the distribution of numerical data and identifying possible outliers.

📌 EDA Workflow Used
Load Dataset
      ↓
Inspect Dataset
      ↓
Check Missing Values
      ↓
Remove Duplicates
      ↓
Clean Inconsistent Data
      ↓
Filter and Select Data
      ↓
Perform Statistical Analysis
      ↓
Analyze Correlations
      ↓
Detect Outliers
      ↓
Handle Outliers
      ↓
Create Visualizations
      ↓
Generate Insights

# 📊 Automatic EDA & Data Visualization

## 📌 Project Overview

This project demonstrates **Automatic Exploratory Data Analysis (EDA)** and **Automatic Data Visualization** using Python libraries.

Instead of manually performing every EDA step, this project uses automated tools to quickly understand a dataset, identify patterns, generate statistics, and create visualizations.

### 🛠️ Tools Used

* Python
* Pandas
* YData Profiling
* AutoViz
* Sweetviz
* Google Colab / Jupyter Notebook

---

## 📂 Dataset

The project uses the **`adult.csv`** dataset.

```python
import pandas as pd

data = pd.read_csv("adult.csv")
```

Pandas is used to load the CSV dataset into a DataFrame for analysis.

---

# 1️⃣ YData Profiling

### What is YData Profiling?

**YData Profiling** is a Python library that automatically generates a detailed EDA report for a dataset.

It provides information such as:

* Dataset overview
* Data types
* Missing values
* Duplicate values
* Descriptive statistics
* Variable information
* Data distributions
* Correlations

### Code

```python
import pandas as pd
from ydata_profiling import ProfileReport

data = pd.read_csv("adult.csv")

profile = ProfileReport(
    data,
    title="Pandas Profiling Report"
)

profile.to_notebook_iframe()
```

### How it works

**Step 1:** Load the dataset using Pandas.

**Step 2:** Create a `ProfileReport`.

**Step 3:** Display the generated report inside the notebook.

### Purpose

YData Profiling helps perform an initial understanding of the dataset quickly without manually writing many EDA commands.

---

# 2️⃣ AutoViz

## What is AutoViz?

**AutoViz** is an automatic visualization library that generates different charts from a dataset.

It can help identify:

* Relationships between variables
* Distributions
* Trends
* Patterns
* Important visual insights

### Code

```python
from autoviz.AutoViz_Class import AutoViz_Class
import pandas as pd

AV = AutoViz_Class()

data = pd.read_csv("adult.csv")

AV.AutoViz(
    filename="adult.csv",
    sep=",",
    depVar="Income",
    dfte=data
)
```

### Important Parameters

#### `filename`

Specifies the CSV file.

```python
filename="adult.csv"
```

#### `sep`

Specifies the separator used in the CSV file.

```python
sep=","
```

#### `depVar`

Specifies the dependent/target variable.

```python
depVar="Income"
```

#### `dfte`

Passes the DataFrame to AutoViz.

```python
dfte=data
```

### Purpose

AutoViz automatically creates visualizations so that we can understand the dataset without manually creating every chart.

---

# 3️⃣ Sweetviz

## What is Sweetviz?

**Sweetviz** is an automated EDA library that generates an HTML report containing information and visual analysis of a dataset.

### Code

```python
import pandas as pd
import sweetviz as sv

data = pd.read_csv("adult.csv")

report = sv.analyze(data)

report.show_html("sweetviz_report.html")
```

### How it works

**Step 1:** Load the dataset.

```python
data = pd.read_csv("adult.csv")
```

**Step 2:** Analyze the dataset.

```python
report = sv.analyze(data)
```

**Step 3:** Generate an HTML report.

```python
report.show_html("sweetviz_report.html")
```

The generated report can be opened in a web browser.

---

# 🔄 Project Workflow

```text
             adult.csv
                 │
                 ▼
          Load Dataset
                 │
                 ▼
              Pandas
                 │
       ┌─────────┼─────────┐
       ▼         ▼         ▼
    YData      AutoViz   Sweetviz
   Profiling
       │         │         │
       ▼         ▼         ▼
     EDA       Charts     HTML
    Report     & Viz     Report
```

---

# 📚 Concepts Learned

Through this project, I practiced:

* Loading CSV files using Pandas
* Automatic Exploratory Data Analysis
* Automated data profiling
* Automatic visualization
* Dataset analysis
* Generating HTML reports
* Working with Python data-analysis libraries
* Understanding the advantages of automated EDA tools

---

# 🎯 Why Automatic EDA?

Traditional EDA requires writing multiple commands to inspect:

* Shape
* Data types
* Missing values
* Duplicates
* Statistics
* Distributions
* Correlations
* Visualizations

Automatic EDA tools can perform many of these operations quickly and generate a structured report.

However, these tools should **support** manual analysis rather than completely replace understanding of the data.

---

# 💻 Technologies

| Technology      | Purpose                       |
| --------------- | ----------------------------- |
| Python          | Programming language          |
| Pandas          | Data loading and manipulation |
| YData Profiling | Automated EDA                 |
| AutoViz         | Automated visualization       |
| Sweetviz        | Automated EDA reports         |
| Google Colab    | Development environment       |

---

# 🚀 How to Run

### 1. Clone the repository

```bash
git clone <your-github-repository-link>
```

### 2. Open the notebook

Open the `.ipynb` file using:

* Google Colab
* Jupyter Notebook
* JupyterLab

### 3. Install required libraries

```bash
pip install pandas ydata-profiling autoviz sweetviz
```

### 4. Add the dataset

Place:

```text
adult.csv
```

in the project directory.

### 5. Run the notebook

Execute the cells step by step.

---

# 📁 Project Structure

```text
Automatic-EDA/
│
├── adult.csv
├── Automatic_Tools.ipynb
├── sweetviz_report.html
└── README.md
```

---

# 📈 Expected Output

The project generates:

### YData Profiling

A detailed interactive EDA report.

### AutoViz

Automatically generated data visualizations.

### Sweetviz

An HTML-based automated EDA report.

---

# 💡 Key Takeaway

This project helped me understand how automated Python tools can speed up the **Exploratory Data Analysis and visualization process**.

I learned how to use **Pandas, YData Profiling, AutoViz, and Sweetviz** to quickly inspect datasets and generate useful analytical reports.

---

## 👨‍💻 Author

**Yogesh Varma**

Aspiring Data Analyst

### Skills

* Python
* SQL
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Exploratory Data Analysis
* Data Visualization

---

⭐ If you find this project useful, feel free to explore the repository and give it a star.
