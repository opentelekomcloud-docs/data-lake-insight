:original_name: dli_09_0097.html

.. _dli_09_0097:

PySpark Example Code
====================

Development Description
-----------------------

Redis supports only enhanced datasource connections.

-  Prerequisites

   An enhanced datasource connection has been created on the DLI management console and bound to a queue in packages.

   .. note::

      Hard-coded or plaintext passwords pose significant security risks. To ensure security, encrypt your passwords, store them in configuration files or environment variables, and decrypt them when needed.

-  Connecting to data sources through DataFrame APIs

   #. Import dependencies.

      ::

         from __future__ import print_function
         from pyspark.sql.types import StructType, StructField, IntegerType, StringType
         from pyspark.sql import SparkSession

   #. Create a session.

      ::

         sparkSession = SparkSession.builder.appName("datasource-redis").getOrCreate()

   #. Set connection parameters.

      ::

         host = "192.168.4.199"
         port = "6379"
         table = "person"
         auth = "@@@@@@"

   #. Create a DataFrame.

      a. Method 1:

         ::

            dataList = sparkSession.sparkContext.parallelize([(1, "Katie", 19),(2,"Tom",20)])
            schema = StructType([StructField("id", IntegerType(), False),
                                 StructField("name", StringType(), False),
                                 StructField("age", IntegerType(), False)])
            dataFrame = sparkSession.createDataFrame(dataList, schema)

      b. Method 2:

         ::

            jdbcDF = sparkSession.createDataFrame([(3,"Jack", 23)])
            dataFrame = jdbcDF.withColumnRenamed("_1", "id").withColumnRenamed("_2", "name").withColumnRenamed("_3", "age")

   #. Import data to Redis.

      ::

         dataFrame.write
           .format("redis")\
           .option("host", host)\
           .option("port", port)\
           .option("table", table)\
           .option("password", auth)\
           .mode("Overwrite")\
           .save()

      .. note::

         -  The options of **mode** are **Overwrite**, **Append**, **ErrorIfExis**, and **Ignore**.
         -  To specify a key, use **.option("key.column", "name")**. **name** indicates the column name.
         -  To save nested DataFrames, use **.option("model", "binary")**.
         -  If you need to specify the data expiration time, use **.option("ttl", 1000)**. The unit is second.

   #. Read data from Redis.

      ::

         sparkSession.read.format("redis").option("host", host).option("port", port).option("table", table).option("password", auth).load().show()

   #. View the operation result.

      |image1|

-  Connecting to data sources through SQL APIs

   #. Create a table to connect to a Redis data source.

      .. code-block::

         sparkSession.sql(
              "CREATE TEMPORARY VIEW person (name STRING, age INT) USING org.apache.spark.sql.redis OPTIONS (
              'host' = '192.168.4.199',
              'port' = '6379',
              'password' = '######',
              table  'person')".stripMargin)

   #. Insert data.

      ::

         sparkSession.sql("INSERT INTO TABLE person VALUES ('John', 30),('Peter', 45)".stripMargin)

   #. Query data.

      ::

         sparkSession.sql("SELECT * FROM person".stripMargin).collect().foreach(println)

-  Submitting a Spark job

   #. Upload the Python code file to DLI.

   #. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

      .. note::

         -  If the Spark version is 2.3.2 (will be offline soon) or 2.4.5, specify the **Module** to **sys.datasource.redis** when you submit a job.

         -  If the Spark version is 3.1.1, you do not need to select a module. Configure **Spark parameters (--conf)**.

            spark.driver.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/redis/\*

            spark.executor.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/redis/\*

Complete Example Code
---------------------

-  Connecting to data sources through DataFrame APIs

   ::

      # _*_ coding: utf-8 _*_
      from __future__ import print_function
      from pyspark.sql.types import StructType, StructField, IntegerType, StringType
      from pyspark.sql import SparkSession
      if __name__ == "__main__":
        # Create a SparkSession session.
        sparkSession = SparkSession.builder.appName("datasource-redis").getOrCreate()

        # Set cross-source connection parameters.
        host = "192.168.4.199"
        port = "6379"
        table = "person"
        auth = "######"

        # Create a DataFrame and initialize the DataFrame data.
        # *******   method noe   *********
        dataList = sparkSession.sparkContext.parallelize([(1, "Katie", 19),(2,"Tom",20)])
        schema = StructType([StructField("id", IntegerType(), False),StructField("name", StringType(), False),StructField("age", IntegerType(), False)])
        dataFrame_one = sparkSession.createDataFrame(dataList, schema)

        # ****** method two ******
        # jdbcDF = sparkSession.createDataFrame([(3,"Jack", 23)])
        # dataFrame = jdbcDF.withColumnRenamed("_1", "id").withColumnRenamed("_2", "name").withColumnRenamed("_3", "age")

        # Write data to the redis table
        dataFrame.write.format("redis").option("host", host).option("port", port).option("table", table).option("password", auth).mode("Overwrite").save()
        # Read data
        sparkSession.read.format("redis").option("host", host).option("port", port).option("table", table).option("password", auth).load().show()

        # close session
        sparkSession.stop()

-  Connecting to data sources through SQL APIs

   ::

      # _*_ coding: utf-8 _*_
      from __future__ import print_function
      from pyspark.sql import SparkSession

      if __name__ == "__main__":
        # Create a SparkSession
        sparkSession = SparkSession.builder.appName("datasource_redis").getOrCreate()

        sparkSession.sql(
          "CREATE TEMPORARY VIEW person (name STRING, age INT) USING org.apache.spark.sql.redis OPTIONS (\
          'host' = '192.168.4.199', \
          'port' = '6379',\
          'password' = '######',\
          'table'= 'person')".stripMargin);

        sparkSession.sql("INSERT INTO TABLE person VALUES ('John', 30),('Peter', 45)".stripMargin)

        sparkSession.sql("SELECT * FROM person".stripMargin).collect().foreach(println)

        # close session
        sparkSession.stop()

.. |image1| image:: /_static/images/en-us_image_0223997787.png
