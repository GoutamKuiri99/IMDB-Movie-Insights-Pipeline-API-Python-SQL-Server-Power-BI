# 🎬 IMDB Movie Insights Pipeline  

<img width="1083" height="452" alt="image" src="https://github.com/user-attachments/assets/c81e37af-eabf-46db-8996-b1bf8b58aa75" />


---

## 📌 Overview  

An end-to-end data pipeline that extracts IMDB movie data from an API, transforms it using Python (Pandas), stores it in SQL Server, and visualizes insights through Power BI dashboards.  
This project demonstrates a real-world ETL workflow and data analysis capabilities.

---

## 🧠 Project Workflow  

- Extract data from RapidAPI (IMDB Movies API)  
- Transform and clean data using Python (Pandas)  
- Load processed data into SQL Server  
- Connect SQL Server to Power BI  
- Build interactive dashboards for insights  

---

## ⚙️ Tech Stack  

- Python (Requests, Pandas)  
- SQL Server  
- SQLAlchemy & PyODBC  
- Power BI  
- RapidAPI  

---

## 🔄 Data Pipeline Flow  
#### **RapidAPI → Python → Data Cleaning → SQL Server → Power BI**


### 🔹 Step 1: Data Extraction from RapidAPI  

In this step, movie data is fetched from RapidAPI using the IMDB Movies API.  
The API provides data in JSON format which will be used for further processing.

- Visit the RapidAPI website  
- Search for **IMDB Movies API**  
- Subscribe to the API  
- Copy your API Key  

📸 **RapidAPI Setup Screenshot:**  

<img width="1891" height="671" alt="image" src="https://github.com/user-attachments/assets/942d6510-12da-4702-a80e-74e47cb084b0" />


---

### 🔧 Python Code  

```python
import requests

url = "https://imdb236.p.rapidapi.com/api/imdb/tt0816692/metascore"

headers = {
	"x-rapidapi-key": "ecbe95a61amsh7c96a24406a46d9p18bc0ajsn72031abca57b",
	"x-rapidapi-host": "imdb236.p.rapidapi.com",
	"Content-Type": "application/json"
}

response = requests.get(url, headers=headers)

print(response.json())
```
### 🔍 How to Test API in Browser (JSON Format)

You can test the API response directly in your browser and view it in a structured JSON format.

#### 📌 Steps:

1. Install a Chrome extension like **JSON Formatter** or **JSON Viewer**  
2. Open Chrome and paste the API URL  

👉 **Example API Link:**  
https://api.themoviedb.org/3/movie/top_rated?api_key=YOUR_API_KEY  

3. The response will appear in raw JSON format  
4. The JSON extension will automatically beautify and structure the data  

---

#### 📸 Screenshot:

<img width="1905" height="981" alt="image" src="https://github.com/user-attachments/assets/e3ad86f9-d3ec-41d6-a5e6-06dde5929efe" />


<img width="383" height="638" alt="image" src="https://github.com/user-attachments/assets/48d88dc4-3f99-4809-b53c-8c8014f731a2" />

---

#### 💡 Why This is Useful:

- Helps understand API response structure  
- Allows you to explore available fields and keys  
- Makes debugging easier  
- Useful for planning data cleaning and transformation steps  

---
## 📦 Step 1: Import Required Libraries  

In this step, we import all the necessary Python libraries required for API extraction, data processing, and database connection.

- **requests** → To fetch data from API  
- **pandas** → To handle and process data  
- **urllib** → To create SQL connection string  
- **sqlalchemy** → To connect Python with SQL Server  

---

### 🔧 Code  

```python
import requests
import pandas as pd
import urllib
from sqlalchemy import create_engine
```
## 🌐 Step 2: Fetch Data from API  

In this step, we fetch movie data from RapidAPI using an HTTP request.  
The API returns data in JSON format.

---

### 🔧 Code  

