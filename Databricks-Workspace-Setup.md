## Databricks Workspace Setup and Data Loading
#### Objective: Setted up a cluster, create a notebook, and load sample data

### Step 1: Created a cluster (done via UI, ensure it's running)


### Step 2: Loaded sample data (e.g., NYC Taxi dataset)

data_path = "dbfs:/databricks-datasets/nyctaxi/tripdata/yellow/yellow_tripdata_2019-01.csv.gz"
df = spark.read.csv(data_path, header=True, inferSchema=True)

![Image](https://github.com/user-attachments/assets/1a16a661-bf55-4a6b-b477-061c032ef184)

### Step 3: Saved to DBFS

df.write.mode("overwrite").parquet("/mnt/sample-data/nyc-taxi")

![Image](https://github.com/user-attachments/assets/e27b82ba-2224-4a04-8786-f81e5e1e18f4)



### Step 4: Displayed sample data

display(df.limit(10))

![Image](https://github.com/user-attachments/assets/1e7e6abb-9ffc-457d-bc1b-f3dbe5d75ed0)
