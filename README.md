# AWS Data Engineer Pipeline  
The following is a top-5 consulting firm’s cloud data engineering intership project challenge. This document intentionally omits any mention of external companies or institutional partnerships, following academic guidelines. The system automatically ingests data, stores and processes it, and produces analytics outputs, all deployed through Infrastructure as Code (AWS CDK in Python). The architecture follows a four-part pattern: ingest → store → analyze → deploy.

Scott Schmidt — Illinois State University — IT497

---
# Project Summary
This pipeline pulls data from public APIs, stores it in Amazon S3, processes it with AWS Lambda, and produces outputs that are ready for analytics. All of the components, including compute, storage, permissions, and event scheduling, are provisioned automatically using the AWS CDK (Python).

The design focuses on:
- Automation  
- Scalability  
- Event-driven processing  
- Maintainability  
- Reproducible infrastructure  

---
# Data Four Part Process
1. **Jupyter Notebook — API Ingestion (Local Sync Version)**  
   - Uses the **BLS public API** (standard datasets + bulk files)  
   - Fetches BLS time-series data from `https://download.bls.gov/pub/time.series/pr/`  
   - Uses compliant `User-Agent` headers  
   - Hashing to detect file changes and avoid duplicate uploads  
   - Cleans and formats JSON/CSV before upload  
   - Enhanced Sync Mode automatically adds, updates, and deletes files in S3
   - Adding a User-Agent header to your request with contact information will comply with the BLS data policies to access data.

2. **AWS Lambda — API → S3**  
   Serverless function that retrieves data from an external API and stores JSON files in S3.

3. **Jupyter Notebook — Data Processing & Reporting**  
   Loads raw S3 datasets, cleans them using Pandas, and produces summarized analytical outputs.

4. **Infrastructure as Code — AWS CDK**  
   CDK project that deploys S3 buckets, Lambda functions, SQS queues, EventBridge schedules, and IAM roles.

5. **CI/CD Workflow (Optional)**  
   GitHub Actions workflow for automated CDK deployment on commit.

---
# Pipeline Architecture
### Ingest
- Lambda calls public APIs such as:
  - BLS Dataset (Producer Price Index):  
    `https://download.bls.gov/pub/time.series/pr/`
  - DataUSA API:  
    `https://honolulu-api.datausa.io`
- A local notebook can also ingest and sync large datasets before cloud processing.

### Store
- Amazon S3 stores raw JSON, CSV files, and processed outputs  
- Organized by dataset type and timestamp following lakehouse-style conventions

### Analyze
- S3 events trigger a processing Lambda (optionally through SQS)  
- Data is cleaned, summarized, and written back into S3

### Deploy
- All infrastructure is created using AWS CDK (Python) for consistent, repeatable deployments  
- Supports deployment from local environments, AWS CloudShell, or CI/CD

---
# Deployment Options
- Local Jupyter Notebook + Python CDK  
- AWS CloudShell CDK deployment  
- GitHub Actions CI/CD (optional)  

---
# AWS Services Used
- Amazon S3 — Raw and processed data storage  
- AWS Lambda — Ingestion and transformation compute  
- Amazon SQS — Event queue for decoupled processing  
- Amazon EventBridge — Scheduled API ingestion  
- AWS CDK (Python) — Infrastructure as Code  
- AWS IAM — Secure role-based permissions  

---
# Technologies
- Python  
- Pandas  
- Boto3  
- AWS Secrets Manager, GitHub Secrets  
- Public APIs: BLS, DataUSA  

---
# How the Pipeline Works
1. Local or Lambda-based ingestion fetches API data and uploads it to S3.  
2. Hash-based sync prevents uploading unchanged files.  
3. S3 events trigger processing Lambdas (optionally via SQS).  
4. Pandas cleans and summarizes the data.  
5. Outputs are stored back into S3 for reporting or dashboard use.  
6. CDK deploys all compute, storage, schedules, and permissions at once.
7. Video Demonstration + User Documentation
