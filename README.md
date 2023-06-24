# Write_to_SQL_Server_by_Pyhton
## This code takes your local folder, takes all csv files inside and writes them in bulk to your SQL Server database.
import os
import pandas as pd
from sqlalchemy import create_engine
import urllib

 

# pyodbc connection string
conn_str = (
   'DRIVER={ODBC Driver 17 for SQL Server};'
   'SERVER=xxxx.rds.amazonaws.com,1433;'
   'DATABASE=xxxx;'
   'UID=xxxx;'
   'PWD=xxxx;'
)


# SQLAlchemy connection string
sqlalchemy_conn_str = "mssql+pyodbc:///?odbc_connect={}".format(
    urllib.parse.quote_plus(conn_str)
)

 

# Create SQLAlchemy engine
engine = create_engine(sqlalchemy_conn_str)

 

# Directory containing the CSV files
dir_path = r'C:\Users\xxxxx.csv'

 

# Read each CSV file and write it to SQL Server
for filename in os.listdir(dir_path):
    if filename.endswith(".csv"):
        df = pd.read_csv(os.path.join(dir_path, filename), encoding='ISO-8859-1')  # Use 'ISO-8859-1' encoding
        table_name = os.path.splitext(filename)[0]  # Use filename (without extension) as table name
        df.to_sql(table_name, engine, if_exists='replace', index=False)

