## End-to-End Azure Databricks Data Lakehouse Pipeline
### Objective: Built a scalable pipeline for data ingestion, transformation, storage, visualization, and automation

### Step 1: Setup and Configuration
#### - Ensure Azure Databricks cluster is running (use free trial)

![Image](https://github.com/user-attachments/assets/0b4a738b-88d5-42ee-853b-25e6243c136a)



#### - Created a storage account in Azure and made an container to store the raw dataset 'nyc-taxi'


#### - Storage confifguration 

![image](https://github.com/user-attachments/assets/e2204a30-a86e-444a-82b5-ad16f4996982)

#### - Enabled hierarchical namespace

![Image](https://github.com/user-attachments/assets/1b3a7787-15ea-4a7e-8a72-fdca234791f2)



#### - Stored raw data csv. in container called 'demo'

![Image](https://github.com/user-attachments/assets/84f7078b-ea07-4910-8615-7623c45d6853)



### - Configured ADLS Gen2 credentials (replace with your details)
spark.conf.set(
    "fs.azure.account.key.<your-storage-account>.dfs.core.windows.net",
    "<your-access-key>"
)
adls_path = "abfss://<container>@<your-storage-account>.dfs.core.windows.net/"


![Image](https://github.com/user-attachments/assets/3dcc3bb4-77f4-4b9b-9f59-61ec94039acf)





### Step 2: Data Ingestion
#### Loaded NYC Taxi dataset from DBFS
taxi_path = "dbfs:/databricks-datasets/nyctaxi/tripdata/yellow/yellow_tripdata_2019-01.csv.gz"
taxi_df = spark.read.csv(taxi_path, header=True, inferSchema=True)




#### Create a simulated weather dataset
from pyspark.sql.functions import lit
weather_data = [
    ("2019-01-01", 32.0, "Snow"),
    ("2019-01-02", 35.0, "Rain"),
    ("2019-01-03", 40.0, "Clear")
]
weather_df = spark.createDataFrame(weather_data, ["date", "temperature", "condition"])

#### Save raw data to ADLS
taxi_df.write.mode("overwrite").parquet(adls_path + "raw/taxi")
weather_df.write.mode("overwrite").parquet(adls_path + "raw/weather")

### Step 3: Data Transformation with PySpark and Spark SQL
#### Clean taxi data
cleaned_taxi_df = taxi_df.filter(
    (taxi_df.passenger_count.isNotNull()) & 
    (taxi_df.trip_distance > 0)
).withColumnRenamed("tpep_pickup_datetime", "pickup_datetime")

#### Register tables for Spark SQL
cleaned_taxi_df.createOrReplaceTempView("taxi_temp")
weather_df.createOrReplaceTempView("weather_temp")

#### Join taxi and weather data
transformed_df = spark.sql("""
SELECT 
    t.pickup_datetime,
    t.passenger_count,
    t.trip_distance,
    t.total_amount,
    w.temperature,
    w.condition
FROM taxi_temp t
JOIN weather_temp w 
ON DATE(t.pickup_datetime) = w.date
""")

#### Aggregate data (e.g., average fare by date)
agg_df = transformed_df.groupBy("pickup_datetime").agg({
    "total_amount": "avg",
    "passenger_count": "sum"
}).withColumnRenamed("avg(total_amount)", "avg_fare")

### Step 4: Save to Delta Lake
#### Save transformed data to Delta Lake
delta_path = adls_path + "delta/taxi_weather"
transformed_df.write.mode("overwrite").format("delta").save(delta_path)

#### Optimize Delta table
spark.sql(f"OPTIMIZE delta.`{delta_path}` ZORDER BY (pickup_datetime)")

#### Create Delta table for querying
spark.sql(f"""
CREATE TABLE IF NOT EXISTS taxi_weather
USING DELTA
LOCATION '{delta_path}'
""")

### Step 5: Visualization (via Databricks SQL)
#### Note: Run this query in Databricks SQL Editor to create a dashboard
"""
SELECT 
    DATE(pickup_datetime) AS trip_date,
    AVG(total_amount) AS avg_fare,
    SUM(passenger_count) AS total_passengers
FROM taxi_weather
GROUP BY DATE(pickup_datetime)
"""
#### - Save as a visualization (e.g., line chart) and add to a dashboard in Databricks UI

### Step 6: Pipeline Automation
#### Note: Deploy this notebook as a job via Databricks UI
#### - Create a job in Workflows
#### - Schedule daily runs
#### - Set permissions to restrict access (e.g., admin only)
#### - Monitor job runs in Databricks UI

### Step 7: Save Outputs for Validation
#### Display sample results
display(transformed_df.limit(10))
display(agg_df.limit(10))
