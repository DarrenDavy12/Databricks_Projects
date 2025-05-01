## Delta Lake Table Management and Optimization
## Objective: Create, manage, and optimize a Delta table

### Prerequisite: Cluster setup

![Image](https://github.com/user-attachments/assets/6c06ed37-266e-497a-a4d7-c34d6093f1dd)


---

### Step 1: Created a Delta table using notebooks
df = spark.read.csv("dbfs:/databricks-datasets/nyctaxi/tripdata/yellow/yellow_tripdata_2019-01.csv.gz", header=True)
df.write.mode("overwrite").format("delta").save("/mnt/delta/nyc-taxi")

![Image](https://github.com/user-attachments/assets/5f398429-4e5c-4ec1-a58e-94c59e2b52db)

---

### Step 2: Updated the table
spark.sql("UPDATE delta.`/mnt/delta/nyc-taxi` SET passenger_count = 0 WHERE passenger_count IS NULL")

---

### Step 3: Time travelled back to show old version 
old_version = spark.read.format("delta").option("versionAsOf", 0).load("/mnt/delta/nyc-taxi")
display(old_version.limit(10))

---

### Step 4: Optimize with Z-ordering to improve reading performance 
spark.sql("OPTIMIZE delta.`/mnt/delta/nyc-taxi` ZORDER BY (tpep_pickup_datetime)")
