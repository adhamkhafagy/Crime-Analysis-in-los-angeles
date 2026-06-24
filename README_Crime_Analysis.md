# 🚔 Crime Analysis in Los Angeles

Professional README generated from the notebook.


![Los Angeles skyline](la_skyline.jpg)

Los Angeles, California 😎. The City of Angels. Tinseltown. The Entertainment Capital of the World! 

Known for its warm weather, palm trees, sprawling coastline, and Hollywood, along with producing some of the most iconic films and songs. However, as with any highly populated city, it isn't always glamorous and there can be a large volume of crime. That's where you can help!

You have been asked to support the Los Angeles Police Department (LAPD) by analyzing crime data to identify patterns in criminal behavior. They plan to use your insights to allocate resources effectively to tackle various crimes in different areas.

## The Data

They have provided you with a single dataset to use. A summary and preview are provided below.

It is a modified version of the original data, which is publicly available from Los Angeles Open Data.

# crimes.csv

| Column     | Description              |
|------------|--------------------------|
| `'DR_NO'` | Division of Records Number: Official file number made up of a 2-digit year, area ID, and 5 digits. |
| `'Date Rptd'` | Date reported - MM/DD/YYYY. |
| `'DATE OCC'` | Date of occurrence - MM/DD/YYYY. |
| `'TIME OCC'` | In 24-hour military time. |
| `'AREA NAME'` | The 21 Geographic Areas or Patrol Divisions are also given a name designation that references a landmark or the surrounding community that it is responsible for. For example, the 77th Street Division is located at the intersection of South Broadway and 77th Street, serving neighborhoods in South Los Angeles. |
| `'Crm Cd Desc'` | Indicates the crime committed. |
| `'Vict Age'` | Victim's age in years. |
| `'Vict Sex'` | Victim's sex: `F`: Female, `M`: Male, `X`: Unknown. |
| `'Vict Descent'` | Victim's descent:<ul><li>`A` - Other Asian</li><li>`B` - Black</li><li>`C` - Chinese</li><li>`D` - Cambodian</li><li>`F` - Filipino</li><li>`G` - Guamanian</li><li>`H` - Hispanic/Latin/Mexican</li><li>`I` - American Indian/Alaskan Native</li><li>`J` - Japanese</li><li>`K` - Korean</li><li>`L` - Laotian</li><li>`O` - Other</li><li>`P` - Pacific Islander</li><li>`S` - Samoan</li><li>`U` - Hawaiian</li><li>`V` - Vietnamese</li><li>`W` - White</li><li>`X` - Unknown</li><li>`Z` - Asian Indian</li> |
| `'Weapon Desc'` | Description of the weapon used (if applicable). |
| `'Status Desc'` | Crime status. |
| `'LOCATION'` | Street address of the crime. |


## 💻 Code

```python
# Re-run this cell
# Import required libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
crimes = pd.read_csv("crimes.csv", dtype={"TIME OCC": str})
crimes.head()
```

### Output

```
       DR_NO   Date Rptd  ...  Status Desc                                 LOCATION
0  220314085  2022-07-22  ...  Invest Cont  2500 S  SYCAMORE                     AV
1  222013040  2022-08-06  ...  Invest Cont  3300    SAN MARINO                   ST
2  220614831  2022-08-18  ...  Invest Cont                        1900    TRANSIENT
3  231207725  2023-02-27  ...  Invest Cont  6200    4TH                          AV
4  220213256  2022-07-14  ...  Invest Cont  1200 W  7TH                          ST

[5 rows x 12 columns]
```


## 💻 Code

```python
# Start coding here
# Use as many cells as you need
```


## 💻 Code

```python
#Exploring the data
crimes.info()
```

### Output

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 185715 entries, 0 to 185714
Data columns (total 12 columns):
 #   Column        Non-Null Count   Dtype 
---  ------        --------------   ----- 
 0   DR_NO         185715 non-null  int64 
 1   Date Rptd     185715 non-null  object
 2   DATE OCC      185715 non-null  object
 3   TIME OCC      185715 non-null  object
 4   AREA NAME     185715 non-null  object
 5   Crm Cd Desc   185715 non-null  object
 6   Vict Age      185715 non-null  int64 
 7   Vict Sex      185704 non-null  object
 8   Vict Descent  185705 non-null  object
 9   Weapon Desc   73502 non-null   object
 10  Status Desc   185715 non-null  object
 11  LOCATION      185715 non-null  object
dtypes: int64(2), object(10)
memory usage: 17.0+ MB

```


## 💻 Code

```python
#Extract the first two digits from "TME OCC" to assign to a new column calles "HOUR OCC" 
#Set the "HOUR OCC" column to int type
crimes["HOUR OCC"]=crimes["TIME OCC"].str[:2].astype(int)
crimes.head()
```

### Output

```
       DR_NO   Date Rptd  ...                                 LOCATION HOUR OCC
0  220314085  2022-07-22  ...  2500 S  SYCAMORE                     AV       11
1  222013040  2022-08-06  ...  3300    SAN MARINO                   ST       16
2  220614831  2022-08-18  ...                        1900    TRANSIENT       12
3  231207725  2023-02-27  ...  6200    4TH                          AV        6
4  220213256  2022-07-14  ...  1200 W  7TH                          ST        9

[5 rows x 13 columns]
```


## 💻 Code

```python
#Create a countplot to find the largest frequency of crimes by "HOUR OCC"
sns.countplot(x="HOUR OCC",data=crimes)
plt.show()
```

### Output

```
<Figure size 640x480 with 1 Axes>
```


## 💻 Code

```python
#Finding the hour where most crimes committed
peak_hour = crimes["HOUR OCC"].value_counts()
peak_crime_hour=peak_hour.idxmax()

```


## 💻 Code

```python
#subsetting the night hours in a new dataframe called night_hours
night_time=crimes[crimes['HOUR OCC'].isin([22,23,0,1,2,3])]
```


## 💻 Code

```python
#Group by are name and count occurrences by hour filtering 
peak_night_crime_location = night_time.groupby("AREA NAME",as_index=False)["HOUR OCC"].count().sort_values("HOUR OCC",ascending=False).iloc[0]["AREA NAME"]
print(peak_night_crime_location)
```

### Output

```
Central

```


## 💻 Code

```python
#Assigning bins and labels
age_bins=[0,17,25,34,44,54,64,np.inf]
age_labels=["0-17","18-25","26-34","35-44","45-54","55-64","65+"]

```


## 💻 Code

```python
#Adding a new column to the crimes df containing binned age bracket values
crimes["Age Bracket"] = pd.cut(crimes["Vict Age"],labels=age_labels,bins=age_bins)
```


## 💻 Code

```python
#Counting crimes by victim age group
victim_ages=crimes["Age Bracket"].value_counts()
print(victim_ages)
```

### Output

```
26-34    47470
35-44    42157
45-54    28353
18-25    28291
55-64    20169
65+      14747
0-17      4528
Name: Age Bracket, dtype: int64

```
