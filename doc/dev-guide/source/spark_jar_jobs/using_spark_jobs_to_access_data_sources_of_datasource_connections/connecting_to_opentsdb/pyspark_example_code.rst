:original_name: dli_09_0081.html

.. _dli_09_0081:

PySpark Example Code
====================

Development Description
-----------------------

The CloudTable OpenTSDB and MRS OpenTSDB can be connected to DLI as data sources.

-  Prerequisites

   A datasource connection has been created on the DLI management console.

   .. note::

      Hard-coded or plaintext passwords pose significant security risks. To ensure security, encrypt your passwords, store them in configuration files or environment variables, and decrypt them when needed.

-  Code implementation

   #. Import dependency packages.

      ::

         from __future__ import print_function
         from pyspark.sql.types import StructType, StructField, StringType, LongType, DoubleType
         from pyspark.sql import SparkSession

   #. Create a session.

      ::

         sparkSession = SparkSession.builder.appName("datasource-opentsdb").getOrCreate()

   #. Create a table to connect to an OpenTSDB data source.

      ::

         sparkSession.sql("create table opentsdb_test using opentsdb options(
           'Host'='opentsdb-3xcl8dir15m58z3.cloudtable.com:4242',
           'metric'='ct_opentsdb',
           'tags'='city,location')")

      .. note::

         For details about the **Host**, **metric**, and **tags** parameters, see :ref:`Table 1 <dli_09_0065__en-us_topic_0190597601_table463015581831>`.

-  Connecting to data sources through SQL APIs

   #. Insert data.

      .. code-block::

         sparkSession.sql("insert into opentsdb_test values('aaa', 'abc', '2021-06-30 18:00:00', 30.0)")

   #. Query data.

      .. code-block::

         result = sparkSession.sql("SELECT * FROM opentsdb_test")

-  Connecting to data sources through DataFrame APIs

   #. Construct a schema.

      ::

         schema = StructType([StructField("location", StringType()),\
                              StructField("name", StringType()), \
                              StructField("timestamp", LongType()),\
                              StructField("value", DoubleType())])

   #. Set data.

      ::

         dataList = sparkSession.sparkContext.parallelize([("aaa", "abc", 123456L, 30.0)])

   #. Create a DataFrame.

      ::

         dataFrame = sparkSession.createDataFrame(dataList, schema)

   #. Import data to OpenTSDB.

      ::

         dataFrame.write.insertInto("opentsdb_test")

   #. Read data from OpenTSDB.

      ::

         jdbdDF = sparkSession.read
             .format("opentsdb")\
             .option("Host","opentsdb-3xcl8dir15m58z3.cloudtable.com:4242")\
             .option("metric","ctopentsdb")\
             .option("tags","city,location")\
             .load()
         jdbdDF.show()

-  Submitting a Spark job

   #. Upload the Python code file to DLI.

   #. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

      .. note::

         -  If the Spark version is 2.3.2 (will be offline soon) or 2.4.5, specify the **Module** to **sys.datasource.opentsdb** when you submit a job.

         -  If the Spark version is 3.1.1, you do not need to select a module. Configure **Spark parameters (--conf)**.

            spark.driver.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/opentsdb/\*

            spark.executor.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/opentsdb/\*

Complete Example Code
---------------------

-  Connecting to MRS OpenTSDB through SQL APIs

   .. code-block::

      # _*_ coding: utf-8 _*_
      from __future__ import print_function
      from pyspark.sql.types import StructType, StructField, StringType, LongType, DoubleType
      from pyspark.sql import SparkSession

      if __name__ == "__main__":
        # Create a SparkSession session.
        sparkSession = SparkSession.builder.appName("datasource-opentsdb").getOrCreate()


        # Create a DLI cross-source association opentsdb data table
        sparkSession.sql(\
          "create table opentsdb_test using opentsdb options(\
          'Host'='10.0.0.171:4242',\
          'metric'='cts_opentsdb',\
          'tags'='city,location')")

        sparkSession.sql("insert into opentsdb_test values('aaa', 'abc', '2021-06-30 18:00:00', 30.0)")

        result = sparkSession.sql("SELECT * FROM opentsdb_test")
        result.show()

        # close session
        sparkSession.stop()

-  Connecting to OpenTSDB through DataFrame APIs

   .. code-block::

      # _*_ coding: utf-8 _*_
      from __future__ import print_function
      from pyspark.sql.types import StructType, StructField, StringType, LongType, DoubleType
      from pyspark.sql import SparkSession

      if __name__ == "__main__":
        # Create a SparkSession session.
        sparkSession = SparkSession.builder.appName("datasource-opentsdb").getOrCreate()

        # Create a DLI cross-source association opentsdb data table
        sparkSession.sql(
          "create table opentsdb_test using opentsdb options(\
          'Host'='opentsdb-3xcl8dir15m58z3.cloudtable.com:4242',\
          'metric'='ct_opentsdb',\
          'tags'='city,location')")

        # Create a DataFrame and initialize the DataFrame data.
        dataList = sparkSession.sparkContext.parallelize([("aaa", "abc", 123456L, 30.0)])

        # Setting schema
        schema = StructType([StructField("location", StringType()),\
                             StructField("name", StringType()),\
                             StructField("timestamp", LongType()),\
                             StructField("value", DoubleType())])

        # Create a DataFrame from RDD and schema
        dataFrame = sparkSession.createDataFrame(dataList, schema)

        # Set cross-source connection parameters
        metric = "ctopentsdb"
        tags = "city,location"
        Host = "opentsdb-3xcl8dir15m58z3.cloudtable.com:4242"

        # Write data to the cloudtable-opentsdb
        dataFrame.write.insertInto("opentsdb_test")
        # ******* Opentsdb does not currently implement the ctas method to save data, so the save() method cannot be used.*******
        # dataFrame.write.format("opentsdb").option("Host", Host).option("metric", metric).option("tags", tags).mode("Overwrite").save()

        # Read data on CloudTable-OpenTSDB
        jdbdDF = sparkSession.read\
            .format("opentsdb")\
            .option("Host",Host)\
            .option("metric",metric)\
            .option("tags",tags)\
            .load()
        jdbdDF.show()

        # close session
        sparkSession.stop()
