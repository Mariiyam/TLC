## Part 2: Data Pipeline Design (20 minutes)
##### Describe a data pipeline that:
1. Ingests: Downloads the NYC Yellow Taxi Trip Records for a specified month from the provided URL.
2. Transforms: Filters the dataset to include only trips with fares over $10 and renames columns to align with the following schema:
trip_id, pickup_time, dropoff_time, pickup_location, dropoff_location, fare_amount
3. Loads: Inserts the transformed data into a PostgreSQL or TimescaleDB database.
In your description, include:
- The tools you would use (e.g., Airflow, Python scripts).
- How you would manage any download or connectivity issues.
- Any data quality or governance measures you would consider.

## Solution:
#### 1. To ingest/ download and handle connectivity/download isses: 
I will use Python using Python with libraries: 
- “request” for handling URL connections.
- “duckdb” for storing and filtering the parquet file based on date.
I will include try/catch blocks to handle connectivity issues by retrying connecting several times before cancelling, catching the common connectivity issues, and by handling the download errors by sending notifications (by email) after a couple of trials.
#### 2. To transform the data: 
I will use pandas library dataframes to filter the trips to filter only the trips with fares over 10$ and to rename columns according to the specified schema:
- trip_id
- pickup_time
- dropoff_time
- pickup_location
- dropoff_location
- fare_amount
#### To apply quality checks or governance measures:
I will use “great_expectations” Python library to validate that the transformed data meets expected standards (e.g., no missing values in critical columns, fare_amount is numeric and above $10). Also, I will use logs, automated tools and frameworks to continuously monitor data quality metrics, create reports and alerts for the status of the data quality, and feedback loops with the stakeholders and subject matter experts.
#### Critical Data Quality Measures
1. Data Validation (Schema -data types- or data values)
2. Duplicate Detection 
3. Error Handling and Logging to facilitate troubleshooting
4. Data Completeness
5. Data Consistency
6. Data Accuracy
#### Critical Governance Measures
1. Oversee compliance with policies and Standards requirements
2. Metadata Management
3. Access Control and Security (role-based access controls)
4. Data Lifecycle Management (retention, archiving, and deletion)
5. Audit logs to track data changes, ETL process executions, and user activities. Regularly review these logs to detect and address any irregularities.

#### 3. To load the data into a PostgreSQL or TimescaleDB database:
I will use python’s library “sqlalchemy” for connecting to the database and and panda’s to_sql() function to insert the data. 
To handle the load issues, I will implement a rollback function in case of failure, and a log function for successful or failure of the insertion process.


#### *** To schedule the entire pipeline, I will use Airflow scheduler to trigger the script every month.

#### Example Workflow
1. Airflow Scheduler triggers the ingestion script on the first day of each month.
2. The script downloads the dataset, retries upon failure, and logs the results.
3. The data is loaded into a Pandas DataFrame, filtered, and transformed.
4. Data quality checks are performed before loading into the database.
5. The transformed data is inserted into PostgreSQL/TimescaleDB, with success and error logs recorded.

By incorporating these tools and practices, the data pipeline will be robust, maintainable, and capable of ensuring high-quality data ingestion and processing.

