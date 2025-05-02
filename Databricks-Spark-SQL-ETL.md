## Spark SQL ETL Pipeline
### Objective: Build an ETL pipeline using Spark SQL

### Prerequisite: Cluster setup

![Image](https://github.com/user-attachments/assets/6c06ed37-266e-497a-a4d7-c34d6093f1dd)


### Step 1: Created a table
spark.sql("""
CREATE TABLE IF NOT EXISTS nyc_taxi
USING DELTA
LOCATION '/mnt/delta/nyc-taxi'
""")


![Image](https://github.com/user-attachments/assets/388c2852-3ed6-4e7a-8c91-7b5b3dc435c3)


---

### Step 2: Cleaned data (e.g., remove nulls)
spark.sql("""
CREATE OR REPLACE VIEW cleaned_taxi AS
SELECT *
FROM nyc_taxi
WHERE passenger_count IS NOT NULL AND trip_distance > 0
""")

![Image](https://github.com/user-attachments/assets/49def860-9839-4c95-aad2-97428a5c45a5)


---

### Step 3: Created a UDF (function)
spark.sql("""
CREATE FUNCTION calculate_fare_per_mile(fare FLOAT, distance FLOAT)
RETURNS FLOAT
RETURN fare / distance
""")


![Image](https://github.com/user-attachments/assets/827bca01-a5b1-4f50-8a16-6cb8a6221a93)

Tip: Use `spark.sql (""" DROP FUNCTION <function_name> """)` to delete the function if error saying there is one that still exists, 
afterwards continue with 'CREATE FUNCTION' command . 



---

### Step 4: Transformed data
spark.sql("""
SELECT *, calculate_fare_per_mile(total_amount, trip_distance) AS fare_per_mile
FROM cleaned_taxi
""").write.mode("overwrite").format("delta").save("/mnt/delta/transformed-taxi")


![Image](https://github.com/user-attachments/assets/208a547d-4c1e-4a76-9854-99f7b8266723)
