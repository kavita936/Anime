# Anime Data Analysis

This notebook performs an initial analysis on a dataset of anime titles, extracting key information like the number of episodes and airing timeframes from the 'Title' column.

## Setup

First, we install the necessary libraries and import them:

```python
!pip install pandas
import pandas as pd
```

## Data Loading

The dataset is loaded from a CSV file named `anime.csv`. The notebook expects the user to upload this file.

```python
from google.colab import files

uploaded = files.upload()

for fn in uploaded.keys():
  print(f'User uploaded file "{fn}" with length {len(uploaded[fn])} bytes')

df1 = pd.read_csv('anime.csv')
```

## Data Exploration (Initial)

We display the first few rows of the DataFrame to understand its structure:

```python
df1.head()
```

## Feature Engineering: Episodes

A custom function `extract_episode` is defined to parse the 'Title' column and extract the number of episodes. This extracted information is then cleaned to retain only numerical values and converted to an integer type.

```python
def extract_episode(txt):
  check = False
  data = ""
  for i in txt:
    if i == '(': # Start of potential episode data
      check = True
    elif i == ')': # End of episode data
      break
    if check and i != '(':
      data += i
  return data

df1['episodes'] = df1["Title"].apply(extract_episode)
df1['episodes'] = df1['episodes'].astype(str).str.replace(r'\\D', '', regex=True).astype(int)
```

## Feature Engineering: Airing Time

Another custom function `extraction_time` is used to extract the airing period of the anime, typically found after the episodes information in the 'Title' column.

```python
def extraction_time(txt):
  data = ""
  for i in range(len(txt)):
    if txt[i] == ')':
      start_extract = i + 1
      end_extract = min(start_extract + 18, len(txt))
      data = txt[start_extract:end_extract].strip()
      return data
  return data

df1['total time'] = df1['Title'].apply(extraction_time)
```

## Final Data View

After feature engineering, the DataFrame is displayed again to show the newly created 'episodes' and 'total time' columns.

```python
df1.head()
```
