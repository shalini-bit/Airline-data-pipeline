# **Airline Data ETL Pipeline** 
Built an end-to-end data pipeline handling ingestion, transformation, and analytics using Azure cloud services.

## **Project Overview**

This project demonstrates an **end-to-end ETL pipeline** for airline flight data using **Azure Data Engineering tools**.
It ingests raw flight data, performs transformations using **PySpark in Databricks**, and loads the clean data into an **Azure SQL Database** for analysis.

---

## **Technologies Used**

* **Azure Data Lake Storage (ADLS Gen2):** Storage for raw and processed data
* **Azure Databricks:** PySpark notebooks for data transformation
* **Azure Data Factory (ADF):** Pipeline orchestration (ETL)
* **Azure SQL Database:** Storing processed data for queries and analysis
* **Python / PySpark & SQL:** Data processing and analytics 

---
![Capture](https://github.com/user-attachments/assets/4303a2fe-353d-4812-a38f-be1f9fe6f9fa)  

### File structure : 
```
Airline-ETL-Pipeline/
│
├── notebooks/
│   └── airline_etl_databricks.ipynb   # Databricks notebook for data transformation
│
├── raw-data/                           # Folder containing original flight CSV files
│
├── final-data/                         # Folder containing transformed CSV files
│
├── README.md                            # Project documentation
│
└── SQL_queries.md                       # SQL queries run on the processed data
```

## **Architecture Diagram** - 
```
Raw Data (ADLS)
      ↓
Azure Data Factory (Pipeline)
      ↓
Azure Databricks (Transformation)
      ↓
Final Data (ADLS)
      ↓
Azure SQL Database
## **Project Structure**
```
---

## 🔁 Data Flow
1. Raw data is stored in ADLS (`raw-data`)
2. Databricks reads and transforms data
3. Processed data is stored in (`final-data`)
4. Data is loaded into Azure SQL
5. SQL queries generate insights

## **Project Steps**

### 1. **Data Ingestion**

* Raw airline flight data (CSV) uploaded to **ADLS `raw-data` folder**.

### 2. **Data Transformation (Databricks)**

* Launch a Databricks notebook attached to a cluster.
* Read raw data from ADLS using PySpark:

```python
df = spark.read.csv("abfss://<container>@<storage_account>.dfs.core.windows.net/raw-data/", header=True, inferSchema=True)
```

* Transformations performed:

  * Standardized column names
  * Converted data types
  * Cleaned inconsistent values
* Write transformed data back to ADLS `final-data` folder:

```python
df_transformed.write.csv("abfss://<container>@<storage_account>.dfs.core.windows.net/final-data/", header=True, mode="overwrite")
```

### 3. **Pipeline Orchestration (ADF)**

* Created a **Copy Data activity** pipeline:

  * **Source:** `raw-data` folder in ADLS
  * **Sink:** `final-data` folder in ADLS
* Debugged pipeline successfully.


### 4. **Data Loading into SQL**

* Created an **Azure SQL Database (`AirlinesDB`)**.
* Loaded processed data from ADLS `final-data` folder into SQL using JDBC in Databricks:

```python
df_transformed.write \
    .format("jdbc") \
    .option("url", "jdbc:sqlserver://<server_name>.database.windows.net:1433;databaseName=AirlinesDB") \
    .option("dbtable", "Flights") \
    .option("user", "<admin_user>") \
    .option("password", "<password>") \
    .mode("overwrite") \
    .save()
```

### 5. **Data Analysis**

* Sample SQL queries:

  * Cheapest airline (Which airline is most budget friendly)
  * Most expensive route (costliest route in Dataset)
  * Average price by class (price difference between classess)
  * Top 5 busiest routes . ( most frequently travelled routes)
  * Direct vs connecting flights count. ( distribution of direct vs stop flights).
  * Price trend based on days left (how ticket price changes as travel date approaches) 
  * Longest duration flight
  * Cheapest flghts for each route.
  * Airline with most flights
---

## **Result / Outcome**

* Successfully built a **complete ETL pipeline**: Raw → Transform → Load → Query
* Data is ready for **analytics and visualization**.
* Demonstrates skills in **cloud data engineering, PySpark, SQL, and pipeline automation**.

---

## **How to Run**

1. Upload your raw CSVs to `raw-data` folder in ADLS.
2. Open the Databricks notebook, attach a cluster, and run cells to transform and write data.
3. Optionally, trigger the ADF pipeline for automation.
4. Connect to Azure SQL Database to query and analyze the processed data.

---

## 📊 Key Insights

- Flights become more expensive as the departure date approaches.
- Economy class flights dominate the dataset.
- Certain routes (e.g., Delhi → Mumbai) have the highest frequency.
- Airlines differ significantly in pricing strategies.


---


