# Netflix-Data-Medallion-Architecture-on-Azure

#Project Overview

In this project, I built an end-to-end Medallion Architecture pipeline (Bronze → Silver → Gold) on the Azure Data Platform using Azure Data Factory (ADF), Azure Databricks, Delta Lake, and Power BI.
I used Netflix datasets from GitHub to demonstrate how to ingest, transform, and serve high-quality data for analytics.

#Objective

My goal was to:
1)Ingest raw Netflix data from GitHub into Azure Data Lake Storage (ADLS).
2)Use Databricks Auto Loader to stream master data into the Bronze layer.
3)Transform raw data in Databricks to create clean Silver layer tables.
4)Build Delta Live Tables (DLT) for the Gold layer with built-in data quality checks.
5)Connect the Gold layer to Power BI for reporting and dashboards.
