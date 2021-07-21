---
date: 02-15-21
id: 31
path: ''
tags: []
title: Spark
type: note
---

# Spark and Python

```python
from pyspark.sql import SparkSession
from pyspark.sql.types import StructType, StructField, StringType, TimestampType, IntegerType

spark = SparkSession.builder.appName("MyCoolApp").getOrCreate()
# Define a data schema:
schema = StructType(fields=[
    StructField("timestamp", TimestampType(), True),
    StructField("remote_ip", StringType(), True),
    StructField("host", StringType(), True),
    StructField("status", IntegerType(), True),
])
# Create a dataframe:
df = spark.read.csv("requests.csv", schema=schema)
# We can just infer the schema from CSV, if there's a header row:
df = spark.read.csv("requests.csv", inferSchema=True, header=True)
# Load a bunch of files into a dataframe;
df = spark.read.load(["requests1.csv", "requests2.csv", "requests3.csv"], format="csv", schema=schema)
# Read in JSON:
df = spark.read.json(filename)
# May need to set the multiline option to true for a multi-line JSON file:
df = spark.read.option('multiline', 'true').json(filename)

# Show some info about the dataframe:
df.printSchema()
df.columns
df.describe().show()
df.show()
type(df["timestamp"])

# Rename a column:
df.withColumnRenamed("oldname", "newname")

# Get some data out of the frame:
df.select("timestamp").show()
df.select(["timestamp", "host", "status"]).show()

# Use SQL to filter data by pushing the data into a temporary view:
df.createOrReplaceTempView("requests")
results = spark.sql("SELECT * FROM requests WHERE host IS NULL")
results.show()

# Use the dataframe.filter() function.
# https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.DataFrame.filter.html#pyspark.sql.DataFrame.filter
df.filter("host is null").show()
df.filter("timestamp is not null").filter("host is null").show()
# Less-magical, refer to the dataframe column (may be named or index-based):
df.filter(df.status == 200).select(["timestamp", "remote_ip", "host"]).show()
# For >1 filter, enclose each filter in brackets:
df.filter( (df.status. == 200) & (df.remote_ip == "203.132.91.67") ).show()
# Use ~ as NOT operator, | as OR operator, & as AND operator
df.filter( (df.status == 200) & ~(df.remote_ip == "203.132.91.67") ).show()
# Gather query results into a list for further operations (shows other method of calling a dataframe column):
result = df.filter(df["status"] == 200).collect()
result[0].asDict()["host"]
# Use a column between function in conjunction with dataframe.where to filter timestamps between two values:
# Note that where() is just an alias of filter()
df.where(df["timestamp"].between("2021-02-16 08:00:00",  "2021-02-16 12:00:00"))

# Missing data can be handled with the na function:
df.na.drop(subset=["host"]).show()
df.filter("host is null").na.fill("UNKNOWN", subset=["host"]).show()

# Select defined columns only:
df.select(df.timestamp, df.host, df.path, df.method, df.status).show()
df.select("timestamp", "host", "path").show()

# Aggregation:
hosts = df.groupBy("host")
hosts.count().show()
# You can aggregrate on a dataframe without groups:
df.agg({"request_time": "avg"}).show()

# You can run a SQL function over all values:
from pyspark.sql.functions import countDistinct, avg
df.select(countDistinct("remote_ip")).show()
# Can alias column outputs:
df.select( avg("request_time").alias("Avg response time (μs)") ).show()
# Ordering:
df.orderBy("request_time").show()
# Descending order requires reference to the column desc() method:
df.orderBy( df["bytes_sent"].desc() ).select(["host", "bytes_sent"]).show()

# Date/time functions
from pyspark.sql.functions import dayofmonth, hour, dayofyear, month, year, weekofyear, format_number, date_format
# Create a new column, just containing year:
df.withColumn("year", year("timestamp").show()
# Create a new column called hour:
new_df = df.withColumn("hour", hour("timestamp")).filter("hour is not null").orderBy("hour")
# Group on the new column, then calculate the average for bytes_sent. Also reformat the number format and the column name.
new_df.groupBy("hour").mean("bytes_sent").select(["hour", format_number('avg(bytes_sent)', 2).alias('avg_bytes_sent')]).show()

# Create a calculated column
df = df.withColumn("mb_sent", df.bytes_sent / 1024 / 1024)
# Round off a float column, and give it a nicer alias
from pyspark.sql.functions import round
df.select(df.timestamp, df.host, df.path, df.method, df.status, round(df.mb_sent, 2).alias("MB_sent"))

# Use the regexp_extract function to extract the file extension from a string
from pyspark.sql.functions import regexp_extract 
df = df.withColumn("extension", regexp_extract(df.path, "\.[0-9a-z]+$", 0))
```

# References
* https://spark.apache.org/docs/latest/sql-programming-guide.html
* https://luminousmen.com/post/azure-blob-storage-with-pyspark
* https://docs.microsoft.com/en-us/azure/synapse-analytics/spark/apache-spark-development-using-notebooks?tabs=classical#read-a-csv-from-azure-blob-storage-as-a-spark-dataframe
* https://sparkbyexamples.com/pyspark/