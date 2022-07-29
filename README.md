# 1202
Data Analysis Tools Analytics
# Python Code to analyse Youtube dataset
- ### Reading the dataset using read_csv function
```python
import pandas as pd
import numpy as np
data=pd.read_csv(r"Path to youtube_dataset.csv", encoding='cp1252')
data.head()  
```
- ### Finding the channel type distribution of top 1000 records
```python
def subset_data_channeltype_distribution(df, no_of_rows):
  subset = df.head(no_of_rows)
  return subset.groupby('channeltype')['channeltype'].count()
#Calling the Function
subset_data_channeltype_distribution(data,1000)
```
- ### Inserting Top 1000 records into CSV file
```python
subset_data_frame = data.head(1000)
# The below step will create a new file "youtube_subset" in the same directory. 
# Index = false will remove the index column of the dataframe while creating the file
subset_data_frame.to_csv("youtube_subset.csv", index=False) 
```
- ### Reading the "youtube_subset" file created in previous Step
```python
# Add the path to the previous file inside the read_csv function.
first_1k_data = pd.read_csv(r"Path to the file youtube_subset.csv saved in above step") 
```
- ### Creating the SQL Database connection
```python
from sqlalchemy import create_engine
# Substitute user_name, username_password and host_name to hide sensitive information
sqlEngine = create_engine('mysql+pymysql://uname:uname_pwd@host_name') 
dbConnection = sqlEngine.connect()
```
- ### New Database creation - You can mention any database name in place of databaseName.
```python
with sqlEngine.connect() as conn:
    conn.execute("commit")
    conn.execute(f"CREATE DATABASE databaseName")
```
- ### Importing data within above database using a created table
```python
# You can replace the tableName and databaseName of your choice in the below command
first_1k_data.to_sql('tableName',con=sqlEngine, schema = 'databaseName',index=False,if_exists='append')
```
- ### Populating the dataframe by reading the data inserted in previous step
```python
# In place of tableName and databaseName use one's created in above steps
frame = pd.read_sql("select * from databaseName.tableName", dbConnection); 
pd.set_option('display.expand_frame_repr', False)
