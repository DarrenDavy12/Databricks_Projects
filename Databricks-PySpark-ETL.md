## PySpark ETL Pipeline
### Objective: Build an ETL pipeline using PySpark and Spark SQL

### Step 1: Load data
df = spark.read.format("delta").load("/mnt/delta/nyc-taxi")

### Step 2: Transform with PySpark
cleaned_df = df.filter(df.passenger_count.isNotNull() & (df.trip_distance > 0))
cleaned_df = cleaned_df.withColumn("fare_per_mile", df.total_amount / df.trip_distance)

### Step 3: Integrate with Spark SQL
cleaned_df.createOrReplaceTempView("temp_taxi")
transformed_df = spark.sql("""
SELECT *, fare_per_mile * 1.1 AS adjusted_fare_per_mile
FROM temp_taxi
""")

### Step 4: Save to Delta
transformed_df.write.mode("overwrite").format("delta").save("/mnt/delta/pyspark-transformed-taxi")
