# covid_analysis.py
# COVID-19 Data Analysis & Visualization (Beginner-Friendly)
# Author: Your Name
# -----------------------------------------

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Set Seaborn style
sns.set(style="whitegrid")

# -------------------------------
# 1. Load Dataset
# -------------------------------
# Make sure you download a COVID-19 dataset from Kaggle
# Example: https://www.kaggle.com/datasets/imdevskp/corona-virus-report
# Save the file as "covid_data.csv" in the same folder as this script
try:
    df = pd.read_csv("covid_data.csv")
except FileNotFoundError:
    print("❌ ERROR: 'covid_data.csv' not found. Please download and place it here.")
    exit()

# -------------------------------
# 2. Clean Data
# -------------------------------
df.rename(columns={
    'ObservationDate': 'Date',
    'Country/Region': 'Country',
    'Province/State': 'Province',
    'Confirmed': 'Cases',
    'Deaths': 'Deaths',
    'Recovered': 'Recovered'
}, inplace=True)

# Convert date column to proper format
df['Date'] = pd.to_datetime(df['Date'])

# -------------------------------
# 3. Aggregate Data by Date
# -------------------------------
daily_data = df.groupby('Date')[['Cases','Deaths','Recovered']].sum().reset_index()

# -------------------------------
# 4. Visualizations
# -------------------------------

# Function to make and save plots
def make_plot(x, y, data, title, ylabel, color, filename, label):
    plt.figure(figsize=(12,6))
    sns.lineplot(x=x, y=y, data=data, color=color, label=label)
    plt.title(title)
    plt.xlabel("Date")
    plt.ylabel(ylabel)
    plt.legend()
    plt.savefig(filename)
    plt.close()

# Daily Cases
make_plot("Date", "Cases", daily_data, "Daily COVID-19 Cases",
          "Number of Cases", "blue", "daily_cases.png", "Cases")

# Daily Deaths
make_plot("Date", "Deaths", daily_data, "Daily COVID-19 Deaths",
          "Number of Deaths", "red", "daily_deaths.png", "Deaths")

# Daily Recoveries
make_plot("Date", "Recovered", daily_data, "Daily COVID-19 Recoveries",
          "Number of Recoveries", "green", "daily_recoveries.png", "Recovered")

# All in one chart
plt.figure(figsize=(14,7))
sns.lineplot(x='Date', y='Cases', data=daily_data, label="Cases", color="blue")
sns.lineplot(x='Date', y='Deaths', data=daily_data, label="Deaths", color="red")
sns.lineplot(x='Date', y='Recovered', data=daily_data, label="Recovered", color="green")
plt.title("COVID-19 Global Trends (Cases, Deaths, Recoveries)")
plt.xlabel("Date")
plt.ylabel("Count")
plt.legend()
plt.savefig("global_trends.png")
plt.close()

# -------------------------------
# 5. End Message
# -------------------------------
print("✅ Analysis complete! Charts saved as:")
print("- daily_cases.png")
print("- daily_deaths.png")
print("- daily_recoveries.png")
print("- global_trends.png")
