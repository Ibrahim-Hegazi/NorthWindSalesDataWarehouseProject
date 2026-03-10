A comprehensive ETL (Extract, Transform, Load) pipeline that transforms the classic Northwind Sales dataset from flat CSV files into a structured Data Warehouse (Star Schema) using PostgreSQL and Apache Airflow.

Side Note: This project was built using the Northwind dataset from Kaggle as a sample sales database.

---

## 📌 Table of Contents

- [🔎 Project Overview](#-project-overview)
- [🚧 Problem Statement](#-problem-statement)
- [🚀 Project Objectives](#-project-objectives)
- [❗ Source and Destination](#-source-and-destination)
- [💡 Dataset Description](#-dataset-description)
- [📈 Proposed Solution](#-proposed-solution)
- [🔧 System Architecture](#-system-architecture)
- [🧪 Tools and Technologies](#-tools-and-technologies)
- [🧬 Database Design (Star Schema)](#-database-design-star-schema)
- [📦 Airflow Workflow Design](#-airflow-workflow-design)
- [🧠 Expected Outcomes](#-expected-outcomes)
- [📝 Project Deliverables](#-project-deliverables)
- [💡 Benefits of the Project](#-benefits-of-the-project)
- [📬 Future Enhancements](#-future-enhancements)
- [👨‍💻 Team Members](#-team-members)

---

## 🔎 Project Overview

The purpose of this project is to design and implement a complete ETL (Extract, Transform, Load) pipeline for the classic Northwind Sales dataset. The pipeline will extract data from multiple source flat files (CSV), perform data cleaning, integration, and transformation operations, and then load the processed data into a PostgreSQL database designed as a **Data Warehouse (Star Schema)** .

To ensure automation, scheduling, and monitoring, the entire ETL workflow will be orchestrated using Apache Airflow. This project demonstrates core Data Engineering concepts including data ingestion, transformation, loading, data warehouse modeling, and workflow orchestration.

## 🚧 Problem Statement

Businesses often store operational data in disparate, raw flat-file formats like CSVs, which are not optimized for analytical queries. The Northwind dataset, while structured, is in a **normalized transactional format (OLTP)** spread across multiple files (Orders, Products, Customers, etc.). This structure requires complex joins and is inefficient for high-level business analysis like sales trends, customer behavior, and product performance.

Without a proper ETL process to transform this transactional data into an analytical format, generating business insights is slow and resource-intensive. This project solves that problem by building an automated pipeline that transforms raw, normalized Northwind data into a structured **Star Schema** inside a PostgreSQL data warehouse, making it ready for fast and intuitive reporting.

## 🚀 Project Objectives

The main objectives of this project are:
- **Extract** data from multiple flat files (CSVs) representing different Northwind tables.
- **Clean and preprocess** the raw data (handle missing values, correct data types).
- **Transform** the normalized source data into a Fact and Dimension (Star Schema) model.
- **Load** the transformed data into fact and dimension tables in a PostgreSQL database.
- **Automate** the entire ETL workflow using Apache Airflow.
- **Enable** reliable and repeatable data processing for analytical purposes.
- **Prepare** the dataset for future integration with BI tools like Tableau or Power BI.

## ❗ Source and Destination

**Source:**
- Multiple Flat files (CSV format) containing the Northwind database tables (e.g., orders, order_details, products, customers, employees, categories, suppliers).

**Destination:**
- PostgreSQL database, modeled as a **Data Warehouse** (Star Schema).

**Automation / Orchestration Tool:**
- Apache Airflow.

## 💡 Dataset Description

The Northwind dataset is a classic sample database representing a fictional specialty foods import/export company. The source data is normalized and includes tables such as:
- **Customers:** Customer information (ID, company, contact, location).
- **Employees:** Employee details (ID, name, title, hire date).
- **Products:** Product information (ID, name, supplier, category, price).
- **Categories:** Product categories (ID, name, description).
- **Suppliers:** Supplier information (ID, name, contact, country).
- **Orders:** Sales order headers (order ID, customer ID, employee ID, order date).
- **Order Details:** Line items for each order (order ID, product ID, quantity, price, discount).

This data will be extracted from raw CSV files and transformed into a structured analytical format.

## 📈 Proposed Solution

The proposed solution is to build an end-to-end ETL pipeline with the following stages:

### Extract
- Read all Northwind CSV files from a source directory.
- Validate file existence and structure.
- Load raw data from each file into corresponding staging tables in PostgreSQL.

### Transform
- Clean data by handling missing values and standardizing formats (e.g., dates).
- Implement business logic to calculate measures like `line_item_total` and `discount_amount`.
- Perform the **star schema transformation**: De-normalize and separate data into Dimensions (e.g., `dim_customer`, `dim_product`, `dim_date`) and a central Fact table (`fact_sales`).
- Resolve surrogate keys for dimension tables.

### Load
- Load transformed data into the final fact and dimension tables in the PostgreSQL data warehouse.
- Implement slowly changing dimensions (SCD) if necessary (e.g., Type 1 for customer updates).
- Ensure data integrity and consistency with appropriate constraints.

### Automation
- Use Apache Airflow DAGs to:
    - Schedule the ETL job (e.g., daily).
    - Manage task dependencies (e.g., load dimensions before the fact table).
    - Monitor execution and log pipeline activity.
    - Handle retries and send failure alerts.

## 🔧 System Architecture

![deepseek_mermaid_20260310_6b9c42](https://github.com/user-attachments/assets/a467d1ad-3866-43a1-87de-c50e3ab1c048)# Northwind Sales ETL Data Engineering Pipeline
    
    H[Apache Airflow<br/>Orchestration] -.-> B
    H -.-> D
    H -.-> F
    
    style C fill:#cd7f32,color:white
    style E fill:#c0c0c0,color:black
    style G fill:#ffd700,color:black



```
Multiple Flat Files (CSVs)  
    ↓  
Extraction Layer (Python)  
    ↓  
Staging Tables (PostgreSQL)  
    ↓  
Transformation Layer (SQL/Python)  
    ↓  
Data Warehouse (PostgreSQL - Star Schema)  
    ↓  
Airflow for Orchestration & Monitoring
```

## 🧪 Tools and Technologies

The following tools and technologies will be used:
- **Python** for ETL scripting and orchestration logic.
- **Pandas / SQLAlchemy** for data manipulation and database interaction.
- **PostgreSQL** as the target data warehouse.
- **Apache Airflow** for workflow orchestration.
- **SQL** for database schema creation and complex transformations.
- **Docker** (optional) for containerizing Airflow and PostgreSQL for easy setup.

## 🧬 Database Design (Star Schema)

The PostgreSQL data warehouse will be modeled as a Star Schema for analytical queries:

### Fact Table: `fact_sales`
| Column | Type | Description |
|--------|------|-------------|
| `sales_key` | Primary Key | Surrogate key for fact table |
| `order_id` | Source ID | Original order identifier |
| `customer_key` | FK to `dim_customer` | Customer dimension reference |
| `product_key` | FK to `dim_product` | Product dimension reference |
| `employee_key` | FK to `dim_employee` | Employee dimension reference |
| `date_key` | FK to `dim_date` | Date dimension reference |
| `quantity` | Integer | Number of units sold |
| `unit_price` | Decimal | Price per unit |
| `discount` | Decimal | Discount applied |
| `total_price` | Decimal | Calculated total after discount |

### Dimension Tables:
- `dim_customer`
- `dim_product` (includes category and supplier details)
- `dim_employee`
- `dim_date` (with attributes like year, quarter, month, day)

## 📦 Airflow Workflow Design

The Airflow DAG will include the following tasks:

1. **Start Task** 🚀
2. **Create Staging Tables** (if not exist) 🏗️
3. **Extract & Load Data into Staging** (from CSVs to PostgreSQL staging) 📤
4. **Load Dimension Tables** (`dim_date`, `dim_customer`, `dim_product`, `dim_employee`) 📊
5. **Load Fact Table** (`fact_sales`) 📈
6. **Run Data Quality Checks** (e.g., row count validation, no NULLs in foreign keys) ✅
7. **Cleanup / Archive Staging** (optional) 🧹
8. **End Task** 🏁

## 🧠 Expected Outcomes

At the end of this project, the following outcomes are expected:
- A fully functional, automated ETL pipeline.
- A PostgreSQL data warehouse with a clean star schema optimized for sales analysis.
- Automated, scheduled execution of the entire ETL process through Airflow.
- A reusable, production-like data engineering workflow.

## 📝 Project Deliverables

The final deliverables of this project will include:
- Python ETL scripts.
- SQL scripts for PostgreSQL schema creation (staging and star schema).
- An Apache Airflow DAG definition file.
- The sample Northwind source dataset (CSV files).
- Comprehensive documentation for setup, configuration, and execution.
- Final project report / presentation.

## 💡 Benefits of the Project

This project provides practical value in several areas:
- Demonstrates a **real-world ETL pipeline** from a normalized source to a warehouse.
- Showcases **workflow automation** using a modern orchestrator like Airflow.
- Applies **data warehouse design** principles (Star Schema, Fact/Dimension modeling).
- Strengthens **advanced SQL** and data transformation skills.
- Builds a foundation for future **business intelligence dashboards** and analytics.

## 📬 Future Enhancements

Possible future improvements include:
- Implementing **incremental loading** instead of full refreshes.
- Building interactive dashboards with **Power BI or Tableau** connected to the warehouse.
- Adding more sophisticated **data quality and validation** steps.
- Containerizing the entire solution using **Docker and Docker Compose**.
- Migrating the pipeline to a **cloud environment** (e.g., using AWS S3 for storage and Amazon Redshift as the warehouse).

## 👨‍💻 Team Members

- **Ibrahim Hegazi** - Data Engineer
- **Ali Sharaf** - Data Engineer
- **Manar Eltyp** - Data Engineer
- **Safaa Mohamoud** - Data Engineer
- **Wafaa Mohamoud** - Data Engineer

---
