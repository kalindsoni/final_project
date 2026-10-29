# 🦠 COVID-19 Data Analysis & Visualization with Python

📌 Table of Contents
📖 About the Project
🎯 Project Objectives
✨ Key Features
🛠️ Technologies Used
📂 Project Structure
📊 Visualizations
🧮 Data Analysis Explained
⚙️ Installation and Setup
▶️ How to Run
🎥 Video Explanation
📈 Key Insights
⚠️ Important Data Note
🚀 Future Improvements
👨‍💻 Author
📖 About the Project

The COVID-19 Data Analysis and Visualization project uses Python to explore COVID-19 statistics and present them through meaningful visualizations.

The project analyzes confirmed cases, deaths, recovered cases, and active cases. It also calculates death percentages, classifies records according to death counts, and compares countries based on reported COVID-19 data.

Using libraries such as Pandas, NumPy, Matplotlib, and Seaborn, raw data is transformed into charts and summary tables that make trends and patterns easier to understand.

This project is suitable for beginners learning Python programming, data cleaning, exploratory data analysis (EDA), data aggregation, and data visualization.

🎯 Project Objectives
Clean and preprocess COVID-19 datasets.
Handle missing values in important numerical columns.
Convert observation dates into a proper datetime format.
Calculate active cases and death percentages.
Categorize records based on the number of reported deaths.
Summarize COVID-19 statistics by country.
Visualize case trends over time.
Compare countries using bar charts and heatmaps.
Practice essential data analysis techniques with Python.
✨ Key Features
Feature Description
🧹 Data Cleaning Handles missing values in confirmed cases, deaths, and recoveries.
📅 Date Processing Converts observation dates into datetime format.
🧪 Active Cases Calculates confirmed cases minus deaths and recoveries.
📉 Death Percentage Calculates the percentage of confirmed cases resulting in reported deaths.
🏷️ Death Classification Assigns death-level categories based on numerical thresholds.
🌍 Country Analysis Generates a country-wise summary of COVID-19 statistics.
📈 Trend Analysis Displays confirmed cases over time.
📊 Country Comparison Compares the ten countries with the highest aggregated confirmed-case counts in the dataset.
🔥 Heatmap Visualizes confirmed cases, deaths, and recoveries together.
🛠️ Technologies Used
Technology Purpose
Python Core programming language
NumPy Numerical calculations and conditional logic
Pandas Data cleaning, transformation, and aggregation
Matplotlib Line charts and bar charts
Seaborn Heatmap visualization
📂 Project Structure
text
COVID-19-Data-Analysis/
│
├── covid_19_data.csv       # Input COVID-19 dataset
├── covid_analysis.py       # Python analysis script
├── README.md               # Project documentation
└── requirements.txt        # Python dependencies (optional)

Note: Save your Python code in covid_analysis.py and place the CSV dataset in the same project directory.
📊 Visualizations

This project creates three visualizations to explore COVID-19 data.

1. 📈 COVID-19 Cases Over Time

Chart type: Line chart

Displays the total confirmed cases recorded on each observation date.

Purpose:

Understand the overall growth of reported cases.
Identify periods of rapid increases.
Observe long-term changes in the pandemic's reported case burden.

Python code:

python
plt.plot(
    daily["ObservationDate"],
    daily["Confirmed"]
)

plt.xlabel("Date")
plt.ylabel("Confirmed Cases")
plt.title("COVID-19 Cases Over Time")

plt.show()

2.🌍 COVID-19 Cases by Country

Chart type: Bar chart

Compares the ten countries with the largest aggregated confirmed-case counts in the dataset.

Purpose:

Compare reported case counts between countries.
Identify countries with the highest calculated totals.
Present country-level statistics in an easy-to-read format.

Python code:

python
plt.figure(figsize=(10, 5))

plt.bar(
    res.index,
    res["totalConfirmed"]
)

plt.xlabel("Country")
plt.ylabel("Confirmed Cases")
plt.title("COVID-19 Cases by Country")

plt.xticks(rotation=45)
plt.show()

3.🔥 COVID-19 Data Heatmap

