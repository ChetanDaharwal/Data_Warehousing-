# Medallion-Data-Warehouse-Architecture Project

Welcome to the **Medallion-Data-Warehouse-Architecture Project** repository! 🚀  
This project demonstrates a comprehensive data warehousing and analytics solution, from building a data warehouse. 
---
## 🏗️ Data Architecture

The data architecture for this project follows Medallion Architecture **Bronze**, **Silver**, and **Gold** layers:
![Data Architecture](data.png)

1. **Bronze Layer**: Stores raw data as-is from the source systems. Data is ingested from CSV Files into Postgresql Server Database.
2. **Silver Layer**: This layer includes data cleansing, standardization, and normalization processes to prepare data for analysis.
3. **Gold Layer**: Houses business-ready data modeled into a star schema required for reporting and analytics.

---
## 📖 Project Overview

This project involves:

1. **Data Architecture**: Designing a Modern Data Warehouse Using Medallion Architecture **Bronze**, **Silver**, and **Gold** layers.
2. **ETL Pipelines**: Extracting, transforming, and loading data from source systems into the warehouse.
3. **Data Modeling**: Developing fact and dimension tables optimized for analytical queries.
4. **Analytics & Reporting**: Creating SQL-based reports and dashboards for actionable insights.

🎯 This project involves topics as:
- SQL Development
- Data Architect
- Data Engineering  
- ETL Pipeline Developer  
- Data Modeling  
   

---
#Some SQL Transformation scripts used for data tranformation,standardization and normalization using advanced function is given below:  
'''
   
      --Calculate the end date of a product record based on the start date of the next record for the same product key (prd_key).
      SELECT CAST(
           LEAD(prd_start_dt) OVER (
               PARTITION BY prd_key 
               ORDER BY prd_start_dt
           ) - 1 AS DATE
       ) AS prd_end_dt
      FROM bronze.crm_prd_info;
   
   
     --Select the most recent record per customer based on the cst_create_date.
     SELECT cst_create_date FROM (
       SELECT *,
              ROW_NUMBER() OVER (
                  PARTITION BY cst_id
                  ORDER BY cst_create_date DESC
              ) AS flag_last
       FROM bronze.crm_cust_info
       WHERE cst_id IS NOT NULL
      ) t
      
      WHERE flag_last = 1;
      
      
      
      SELECT
    -- Normalize Customer ID: remove 'NAS' prefix if present
    CASE 
        WHEN cid LIKE 'NAS%' THEN SUBSTRING(cid, 4, LEN(cid))
        ELSE cid
    END AS cid,

    -- Clean invalid birthdates (future dates)
    CASE 
        WHEN bdate > GETDATE() THEN NULL
        ELSE bdate
    END AS bdate,

    -- Standardize gender values
    CASE 
        WHEN UPPER(TRIM(gen)) IN ('F', 'FEMALE') THEN 'Female'
        WHEN UPPER(TRIM(gen)) IN ('M', 'MALE') THEN 'Male'
        ELSE 'n/a'
    END AS gen

      FROM bronze.erp_cust_az12;




'''
##Data Modeling - In this project we have total 6 table contatining customers and product informations. 
The data architecture for this project follows Star schema:
![Star Schema](datamodel.png)


##🏗️ Data Flow Across Layers – From Raw to Insight
The image showcases the end-to-end data pipeline that takes raw data through a structured process of transformation and modeling. Let's break it down layer by layer:

🧱 1. Raw Layer (Landing Zone)
- This is the initial stage where raw data lands directly from the source systems.

- Data can come from different sources such as databases, APIs, logs, or files (CSV, JSON, etc.).

- No transformations are applied here – it's untouched data, used as a source of truth.


🔄 2. Staging Layer
- Here, SQL queries are used to clean and prepare the raw data.

- Basic transformations occur: fixing nulls, standardizing formats, filtering unnecessary records, etc.

- This stage acts as a preparation zone, making the data consistent and ready for analysis.


📊 3. EDA Layer (Exploratory Data Analysis)
- In this layer, data analysts and engineers use EDA techniques to explore trends, patterns, and anomalies.

- Helps identify relationships, validate assumptions, and check data quality.

- Common tools: SQL, Python (Pandas, Matplotlib), and notebooks.

🧩 4. Modeling Layer (Star Schema)
- Data is now structured into Fact and Dimension Tables, forming a dimensional model:

- Fact Tables contain measurable, quantitative data (e.g., Sales Amount).

- Dimension Tables hold descriptive data (e.g., Product, Customer, Time).

- This star schema design improves query performance and makes it easier for reporting tools (like Power BI, Tableau) to consume.

- The goal is to enable business intelligence and reporting with a user-friendly schema.

✅ Summary of the Flow
Raw – untouched data.

Staging – cleaned and standardized using SQL.

EDA – explore and validate data insights.

Modeling – build fact/dimension tables for analytics.

![Data Flow From Diffrent Layers](dataflow.png)




---

## 🚀 Project Requirements

### Building the Data Warehouse (Data Engineering)

#### Objective
Develop a modern data warehouse using PGAdmin4 Server to consolidate sales data, enabling analytical reporting and informed decision-making.

#### Specifications
- **Data Sources**: Import data from two source systems (ERP and CRM) provided as CSV files.
- **Data Quality**: Cleanse and resolve data quality issues prior to analysis.
- **Integration**: Combine both sources into a single, user-friendly data model designed for analytical queries.
- **Scope**: Focus on the latest dataset only; historization of data is not required.
- **Documentation**: Provide clear documentation of the data model to support both business stakeholders and analytics teams.

---


## 📂 Repository Structure
```
data-warehouse-project/
│
├── datasets/                           # Raw datasets used for the project (ERP and CRM data)
│
│── scripts/                            # SQL scripts for ETL and transformations
│   ├── bronze/                         # Scripts for extracting and loading raw data
│   ├── silver/                         # Scripts for cleaning and transforming data
│   ├── gold/                           # Scripts for creating analytical models
│
├── tests/                              # Test scripts and quality files
│
├── README.md                           # Project overview and instructions
├── LICENSE                             # License information for the repository
                          
```
---

---

## 🛡️ License

This project is licensed under the [MIT License]. You are free to use, modify, and share this project with proper attribution.

## 🌟 About Me

Hi there! I'm Chetan Daharwal 👋

I'm a passionate data enthusiast, continuously sharpening my skills in the world of data engineering. With a strong interest in building robust data pipelines and scalable solutions, I’m always exploring modern tools and technologies that power data-driven decision-making.


🎓 B.Tech in Data Science & AI

⚙️ Skilled in Python, SQL, Azure Data Factory, and Databricks

🧠 Focused on Big Data and Cloud-based Data Engineering

🛠️ Currently exploring modern data architecture (Bronze-Silver-Gold layers)

📈 Interested in building efficient ETL pipelines and real-time analytics

Let’s connect, collaborate, and contribute to the world of data! 🌍✨


