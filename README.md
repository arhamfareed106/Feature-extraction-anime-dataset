# Feature-extraction-anime-dataset

This project focuses on data cleaning, feature extraction, and exploratory data analysis of an anime dataset. The primary goal is to transform raw, unstructured data from a CSV file into a clean, well-structured format suitable for analysis and machine learning tasks.

## Dataset

The dataset is contained in `anime.csv` and has the following columns initially:
- `Rank`: The ranking of the anime.
- `Title`: A string containing various pieces of information like the title, type, episode count, airing dates, and member count.
- `Score`: The score of the anime.

The `Title` column is messy and contains multiple features that need to be extracted.

## Feature Extraction and Engineering

The core of this project is extracting meaningful features from the `Title` column and creating new, insightful columns in the DataFrame.

### 1. Loading and Initial Inspection

The first step is to load the `anime.csv` file into a pandas DataFrame and inspect its structure.

```python
import pandas as pd
import numpy as np

file_path = 'anime.csv'
df = pd.read_csv(file_path)
display(df.head())
```

### 2. Extracting Episode Count

The number of episodes is embedded within the `Title` string, typically in the format `(X eps)`. A function is used to parse this information.

- **`Episode`**: A new column is created to store the number of episodes. This is extracted from the `Title` column, cleaned of non-numeric characters, and converted to an integer type.

```python
def extract_episodes(txt):
    # ... implementation ...
df['Episode'] = df['Title'].apply(extract_episodes)
df['Episode'] = df['Episode'].str.replace(" eps", "").astype(int)
```

### 3. Extracting Airing Time and Duration

The airing period is also extracted from the `Title` column.

- **`Total Time`**: This column stores the airing period string (e.g., "Apr 2009 - Jul 2010").
- **`Month`**: The duration of the airing period is calculated in months and stored in this column.
- **`Start Year`** and **`End Year`**: The start and end years of the airing period are extracted.

```python
def extraction_time(txt):
    # ... implementation ...
df['Total Time'] = df['Title'].apply(extraction_time)

from dateutil.relativedelta import relativedelta
from datetime import datetime

def get_months(period):
    # ... implementation ...
df['Month'] = df['Total Time'].apply(get_months)

def extract_years(period):
    # ... implementation ...
df[['Start Year', 'End Year']] = df['Total Time'].apply(lambda x: pd.Series(extract_years(x)))
```

### 4. Extracting Member Count

The popularity of an anime, indicated by the member count, is extracted.

- **`Member Count`**: This column stores the number of members who have the anime on their list. It's extracted using regular expressions and converted to a numeric type.

```python
import re

def extract_member_count(title):
    # ... implementation ...
df['Member Count'] = df['Title'].apply(extract_member_count)
df['Member Count'] = pd.to_numeric(df['Member Count'], errors='coerce')
```

### 5. Extracting Anime Type

- **`Type`**: The format of the anime (e.g., TV, Movie, OVA) is extracted from the `Title` and stored in this new column.

### 6. Creating Categorical and Derived Features

- **`top5_show`**: A boolean flag indicating if the anime is in the top 5 based on its `Score`.
- **`longest_running`**: A boolean flag for the anime with the longest airing duration in months.
- **`Avg Episodes Per Year`**: A derived feature calculating the average number of episodes released per year.
- **`Score Category`**: The `Score` is binned into categories like "High", "Medium", and "Low" for easier analysis.

## Final DataFrame

After all the feature extraction and engineering steps, the final DataFrame is much richer and ready for in-depth analysis and visualization. It contains the original data plus all the newly created features, providing a solid foundation for any data science project.