Chart type: Heatmap

Shows the relationship in magnitude between confirmed cases, deaths, and recovered cases across the selected countries.

Purpose:

Compare multiple numerical indicators.
Identify high and low values quickly.
Present several statistics in one visualization.

Python code:

python
sns.heatmap(
    res[
        [
            "totalConfirmed",
            "totalDeaths",
            "totalRecovered"
        ]
    ],
    annot=True
)

plt.title("COVID-19 Data Heatmap")
plt.show()

Visualization tip: Because confirmed cases, deaths, and recoveries can have very different magnitudes, consider normalizing the columns before plotting if you want to compare their relative patterns more clearly.
🧮 Data Analysis Explained
Step 1: Import Libraries and Load the Dataset
python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv("covid_19_data.csv")

Imports the required libraries and loads the CSV file into a Pandas DataFrame.

Step 2: Inspect and Clean the Data
python
print(df.head())
print(df.isna().sum())

colFill = ["Confirmed", "Deaths", "Recovered"]

df[colFill] = df[colFill].fillna(0)

head() displays the first five rows.
isna().sum() counts missing values in each column.
fillna(0) replaces missing values in the three selected numerical columns with zero.

Important: Replacing missing values with zero is an assumption, not proof that the true value was zero.

Step 3: Convert Dates
python
df["ObservationDate"] = pd.to_datetime(
    df["ObservationDate"]
)

Converts the observation date column into a datetime data type, making it easier to group, sort, and plot observations over time.

Step 4: Calculate Active Cases
python
df["Active"] = (
    df["Confirmed"]
    - df["Deaths"]
    - df["Recovered"]
)

Calculates active cases using:

Active Cases = Confirmed Cases − Deaths − Recovered Cases

This calculation assumes that the input values are consistent and refer to the same reporting snapshot.

Step 5: Calculate Death Percentage
python
df["Death_Percentage"] = np.where(
    df["Confirmed"] > 0,
    (df["Deaths"] / df["Confirmed"]) * 100,
    0
)

Calculates the percentage of confirmed cases represented by reported deaths.

If confirmed cases are zero, the result is set to zero to avoid division by zero.

This is a reported case-fatality percentage, not an estimate of the infection fatality rate.

Step 6: Classify Case Status
python
df["Result"] = np.where(
    df["Confirmed"] > 0,
    "Cases Found",
    "No Cases"
)

Creates a categorical column indicating whether a record contains any confirmed cases.

Cases Found: Confirmed cases are greater than zero.
No Cases: Confirmed cases are zero or less.
Step 7: Classify Death Levels
python
grading = [
    df["Deaths"] >= 100,
    df["Deaths"] >= 50,
    df["Deaths"] >= 10,
    df["Deaths"] > 0
]

df["Death_Level"] = np.select(
    grading,
    ["Very High", "High", "Medium", "Low"],
    default="No Deaths"
)

Assigns categories according to the death count in each individual row.

Death count Category
100 or more Very High
50–99 High
10–49 Medium
1–9 Low
0 or less No Deaths

These are custom analytical categories, not official medical severity classifications.

Step 8: Summarize Data by Country
python
res = df.groupby("Country/Region").agg(
    totalConfirmed=("Confirmed", "sum"),
    totalDeaths=("Deaths", "sum"),
    totalRecovered=("Recovered", "sum"),
    totalActive=("Active", "sum")
).round(1)

res = res.sort_values(
    by="totalConfirmed",
    ascending=False
).head(10)

print(res)

Groups records by country, calculates sums for each numerical indicator, sorts the results by confirmed cases, and selects the top ten countries.

Note: These are sums across all dates in the dataset, so they should not be interpreted as each country's final number of cases. See the important data note below for a more appropriate way to calculate latest reported totals.

Step 9: Aggregate Daily Confirmed Cases
python
daily = df.groupby("ObservationDate").agg(
    Confirmed=("Confirmed", "sum")
).reset_index()

Groups observations by date and adds the confirmed-case counts across all geographic records for each date.

The result is used to create the line chart showing the reported cumulative case burden over time.

