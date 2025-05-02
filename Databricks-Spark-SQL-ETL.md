## Spark SQL ETL Pipeline
## Objective: Build an ETL pipeline using Spark SQL


### Step 1: Created a table
spark.sql("""
CREATE TABLE IF NOT EXISTS nyc_taxi
USING DELTA
LOCATION '/mnt/delta/nyc-taxi'
""")

---

### Step 2: Cleaned data (e.g., remove nulls)
spark.sql("""
CREATE OR REPLACE VIEW cleaned_taxi AS
SELECT *
FROM nyc_taxi
WHERE passenger_count IS NOT NULL AND trip_distance > 0
""")

---

### Step 3: Created a UDF
spark.sql("""
CREATE FUNCTION calculate_fare_per_mile(fare FLOAT, distance FLOAT)
RETURNS FLOAT
RETURN fare / distance
""")

---

### Step 4: Transformed data
spark.sql("""
SELECT *, calculate_fare_per_mile(total_amount, trip_distance) AS fare_per_mile
FROM cleaned_taxi
""").write.mode("overwrite").format("delta").save("/mnt/delta/transformed-taxi")

