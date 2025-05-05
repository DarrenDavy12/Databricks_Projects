## Databricks Workspace Setup
### Objective: Setted up a cluster, created a notebook, and loaded sample data

***

### Prerequisite: 

### I needed somewhere to store the dataset as any cloud storage so I chose azure for a blob storage container. 

### First I made a storage account. 
![Image](https://github.com/user-attachments/assets/fc39900a-6b1c-4c6c-a4c9-fdd14750452f)


--- 



### Step 1: Created storage account in azure. Made a container named 'demo' and imported the .csv file. 


![Image](https://github.com/user-attachments/assets/d99cb7ea-b192-47c2-bdc5-0aae4c4052d2)

![Image](https://github.com/user-attachments/assets/65b77b90-2b64-48f4-9659-83046d923668)

![Image](https://github.com/user-attachments/assets/98706bcf-ebb2-4d5a-8cea-9dc9f7674a70)


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




