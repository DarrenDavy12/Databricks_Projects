## Delta Lake Table Management and Optimization
#### Objective: Create, manage, and optimize a Delta table

### Step 1: Create a Delta table

df = spark.read.csv("dbfs:/databricks-datasets/nyctaxi/tripdata/yellow/yellow_tripdata_2019-01.csv.gz", header=True)
df.write.mode("overwrite").format("delta").save("/mnt/delta/nyc-taxi")

![Image](https://github.com/user-attachments/assets/cdf59b9f-8274-45bf-b3c5-7700bece51d9)



### Step 2: Update table

spark.sql("UPDATE delta.`/mnt/delta/nyc-taxi` SET passenger_count = 0 WHERE passenger_count IS NULL")

![Image](https://github.com/user-attachments/assets/806976da-2168-4611-9287-b9ebd25aaeb8)



### Step 3: Time travel

old_version = spark.read.format("delta").option("versionAsOf", 0).load("/mnt/delta/nyc-taxi")
display(old_version.limit(10))


![Image](https://github.com/user-attachments/assets/49a01181-907f-4946-b8bf-43ad2f950b57)



### Step 4: Optimize with Z-ordering

spark.sql("OPTIMIZE delta.`/mnt/delta/nyc-taxi` ZORDER BY (tpep_pickup_datetime)")

![Image](https://github.com/user-attachments/assets/482aa4cf-bdfa-4a93-9d47-bca3394b2dae)
