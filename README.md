# Investigating_Netflix_Movies

## Table of Contents

- [Project Overview](#project-overview)
- [Data Source](#data-source)
- [Tools](#tools)
- [Steps](#steps)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Result/Findings](#resultfindings)
- [Recommendations](#recommendations)
- [Limitations](#limitations)
- [References](#references)
  
### Project Overview

This exploratory data analysis is to investigate if the average duration of movies has been declining and explain some of the contributing factors, if any.

![Movie Duration by Year of Release](https://github.com/user-attachments/assets/9c476da6-b28c-4e0f-8d4d-24514cdbb2e5)

### Data Source

Data: The primary data used for this analysis is the "netflix_data.csv" file, containing detailed information about the movies showed by the Netflix company.

### Tools

- Python

### Steps
In the initial phase, I performed:
- Data Loading and inspection
- Handling missing value

### Exploratory Data Analysis

EDA involves exploring netflix data to answer key question, such as:
 - What is the title of the movie with the shortest duration, including its country of release, genre and release year ?
 - Use scatter plot to explore if movie duration are getting shorter:
    - Color code "Children" in "genre" variable as red, "Documentaries" as blue, "Stand-Up" as green, others as black
 - Are we certain that movies are getting shorter?

### Data Analysis

Include some interesting code/features worked with

```python
# Importing pandas and matplotlib
import pandas as pd
import matplotlib.pyplot as plt

# Read in the Netflix CSV as a DataFrame
netflix_df = pd.read_csv("netflix_data.csv")

# Subset the DataFrame for type "Movie"
netflix_subset = netflix_df[netflix_df["type"] == "Movie"]

# Select only the columns of interest
netflix_movies = netflix_subset[["title", "country", "genre", "release_year", "duration"]]

# Filter for durations shorter than 60 minutes
short_movies = netflix_movies[netflix_movies.duration < 60]

# Checking for missing values
print(short_movies.isna().sum())

threshold = len(short_movies) * 0.05

print(threshold)

col_drop = short_movies.columns[short_movies.isna().sum() <= threshold]

print(col_drop)

# Impute the mode for the rows with missing values in country variable
short_movies["country"] = short_movies["country"].fillna(short_movies["country"].mode()[0])

# Checking for missing values
print(short_movies.isna().sum())

threshold = len(short_movies) * 0.05

print(threshold)

col_drop = short_movies.columns[short_movies.isna().sum() <= threshold]

print(col_drop)

# Impute the mode for the rows with missing values in country variable
short_movies["country"] = short_movies["country"].fillna(short_movies["country"].mode()[0])

print(short_movies.isna().sum())

# Movie with the shortest duration
shortest_duration = short_movies[short_movies["duration"] == short_movies["duration"].min()]

Movie_with_shortest_duration = shortest_duration["title"]

print(Movie_with_shortest_duration)

# Define an empty list
colors = []

# Iterate over rows of netflix_movies
for label, row in netflix_movies.iterrows() :
    if row["genre"] == "Children" :
        colors.append("red")
    elif row["genre"] == "Documentaries" :
        colors.append("blue")
    elif row["genre"] == "Stand-Up":
        colors.append("green")
    else:
        colors.append("black")
        
# Inspect the first 10 values in your list        
colors[:10]

# Set the figure style and initalize a new figure
fig = plt.figure(figsize=(12,8))

# Create a scatter plot of duration versus release_year
plt.scatter(netflix_movies.release_year, netflix_movies.duration, c=colors)

# Create a title and axis labels
plt.title("Movie Duration by Year of Release")
plt.xlabel("Release year")
plt.ylabel("Duration (min)")

# Show the plot
plt.show()
```

### Result/Findings

The Analysis results are summarize as follows:
1. The shortest duration time for movie shown by Netflix company was 3 minutes.
2. The title of the movie is Silent, which belongs to the Children genre released in 2014 from the United States.
3. There wasn't certainty from the scatter plot if movies duration are getting shorter.

### Recommendations

- Further data need to be collected regarding viewers' appraisal

### Limitations
None

### References

1. Python For Data Analysis 3E (Online) by Wes Mckinney [Click here to preview](https://wesmckinney.com/book)
2. Mataplotlib Customization in Intermediate Python Course for Associate Data Scientist in Python Carrer Track in DataCamp by Hugo Bowne-Henderson
3. Filtering DataFrames in Intermediate Python Course for Associate Data Scientist in Python Carrer Track in DataCamp Inc by Hugo Bowne-Henderson
4. For loop in Intermediate Python Course for Associate Data Scientist in Python Carrer Track in DataCamp Inc by Hugo Bowne-Henderson
 
