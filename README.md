![deepseek_mermaid_20260310_6b9c42](https://github.com/user-attachments/assets/a467d1ad-3866-43a1-87de-c50e3ab1c048)# Northwind Sales ETL Data Engineering Pipeline

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

The project architecture will follow this flow:

![Uplo<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns:xhtml="http://www.w3.org/1999/xhtml" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns="http://www.w3.org/2000/svg" id="mermaid-svg-17" width="100%" class="flowchart" style="max-width: 100%;" viewBox="-24.67159118652344 -24.67159118652344 542.7750061035156 1435.045574951172" height="100%"><style>#mermaid-svg-17{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;fill:#ccc;}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#mermaid-svg-17 .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#mermaid-svg-17 .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#mermaid-svg-17 .error-icon{fill:#a44141;}#mermaid-svg-17 .error-text{fill:#ddd;stroke:#ddd;}#mermaid-svg-17 .edge-thickness-normal{stroke-width:1px;}#mermaid-svg-17 .edge-thickness-thick{stroke-width:3.5px;}#mermaid-svg-17 .edge-pattern-solid{stroke-dasharray:0;}#mermaid-svg-17 .edge-thickness-invisible{stroke-width:0;fill:none;}#mermaid-svg-17 .edge-pattern-dashed{stroke-dasharray:3;}#mermaid-svg-17 .edge-pattern-dotted{stroke-dasharray:2;}#mermaid-svg-17 .marker{fill:lightgrey;stroke:lightgrey;}#mermaid-svg-17 .marker.cross{stroke:lightgrey;}#mermaid-svg-17 svg{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;}#mermaid-svg-17 p{margin:0;}#mermaid-svg-17 .label{font-family:"trebuchet ms",verdana,arial,sans-serif;color:#ccc;}#mermaid-svg-17 .cluster-label text{fill:#F9FFFE;}#mermaid-svg-17 .cluster-label span{color:#F9FFFE;}#mermaid-svg-17 .cluster-label span p{background-color:transparent;}#mermaid-svg-17 .label text,#mermaid-svg-17 span{fill:#ccc;color:#ccc;}#mermaid-svg-17 .node rect,#mermaid-svg-17 .node circle,#mermaid-svg-17 .node ellipse,#mermaid-svg-17 .node polygon,#mermaid-svg-17 .node path{fill:#1f2020;stroke:#ccc;stroke-width:1px;}#mermaid-svg-17 .rough-node .label text,#mermaid-svg-17 .node .label text,#mermaid-svg-17 .image-shape .label,#mermaid-svg-17 .icon-shape .label{text-anchor:middle;}#mermaid-svg-17 .node .katex path{fill:#000;stroke:#000;stroke-width:1px;}#mermaid-svg-17 .rough-node .label,#mermaid-svg-17 .node .label,#mermaid-svg-17 .image-shape .label,#mermaid-svg-17 .icon-shape .label{text-align:center;}#mermaid-svg-17 .node.clickable{cursor:pointer;}#mermaid-svg-17 .root .anchor path{fill:lightgrey!important;stroke-width:0;stroke:lightgrey;}#mermaid-svg-17 .arrowheadPath{fill:lightgrey;}#mermaid-svg-17 .edgePath .path{stroke:lightgrey;stroke-width:2.0px;}#mermaid-svg-17 .flowchart-link{stroke:lightgrey;fill:none;}#mermaid-svg-17 .edgeLabel{background-color:hsl(0, 0%, 34.4117647059%);text-align:center;}#mermaid-svg-17 .edgeLabel p{background-color:hsl(0, 0%, 34.4117647059%);}#mermaid-svg-17 .edgeLabel rect{opacity:0.5;background-color:hsl(0, 0%, 34.4117647059%);fill:hsl(0, 0%, 34.4117647059%);}#mermaid-svg-17 .labelBkg{background-color:rgba(87.75, 87.75, 87.75, 0.5);}#mermaid-svg-17 .cluster rect{fill:hsl(180, 1.5873015873%, 28.3529411765%);stroke:rgba(255, 255, 255, 0.25);stroke-width:1px;}#mermaid-svg-17 .cluster text{fill:#F9FFFE;}#mermaid-svg-17 .cluster span{color:#F9FFFE;}#mermaid-svg-17 div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:12px;background:hsl(20, 1.5873015873%, 12.3529411765%);border:1px solid rgba(255, 255, 255, 0.25);border-radius:2px;pointer-events:none;z-index:100;}#mermaid-svg-17 .flowchartTitleText{text-anchor:middle;font-size:18px;fill:#ccc;}#mermaid-svg-17 rect.text{fill:none;stroke-width:0;}#mermaid-svg-17 .icon-shape,#mermaid-svg-17 .image-shape{background-color:hsl(0, 0%, 34.4117647059%);text-align:center;}#mermaid-svg-17 .icon-shape p,#mermaid-svg-17 .image-shape p{background-color:hsl(0, 0%, 34.4117647059%);padding:2px;}#mermaid-svg-17 .icon-shape rect,#mermaid-svg-17 .image-shape rect{opacity:0.5;background-color:hsl(0, 0%, 34.4117647059%);fill:hsl(0, 0%, 34.4117647059%);}#mermaid-svg-17 .label-icon{display:inline-block;height:1em;overflow:visible;vertical-align:-0.125em;}#mermaid-svg-17 .node .label-icon path{fill:currentColor;stroke:revert;stroke-width:revert;}#mermaid-svg-17 :root{--mermaid-font-family:"trebuchet ms",verdana,arial,sans-serif;}</style><g><marker id="mermaid-svg-17_flowchart-v2-pointEnd" class="marker flowchart-v2" viewBox="0 0 10 10" refX="5" refY="5" markerUnits="userSpaceOnUse" markerWidth="8" markerHeight="8" orient="auto"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowMarkerPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"/></marker><marker id="mermaid-svg-17_flowchart-v2-pointStart" class="marker flowchart-v2" viewBox="0 0 10 10" refX="4.5" refY="5" markerUnits="userSpaceOnUse" markerWidth="8" markerHeight="8" orient="auto"><path d="M 0 5 L 10 10 L 10 0 z" class="arrowMarkerPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"/></marker><marker id="mermaid-svg-17_flowchart-v2-circleEnd" class="marker flowchart-v2" viewBox="0 0 10 10" refX="11" refY="5" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><circle cx="5" cy="5" r="5" class="arrowMarkerPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"/></marker><marker id="mermaid-svg-17_flowchart-v2-circleStart" class="marker flowchart-v2" viewBox="0 0 10 10" refX="-1" refY="5" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><circle cx="5" cy="5" r="5" class="arrowMarkerPath" style="stroke-width: 1; stroke-dasharray: 1, 0;"/></marker><marker id="mermaid-svg-17_flowchart-v2-crossEnd" class="marker cross flowchart-v2" viewBox="0 0 11 11" refX="12" refY="5.2" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><path d="M 1,1 l 9,9 M 10,1 l -9,9" class="arrowMarkerPath" style="stroke-width: 2; stroke-dasharray: 1, 0;"/></marker><marker id="mermaid-svg-17_flowchart-v2-crossStart" class="marker cross flowchart-v2" viewBox="0 0 11 11" refX="-1" refY="5.2" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto"><path d="M 1,1 l 9,9 M 10,1 l -9,9" class="arrowMarkerPath" style="stroke-width: 2; stroke-dasharray: 1, 0;"/></marker><g class="root"><g class="clusters"/><g class="edgePaths"><path d="M138,110L138,114.167C138,118.333,138,126.667,138.07,134.417C138.141,142.167,138.281,149.334,138.351,152.917L138.422,156.501" id="L_A_B_0" class="edge-thickness-normal edge-pattern-solid edge-thickness-normal edge-pattern-solid flowchart-link" style="" marker-end="url(#mermaid-svg-17_flowchart-v2-pointEnd)"/><path d="M138.5,342.261L138.417,346.345C138.333,350.428,138.167,358.595,138.083,366.178C138,373.761,138,380.761,138,384.261L138,387.761" id="L_B_C_0" class="edge-thickness-normal edge-pattern-solid edge-thickness-normal edge-pattern-solid flowchart-link" style="" marker-end="url(#mermaid-svg-17_flowchart-v2-pointEnd)"/><path d="M138,513.541L138,517.708C138,521.874,138,530.208,143.399,544.812C148.798,559.417,159.596,580.293,164.995,590.731L170.394,601.169" id="L_C_D_0" class="edge-thickness-normal edge-pattern-solid edge-thickness-normal edge-pattern-solid flowchart-link" style="" marker-end="url(#mermaid-svg-17_flowchart-v2-pointEnd)"/><path d="M212.912,803.825L212.829,807.908C212.745,811.992,212.579,820.158,212.495,827.742C212.412,835.325,212.412,842.325,212.412,845.825L212.412,849.325" id="L_D_E_0" class="edge-thickness-normal edge-pattern-solid edge-thickness-normal edge-pattern-solid flowchart-link" style="" marker-end="url(#mermaid-svg-17_flowchart-v2-pointEnd)"/><path d="M212.412,977.342L212.412,981.509C212.412,985.675,212.412,994.009,219.003,1007.9C225.593,1021.791,238.775,1041.24,245.365,1050.964L251.956,1060.688" id="L_E_F_0" class="edge-thickness-normal edge-pattern-solid edge-thickness-normal edge-pattern-solid flowchart-link" style="" marker-end="url(#mermaid-svg-17_flowchart-v2-pointEnd)"/><path d="M290.358,1207.274L290.275,1211.357C290.191,1215.44,290.025,1223.607,289.941,1231.19C289.858,1238.774,289.858,1245.774,289.858,1249.274L289.858,1252.774" id="L_F_G_0" class="edge-thickness-normal edge-pattern-solid edge-thickness-normal edge-pattern-solid flowchart-link" style="" marker-end="url(#mermaid-svg-17_flowchart-v2-pointEnd)"/><path d="M334.807,98L324.228,104.167C313.648,110.333,292.489,122.667,268.375,140.747C244.261,158.826,217.193,182.653,203.659,194.566L190.124,206.479" id="L_H_B_0" class="edge-thickness-normal edge-pattern-dotted edge-thickness-normal edge-pattern-solid flowchart-link" style="" marker-end="url(#mermaid-svg-17_flowchart-v2-pointEnd)"/><path d="M372.237,98L367.576,104.167C362.915,110.333,353.592,122.667,348.931,148.147C344.27,173.627,344.27,212.254,344.27,250.881C344.27,289.508,344.27,328.134,344.27,361.763C344.27,395.391,344.27,424.021,344.27,452.651C344.27,481.281,344.27,509.911,332.343,537.501C320.416,565.09,296.562,591.639,284.635,604.914L272.708,618.188" id="L_H_D_0" class="edge-thickness-normal edge-pattern-dotted edge-thickness-normal edge-pattern-solid flowchart-link" style="" marker-end="url(#mermaid-svg-17_flowchart-v2-pointEnd)"/><path d="M422.242,98L425.488,104.167C428.733,110.333,435.225,122.667,438.47,148.147C441.716,173.627,441.716,212.254,441.716,250.881C441.716,289.508,441.716,328.134,441.716,361.763C441.716,395.391,441.716,424.021,441.716,452.651C441.716,481.281,441.716,509.911,441.716,548.375C441.716,586.838,441.716,635.136,441.716,683.433C441.716,731.73,441.716,780.028,441.716,818.678C441.716,857.328,441.716,886.331,441.716,915.333C441.716,944.336,441.716,973.339,425.537,1000.204C409.359,1027.068,377.001,1051.795,360.823,1064.158L344.644,1076.521" id="L_H_F_0" class="edge-thickness-normal edge-pattern-dotted edge-thickness-normal edge-pattern-solid flowchart-link" style="" marker-end="url(#mermaid-svg-17_flowchart-v2-pointEnd)"/></g><g class="edgeLabels"><g class="edgeLabel"><g class="label" transform="translate(0, 0)"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" class="labelBkg" style="display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g class="label" transform="translate(0, 0)"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" class="labelBkg" style="display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g class="label" transform="translate(0, 0)"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" class="labelBkg" style="display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g class="label" transform="translate(0, 0)"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" class="labelBkg" style="display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g class="label" transform="translate(0, 0)"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" class="labelBkg" style="display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g class="label" transform="translate(0, 0)"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" class="labelBkg" style="display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g class="label" transform="translate(0, 0)"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" class="labelBkg" style="display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g class="label" transform="translate(0, 0)"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" class="labelBkg" style="display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;"><span class="edgeLabel"></span></div></foreignObject></g></g><g class="edgeLabel"><g class="label" transform="translate(0, 0)"><foreignObject width="0" height="0"><div xmlns="http://www.w3.org/1999/xhtml" class="labelBkg" style="display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;"><span class="edgeLabel"></span></div></foreignObject></g></g></g><g class="nodes"><g class="node default" id="flowchart-A-0" transform="translate(138, 59)"><rect class="basic label-container" style="" x="-130" y="-51" width="260" height="102"/><g class="label" style="" transform="translate(-100, -36)"><rect/><foreignObject width="200" height="72"><div xmlns="http://www.w3.org/1999/xhtml" style="display: table; white-space: break-spaces; line-height: 1.5; max-width: 200px; text-align: center; width: 200px;"><span class="nodeLabel"><p>CSV Files<br />Orders, Products, Customers...</p></span></div></foreignObject></g></g><g class="node default" id="flowchart-B-1" transform="translate(138, 250.88068389892578)"><polygon points="90.88068389892578,0 181.76136779785156,-90.88068389892578 90.88068389892578,-181.76136779785156 0,-90.88068389892578" class="label-container" transform="translate(-90.38068389892578, 90.88068389892578)"/><g class="label" style="" transform="translate(-63.88068389892578, -12)"><rect/><foreignObject width="127.76136779785156" height="24"><div xmlns="http://www.w3.org/1999/xhtml" style="display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;"><span class="nodeLabel"><p>Python Extraction</p></span></div></foreignObject></g></g><g class="node default" id="flowchart-C-3" transform="translate(138, 452.6511878967285)"><path d="M0,11.593213888121154 a54.04545593261719,11.593213888121154 0,0,0 108.09091186523438,0 a54.04545593261719,11.593213888121154 0,0,0 -108.09091186523438,0 l0,98.59321388812116 a54.04545593261719,11.593213888121154 0,0,0 108.09091186523438,0 l0,-98.59321388812116" class="basic label-container" style="fill:#cd7f32 !important" transform="translate(-54.04545593261719, -60.889820832181734)"/><g class="label" style="color:white !important" transform="translate(-46.54545593261719, -26)"><rect/><foreignObject width="93.09091186523438" height="72"><div xmlns="http://www.w3.org/1999/xhtml" style="color: white !important; display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;"><span style="color:white !important" class="nodeLabel"><p>Bronze Layer<br />PostgreSQL<br />Raw Tables</p></span></div></foreignObject></g></g><g class="node default" id="flowchart-D-5" transform="translate(212.41193389892578, 683.4330520629883)"><polygon points="119.89204406738281,0 239.78408813476562,-119.89204406738281 119.89204406738281,-239.78408813476562 0,-119.89204406738281" class="label-container" transform="translate(-119.39204406738281, 119.89204406738281)"/><g class="label" style="" transform="translate(-80.89204406738281, -24)"><rect/><foreignObject width="161.78408813476562" height="48"><div xmlns="http://www.w3.org/1999/xhtml" style="display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;"><span class="nodeLabel"><p>Python Transformation<br />Cleaning &amp; Validation</p></span></div></foreignObject></g></g><g class="node default" id="flowchart-E-7" transform="translate(212.41193389892578, 915.3334999084473)"><path d="M0,12.341772151898734 a60.9375,12.341772151898734 0,0,0 121.875,0 a60.9375,12.341772151898734 0,0,0 -121.875,0 l0,99.34177215189874 a60.9375,12.341772151898734 0,0,0 121.875,0 l0,-99.34177215189874" class="basic label-container" style="fill:#c0c0c0 !important" transform="translate(-60.9375, -62.01265822784811)"/><g class="label" style="color:black !important" transform="translate(-53.4375, -26)"><rect/><foreignObject width="106.875" height="72"><div xmlns="http://www.w3.org/1999/xhtml" style="color: black !important; display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;"><span style="color:black !important" class="nodeLabel"><p>Silver Layer<br />PostgreSQL<br />Cleaned Tables</p></span></div></foreignObject></g></g><g class="node default" id="flowchart-F-9" transform="translate(289.8579559326172, 1117.0578155517578)"><polygon points="89.71591186523438,0 179.43182373046875,-89.71591186523438 89.71591186523438,-179.43182373046875 0,-89.71591186523438" class="label-container" transform="translate(-89.21591186523438, 89.71591186523438)"/><g class="label" style="" transform="translate(-50.715911865234375, -24)"><rect/><foreignObject width="101.43182373046875" height="48"><div xmlns="http://www.w3.org/1999/xhtml" style="display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;"><span class="nodeLabel"><p>Data Modeling<br />SQL/Python</p></span></div></foreignObject></g></g><g class="node default" id="flowchart-G-11" transform="translate(289.8579559326172, 1317.2380561828613)"><path d="M0,11.309554149707498 a51.63068771362305,11.309554149707498 0,0,0 103.2613754272461,0 a51.63068771362305,11.309554149707498 0,0,0 -103.2613754272461,0 l0,98.3095541497075 a51.63068771362305,11.309554149707498 0,0,0 103.2613754272461,0 l0,-98.3095541497075" class="basic label-container" style="fill:#ffd700 !important" transform="translate(-51.63068771362305, -60.46433122456125)"/><g class="label" style="color:black !important" transform="translate(-44.13068771362305, -26)"><rect/><foreignObject width="88.2613754272461" height="72"><div xmlns="http://www.w3.org/1999/xhtml" style="color: black !important; display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;"><span style="color:black !important" class="nodeLabel"><p>Gold Layer<br />PostgreSQL<br />Star Schema</p></span></div></foreignObject></g></g><g class="node default" id="flowchart-H-12" transform="translate(401.7159118652344, 59)"><rect class="basic label-container" style="" x="-83.71591186523438" y="-39" width="167.43182373046875" height="78"/><g class="label" style="" transform="translate(-53.715911865234375, -24)"><rect/><foreignObject width="107.43182373046875" height="48"><div xmlns="http://www.w3.org/1999/xhtml" style="display: table-cell; white-space: nowrap; line-height: 1.5; max-width: 200px; text-align: center;"><span class="nodeLabel"><p>Apache Airflow<br />Orchestration</p></span></div></foreignObject></g></g></g></g></g></svg>ading deepseek_mermaid_20260310_6b9c42.svg…]()




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
