# second-app
visualization of phonepe pulse data
# This is my Phonepe pulse data  visualization project

# Importing required libraries
```python
import streamlit as st
from PIL import Image
import os
import json
from streamlit_option_menu import option_menu
import pandas as pd
import sqlite3
import plotly.express as px
```
# if the module shows any error or module not found it can be overcome by using below command

```python
pip install<module name>
```
# Inorder to clone the github data into to working environment use below command
```python
import requests
response = requests.get(url)
repo = response.json()
clone_url = repo['clone_url']
repo_name = "pulse"
clone_dir = os.path.join(os.getcwd(), repo_name)
```

# create the app using streamlit[Reference](https://docs.streamlit.io/library/api-reference)

# creating page configuration
```python
st.set_page_config(page_title=name,page_icon=image,layout='wide')
st.title(' PhonePe Pulse Data Visualization ')
```
# after cloning the data from github create the dataframes using pandas Data frame
```python
clm={'State':[], 'Year':[],'Quater':[],'Transaction_type':[], 'Transaction_count':[], 'Transaction_amount':[]}
for i in Agg_state_list:
    p_i=path+i+"/"
    Agg_yr=os.listdir(p_i)
    for j in Agg_yr:
        p_j=p_i+j+"/"
        Agg_yr_list=os.listdir(p_j)
        for k in Agg_yr_list:
            p_k=p_j+k
            Data=open(p_k,'r')
            D=json.load(Data)
            for z in D['data']['transactionData']:
              Name=z['name']
              count=z['paymentInstruments'][0]['count']
              amount=z['paymentInstruments'][0]['amount']
              clm['Transaction_type'].append(Name)
              clm['Transaction_count'].append(count)
              clm['Transaction_amount'].append(amount)
              clm['State'].append(i)
              clm['Year'].append(j)
              clm['Quater'].append(int(k.strip('.json')))

# Successfully created a dataframe
df_aggregated_transaction=pd.DataFrame(clm)
```
# checking for the missing values
```python
 df_aggregated_transaction.info()
```
# To Establish the connection with sql server
```python
connection = sqlite3.connect("phonepe pulse.db")
cursor = connection.cursor()
```
# After creating the dataframe store it in sql server
```python
# Inserting each Data frame into sql server
df_aggregated_transaction.to_sql('aggregated_transaction', connection, if_exists='replace')
```
# Create sql queries to fetch the data as per the user requirement
```python
SELECT * FROM "Table"
WHERE "Condition"
GROUP BY "Columns"
ORDER BY "Data"
```
# visualizing the data with plotly and streamlit
```python
 fig=px.bar(df,x=column name,y=column name)
            tab1, tab2 = st.tabs(["Streamlit theme (default)", "Plotly native theme"])
            with tab1:
                st.plotly_chart(fig, theme="streamlit", use_container_width=True)
            with tab2:
                st.plotly_chart(fig, theme=None, use_container_width=True)
```

