# Netflix-Data-Medallion-Architecture-on-Azure

## **📌Project Overview:**


In this project, I built an end-to-end Medallion Architecture pipeline (Bronze → Silver → Gold) on the Azure Data Platform using Azure Data Factory (ADF), Azure Databricks, Delta Lake, and Power BI.
I used Netflix datasets from GitHub to demonstrate how to ingest, transform, and serve high-quality data for analytics.

## **🎯Objective:**

My goal was to:

1)Ingest raw Netflix data from GitHub into Azure Data Lake Storage (ADLS).<br>
2)Use Databricks Auto Loader to stream master data into the Bronze layer.<br>
3)Transform raw data in Databricks to create clean Silver layer tables.<br>
4)Build Delta Live Tables (DLT) for the Gold layer with built-in data quality checks.<br>
5)Connect the Gold layer to Power BI for reporting and dashboards.<br>

## **⚙️Services & Tools I Used**

✅ Azure Resource Group<br>
✅ Azure Data Factory (ADF)<br>
✅ Azure Data Lake Storage Gen2 (ADLS) with containers: raw, bronze, silver, metastore<br>
✅ Azure Databricks with Unity Catalog<br>
✅ Databricks Jobs and Notebooks<br>
✅ Delta Live Tables (DLT)<br>
✅ Power BI (via Partner Connect)<br>

## **🏗️My End-to-End Pipeline**

1️⃣) Setup & Resources<br>
I started by creating a Resource Group in Azure to keep all related resources organized. Then, I created:
Azure Data Factory (ADF) for ingestion.ADLS with multiple containers: raw, bronze, silver, metastore.
Azure Databricks Workspace.An Access Connector to securely connect Databricks and ADLS.In IAM, I added the Storage Blob Contributor role for Databricks to access storage.

2️⃣) Ingesting Data with Azure Data Factory (ADF)<br>
I used ADF to pull Netflix data from GitHub and load it into the Bronze container:<br>
Created Linked Services for GitHub and ADLS.<br>
Built a parameterized pipeline so I could dynamically ingest multiple files with a single pipeline run.<br>
I defined:<br>
file_name and folder_name parameters for the source and sink.<br>
Used dynamic expressions to pass file names and paths.<br>
Added a ForEach activity to loop through an array (type_array) instead of maintaining separate JSON files.<br>
Added a Validation Activity to check if the netflix_titles.csv file was present in the raw before loading.<br>
Included a Web Activity to get metadata from the GitHub repository.<br>
By running this single pipeline, I successfully ingested all four files into my Bronze container dynamically.

3️⃣) Loading Master Data with Databricks Auto Loader<br>
For my master data (netflix_titles), I used Databricks Auto Loader.<br>
Set up Unity Catalog in Databricks for centralized governance.<br>
Created a new Unity Catalog and linked it to my metastore container.<br>
Created a Credential using the Access Connector ID.<br>
Defined External Locations for all files.<br>
I wrote an Auto Loader notebook to stream CSV files:<br>

4️⃣) Bronze ➜ Silver Transformations<br>
I performed transformations on my Bronze data and wrote it to Silver using Databricks Notebooks and Jobs.<br>
✅ Notebook 2_silver:<br>
Used dbutils.widgets to pass sourcefolder and targetfolder parameters dynamically.<br>
Read the Bronze data and wrote it to the Silver container in Delta format.<br>
✅ Notebook 3_LookupNoteBook:<br>
Stored an array of files and returned it to the job dynamically.<br>
✅ Databricks Job:<br>
Added two tasks:<br>
Lookup_Location: Runs 3_LookupNoteBook to get the array.<br>
SilverNotebook: Runs 2_silver in a loop for each file using the array.<br>
✅ Notebook 4_silver:<br>
For the master table (netflix_titles), I did additional PySpark transformations.<br>

5️⃣ Silver ➜ Gold with Delta Live Tables (DLT)<br>
I used Delta Live Tables for my Gold layer and added data quality expectations.<br>
For netflix_titles:<br>
Created a staging table, added a new flag, and a final table with more expectations.<br>
I created a DLT Pipeline, added my notebook, validated, and ran it successfully to generate the Gold tables.<br>

6️⃣ Power BI Connection<br>
Finally, I connected my Gold layer to Power BI using Databricks Partner Connect, enabling me to build dashboards and visualizations on top of my trusted Gold layer data.<br>

## **✅ What I Learned**<br>
Building parameterized pipelines in ADF saves time and makes ingestion dynamic.<br>
Databricks Unity Catalog simplifies data governance and access management.<br>
Databricks Auto Loader is super efficient for streaming and handling new files automatically.<br>
Delta Live Tables help enforce data quality and make Gold layer pipelines robust and maintainable.<br>

## **✨ Conclusion**<br>
This project shows how to build a modern Lakehouse architecture on Azure using best practices.<br>