```python
# API call
url = "https://imdb-top-100-movies.p.rapidapi.com/"

headers = {
    "x-rapidapi-key": "YOUR_API_KEY",
    "x-rapidapi-host": "imdb-top-100-movies.p.rapidapi.com",
    "Content-Type": "application/json"
}

response = requests.get(url, headers=headers)
data = response.json()

# Preview data
data
```
<img width="1316" height="852" alt="image" src="https://github.com/user-attachments/assets/ee4925e4-b987-46cf-a778-911cf58cebbb" />

### 🔹 Step 3: Convert JSON to DataFrame  

The API response is converted into a Pandas DataFrame for easier data manipulation and analysis.

---

### 🔧 Code  

```python
import pandas as pd

df = pd.DataFrame(data)

# Preview DataFrame
df.head()
```
<img width="1312" height="713" alt="image" src="https://github.com/user-attachments/assets/6c14b0d9-79bd-45a4-a266-f1e43f2f272c" />

### 🔹 Step 4: Data Cleaning & Transformation  

In this step, the raw data is cleaned and transformed to ensure consistency and usability for further analysis.

---

### 🔧 Cleaning Operations Performed  

- Converted **genre list** into a readable string  
- Converted **rating and year** into numeric values  
- Handled missing values  

---

### 🔧 Code  

```python
df['genre'] = df['genre'].apply(lambda x: ', '.join(x))
df['rating'] = pd.to_numeric(df['rating'], errors='coerce')
df['year'] = pd.to_numeric(df['year'], errors='coerce')

df = df.fillna(0)
```

### 🔹 Step 5: Create SQL Server Database  

Before loading the processed data, a database needs to be created in SQL Server to store the data.

---

### 🗄️ SQL Command  

```sql
CREATE DATABASE MovieDB;
```
### 🔹 Step 6: Connect Python to SQL Server  

In this step, we establish a connection between Python and SQL Server and define SQL data types for proper data storage.

---

### 🔧 Code  

```python
# SQL connection
params = urllib.parse.quote_plus(
        "DRIVER={ODBC Driver 17 for SQL Server};"
    "SERVER=JAISHREERAM\\SQLEXPRESS01;"
    "DATABASE=MovieDB;"
    "Trusted_Connection=yes;"
)

engine = create_engine(f"mssql+pyodbc:///?odbc_connect={params}")

# SQL datatypes

dtype = {
    'rank': types.Integer(),
    'title': types.VARCHAR(255),
    'description': types.TEXT(),
    'genre': types.VARCHAR(255),
    'rating': types.Float(),
    'year': types.Integer(),
    'imdbid': types.VARCHAR(50),
    'imdb_link': types.VARCHAR(255),
    'image': types.TEXT(),
    'thumbnail': types.TEXT()
}
```

### 🔹 Step 7: Load Data into SQL Server  

In this step, the cleaned DataFrame is uploaded to SQL Server as a table.

---

### 🔧 Code  

```python
# Upload
df.to_sql('imdb_movies', engine, if_exists='replace', index=False, dtype=dtype)

print("Data successfully pushed to SQL Server")
```

<img width="1317" height="677" alt="image" src="https://github.com/user-attachments/assets/d768a8dc-4b51-46bb-a747-327dc98f9b54" />

### 🔹 Step 8: Connect SQL Server to Power BI  

In this step, the SQL Server database is connected to Power BI to load the processed data for visualization.

---

### 📌 Steps  

1. Open **Power BI Desktop**  
2. Click on **Get Data → SQL Server**  
3. Enter the following details:  
   - **Server name:** `JAISHREERAM\SQLEXPRESS01`  
   - **Database:** `MovieDB`  
4. Select the table: `imdb_movies`  
5. Click **Load**  

---

📸 **Screenshot:**  
<img width="1896" height="710" alt="image" src="https://github.com/user-attachments/assets/cf25ac5d-d26c-4c70-a00b-af5e117e27d1" />


