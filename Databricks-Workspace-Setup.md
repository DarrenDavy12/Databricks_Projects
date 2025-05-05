## Databricks Workspace Setup and Data Loading
#### Objective: Setted up a cluster, create a notebook, and load sample data

### Step 1: Created a cluster (done via UI, ensure it's running)


### Step 2: Loaded sample data (e.g., NYC Taxi dataset)

data_path = "dbfs:/databricks-datasets/nyctaxi/tripdata/yellow/yellow_tripdata_2019-01.csv.gz"
df = spark.read.csv(data_path, header=True, inferSchema=True)

![Image](https://github.com/user-attachments/assets/7257e16a-6b7a-46cc-b300-e36d45b37aa1)


### Step 3: Saved to DBFS

df.write.mode("overwrite").parquet("/mnt/sample-data/nyc-taxi")

![image](https://github.com/user-attachments/assets/c269ae81-8681-474b-814d-39668370259c)



### Step 4: Displayed sample data

display(df.limit(10))

![Image](https://github.com/user-attachments/assets/8bdc1976-31f9-4414-a606-fd526928e1f4)
