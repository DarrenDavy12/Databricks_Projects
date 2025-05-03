## PySpark ETL Pipeline
### Objective: Build an ETL pipeline using PySpark and Spark SQL

### Prerequisite: Cluster setup

![Image](https://github.com/user-attachments/assets/6c06ed37-266e-497a-a4d7-c34d6093f1dd)



### Step 1: Load data
df = spark.read.format("delta").load("/mnt/delta/nyc-taxi")

![Image](https://github.com/user-attachments/assets/f2fc4bb5-05f1-4f7f-925a-84903d185d7f)



### Step 2: Transform with PySpark
cleaned_df = df.filter(df.passenger_count.isNotNull() & (df.trip_distance > 0))
cleaned_df = cleaned_df.withColumn("fare_per_mile", df.total_amount / df.trip_distance)


![Image](https://github.com/user-attachments/assets/a09a6daf-40ba-4591-9944-8cdd73adaaef)



### Step 3: Integrate with Spark SQL
cleaned_df.createOrReplaceTempView("temp_taxi")
transformed_df = spark.sql("""
SELECT *, fare_per_mile * 1.1 AS adjusted_fare_per_mile
FROM temp_taxi
""")

![Image](https://github.com/user-attachments/assets/354d55a0-6a2a-42ec-ad99-85ff45d39457)


### Step 4: Save to Delta
transformed_df.write.mode("overwrite").format("delta").save("/mnt/delta/pyspark-transformed-taxi")


