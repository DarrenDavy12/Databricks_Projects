## Databricks - Cluster creation, notebooks & loading datasets
### Objective: Setted up a cluster, created a notebook, and loaded sample data

***


### Step 1: Created a cluster (done via UI, ensured it's running)
I was using the community edition of databricks hence why not much configuration options in image below.

![image](https://github.com/user-attachments/assets/26af636f-ddae-4945-8772-713b184f2338)


Waited for my cluster to start up. 


It was successfully created.

![Image](https://github.com/user-attachments/assets/7beb373e-cf92-4352-bca9-17e3dfc73dc3)


--- 


### Step 2: Loaded sample data (e.g., NYC Taxi dataset)
data_path = "dbfs:/databricks-datasets/nyctaxi/tripdata/yellow/yellow_tripdata_2019-01.csv.gz"
df = spark.read.csv(data_path, header=True, inferSchema=True)

![image](https://github.com/user-attachments/assets/6e0b76d9-1943-4608-af44-d05264783e32)



### Step 3: Saved to DBFS
df.write.mode("overwrite").parquet("/mnt/sample-data/nyc-taxi")

![image](https://github.com/user-attachments/assets/02b38aee-1ee6-4c7f-b253-b1bc21261946)



### Step 4: Displayed sample data
display(df.limit(10))

![image](https://github.com/user-attachments/assets/5ace930a-dc7d-42cc-b407-6c7152a940f8)



### Overview:


![Image](https://github.com/user-attachments/assets/3dcf3fd5-f622-46b7-9150-7f7a71b7d1d1)

