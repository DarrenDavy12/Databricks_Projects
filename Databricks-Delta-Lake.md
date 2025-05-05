# Delta Lake Table Management and Optimization
# Objective: Simulate Delta table operations in a free trial environment with restricted DBFS

# Step 1: Load sample data
# Use a file uploaded to /FileStore/ (e.g., a small CSV like yellow_tripdata_2019-01_subset.csv)
data_path = "/FileStore/yellow_tripdata_2019-01_subset.csv"  # Update with your uploaded file path
df = spark.read.csv(data_path, header=True, inferSchema=True)

# Step 2: Create a temporary view (instead of a Delta table)
# Delta table writes to DBFS (e.g., /mnt/delta/nyc-taxi) are disabled in free trial
df.createOrReplaceTempView("nyc_taxi_temp")

# Step 3: Simulate an update with filtering
# Instead of UPDATE on a Delta table, use SQL to create a new DataFrame with modified data
updated_df = spark.sql("""
    SELECT *,
           COALESCE(passenger_count, 0) AS passenger_count
    FROM nyc_taxi_temp
""")
updated_df.createOrReplaceTempView("nyc_taxi_updated")

# Step 4: Simulate time travel
# Time travel requires Delta table versioning, which is not possible without DBFS writes
# Instead, display the original data (pretending it's version 0)
display(df.limit(10))

# Step 5: Simulate optimization
# OPTIMIZE and ZORDER require Delta tables, so we simulate by ordering the data
optimized_df = spark.sql("SELECT * FROM nyc_taxi_updated ORDER BY tpep_pickup_datetime")
display(optimized_df.limit(10))

# Optional: Export results as CSV to /FileStore/ (if allowed)
# Small-scale CSV writes may work; test with a small dataset
optimized_df.limit(100).write.mode("overwrite").csv("/FileStore/nyc_taxi_output.csv", header=True)
