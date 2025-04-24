# Databricks-Certification
topic-specific projects and end-to-end project



# Databricks-Workspace-Setup


## Databricks Workspace Setup and Data Loading
## Objective: Setted up a cluster, created a notebook, and loaded sample data

## Step 1: Created a cluster (done via UI, ensured it's running)




## Step 2: Loaded sample data (e.g., NYC Taxi dataset)
data_path = "dbfs:/databricks-datasets/nyctaxi/tripdata/yellow/yellow_tripdata_2019-01.csv.gz"
df = spark.read.csv(data_path, header=True, inferSchema=True)


## Step 3: Saved to DBFS
df.write.mode("overwrite").parquet("/mnt/sample-data/nyc-taxi")



## Step 4: Displayed sample data
display(df.limit(10))
