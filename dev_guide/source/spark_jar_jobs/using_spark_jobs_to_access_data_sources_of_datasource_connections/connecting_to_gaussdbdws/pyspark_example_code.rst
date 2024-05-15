:original_name: dli_09_0087.html

.. _dli_09_0087:

PySpark Example Code
====================

Scenario
--------

This section provides PySpark example code that demonstrates how to use a Spark job to access data from the GaussDB(DWS) data source.

A datasource connection has been created and bound to a queue on the DLI management console.

.. note::

   Hard-coded or plaintext passwords pose significant security risks. To ensure security, encrypt your passwords, store them in configuration files or environment variables, and decrypt them when needed.

Preparations
------------

#. Import dependency packages.

   ::

      from __future__ import print_function
      from pyspark.sql.types import StructType, StructField, IntegerType, StringType
      from pyspark.sql import SparkSession

#. Create a session.

   ::

      sparkSession = SparkSession.builder.appName("datasource-dws").getOrCreate()

Accessing a Data Source Using a DataFrame API
---------------------------------------------

#. Set connection parameters.

   ::

      url = "jdbc:postgresql://to-dws-1174404951-W8W4cW8I.datasource.com:8000/postgres"
      dbtable = "customer"
      user = "dbadmin"
      password = "######"
      driver = "org.postgresql.Driver"

#. Set data.

   ::

      dataList = sparkSession.sparkContext.parallelize([(1, "Katie", 19)])

#. Configure the schema.

   ::

      schema = StructType([StructField("id", IntegerType(), False),\
                           StructField("name", StringType(), False),\
                           StructField("age", IntegerType(), False)])

#. Create a DataFrame.

   ::

      dataFrame = sparkSession.createDataFrame(dataList, schema)

#. Save the data to GaussDB(DWS).

   ::

      dataFrame.write \
          .format("jdbc") \
          .option("url", url) \
          .option("dbtable", dbtable) \
          .option("user", user) \
          .option("password", password) \
          .option("driver", driver) \
          .mode("Overwrite") \
          .save()

   .. note::

      The options of **mode** can be one of the following:

      -  **ErrorIfExis**: If the data already exists, the system throws an exception.
      -  **Overwrite**: If the data already exists, the original data will be overwritten.
      -  **Append**: If the data already exists, the system saves the new data.
      -  **Ignore**: If the data already exists, no operation is required. This is similar to the SQL statement **CREATE TABLE IF NOT EXISTS**.

#. Read data from GaussDB(DWS).

   ::

      jdbcDF = sparkSession.read \
          .format("jdbc") \
          .option("url", url) \
          .option("dbtable", dbtable) \
          .option("user", user) \
          .option("password", password) \
          .option("driver", driver) \
          .load()
      jdbcDF.show()

#. View the operation result.

   |image1|

Accessing a Data Source Using a SQL API
---------------------------------------

#. Create a table to connect to a GaussDB(DWS) data source.

   ::

      sparkSession.sql(
          "CREATE TABLE IF NOT EXISTS dli_to_dws USING JDBC OPTIONS (
          'url'='jdbc:postgresql://to-dws-1174404951-W8W4cW8I.datasource.com:8000/postgres',\
          'dbtable'='customer',\
          'user'='dbadmin',\
          'password'='######',\
          'driver'='org.postgresql.Driver')")

   .. note::

      For details about table creation parameters, see :ref:`Table 1 <dli_09_0069__table193741955203417>`.

#. Insert data.

   ::

      sparkSession.sql("insert into dli_to_dws values(2,'John',24)")

#. Query data.

   ::

      jdbcDF = sparkSession.sql("select * from dli_to_dws").show()

#. View the operation result.

   |image2|

Submitting a Spark Job
----------------------

#. Upload the Python code file to DLI.
#. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

   .. note::

      -  If the Spark version is 2.3.2 (will be offline soon) or 2.4.5, set **Module** to **sys.datasource.hbase** when you submit a job.

      -  If the Spark version is 3.1.1, you do not need to select a module. Set **Spark parameters (--conf)**.

         spark.driver.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/dws/\*

         spark.executor.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/dws/\*

Complete Example Code
---------------------

-  Connecting to data sources through DataFrame APIs

   .. note::

      Hard-coded or plaintext passwords pose significant security risks. To ensure security, encrypt your passwords, store them in configuration files or environment variables, and decrypt them when needed.

   ::

      # _*_ coding: utf-8 _*_
      from __future__ import print_function
      from pyspark.sql.types import StructType, StructField, IntegerType, StringType
      from pyspark.sql import SparkSession

      if __name__ == "__main__":
        # Create a SparkSession session.
        sparkSession = SparkSession.builder.appName("datasource-dws").getOrCreate()

        # Set cross-source connection parameters
        url = "jdbc:postgresql://to-dws-1174404951-W8W4cW8I.datasource.com:8000/postgres"
        dbtable = "customer"
        user = "dbadmin"
        password = "######"
        driver = "org.postgresql.Driver"

        # Create a DataFrame and initialize the DataFrame data.
        dataList = sparkSession.sparkContext.parallelize([(1, "Katie", 19)])

        # Setting schema
        schema = StructType([StructField("id", IntegerType(), False),\
                             StructField("name", StringType(), False),\
                             StructField("age", IntegerType(), False)])

        # Create a DataFrame from RDD and schema
        dataFrame = sparkSession.createDataFrame(dataList, schema)

        # Write data to the DWS table
        dataFrame.write \
            .format("jdbc") \
            .option("url", url) \
            .option("dbtable", dbtable) \
            .option("user", user) \
            .option("password", password) \
            .option("driver", driver) \
            .mode("Overwrite") \
            .save()

        # Read data
        jdbcDF = sparkSession.read \
            .format("jdbc") \
            .option("url", url) \
            .option("dbtable", dbtable) \
            .option("user", user) \
            .option("password", password) \
            .option("driver", driver) \
            .load()
        jdbcDF.show()

        # close session
        sparkSession.stop()

-  Connecting to data sources through SQL APIs

   ::

      # _*_ coding: utf-8 _*_
      from __future__ import print_function
      from pyspark.sql import SparkSession

      if __name__ == "__main__":
        # Create a SparkSession session.
        sparkSession = SparkSession.builder.appName("datasource-dws").getOrCreate()

        # Create a data table for DLI - associated GaussDB(DWS)
        sparkSession.sql(
            "CREATE TABLE IF NOT EXISTS dli_to_dws USING JDBC OPTIONS (\
            'url'='jdbc:postgresql://to-dws-1174404951-W8W4cW8I.datasource.com:8000/postgres',\
            'dbtable'='customer',\
            'user'='dbadmin',\
            'password'='######',\
            'driver'='org.postgresql.Driver')")

        # Insert data into the DLI data table
        sparkSession.sql("insert into dli_to_dws values(2,'John',24)")

        # Read data from DLI data table
        jdbcDF = sparkSession.sql("select * from dli_to_dws").show()

        # close session
        sparkSession.stop()

.. |image1| image:: /_static/images/en-us_image_0000001757793769.png
.. |image2| image:: /_static/images/en-us_image_0000001709994304.png
