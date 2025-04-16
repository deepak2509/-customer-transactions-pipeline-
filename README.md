# -customer-transactions-pipeline-

# 📊 Customer Transactions Data Pipeline (AWS Glue ETL)

This project demonstrates a scalable data pipeline for ingesting, transforming, and analyzing customer transaction data using **AWS Glue**, **Amazon S3**, and **Amazon Athena**. The pipeline automates the ETL process and enables efficient querying for downstream analytics.

---

## 🚀 Project Summary / Final Goal

The objective of this project is to build a **serverless ETL (Extract, Transform, Load) pipeline** using AWS services that processes raw customer transaction data (from `.xlsx`) and stores it in a queryable, analytics-friendly format (CSV or Parquet). The final outcome enables **real-time or batch querying via Amazon Athena**, supporting business intelligence use cases such as sales analysis and customer behavior tracking.

---

## 🧱 Tech Stack

- **Amazon S3** – Raw and processed data storage
- **AWS Glue Crawler** – Metadata extraction & schema creation
- **AWS Glue Data Catalog** – Central metadata repository
- **AWS Glue Job** – Python-based ETL job (PySpark)
- **Amazon Athena** – Serverless querying and analysis
- **Apache Airflow / AWS Glue Workflows** – Automation (optional)

---

## 🔁 ETL Workflow

### 1. **Raw Data Ingestion**
- Upload `.xlsx` file containing customer transactions into:

### 2. **Metadata Extraction**
- AWS Glue Crawler scans raw data and creates a table (`raw`) in the **Glue Data Catalog**

### 3. **Schema & Table Creation**
- The crawler stores metadata including schema, partitions, and file formats for the dataset

### 4. **Data Transformation (ETL Job)**
- AWS Glue Job performs:
- Data cleaning (e.g., handling nulls, dropping invalid rows)
- Derived column creation (`totalprice = quantity * unitprice`)
- Conversion to **CSV** or **Parquet** based on config

### 5. **Processed Data Storage**
- Cleaned data is stored in:
s3://customer.transactions/processed/

### 6. **Querying and Analysis**
- Amazon Athena queries processed data using SQL
- Example insights:
- Top countries by revenue
- Products with the highest sales
- Frequent customers

### 7. **Automation (Optional)**
- Set up **Glue Workflows** or **Apache Airflow** to automate:
- Crawlers
- ETL job execution
- Notifications

---

## 🧮 Sample Athena SQL Queries

```sql
-- View total sales by country
SELECT country, SUM(totalprice) AS total_sales
FROM customer_transactions_cleaned
GROUP BY country
ORDER BY total_sales DESC
LIMIT 10;

-- Top 5 products sold
SELECT description, SUM(quantity) AS total_quantity
FROM customer_transactions_cleaned
GROUP BY description
ORDER BY total_quantity DESC
LIMIT 5;

