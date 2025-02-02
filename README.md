# Spark Data Analysis: Home Sales

This project demonstrates how to use Apache Spark for data analysis by processing a dataset of home sales using PySpark. The analysis includes various queries on home price trends and optimizations like caching and partitioning.

## Prerequisites

- Python 3.x
- Apache Spark 3.x
- Java 11
- PySpark

## Setup

### Installation

1. Install Java 11 and Apache Spark:
    ```bash
    !apt-get update
    !apt-get install openjdk-11-jdk-headless -qq > /dev/null
    !wget -q http://www.apache.org/dist/spark/$SPARK_VERSION/$SPARK_VERSION-bin-hadoop3.tgz
    !tar xf $SPARK_VERSION-bin-hadoop3.tgz
    !pip install -q findspark
    ```

2. Set environment variables for Spark and Java:
    ```python
    import os
    os.environ["JAVA_HOME"] = "/usr/lib/jvm/java-11-openjdk-amd64"
    os.environ["SPARK_HOME"] = f"/content/spark-3.4.4-bin-hadoop3"
    ```

3. Start a Spark session:
    ```python
    from pyspark.sql import SparkSession
    spark = SparkSession.builder.appName("SparkSQL").getOrCreate()
    ```

## Data Analysis

1. **Load Data from CSV**:
    ```python
    url = "https://2u-data-curriculum-team.s3.amazonaws.com/dataviz-classroom/v1.2/22-big-data/home_sales_revised.csv"
    spark.sparkContext.addFile(url)
    df = spark.read.csv("file://" + SparkFiles.get("home_sales_revised.csv"), header=True, inferSchema=True)
    df.createOrReplaceTempView("home_sales")
    ```

2. **Run SQL Queries on Data**:
    - **Query 1**: Average price of four-bedroom homes sold per year.
    - **Query 2**: Average price of three-bedroom, three-bathroom homes sold per year.
    - **Query 3**: Average price of homes with specific features (3-bed, 3-bath, 2-floors, ≥ 2000 sqft).
    - **Query 4**: Average price of homes with a view rating, price ≥ $350,000.

## Performance Optimization

1. **Cache the Data**:
    ```python
    spark.catalog.cacheTable("home_sales")
    ```

2. **Partition and Write Data to Parquet**:
    ```python
    df.write.partitionBy("date_built").mode("overwrite").parquet("/tmp/home_sales_parquet")
    ```

3. **Re-run Queries on Cached and Partitioned Data**.