---

### 💡 Explanation  

- Power BI connects directly to SQL Server  
- Data is imported into Power BI for analysis  
- Enables creation of visual dashboards  

---

### 🔹 Step 9: Build Dashboard  

After loading the data, interactive dashboards are created in Power BI to generate insights.

---

### 📊 Dashboard Visuals  

- Top Rated Movies  
- Genre Distribution  
- Year-wise Trends  
- Rating Analysis  

### 💡 Why This Step is Important  

- Converts raw data into meaningful insights  
- Helps in better decision-making  
- Makes data visually understandable  

---
## 🧾 Complete Code (End-to-End Pipeline)

Below is the complete code for the entire data pipeline:

---

```python
import requests
import pandas as pd
import urllib
from sqlalchemy import create_engine, types

# API call
url = "https://imdb-top-100-movies.p.rapidapi.com/"

headers = {
    "x-rapidapi-key": "YOUR_API_KEY",
    "x-rapidapi-host": "imdb-top-100-movies.p.rapidapi.com",
    "Content-Type": "application/json"
}

response = requests.get(url, headers=headers)
data = response.json()

# DataFrame
df = pd.DataFrame(data)

# Cleaning
df['genre'] = df['genre'].apply(lambda x: ', '.join(x))
df['rating'] = pd.to_numeric(df['rating'], errors='coerce')
df['year'] = pd.to_numeric(df['year'], errors='coerce')

df = df.fillna(0)

# SQL connection
params = urllib.parse.quote_plus(
    "DRIVER={ODBC Driver 17 for SQL Server};"
    "SERVER=JAISHREERAM\\SQLEXPRESS01;"
    "DATABASE=MovieDB;"
    "Trusted_Connection=yes;"
)

engine = create_engine(f"mssql+pyodbc:///?odbc_connect={params}")

# SQL datatypes
dtype = {
    'rank': types.Integer(),
    'title': types.VARCHAR(255),
    'description': types.TEXT(),
    'genre': types.VARCHAR(255),
    'rating': types.Float(),
    'year': types.Integer(),
    'imdbid': types.VARCHAR(50),
    'imdb_link': types.VARCHAR(255),
    'image': types.TEXT(),
    'thumbnail': types.TEXT()
}

# Upload
df.to_sql('imdb_movies', engine, if_exists='replace', index=False, dtype=dtype)

print("Data successfully pushed to SQL Server")
```
## 📌 Conclusion  

This project highlights the importance of building an end-to-end data pipeline in real-world scenarios.

In many practical cases, data is not directly available from a database. Instead, organizations provide data through APIs.  
For example, platforms like ERP systems (e.g., Odoo), ticketing systems (e.g., IRCTC), and movie databases (e.g., TMDB) often restrict direct database access and expose data via APIs.

In such situations, it becomes essential to:

- Extract data from APIs  
- Process and clean the data using Python  
- Store the data in a structured database (SQL Server)  
- Connect the database to visualization tools like Power BI  

This project demonstrates that **data collection is a critical first step** in any data analysis workflow.  
Without data, meaningful insights, reporting, and decision-making are not possible.

---

### 💡 Key Takeaway  

> Data is the foundation of analytics.  
> If data is not available in a structured format, it must be collected, transformed, and stored before analysis.

---

### 🚀 Real-World Impact  

- Builds understanding of API-based data extraction  
- Strengthens ETL (Extract, Transform, Load) concepts  
- Prepares for real-world data engineering and analytics tasks  
- Bridges the gap between raw data and business insights  

---

## 👨‍💻 About the Author
- **Name**: Goutam Kuiri
- **Contact**: gkuiri26@gmail.com
- **LinkedIn**: [Goutam Kuiri](https://www.linkedin.com/in/goutam-kuiri-949b632a6)
- **Medium**: [Goutam Kuiri](https://medium.com/@goutamkuiri)
