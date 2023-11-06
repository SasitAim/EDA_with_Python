# EDA_with_Python


### Import library
```python
# import library
import opendatasets as od
import os
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
```
### Import data from Kaggle
```python
# กำหนด URL ของ dataset บน kaggle
url_data = 'https://www.kaggle.com/datasets/iamsouravbanerjee/airline-dataset'
# download dataset form kaggle โดยต้องระบุ URL ของ dataset ที่ต้องการโหดลก่อน
# ใช้ Kaggle AIP username and key
od.download(url_data)
```

```python
# ระบุ directory ที่จะทำการบันทึก data 
data_dir = './airline-dataset'
data_dir
```

```python
# download file ที่อยู่ใน directory ที่ระบุ
# output is file name 
os.listdir(data_dir)

```

### Create data frame
```python
# Create data frame
file_path = data_dir + '\Airline Dataset Updated.csv' # file_path = data_dir + 'file name'
df = pd.read_csv(file_path)
df
```

### Overview of Data
```python
# show first 5 rows of data
df.head()
# Check information of data as number of rows and columns, all column name, data type and Non-Null Count 
df.info()
# Check missing value in dataset
df.isnull().sum() 
# Show statistics about data (show only numeric data)
df.describe()
```
### leaning data
```python
# Cleaning data
# separate day and month from Departure Date
df['Departure Date'] = pd.to_datetime(df['Departure Date']) # แปลงคอลัม 'Departure Date' เป็น datetime
df['Day_of_week'] = df['Departure Date'].dt.dayofweek # เพิ่มคอลัม 'Day_of_Week' โดยใช้ .dt.dayofweek
df['Month'] = df['Departure Date'].dt.month
```

```python
# Check data after clean
df.info()
```

### Visualization 
```python
# Create data frame show only Nationality is Thailand
df_th = df.query('Nationality == "Thailand"').reset_index().drop('index', axis = 1 )
df_th
```

```python
# Create Chart
plt.figure(figsize=(25,5)) 
age_counts = df_th['Age'].value_counts()
sns.barplot(x = age_counts.index, y=age_counts.values)
plt.title('Age Distribution')
plt.xlabel('Age')
plt.ylabel('Count age')
```

```python
# Create Chart
pass_counts = df_th.value_counts()
sns.barplot(x = 'Month', y = age_counts.values)

plt.figure(figsize=(8,5))
sns.countplot(x= 'Month', data = df_th)
plt.title('Distribution of Thailand passengers by Month')
plt.xlabel('Month')
plt.ylabel('Amount of passengers')
```


```python
top_20 = df['Nationality'].value_counts().nlargest(20)

plt.figure(figsize=(12, 12))
sns.set(style="whitegrid")

ax = top_20.plot.pie(autopct='%1.1f%%', pctdistance=0.85, startangle= 90, textprops={'fontsize': 9})

centre_circle = plt.Circle((0,0),0.70,fc='white')
fig = plt.gcf()
fig.gca().add_artist(centre_circle)
plt.ylabel('')

plt.title('Distribution of passenger nationalities', fontsize=16)
```
