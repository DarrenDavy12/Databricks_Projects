## PySpark ETL Pipeline
### Objective: Build an ETL pipeline using PySpark and Spark SQL




### Step 1: Load data
df = spark.read.format("delta").load("/mnt/delta/nyc-taxi")

![Image](https://github.com/user-attachments/assets/32a56d11-39b6-4ebf-96dc-40f4730702cd)



### Step 2: Transform with PySpark
cleaned_df = df.filter(df.passenger_count.isNotNull() & (df.trip_distance > 0))
cleaned_df = cleaned_df.withColumn("fare_per_mile", df.total_amount / df.trip_distance)


![Image](https://github.com/user-attachments/assets/6f9406ac-6307-4dc6-95f9-c9d077be2a64)


### Step 3: Integrate with Spark SQL
cleaned_df.createOrReplaceTempView("temp_taxi")
transformed_df = spark.sql("""
SELECT *, fare_per_mile * 1.1 AS adjusted_fare_per_mile
FROM temp_taxi
""")


![Image](https://github.com/user-attachments/assets/ca300563-dd8b-4825-afda-c272bb02e53f)



### Step 4: Save to Delta
transformed_df.write.mode("overwrite").format("delta").save("/mnt/delta/pyspark-transformed-taxi")


![Image](https://github.com/user-attachments/assets/71d29ba0-d9cc-4fa7-a679-34f83581fa8b)
