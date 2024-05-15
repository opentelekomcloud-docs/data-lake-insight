:original_name: dli_09_0117.html

.. _dli_09_0117:

PySpark Example Code
====================

Development Description
-----------------------

Mongo can be connected only through enhanced datasource connections.

.. note::

   DDS is compatible with the MongoDB protocol.

-  Prerequisites

   An enhanced datasource connection has been created on the DLI management console and bound to a queue in packages.

   .. note::

      Hard-coded or plaintext passwords pose significant security risks. To ensure security, encrypt your passwords, store them in configuration files or environment variables, and decrypt them when needed.

-  Connecting to data sources through DataFrame APIs

   #. Import dependencies.

      .. code-block::

         from __future__ import print_function
         from pyspark.sql.types import StructType, StructField, IntegerType, StringType
         from pyspark.sql import SparkSession

   #. Create a session.

      ::

         sparkSession = SparkSession.builder.appName("datasource-mongo").getOrCreate()

   #. Set connection parameters.

      ::

         url = "192.168.4.62:8635,192.168.5.134:8635/test?authSource=admin"
         uri = "mongodb://username:pwd@host:8635/db"
         user = "rwuser"
         database = "test"
         collection = "test"
         password = "######"

      .. note::

         For details about the parameters, see :ref:`Table 1 <dli_09_0114__en-us_topic_0204096844_table2072415395012>`.

   #. Create a DataFrame.

      ::

         dataList = sparkSession.sparkContext.parallelize([(1, "Katie", 19),(2,"Tom",20)])
         schema = StructType([StructField("id", IntegerType(), False),
                              StructField("name", StringType(), False),
                              StructField("age", IntegerType(), False)])
         dataFrame = sparkSession.createDataFrame(dataList, schema)

   #. Import data to Mongo.

      ::

         dataFrame.write.format("mongo")
           .option("url", url)\
           .option("uri", uri)\
           .option("user",user)\
           .option("password",password)\
           .option("database",database)\
           .option("collection",collection)\
           .mode("Overwrite")\
           .save()

   #. Read data from Mongo.

      ::

         jdbcDF = sparkSession.read
           .format("mongo")\
           .option("url", url)\
           .option("uri", uri)\
           .option("user",user)\
           .option("password",password)\
           .option("database",database)\
           .option("collection",collection)\
           .load()
         jdbcDF.show()

   #. View the operation result.

      |image1|

-  Connecting to data sources through SQL APIs

   #. Create a table to connect to a Mongo data source.

      .. code-block::

         sparkSession.sql(
               "create table test_dds(id string, name string, age int) using mongo options(
               'url' = '192.168.4.62:8635,192.168.5.134:8635/test?authSource=admin',
               'uri' = 'mongodb://username:pwd@host:8635/db',
               'database' = 'test',
               'collection' = 'test',
               'user' = 'rwuser',
               'password' = '######')")

      .. note::

         For details about the parameters, see :ref:`Table 1 <dli_09_0114__en-us_topic_0204096844_table2072415395012>`.

   #. Insert data.

      ::

         sparkSession.sql("insert into test_dds values('3', 'Ann',23)")

   #. Query data.

      ::

         sparkSession.sql("select * from test_dds").show()

-  Submitting a Spark job

   #. Upload the Python code file to DLI.

   #. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

      .. note::

         -  If the Spark version is 2.3.2 (will be offline soon) or 2.4.5, specify the **Module** to **sys.datasource.mongo** when you submit a job.

         -  If the Spark version is 3.1.1, you do not need to select a module. Configure **Spark parameters (--conf)**.

            spark.driver.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/mongo/\*

            spark.executor.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/mongo/\*

Complete Example Code
---------------------

-  Connecting to data sources through DataFrame APIs

   .. code-block::

      from __future__ import print_function
      from pyspark.sql.types import StructType, StructField, IntegerType, StringType
      from pyspark.sql import SparkSession

      if __name__ == "__main__":
        # Create a SparkSession session.
        sparkSession = SparkSession.builder.appName("datasource-mongo").getOrCreate()

        # Create a DataFrame and initialize the DataFrame data.
        dataList = sparkSession.sparkContext.parallelize([("1", "Katie", 19),("2","Tom",20)])

        # Setting schema
        schema = StructType([StructField("id", IntegerType(), False),StructField("name", StringType(), False), StructField("age", IntegerType(), False)])

        # Create a DataFrame from RDD and schema
        dataFrame = sparkSession.createDataFrame(dataList, schema)

        # Setting connection parameters
        url = "192.168.4.62:8635,192.168.5.134:8635/test?authSource=admin"
        uri = "mongodb://username:pwd@host:8635/db"
        user = "rwuser"
        database = "test"
        collection = "test"
        password = "######"

        # Write data to the mongodb table
        dataFrame.write.format("mongo")
          .option("url", url)\
          .option("uri", uri)\
          .option("user",user)\
          .option("password",password)\
          .option("database",database)\
          .option("collection",collection)
          .mode("Overwrite").save()

        # Read data
        jdbcDF = sparkSession.read.format("mongo")
          .option("url", url)\
          .option("uri", uri)\
          .option("user",user)\
          .option("password",password)\
          .option("database",database)\
          .option("collection",collection)\
          .load()
        jdbcDF.show()

        # close session
        sparkSession.stop()

-  Connecting to data sources through SQL APIs

   .. code-block::

      from __future__ import print_function
      from pyspark.sql import SparkSession

      if __name__ == "__main__":
        # Create a SparkSession session.
        sparkSession = SparkSession.builder.appName("datasource-mongo").getOrCreate()

        # Create a data table for DLI - associated mongo
          sparkSession.sql(
            "create table test_dds(id string, name string, age int) using mongo options(\
            'url' = '192.168.4.62:8635,192.168.5.134:8635/test?authSource=admin',\
            'uri' = 'mongodb://username:pwd@host:8635/db',\
            'database' = 'test',\
            'collection' = 'test', \
            'user' = 'rwuser', \
            'password' = '######')")

        # Insert data into the DLI-table
        sparkSession.sql("insert into test_dds values('3', 'Ann',23)")

        # Read data from DLI-table
        sparkSession.sql("select * from test_dds").show()

        # close session
        sparkSession.stop()

.. |image1| image:: /_static/images/en-us_image_0223996999.png
