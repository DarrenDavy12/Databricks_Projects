# Databricks-Certification
topic-specific projects and end-to-end project



## Databricks-Workspace-Setup
### Databricks Workspace Setup and Data Loading
### Objective: Setted up a cluster, created a notebook, and loaded sample data

***

## Step 1: Created a cluster (done via UI, ensured it's running)
I was using the community edition of databricks hence why not much configuration options in image below.

![image](https://github.com/user-attachments/assets/26af636f-ddae-4945-8772-713b184f2338)


Waited for my cluster to start up. 


It was successfully created.

![Image](https://github.com/user-attachments/assets/7beb373e-cf92-4352-bca9-17e3dfc73dc3)


--- 

Launched the workspace and created a cluster, showing that it is up and running.



## Step 2: Loaded sample data (e.g., NYC Taxi dataset)
data_path = "dbfs:/databricks-datasets/nyctaxi/tripdata/yellow/yellow_tripdata_2019-01.csv.gz"
df = spark.read.csv(data_path, header=True, inferSchema=True)


## Step 3: Saved to DBFS
df.write.mode("overwrite").parquet("/mnt/sample-data/nyc-taxi")



## Step 4: Displayed sample data
display(df.limit(10))
