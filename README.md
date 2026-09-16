# Experiment 4 - Data Wrangling and Data Visualization

####   NAME: Sen Airish B. Bravo
####  2ECE-C

## Objectives:
1. filter tabular data using several categorical and numerical conditions;
2. construct focused DataFrames by selecting relevant features;
3. summarize the relationship between categorical features and a numerical variable; and
4. communicate a data comparison using clear and correctly labeled plots.

## A. Visayas Communication DataFrame
Create a DataFrame named VisComm containing students whose Hometown is Visayas and whose Track is Communication. Retain only Name, Gender, Math, Electronics, and Average.

* `df["Hometown"] == "Visayas"` - filters students from Visayas
* `df["Track"] == "Communication"` - filters students under the Communication track
* `&` - applies both conditions
* `VisComm[[...]]` - selects only the required columns
* `len(VisComm)` - gets the number of rows

#### Code
```python
import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_excel("board2.xlsx")

# Derive Average from the four subject scores
df["Average"] = (
    df["Math"] +
    df["Electronics"] +
    df["GEAS"] +
    df["Communication"]
) / 4

df["Average"] = df["Average"].round(2)

print("Data loaded successfully")
print(df.head())

# PART A - Visayas Communication DataFrame
visayas_students = df[df["Hometown"] == "Visayas"]

viscomm_students = visayas_students[
    visayas_students["Track"] == "Communication"
]

VisComm = viscomm_students[
    ["Name", "Gender", "Math", "Electronics", "Average"]
]

print("A. VISAYAS COMMUNICATION DATAFRAME")
print(VisComm)
print(f"Number of rows: {len(VisComm)}")
```

## B. Visayas Female DataFrame
Create a second DataFrame named VisFemale containing students whose Hometown is Visayas and whose Gender is Female. Retain only Name, Track, GEAS, Electronics, and Average.
Then display only the rows of VisFemale whose Average is at least 60 without overwriting VisFemale.

* `df["Hometown"] == "Visayas"` - filters students from Visayas
* `df["Gender"] == "Female"` - filters female students
* `VisFemale[VisFemale["Average"] >= 60]` - filters students with an Average of at least 60
* `passing_students` - stores the filtered result without changing VisFemale

#### Code
```python
# PART B - Visayas Female DataFrame
visayas_students = df[df["Hometown"] == "Visayas"]

visfemale_students = visayas_students[
    visayas_students["Gender"] == "Female"
]

VisFemale = visfemale_students[
    ["Name", "Track", "GEAS", "Electronics", "Average"]
]

print("B. VISAYAS FEMALE DATAFRAME")
print(VisFemale)
print(f"Number of rows: {len(VisFemale)}")

# Filter Average >= 60 without overwriting VisFemale
passing_students = VisFemale[
    VisFemale["Average"] >= 60
]

print("\nVis Female students with Average >= 60:")
print(passing_students)
print(f"Number of passing students: {len(passing_students)}")
```

## C. Category-Average Visualization
Calculate the mean Average for every category under Track, Gender, and Hometown. Display the three summary tables and create one figure containing three bar charts.

* `df.groupby("Track")["Average"].mean()` - calculates the mean Average for each Track
* `df.groupby("Gender")["Average"].mean()` - calculates the mean Average for each Gender
* `df.groupby("Hometown")["Average"].mean()` - calculates the mean Average for each Hometown
* `plt.subplots(1, 3)` - creates three bar charts in one figure
* `idxmax()` - identifies the category with the highest sample mean

#### Code
```python
# PART C - Category-Average Visualization
track_means = df.groupby("Track")["Average"].mean().round(2)
gender_means = df.groupby("Gender")["Average"].mean().round(2)
hometown_means = df.groupby("Hometown")["Average"].mean().round(2)

print("Mean Average by Track:")
print(track_means)
print("\nMean Average by Gender:")
print(gender_means)
print("\nMean Average by Hometown:")
print(hometown_means)

# Create one figure containing three bar charts
fig, axes = plt.subplots(1, 3, figsize=(15, 5))

# Mean Average by Track
axes[0].bar(
    track_means.index,
    track_means.values,
    color="blue"
)
axes[0].set_title("Mean Average by Track")
axes[0].set_xlabel("Track")
axes[0].set_ylabel("Mean Average Score")
axes[0].set_ylim(0, 100)
for i, val in enumerate(track_means.values):
    axes[0].text(i, val + 1, f"{val:.2f}", ha="center")

# Mean Average by Gender
axes[1].bar(
    gender_means.index,
    gender_means.values,
    color="orange"
)
axes[1].set_title("Mean Average by Gender")
axes[1].set_xlabel("Gender")
axes[1].set_ylabel("Mean Average Score")
axes[1].set_ylim(0, 100)
for i, val in enumerate(gender_means.values):
    axes[1].text(i, val + 1, f"{val:.2f}", ha="center")

# Mean Average by Hometown
axes[2].bar(
    hometown_means.index,
    hometown_means.values,
    color="green"
)
axes[2].set_title("Mean Average by Hometown")
axes[2].set_xlabel("Hometown")
axes[2].set_ylabel("Mean Average Score")
axes[2].set_ylim(0, 100)
for i, val in enumerate(hometown_means.values):
    axes[2].text(i, val + 1, f"{val:.2f}", ha="center")

plt.tight_layout()
plt.show()
```

## Interpretation Statements
Identify the category with the highest sample mean for Track, Gender, and Hometown.

#### Code
```python
track_highest = track_means.idxmax()
track_highest_value = track_means.max()

gender_highest = gender_means.idxmax()
gender_highest_value = gender_means.max()

hometown_highest = hometown_means.idxmax()
hometown_highest_value = hometown_means.max()

print("INTERPRETATION STATEMENTS")
print("-" * 50)
print(f"1. {track_highest} has the highest mean Average: {track_highest_value:.2f}")
print(f"2. {gender_highest} has the highest mean Average: {gender_highest_value:.2f}")
print(f"3. {hometown_highest} has the highest mean Average: {hometown_highest_value:.2f}")
print("\nNote: These are observations from this dataset only.")
print("Group differences do not prove causation.")
```

---
**README file Version history**
September 17, 2026 - Initial README output uploaded.
