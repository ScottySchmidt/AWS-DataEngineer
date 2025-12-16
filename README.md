# AWS Data Engineer Pipeline
The following data engineer system automatically ingests data, stores and processes it, and produces analytics outputs, all deployed through Infrastructure as Code (AWS CDK in Python). The architecture follows a four-part pattern: ingest → store → analyze → deploy. This project is adapted from a top industry data-engineering interview "internship" challenge. 
- Scott Schmidt — Illinois State University - Masters Computer Science for IT494 
- Updated GitHub Version will be here:  https://github.com/ScottySchmidt/AWS-DataEngineer/tree/main

## Project Summary
This pipeline pulls data from public APIs, stores it in Amazon S3, processes it with AWS Lambda, and produces outputs suitable for analytics and reporting. All components, including compute, storage, permissions, and scheduling, are provisioned automatically using the AWS CDK in Python.

---
# Pipeline Architecture

#### Part 1: AWS S3 & Sourcing Datasets
1. Republish [this open dataset](https://download.bls.gov/pub/time.series/pr/) in Amazon S3 and share with us a link.
    - You may run into 403 Forbidden errors as you test accessing this data. There is a way to comply with the BLS data access policies and re-gain access to fetch this data programatically. The BLS data access policies can be found here: https://www.bls.gov/bls/pss.htm . BLS also reserves the right to block robots that do not contain information that can be used to contact the owner. Blocking may occur in real time. An API key and  Adding a <code>User-Agent</code> header to your request with contact information will comply with the BLS data policies and allow you to keep accessing their data programmatically. 
2. Script this process so the files in the S3 bucket are kept in sync with the source when data on the website is updated, added, or deleted.
    - Don't rely on hard coded names - the script should be able to handle added or removed files.
    - Ensure the script doesn't upload the same file more than once.

#### Part 2: APIs
1. Create a script that will fetch data from [this API](https://honolulu-api.datausa.io/tesseract/data.jsonrecords?cube=acs_yg_total_population_1&drilldowns=Year%2CNation&locale=en&measures=Population).
   You can read the documentation [here](https://datausa.io/about/api/).
2. Save the result of this API call as a JSON file in S3.

#### Part 3: Data Analytics
0. Load both the csv file from **Part 1** `pr.data.0.Current` and the json file from **Part 2**
   as dataframes ([Spark](https://spark.apache.org/docs/1.6.1/api/java/org/apache/spark/sql/DataFrame.html),
                  [Pyspark](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.DataFrame.html),
                  [Pandas](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html),
                  [Koalas](https://koalas.readthedocs.io/en/latest/),
                  etc).

1. Using the dataframe from the population data API (Part 2),
   generate the mean and the standard deviation of the annual US population across the years [2013, 2018] inclusive.

2. Using the dataframe from the time-series (Part 1),
   For every series_id, find the *best year*: the year with the max/largest sum of "value" for all quarters in that year. Generate a report with each series id, the best year for that series, and the summed value for that year.
   For example, if the table had the following values:

    | series_id   | year | period | value |
    |-------------|------|--------|-------|
    | PRS30006011 | 1995 | Q01    | 1     |
    | PRS30006011 | 1995 | Q02    | 2     |
    | PRS30006011 | 1996 | Q01    | 3     |
    | PRS30006011 | 1996 | Q02    | 4     |
    | PRS30006012 | 2000 | Q01    | 0     |
    | PRS30006012 | 2000 | Q02    | 8     |
    | PRS30006012 | 2001 | Q01    | 2     |
    | PRS30006012 | 2001 | Q02    | 3     |

    the report would generate the following table:

    | series_id   | year | value |
    |-------------|------|-------|
    | PRS30006011 | 1996 | 7     |
    | PRS30006012 | 2000 | 8     |

#### Part 3: Data Analytics
0. Load both the csv file from **Part 1** `pr.data.0.Current` and the json file from **Part 2**
   as dataframes ([Spark](https://spark.apache.org/docs/1.6.1/api/java/org/apache/spark/sql/DataFrame.html),
                  [Pyspark](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.DataFrame.html),
                  [Pandas](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html)

1. Using the dataframe from the population data API (Part 2),
   generate the mean and the standard deviation of the annual US population across the years [2013, 2018] inclusive.

2. Using the dataframe from the time-series (Part 1),
   For every series_id, find the *best year*: the year with the max/largest sum of "value" for all quarters in that year. Generate a report with each series id, the best year for that series, and the summed value for that year.
   For example, if the table had the following values:

    | series_id   | year | period | value |
    |-------------|------|--------|-------|
    | PRS30006011 | 1995 | Q01    | 1     |
    | PRS30006011 | 1995 | Q02    | 2     |
    | PRS30006011 | 1996 | Q01    | 3     |
    | PRS30006011 | 1996 | Q02    | 4     |
    | PRS30006012 | 2000 | Q01    | 0     |
    | PRS30006012 | 2000 | Q02    | 8     |
    | PRS30006012 | 2001 | Q01    | 2     |
    | PRS30006012 | 2001 | Q02    | 3     |

    the report would generate the following table:

    | series_id   | year | value |
    |-------------|------|-------|
    | PRS30006011 | 1996 | 7     |
    | PRS30006012 | 2000 | 8     |

#### Part 3: Data Analytics

**0. Load the data**

Load both:
- the CSV file from **Part 1**: `pr.data.0.Current`
- the JSON file from **Part 2**

as dataframes using one of the following:
- [Spark DataFrame](https://spark.apache.org/docs/1.6.1/api/java/org/apache/spark/sql/DataFrame.html)
- [PySpark DataFrame](https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.DataFrame.html)
- [Pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html)

---

**1. Population statistics**

Using the dataframe from the population data API (**Part 2**), compute:
- the **mean**
- the **standard deviation**

of the annual U.S. population for the years **2013–2018 (inclusive)**.

---

**2. Best year by series**

Using the time-series dataframe (**Part 1**):

For each `series_id`, find the **best year**, defined as the year with the **maximum total sum of `value` across all quarters**.

Generate a report with:
- `series_id`
- best `year`
- summed `value` for that year

**Example input:**

| series_id   | year | period | value |
|-------------|------|--------|-------|
| PRS30006011 | 1995 | Q01    | 1     |
| PRS30006011 | 1995 | Q02    | 2     |
| PRS30006011 | 1996 | Q01    | 3     |
| PRS30006011 | 1996 | Q02    | 4     |
| PRS30006012 | 2000 | Q01    | 0     |
| PRS30006012 | 2000 | Q02    | 8     |
| PRS30006012 | 2001 | Q01    | 2     |
| PRS30006012 | 2001 | Q02    | 3     |

**Expected output:**

| series_id   | year | value |
|-------------|------|-------|
| PRS30006011 | 1996 | 7     |
| PRS30006012 | 2000 | 8     |

---

**3. Join population and series data**

Using **both** dataframes from Parts 1 and 2, generate a report that includes:
- `value` for `series_id = PRS30006032`
- `period = Q01`
- `population` for the corresponding year (if available)

**Example output row:**

| series_id   | year | period | value | Population |
|-------------|------|--------|-------|------------|
| PRS30006032 | 2018 | Q01    | 1.9   | 327167439  |

---

**Hints**

When working with public datasets, data cleaning may be required.  
For example, you may need to trim whitespace before filtering or joining:

- [Trimming whitespace in Spark](https://stackoverflow.com/questions/35540974/remove-blank-space-from-data-frame-column-values-in-spark)



4. Submit your analysis, your queries, and the outcome of the reports as a [.ipynb](https://fileinfo.com/extension/ipynb) file.


---

## How the Pipeline Works
1. Local or Lambda-based ingestion fetches API data and uploads it to S3  
2. Hash-based sync prevents uploading unchanged files  
3. S3 events trigger processing Lambdas (optionally via SQS)  
4. Pandas cleans and summarizes the data  
5. Outputs are written back to S3 for reporting or dashboard use  
6. CDK deploys all compute, storage, scheduling, and permissions  
7. Video demonstration and user documentation (if applicable)
8. Document use AI as a reference tool and exhibit full understanding.

---- 
### AWS Infrastructure
- Amazon S3 — Raw and processed data storage  
- AWS Lambda — Ingestion and transformation compute  
- Amazon SQS — Event queue for decoupled processing  
- Amazon EventBridge — Scheduled API ingestion  
- AWS CDK (Python) — Infrastructure as Code  
- AWS IAM — Secure role-based permissions  

### Technologies
- Python using Pandas and Boto3  
- AWS Secrets Manager, GitHub Secrets  
- Public APIs: BLS, DataUSA
  
---
#### Enhanced Ideas
* **CI/CD Workflow (Optional)**  GitHub Actions workflow for automated CDK deployment on commit.
* Use Amazon Redshift and Amazon Athena for SQL queries for the below:
https://github.com/rearc-data/analytics-quest
* Replace the processing Lambda with AWS Glue or AWS Step Functions for complex orchestration.
* Introduce cost anomaly detection using AWS Budgets and SNS alerts.
