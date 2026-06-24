# 🚔 Crime Analysis in Los Angeles

## 📌 Project Overview

This project analyzes crime incidents reported by the Los Angeles Police Department (LAPD) to uncover patterns in criminal activity across time, location, and victim demographics.

Using Python and exploratory data analysis techniques, the project identifies the busiest crime hours, the area with the highest nighttime crime activity, and the most affected victim age group.

---

## 🎯 Business Problem

The Los Angeles Police Department (LAPD) wants to better understand crime patterns in the city.

The analysis focuses on answering three key questions:

1. What hour has the highest frequency of crimes?
2. Which area experiences the most crimes during nighttime hours?
3. Which age group is most frequently victimized?

---

## 📂 Dataset

**File:** `crimes.csv`

### Dataset Overview

| Metric | Value |
|----------|----------:|
| Records | 185,715 |
| Features | 12 |
| Source | LAPD Crime Data |

---

# 📥 Load Libraries

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

crimes = pd.read_csv("crimes.csv", dtype={"TIME OCC": str})
crimes.head()
```

### Sample Output

| DR_NO | AREA NAME | Vict Age |
|--------:|-----------|----------:|
| 220314085 | Southwest | 27 |
| 222013040 | Olympic | 60 |
| 220614831 | Hollywood | 28 |

---

# 🔍 Explore Dataset Structure

```python
crimes.info()
```

### Dataset Summary

| Metric | Value |
|----------|----------:|
| Total Rows | 185,715 |
| Total Columns | 12 |
| Memory Usage | ~17 MB |

---

# ⚙️ Feature Engineering

## Extract Crime Hour

```python
crimes["HOUR OCC"] = crimes["TIME OCC"].str[:2].astype(int)
crimes.head()
```

### Example Output

| TIME OCC | HOUR OCC |
|----------|----------:|
| 1110 | 11 |
| 1630 | 16 |
| 2350 | 23 |

---

# 📊 Crime Frequency by Hour

```python
sns.countplot(x="HOUR OCC", data=crimes)
plt.show()
```

### Visualization

```text
Visuals/crime_frequency_by_hour.png
```

### Insight

Crime incidents are not evenly distributed throughout the day. Certain hours show significantly higher crime activity.

---

# 🕛 Peak Crime Hour Analysis

```python
peak_hour = crimes["HOUR OCC"].value_counts()
peak_crime_hour = peak_hour.idxmax()
```

### Result

| Metric | Value |
|----------|----------|
| Peak Crime Hour | 12 PM |

✅ Noon is the busiest hour for reported crimes.

---

# 🌙 Nighttime Crime Analysis

## Filter Night Hours

```python
night_time = crimes[crimes["HOUR OCC"].isin([22,23,0,1,2,3])]
```

## Find Highest Nighttime Crime Area

```python
peak_night_crime_location = (
    night_time.groupby("AREA NAME", as_index=False)["HOUR OCC"]
    .count()
    .sort_values("HOUR OCC", ascending=False)
    .iloc[0]["AREA NAME"]
)

print(peak_night_crime_location)
```

### Output

```python
Central
```

### Result

| Metric | Value |
|----------|----------|
| Highest Nighttime Crime Area | Central Division |

---

# 👤 Victim Age Analysis

## Create Age Groups

```python
age_bins = [0,17,25,34,44,54,64,np.inf]

age_labels = [
    "0-17",
    "18-25",
    "26-34",
    "35-44",
    "45-54",
    "55-64",
    "65+"
]
```

## Assign Age Brackets

```python
crimes["Age Bracket"] = pd.cut(
    crimes["Vict Age"],
    bins=age_bins,
    labels=age_labels
)
```

## Count Crimes by Age Group

```python
victim_ages = crimes["Age Bracket"].value_counts()
print(victim_ages)
```

### Output

| Age Group | Crime Count |
|------------|-----------:|
| 26–34 | 47,470 |
| 35–44 | 42,157 |
| 45–54 | 28,353 |
| 18–25 | 28,291 |
| 55–64 | 20,169 |
| 65+ | 14,747 |
| 0–17 | 4,528 |

### Result

✅ The **26–34** age group experiences the highest number of crimes.

---

# 📊 Key Findings

| Analysis Area | Result |
|---------------|--------|
| Peak Crime Hour | 🕛 12 PM |
| Highest Nighttime Crime Area | 🌙 Central Division |
| Most Affected Age Group | 👤 26–34 Years |
| Records Analyzed | 📂 185,715 |

---

# 🎯 Conclusion

The analysis reveals clear temporal and demographic crime patterns within Los Angeles.

### Key Insights

- Crime activity peaks around noon.
- Central Division records the highest number of nighttime crimes.
- Individuals aged 26–34 are the most frequently victimized demographic.

These findings can help support data-driven policing and resource allocation decisions.

---

# 🚀 Skills Demonstrated

- Python
- Pandas
- NumPy
- Data Cleaning
- Exploratory Data Analysis (EDA)
- Feature Engineering
- Data Visualization
- GroupBy Aggregation
- Business Insight Generation

---

## 👤 Author

Adham Refaat Soliman

Data Analyst | Python | SQL | Power BI | Sports Analytics
