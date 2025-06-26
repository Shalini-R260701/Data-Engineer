# Data-Engineer

# 🚀 Mini ETL Pipeline Using PySpark, FastAPI & Airflow

**🔍 Project Overview**  
This project demonstrates a mini ETL (Extract, Transform, Load) pipeline using PySpark for data processing, FastAPI for data exposure, and Airflow for orchestration. It processes the Global Terrorism Database (GTD), a real-world, complex dataset, to clean, transform, and serve usable data.

**📦 Dataset**  
The dataset used is `GlobalTerrorismDB0718dsrsex.csv`, containing:  
- 135 columns  
- 36,883 records  
This dataset captures various features of terrorism-related incidents worldwide, making it ideal for ETL demonstration.

**⚙️ Environment Setup**  
To run this pipeline, the following are required:  
- **Java 11**  
- **Apache Spark 3.2.1**  
- **Python Libraries**: `pyspark`, `findspark`, `pandas`, `fastapi`, `uvicorn`, `pyngrok`  
- **Apache Airflow 2.6.3** (for orchestration)  

**📤 Extract Phase**  
- Data is read from a CSV file using PySpark.  
- Header row is used for column names.  
- Schema is inferred automatically by Spark.  
- Loads raw data into a Spark DataFrame for transformation.

**🧹 Transform Phase**  
- **Null Handling**:
  - Drop rows with missing critical values (`eventid`, `iyear`, `country`, etc.).
  - Fill nulls in numeric columns (`nkill`, `nwound`) with `0`.
  - Fill nulls in textual columns (`city`, `summary`) with `"Unknown"`.
- **Deduplication**:
  - Drop exact duplicate rows.
  - Remove duplicate `eventid` values.
- **Feature Engineering**:
  - `casualties`: Sum of killed and wounded.
  - `is_successful`: Boolean flag based on success column.
  - `is_suicide_attack`: Boolean flag.
  - `year_month`: Combined `iyear` and `imonth` into a single string.

**💾 Load Phase**  
- Transformed data is saved into a mock S3 path:  
  `content/mock_s3_bucket/global_terrorism_cleaned/`  
- Stored as CSV with headers for clarity.

**🌐 API Exposure Using FastAPI**  
- A FastAPI app is built to expose cleaned data.  
- Endpoint `/sample?n=5` returns 5 random records.  
- Uvicorn runs the server; pyngrok exposes it publicly.  
- Makes data accessible to external users like analysts.

**🎼 Orchestration Using Apache Airflow**  
- Airflow DAG is used to automate ETL steps.  
- Defines extract, clean, transform, load, and expose as tasks.  
- Ensures proper execution order, retries, logging, and monitoring.  
- Mimics production-grade pipeline automation.

**📌 Key Learnings**  
- Efficient ETL pipelines are key to transforming chaotic raw data into valuable, actionable datasets.  
- PySpark enables fast and scalable transformation.  
- FastAPI simplifies sharing of cleaned data.  
- Airflow brings automation, reliability, and visibility to the pipeline.