⚙️ Installation and Setup
Prerequisites
Python 3.10 or later recommended.
A code editor such as Visual Studio Code or PyCharm, or Jupyter Notebook.
The covid_19_data.csv dataset.
Clone the Repository

Replace YOUR_USERNAME and YOUR_REPOSITORY with your actual GitHub details.

Navigate to the Project Folder
bash
cd YOUR_REPOSITORY

Install Required Libraries
bash
pip install numpy pandas matplotlib seaborn

Alternatively, create a requirements.txt file containing:

text
numpy
pandas
matplotlib
seaborn

Then install the dependencies:

bash
pip install -r requirements.txt

▶️ How to Run
Download or obtain the COVID-19 CSV dataset.
Place covid_19_data.csv in your project directory.
Save the analysis code as covid_analysis.py.
Open a terminal in the project directory.
Run the following command:
bash
python covid_analysis.py

The program will print the dataset preview, missing-value counts, and country summary. It will then display three plots:

COVID-19 Cases Over Time
COVID-19 Cases by Country
COVID-19 Data Heatmap
🎥 Video Explanation

Watch the project walkthrough here:

▶️ Project Demonstration Video: Add Your YouTube Video Link Here

The video can cover the following topics:

Introduction: Project purpose and dataset overview.
Data Cleaning: Handling missing values and converting dates.
Feature Engineering: Calculating active cases, death percentages, and death-level categories.
Country Analysis: Grouping and summarizing COVID-19 statistics.
Visualization: Explaining the line chart, bar chart, and heatmap.
Conclusion: Key findings, limitations, and possible improvements.
Replace the placeholder with your actual YouTube video URL once your explanation video is uploaded. The link above is a general YouTube link, not a project-specific video.
📈 Key Insights

The analysis is designed to help explore questions such as:

How did the reported cumulative COVID-19 case burden change over time?
Which countries have the largest aggregated case counts in the dataset?
How do reported deaths and recoveries compare across countries?
How can missing data affect statistical results?
How can Python transform raw health statistics into interpretable visualizations?

Actual findings should be reported only after running the code against the supplied dataset. The project documentation does not assume specific numerical results.

⚠️ Important Data Note

The current country summary uses .sum() across all observation dates. COVID-19 datasets commonly contain cumulative case counts for each date, so summing those daily snapshots repeatedly counts the same cases.

For a more meaningful comparison of countries, select the latest available observation for each country before creating the country summary.

For example:

python
latest_date = df["ObservationDate"].max()

latest_df = df[
    df["ObservationDate"] == latest_date
]

latest_res = latest_df.groupby(
    "Country/Region"
).agg(
    totalConfirmed=("Confirmed", "sum"),
    totalDeaths=("Deaths", "sum"),
    totalRecovered=("Recovered", "sum"),
    totalActive=("Active", "sum")
).sort_values(
    by="totalConfirmed",
    ascending=False
).head(10)

print(latest_res)

Use latest_res instead of res in the country bar chart and heatmap if you want those charts to show the latest available snapshot.

Other considerations:

Verify whether missing values mean zero or unknown.
Check for negative active-case calculations and inconsistent records.
Confirm the dataset's reporting conventions before drawing epidemiological conclusions.
Treat the death percentage as a descriptive statistic, not a causal or individual risk estimate.
🚀 Future Improvements

Potential enhancements include:

Add an interactive dashboard using Streamlit or Plotly.
Create country-specific trend charts.
Compare daily new cases instead of only cumulative cases.
Add filters for dates and countries.
Improve heatmap readability with normalization.
Export summary tables to CSV or Excel.
Add automated data-quality checks.
Include a reproducible dataset source and reporting date.
👨‍💻 Author

Your Name

Aspiring Data Analyst | Python | Data Visualization | Exploratory Data Analysis

GitHub: Your GitHub Profile
LinkedIn: Your LinkedIn Profile

Replace the placeholder details with your own profile links.

⭐ Support

If you find this project useful for learning Python and data analysis, consider giving the repository a star ⭐ on GitHub.

Built with Python, curiosity, and data-driven thinking. 🐍📊
