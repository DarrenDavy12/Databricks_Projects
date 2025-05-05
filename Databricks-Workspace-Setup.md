## Databricks Workspace Setup and Data Loading
#### Objective: Set up a cluster, create a notebook, and load/display sample data in a free trial environment



### Step 1: Created a cluster (done via UI, ensure it's running)




### Step 2: Loaded sample data
### Used a file uploaded to /FileStore/ (e.g., a small CSV like yellow_tripdata_2019-01_subset.csv)

data_path = "/FileStore/yellow_tripdata_2019-01_subset.csv"  # Update with your uploaded file path
df = spark.read.csv(data_path, header=True, inferSchema=True)



![Image](https://github.com/user-attachments/assets/0adc5d42-9c8b-40f9-a70f-1a431dd9b36e)



### Step 3: Avoided writing to DBFS
### Instead of saving to Parquet, created a temporary view for further processing

df.createOrReplaceTempView("nyc_taxi_temp")


![Image](https://github.com/user-attachments/assets/b28b6df1-f3fe-4a88-bd91-c1525a2fa5f1)



### Step 4: Displayed sample data

display(df.limit(10))


![Image](https://github.com/user-attachments/assets/f7689e14-5893-4d74-b784-bfa5b877ab76)



### Optional: Queried the temporary view to verify

spark.sql("SELECT * FROM nyc_taxi_temp LIMIT 10").show()


![Image](https://github.com/user-attachments/assets/da98a439-4ef9-4be9-ba7c-3601844fe862)





