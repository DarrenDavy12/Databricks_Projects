## Databricks Workspace Setup
### Objective: Setted up a cluster, created a notebook, and loaded sample data

***



### Step 1: Created a cluster (done via UI, ensured it's running)
I was using Azure databricks workspace through microsoft azure.

![Image](https://github.com/user-attachments/assets/1cd2fc80-24c6-42f9-a579-caec0824ed24)

![Image](https://github.com/user-attachments/assets/dce10c2f-ebb3-4251-998d-91ccb41efa71)



---

### Step 2: Created a folder called demo inside of the default workspace folder. 
This is where I created the notebook to load the sample data,
and the location in which the other notebooks will be kept too. 

![Image](https://github.com/user-attachments/assets/e194b2d5-b0d1-4797-a98b-fa31e4caffde)




--- 


### Step 3: Loaded sample data (e.g., NYC Taxi dataset)
data_path = "dbfs:/databricks-datasets/nyctaxi/tripdata/yellow/yellow_tripdata_2019-01.csv.gz"
df = spark.read.csv(data_path, header=True, inferSchema=True)


![image](https://github.com/user-attachments/assets/6e0b76d9-1943-4608-af44-d05264783e32)

--- 




### Step 4: Saved to DBFS
df.write.mode("overwrite").parquet("/mnt/sample-data/nyc-taxi")

![image](https://github.com/user-attachments/assets/02b38aee-1ee6-4c7f-b253-b1bc21261946)





---

### Step 5: Displayed sample data
display(df.limit(10))

![image](https://github.com/user-attachments/assets/5ace930a-dc7d-42cc-b407-6c7152a940f8)




