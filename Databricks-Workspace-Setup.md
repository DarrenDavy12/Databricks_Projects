## Databricks Workspace Setup
### Objective: Setted up a cluster, created a notebook, and loaded sample data

***


### Step 1: Created a cluster on databricks community edition 

![Image](https://github.com/user-attachments/assets/fcc7da1d-31c0-4340-b232-7f760c333526)

--- 


### Step 2: Loaded sample data (e.g., NYC Taxi dataset)
data_path = "dbfs:/databricks-datasets/nyctaxi/tripdata/yellow/yellow_tripdata_2019-01.csv.gz"
df = spark.read.csv(data_path, header=True, inferSchema=True)


![Image](https://github.com/user-attachments/assets/eca723e6-9d2a-4c02-9f95-77d6f9bae96c)

--- 




### Step 3: Saved to DBFS
df.write.mode("overwrite").parquet("/mnt/sample-data/nyc-taxi")

![image](https://github.com/user-attachments/assets/02b38aee-1ee6-4c7f-b253-b1bc21261946)





---

### Step 4: Displayed sample data
display(df.limit(10))

![image](https://github.com/user-attachments/assets/5ace930a-dc7d-42cc-b407-6c7152a940f8)




