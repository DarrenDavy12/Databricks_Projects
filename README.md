# Databricks-Certification
topic-specific projects and end-to-end project



## Databricks-Workspace-Setup
### Databricks Workspace Setup and Data Loading
### Objective: Setted up a cluster, created a notebook, and loaded sample data

***

## Step 1: Created a cluster (done via UI, ensured it's running)


Inside Azure portal, I created a databricks workspace.

![Image](https://github.com/user-attachments/assets/a88f6a81-5806-422f-9a5f-ff971e009f6e)



Launched the workspace and created a cluster, showing that it is up and running.

![image](https://github.com/user-attachments/assets/cf216769-df1a-4111-afda-2b0d27ee31be)


## Step 2: Loaded sample data (e.g., NYC Taxi dataset)
data_path = "dbfs:/databricks-datasets/nyctaxi/tripdata/yellow/yellow_tripdata_2019-01.csv.gz"
df = spark.read.csv(data_path, header=True, inferSchema=True)


## Step 3: Saved to DBFS
df.write.mode("overwrite").parquet("/mnt/sample-data/nyc-taxi")



## Step 4: Displayed sample data
display(df.limit(10))
