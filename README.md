# AWS End-to-End Airline Data Ingestion Pipeline

#### Data Source
- **S3 Data Lake:**
  - Daily flight data arrives in Amazon S3 organized using Hive-style partitioning (`year/month/day`).

#### Dimension and Fact Tables
- **Dimensional Table:**
  - `airport_dim.csv`: Preloaded dimension table containing airport codes stored in S3.
  - Loaded into Amazon Redshift as `airport_dim`.

- **Fact Table:**
  - `flight_fact`: Contains measurable attributes related to flights, updated daily.

#### Workflow Components and Architecture
1. **Amazon S3 Bucket (Data Lake):**
   - Stores daily incoming flight data files (Hive partitioned).
   - Stores `airport_dim.csv` for dimension table initialization.

2. **AWS EventBridge Rule:**
   - Scheduled or event-based trigger configured to initiate the ETL workflow daily upon new data arrival.

3. **AWS Step Functions:**
   - Orchestrates the entire ETL workflow, coordinating the sequential execution of Glue Crawlers and Jobs.
   - Manages error handling and retries, ensuring reliable execution and completion.

4. **AWS Glue Crawler:**
   - Automatically catalogs new flight data files in the AWS Glue Data Catalog.
   - Operates in a polling loop, ensuring successful detection and cataloging of daily flight data partitions before initiating subsequent ETL processes.

5. **AWS Glue Job:**
   - Performs ETL processes including extraction, transformation, and loading of flight data from the Glue Data Catalog into Amazon Redshift.
   - Utilizes AWS Glue transformations to maintain data quality, schema alignment, and compliance.

6. **Amazon Redshift Data Warehouse:**
   - Stores structured and optimized dimension and fact tables (`airport_dim` and `flight_fact`).
   - Supports analytical queries and business intelligence applications.

7. **AWS SNS (Simple Notification Service):**
   - Sends notifications upon successful completion or failure of the ETL workflow.
   - Enhances monitoring and alerting capabilities, allowing prompt responses to potential issues.

