# Step-1:

## pip install poetry

# Step-2:

### Create a new directory for your project
## mkdir iceberg_pyspark_project
## cd iceberg_pyspark_project

### Initialize a new Poetry project
## cd iceberg_pyspark_project
## poetry init

# Step-3:

## Create a virtual environment.
### python -m venv .venv
### source .venv/bin/activate

# Step-4:

##  Add PySpark as a development dependency
### poetry add --group=dev pyspark ipykernel


# Step-5: Setpu jupyter

#### Below command,

from pyspark.sql import SparkSession

# Initialize Spark session with Iceberg configurations
spark = SparkSession.builder \
  .appName("IcebergLocalDevelopment") \
  .config('spark.jars.packages', 'org.apache.iceberg:iceberg-spark-runtime-3.5_2.12:1.5.2') \
  .config("spark.sql.extensions", "org.apache.iceberg.spark.extensions.IcebergSparkSessionExtensions") \
  .config("spark.sql.catalog.local", "org.apache.iceberg.spark.SparkCatalog") \
  .config("spark.sql.catalog.local.type", "hadoop") \
  .config("spark.sql.catalog.local.warehouse", "spark-warehouse/iceberg") \
  .getOrCreate()

# Verify Spark session creation
spark.sql("SHOW DATABASES").show()

# Create an Iceberg table
spark.sql("""
  CREATE TABLE local.schema.users (
    id INT,
    name STRING,
    age INT
  ) USING iceberg""")

# Insert some sample data
spark.sql("""
  INSERT INTO local.schema.users VALUES
    (1, 'Alice', 30),
    (2, 'Bob', 25),
    (3, 'Charlie', 35)""")

# Query the data
result = spark.sql("SELECT * FROM local.schema.users")
result.show()

# Step-6: change  interpreter

## Get python interpreter path,
### poetry env info
#### /workspaces/ICEBERG/iceberg_pyspark_project/.venv
## Change jupyter notebook kernal