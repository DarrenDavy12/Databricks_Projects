# Databricks-Certification
topic-specific projects and end-to-end project





# Databricks-Workspace-Setup

# Databricks Workspace Setup and Data Loading
# Objective: Set up a cluster, create a notebook, and load sample data

# Step 1: Create a cluster (done via UI, ensure it's running)
# Step 2: Load sample data (e.g., NYC Taxi dataset)
data_path = "dbfs:/databricks-datasets/nyctaxi/tripdata/yellow/yellow_tripdata_2019-01.csv.gz"
df = spark.read.csv(data_path, header=True, inferSchema=True)

# Step 3: Save to DBFS
df.write.mode("overwrite").parquet("/mnt/sample-data/nyc-taxi")

# Step 4: Display sample data
display(df.limit(10))
