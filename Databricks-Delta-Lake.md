## Delta Lake Table Management and Optimization
## Objective: Create, manage, and optimize a Delta table

### Prerequisite: Cluster setup

![Image](https://github.com/user-attachments/assets/6c06ed37-266e-497a-a4d7-c34d6093f1dd)


---

### Step 1: Created a Delta table using notebooks
df = spark.read.csv("dbfs:/databricks-datasets/nyctaxi/tripdata/yellow/yellow_tripdata_2019-01.csv.gz", header=True)
df.write.mode("overwrite").format("delta").save("/mnt/delta/nyc-taxi")


![image](https://github.com/user-attachments/assets/cc672d2a-7f5d-428f-90c0-68e78e8d74b3)


---

### Step 2: Updated the table
spark.sql("UPDATE delta.`/mnt/delta/nyc-taxi` SET passenger_count = 0 WHERE passenger_count IS NULL")

![Image](https://github.com/user-attachments/assets/0bc74283-2d81-41a0-b8ff-6ac7867a2f07)

---

### Step 3: Time travelled back to show old version 
old_version = spark.read.format("delta").option("versionAsOf", 0).load("/mnt/delta/nyc-taxi")
display(old_version.limit(10))

![image](https://github.com/user-attachments/assets/fd3334fc-a0aa-4702-933e-f8f1209f393b)


---

### Step 4: Optimize with Z-ordering to improve reading performance 
spark.sql("OPTIMIZE delta.`/mnt/delta/nyc-taxi` ZORDER BY (tpep_pickup_datetime)")

![image](https://github.com/user-attachments/assets/60fb9f96-9b51-448b-b541-9fc7e2e53ecf)
