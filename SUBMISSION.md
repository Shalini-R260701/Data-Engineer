# 📄 Submission: Mini ETL Pipeline with PySpark, FastAPI, and Airflow

**🧠 Problem Statement**  
Data often arrives in messy, large, and inconsistent formats. Manually handling this chaos is time-consuming and prone to errors. This project builds a mini ETL pipeline to automatically extract, clean, transform, and serve data efficiently using PySpark, expose it via a FastAPI service, and orchestrate the process using Airflow.

**📂 Dataset Used**  
- Dataset: `GlobalTerrorismDB0718dsrsex.csv`  
- Rows: ~36,883  
- Columns: 135  
- Real-world terrorism incidents dataset, rich in missing values and domain-specific information — ideal for showcasing data cleaning and transformation tasks.

**🛠️ Tools and Technologies**  
- **PySpark**: Used for distributed data processing and transformation.  
- **FastAPI**: Serves as the backend to expose cleaned data via API.  
- **Uvicorn**: Runs the FastAPI app.  
- **Ngrok**: Creates a public URL for accessing the FastAPI endpoint.  
- **Apache Airflow**: Handles orchestration, automating the ETL process into manageable tasks.

**🔄 ETL Pipeline Design**  

**1. Extract**  
- Load raw data using PySpark’s `read.csv` method.  
- Use header row and enable schema inference.  
- Load 36,883 rows into a DataFrame for processing.

**2. Transform**  
- **Null Handling**:
  - Drop rows with nulls in critical columns like `eventid`, `iyear`, `country`, and `attacktype1`.  
  - Fill numeric nulls (e.g., `nkill`, `nwound`) with `0`.  
  - Fill text nulls (e.g., `city`, `summary`) with `"Unknown"`.  
- **Deduplication**:
  - Drop full duplicate rows.  
  - Drop duplicate `eventid` entries to maintain uniqueness.  
- **Feature Engineering**:
  - `casualties`: Total of killed and wounded.  
  - `is_successful`: Boolean flag from `success`.  
  - `is_suicide_attack`: Boolean flag from `suicide`.  
  - `year_month`: Concatenate year and month for trend analysis.

**3. Load**  
- Save cleaned DataFrame to a mock S3 bucket (`/content/mock_s3_bucket/global_terrorism_cleaned/`).  
- Use `.csv` format with header for downstream usability.

**🌐 API Exposure**  
- A FastAPI service is created with endpoint `/sample?n=5` to return random `n` rows.  
- Makes clean data accessible to external tools or analysts via a public Ngrok tunnel.  
- Designed for simplicity and ease of access for non-technical users.

**🎼 Airflow Orchestration**  
- Airflow DAG defines task order: extract → transform → load → expose.  
- Allows scheduled automation and retry logic for robustness.  
- Mimics production pipelines used in industry-grade systems.

**📈 Model Decisions**  
- No ML model used; focus was on data engineering practices.  
- Feature creation is designed to aid future downstream ML tasks such as classification or forecasting.

**📚 Key Learnings**  
- Spark is powerful for handling large datasets and parallel processing.  
- Clean pipelines reduce downstream errors and promote data trust.  
- APIs are essential for democratizing access to cleaned datasets.  
- Airflow makes the pipeline repeatable and production-ready.  
- Even without ML, a solid ETL pipeline brings immense value to any data system.

