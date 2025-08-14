# On-Prem to Azure Cloud Migration - End-to-End Data Engineering Project



## Premise:
This project aims to create an end-to-end data engineering framework for migrating an on-prem SQL DB to Azure Cloud. The pipeline includes data ingestion using Azure Data Factory, data transformation using Azure Databricks, data storage using Azure Data Lake Gen2, data loading and modeling via Azure Synapse Analytics followed by reporting using PowerBI. The specific details about each stage of the end-to-end pipeline are given below.


## Project Flow:

### Step 1 - Source:
- This project uses AdventureWorks2022 OLTP data as its source. The backup file was downloaded from the link below, and the database was created in the local on-prem SQL Server Management Studio

- Link to AW2022 backup: https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2022.bak	

- Link to all AW backups: https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms

### Step 2 - Data Ingestion using Azure Data Factory:
-	Since this is an on-prem server, the ingestion pipeline required the creation of a self-hosted integration runtime (SHIR) to run it. 
-	The pipeline was created using Lookup, ForEach and Copy Data activities from ADF to move multiple Sales tables from the AdventureWorks2022 database to Azure Data Lake in parquet format.

### Step 3 - Data Storage using Azure Data Lake Gen2:
-	The project tried to emulate the lakehouse architecture for data storage. The data lake was divided into multiple layers – Bronze, Silver and Gold to contain different degrees of transformation. 
-	Bronze contains a true copy of the source; Silver contains a slightly transformed version of the data followed by Gold which has the cleanest version of the data. 

### Step 4 – Data Transformation using Azure Databricks:
-	This layer is used for transforming the raw data to the cleanest, best version of it in the Gold layer. The dev work for transforming the data is done in PySpark.

### Step 5 – Data Modeling using Azure Synapse Analytics:
-	Data from the Gold layer will be pushed to a data warehousing type model for the data to be consumed by the reporting layer. 
-	A SQL database is created in Synapse to host the different tables from the Gold layer into views for reporting layer consumption. 
-	Another pipeline was also created to create multiple views for each of the tables present in the Gold layer. This pipeline was made using Get Metadata, ForEach and Stored Procedure activities to run the create view SP against each table in the serverless Azure SQL DB.

### Step 6 – Data Visualization using PowerBI:
An interactive dashboard was created from the views present in Synapse by importing them to PowerBI. The dashboard explores factors like countries with maximum sales, which factor led to maximum sales, which discount/special offer brought in the highest number of sales among other analysis.


