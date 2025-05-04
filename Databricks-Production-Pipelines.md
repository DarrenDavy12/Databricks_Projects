# Production Pipeline Setup
## Objective: Deploy an ETL pipeline as a job with monitoring and permissions




##  Step 1: Reuse PySpark ETL code
df = spark.read.format("delta").load("/mnt/delta/nyc-taxi")
cleaned_df = df.filter(df.passenger_count.isNotNull() & (df.trip_distance > 0))
cleaned_df.write.mode("overwrite").format("delta").save("/mnt/delta/production-taxi")





## Step 2: Note: Job scheduling and permissions are set via UI

### - Created a job in Databricks UI, link to this notebook






### - Scheduled daily runs



### - Set permissions to restrict access to admin users



### - Monitored job runs via Databricks Workflows
