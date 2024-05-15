:original_name: dli_09_0084.html

.. _dli_09_0084:

PySpark Example Code
====================

Development Description
-----------------------

-  Prerequisites

   A datasource connection has been created and bound to a queue on the DLI management console.

   .. note::

      Hard-coded or plaintext passwords pose significant security risks. To ensure security, encrypt your passwords, store them in configuration files or environment variables, and decrypt them when needed.

-  Code implementation

   #. Import dependency packages.

      ::

         from __future__ import print_function
         from pyspark.sql.types import StructType, StructField, IntegerType, StringType
         from pyspark.sql import SparkSession

   #. Create a session.

      ::

         sparkSession = SparkSession.builder.appName("datasource-rds").getOrCreate()

-  Connecting to data sources through DataFrame APIs

   #. Configure datasource connection parameters.

      ::

         url = "jdbc:mysql://to-rds-1174404952-ZgPo1nNC.datasource.com:3306"
         dbtable = "test.customer"
         user = "root"
         password = "######"
         driver = "com.mysql.jdbc.Driver"

      For details about the parameters, see :ref:`Table 1 <dli_09_0067__en-us_topic_0190647826_table127421320141311>`.

   #. Set data.

      ::

         dataList = sparkSession.sparkContext.parallelize([(123, "Katie", 19)])

   #. Configure the schema.

      ::

         schema = StructType([StructField("id", IntegerType(), False),\
                              StructField("name", StringType(), False),\
                              StructField("age", IntegerType(), False)])

   #. Create a DataFrame.

      ::

         dataFrame = sparkSession.createDataFrame(dataList, schema)

   #. Save data to RDS.

      ::

         dataFrame.write \
             .format("jdbc") \
             .option("url", url) \
             .option("dbtable", dbtable) \
             .option("user", user) \
             .option("password", password) \
             .option("driver", driver) \
             .mode("Append") \
             .save()

      .. note::

         The value of **mode** can be one of the following:

         -  **ErrorIfExis**: If the data already exists, the system throws an exception.
         -  **Overwrite**: If the data already exists, the original data will be overwritten.
         -  **Append**: If the data already exists, the system saves the new data.
         -  **Ignore**: If the data already exists, no operation is required. This is similar to the SQL statement **CREATE TABLE IF NOT EXISTS**.

   #. Read data from RDS.

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

-  Connecting to data sources through SQL APIs

   #. Create a table to connect to an RDS data source and set connection parameters.

      ::

         sparkSession.sql(
             "CREATE TABLE IF NOT EXISTS dli_to_rds USING JDBC OPTIONS (\
             'url'='jdbc:mysql://to-rds-1174404952-ZgPo1nNC.datasource.com:3306',\
             'dbtable'='test.customer',\
             'user'='root',\
             'password'='######',\
             'driver'='com.mysql.jdbc.Driver')")

      For details about the parameters for creating a table, see :ref:`Table 1 <dli_09_0067__en-us_topic_0190647826_table127421320141311>`.

   #. Insert data.

      ::

         sparkSession.sql("insert into dli_to_rds values(3,'John',24)")

   #. Query data.

      ::

         jdbcDF_after = sparkSession.sql("select * from dli_to_rds")
         jdbcDF_after.show()

   #. View the operation result.

      |image2|

-  Submitting a Spark job

   #. Upload the Python code file to DLI.

   #. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

   #. After the Spark job is created, click **Execute** in the upper right corner of the console to submit the job. If the message "Spark job submitted successfully." is displayed, the Spark job is successfully submitted. You can view the status and logs of the submitted job on the **Spark Jobs** page.

      .. note::

         -  The queue you select for creating a Spark job is the one bound when you create the datasource connection.

         -  If the Spark version is 2.3.2 (will be offline soon) or 2.4.5, specify the **Module** to **sys.datasource.rds** when you submit a job.

         -  If the Spark version is 3.1.1, you do not need to select a module. Configure **Spark parameters (--conf)**.

            spark.driver.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/rds/\*

            spark.executor.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/rds/\*

Complete Example Code
---------------------

.. note::

   If the following sample code is directly copied to the **.py** file, note that unexpected characters may exist after the backslashes (\\) in the file content. You need to delete the indentations or spaces after the backslashes (\\).

-  Connecting to data sources through DataFrame APIs

   .. code-block::

      # _*_ coding: utf-8 _*_
      from __future__ import print_function
      from pyspark.sql.types import StructType, StructField, IntegerType, StringType
      from pyspark.sql import SparkSession
      if __name__ == "__main__":
        # Create a SparkSession session.
        sparkSession = SparkSession.builder.appName("datasource-rds").getOrCreate()

        # Set cross-source connection parameters.
        url = "jdbc:mysql://to-rds-1174404952-ZgPo1nNC.datasource.com:3306"
        dbtable = "test.customer"
        user = "root"
        password = "######"
        driver = "com.mysql.jdbc.Driver"

        # Create a DataFrame and initialize the DataFrame data.
        dataList = sparkSession.sparkContext.parallelize([(123, "Katie", 19)])

        # Setting schema
        schema = StructType([StructField("id", IntegerType(), False),\
                             StructField("name", StringType(), False),\
                             StructField("age", IntegerType(), False)])

        # Create a DataFrame from RDD and schema
        dataFrame = sparkSession.createDataFrame(dataList, schema)

        # Write data to the RDS.
        dataFrame.write \
            .format("jdbc") \
            .option("url", url) \
            .option("dbtable", dbtable) \
            .option("user", user) \
            .option("password", password) \
            .option("driver", driver) \
            .mode("Append") \
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

   .. code-block::

      # _*_ coding: utf-8 _*_
      from __future__ import print_function
      from pyspark.sql import SparkSession

      if __name__ == "__main__":
        # Create a SparkSession session.
        sparkSession = SparkSession.builder.appName("datasource-rds").getOrCreate()

        # Create a data table for DLI - associated RDS
        sparkSession.sql(
             "CREATE TABLE IF NOT EXISTS dli_to_rds USING JDBC OPTIONS (\
             'url'='jdbc:mysql://to-rds-1174404952-ZgPo1nNC.datasource.com:3306',\
             'dbtable'='test.customer',\
             'user'='root',\
             'password'='######',\
             'driver'='com.mysql.jdbc.Driver')")

        # Insert data into the DLI data table
        sparkSession.sql("insert into dli_to_rds values(3,'John',24)")

        # Read data from DLI data table
        jdbcDF = sparkSession.sql("select * from dli_to_rds")
        jdbcDF.show()

        # close session
        sparkSession.stop()

.. |image1| image:: /_static/images/en-us_image_0223997424.png
.. |image2| image:: /_static/images/en-us_image_0223997425.png
