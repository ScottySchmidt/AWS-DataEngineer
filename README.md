# AWS Data Engineer Pipeline
The following project is based on a cloud data engineering internship-style challenge. This document intentionally omits any mention of external companies following academic guidelines. The system automatically ingests data, stores and processes it, and produces data analytics outputs, all deployed using Infrastructure as Code (AWS CDK in Python). The architecture follows a four-part pattern: ingest → store → analyze → deploy

Scott Schmidt — Illinois State University — IT494 

Updated GitHub Version:  https://github.com/ScottySchmidt/AWS-DataEngineer/tree/main

---

## Project Summary
This pipeline pulls data from public APIs, stores it in Amazon S3, processes it with AWS Lambda, and produces outputs suitable for analytics and reporting. All components—including compute, storage, permissions, and scheduling—are provisioned automatically using the AWS CDK (Python).

The design emphasizes:
- Automation  
- Scalability  
- Event-driven processing  
- Maintainability  
- Reproducible infrastructure  

---

## Data Pipeline Process

1. **Jupyter Notebook — API Ingestion (Local Sync Version)**  
   - Uses the **BLS public API** (standard datasets and bulk files)  
   - Fetches BLS time-series data from  
     `https://download.bls.gov/pub/time.series/pr/`  
   - Uses compliant `User-Agent` headers per BLS data access guidelines  
   - Hash-based change detection to prevent duplicate uploads  
   - Cleans and formats JSON/CSV prior to upload  
   - Enhanced sync mode automatically adds, updates, and deletes files in S3  

2. **AWS Lambda — API → S3**  
   Serverless function that retrieves data from public APIs and stores JSON files in Amazon S3.

3. **Jupyter Notebook — Data Processing & Reporting**  
   Loads raw S3 datasets, cleans them using Pandas, and generates summarized analytical outputs.

4. **Infrastructure as Code — AWS CDK**  
   CDK project that deploys:
   - S3 buckets  
   - Lambda functions  
   - SQS queues  
   - EventBridge schedules  
   - IAM roles  

5. **CI/CD Workflow (Optional)**  
   GitHub Actions workflow for automated CDK deployment on commit.

---

## Pipeline Architecture

### Ingest
- Lambda functions call public APIs such as:
  - BLS Dataset (Producer Price Index):  
    `https://download.bls.gov/pub/time.series/pr/`
  - DataUSA API:  
    `https://honolulu-api.datausa.io`
- A local notebook can also ingest and sync large datasets prior to cloud processing.

### Store
- Amazon S3 stores raw JSON, CSV files, and processed outputs  
- Data is organized by dataset type and timestamp using lakehouse-style conventions

### Analyze
- S3 events trigger processing Lambdas (optionally through SQS)  
- Data is cleaned, aggregated, and written back to S3

### Deploy
- All infrastructure is created using AWS CDK (Python)  
- Supports deployment from local environments, AWS CloudShell, or CI/CD pipelines

---

## Deployment Options
- Local Jupyter Notebook + Python CDK  
- AWS CloudShell CDK deployment  
- GitHub Actions CI/CD (optional)  

---

## AWS Services Used
- Amazon S3 — Raw and processed data storage  
- AWS Lambda — Ingestion and transformation compute  
- Amazon SQS — Event queue for decoupled processing  
- Amazon EventBridge — Scheduled API ingestion  
- AWS CDK (Python) — Infrastructure as Code  
- AWS IAM — Secure role-based permissions  

---
## Technologies
- Python  
- Pandas  
- Boto3  
- AWS Secrets Manager, GitHub Secrets  
- Public APIs: BLS, DataUSA
  
## How the Pipeline Works
1. Local or Lambda-based ingestion fetches API data and uploads it to S3  
2. Hash-based sync prevents uploading unchanged files  
3. S3 events trigger processing Lambdas (optionally via SQS)  
4. Pandas cleans and summarizes the data  
5. Outputs are written back to S3 for reporting or dashboard use  
6. CDK deploys all compute, storage, scheduling, and permissions  
7. Video demonstration and user documentation (if applicable)
   
---
### Enchanced Ideas:
Use Amazon Redshift and Amazon Athena for SQL queries using data analytics below:
https://github.com/rearc-data/analytics-quest
