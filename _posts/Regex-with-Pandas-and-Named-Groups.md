
---
layout: post
title: Text Mining Basics with Regular Expressions
---

# Working with Text Data in pandas


```python
import pandas as pd

time_sentences = ["Monday: The doctor's appointment is at 2:45pm.", 
                  "Tuesday: The dentist's appointment is at 11:30 am.",
                  "Wednesday: At 7:00pm, there is a basketball game!",
                  "Thursday: Be back home by 11:15 pm at the latest.",
                  "Friday: Take the train at 08:10 am, arrive at 09:00am."]

df = pd.DataFrame(time_sentences, columns=['text'])
df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>text</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Monday: The doctor's appointment is at 2:45pm.</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Tuesday: The dentist's appointment is at 11:30...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Wednesday: At 7:00pm, there is a basketball game!</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Thursday: Be back home by 11:15 pm at the latest.</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Friday: Take the train at 08:10 am, arrive at ...</td>
    </tr>
  </tbody>
</table>
</div>




```python
# find the number of characters for each string in df['text']
df['text'].str.len()
```




    0    46
    1    50
    2    49
    3    49
    4    54
    Name: text, dtype: int64




```python
# find the number of tokens for each string in df['text']
df['text'].str.split().str.len()
```




    0     7
    1     8
    2     8
    3    10
    4    10
    Name: text, dtype: int64




```python
# find which entries contain the word 'appointment'
df['text'].str.contains('appointment')
```




    0     True
    1     True
    2    False
    3    False
    4    False
    Name: text, dtype: bool




```python
# find how many times a digit occurs in each string
df['text'].str.count(r'\d')
```




    0    3
    1    4
    2    3
    3    4
    4    8
    Name: text, dtype: int64




```python
# find all occurances of the digits
df['text'].str.findall(r'\d')
```




    0                   [2, 4, 5]
    1                [1, 1, 3, 0]
    2                   [7, 0, 0]
    3                [1, 1, 1, 5]
    4    [0, 8, 1, 0, 0, 9, 0, 0]
    Name: text, dtype: object




```python
# group and find the hours and minutes
df['text'].str.findall(r'(\d?\d):(\d\d)')
```




    0               [(2, 45)]
    1              [(11, 30)]
    2               [(7, 00)]
    3              [(11, 15)]
    4    [(08, 10), (09, 00)]
    Name: text, dtype: object




```python
# replace weekdays with '???'
df['text'].str.replace(r'\w+day\b', '???')
```




    0          ???: The doctor's appointment is at 2:45pm.
    1       ???: The dentist's appointment is at 11:30 am.
    2          ???: At 7:00pm, there is a basketball game!
    3         ???: Be back home by 11:15 pm at the latest.
    4    ???: Take the train at 08:10 am, arrive at 09:...
    Name: text, dtype: object




```python
# replace weekdays with 3 letter abbrevations
df['text'].str.replace(r'(\w+day\b)', lambda x: x.groups()[0][:3])
```




    0          Mon: The doctor's appointment is at 2:45pm.
    1       Tue: The dentist's appointment is at 11:30 am.
    2          Wed: At 7:00pm, there is a basketball game!
    3         Thu: Be back home by 11:15 pm at the latest.
    4    Fri: Take the train at 08:10 am, arrive at 09:...
    Name: text, dtype: object




```python
# create new columns from first match of extracted groups
df['text'].str.extract(r'(\d?\d):(\d\d)')
```

    /opt/conda/lib/python3.6/site-packages/ipykernel/__main__.py:2: FutureWarning: currently extract(expand=None) means expand=False (return Index/Series/DataFrame) but in a future version of pandas this will be changed to expand=True (return DataFrame)
      from ipykernel import kernelapp as app





<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>45</td>
    </tr>
    <tr>
      <th>1</th>
      <td>11</td>
      <td>30</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7</td>
      <td>00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>11</td>
      <td>15</td>
    </tr>
    <tr>
      <th>4</th>
      <td>08</td>
      <td>10</td>
    </tr>
  </tbody>
</table>
</div>




```python
# extract the entire time, the hours, the minutes, and the period
df['text'].str.extractall(r'((\d?\d):(\d\d) ?([ap]m))')
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
    </tr>
    <tr>
      <th></th>
      <th>match</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <th>0</th>
      <td>2:45pm</td>
      <td>2</td>
      <td>45</td>
      <td>pm</td>
    </tr>
    <tr>
      <th>1</th>
      <th>0</th>
      <td>11:30 am</td>
      <td>11</td>
      <td>30</td>
      <td>am</td>
    </tr>
    <tr>
      <th>2</th>
      <th>0</th>
      <td>7:00pm</td>
      <td>7</td>
      <td>00</td>
      <td>pm</td>
    </tr>
    <tr>
      <th>3</th>
      <th>0</th>
      <td>11:15 pm</td>
      <td>11</td>
      <td>15</td>
      <td>pm</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">4</th>
      <th>0</th>
      <td>08:10 am</td>
      <td>08</td>
      <td>10</td>
      <td>am</td>
    </tr>
    <tr>
      <th>1</th>
      <td>09:00am</td>
      <td>09</td>
      <td>00</td>
      <td>am</td>
    </tr>
  </tbody>
</table>
</div>




```python
# extract the entire time, the hours, the minutes, and the period with group names
df['text'].str.extractall(r'(?P<time>(?P<hour>\d?\d):(?P<minute>\d\d) ?(?P<period>[ap]m))')
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>time</th>
      <th>hour</th>
      <th>minute</th>
      <th>period</th>
    </tr>
    <tr>
      <th></th>
      <th>match</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <th>0</th>
      <td>2:45pm</td>
      <td>2</td>
      <td>45</td>
      <td>pm</td>
    </tr>
    <tr>
      <th>1</th>
      <th>0</th>
      <td>11:30 am</td>
      <td>11</td>
      <td>30</td>
      <td>am</td>
    </tr>
    <tr>
      <th>2</th>
      <th>0</th>
      <td>7:00pm</td>
      <td>7</td>
      <td>00</td>
      <td>pm</td>
    </tr>
    <tr>
      <th>3</th>
      <th>0</th>
      <td>11:15 pm</td>
      <td>11</td>
      <td>15</td>
      <td>pm</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">4</th>
      <th>0</th>
      <td>08:10 am</td>
      <td>08</td>
      <td>10</td>
      <td>am</td>
    </tr>
    <tr>
      <th>1</th>
      <td>09:00am</td>
      <td>09</td>
      <td>00</td>
      <td>am</td>
    </tr>
  </tbody>
</table>
</div>


