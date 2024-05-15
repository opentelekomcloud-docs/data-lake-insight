:original_name: dli_09_0069.html

.. _dli_09_0069:

Scala Example Code
==================

Scenario
--------

This section provides Scala example code that demonstrates how to use a Spark job to access data from the GaussDB(DWS) data source.

A datasource connection has been created and bound to a queue on the DLI management console.

.. note::

   Hard-coded or plaintext passwords pose significant security risks. To ensure security, encrypt your passwords, store them in configuration files or environment variables, and decrypt them when needed.

Preparations
------------

Constructing dependency information and creating a Spark session

#. Import dependencies

   Involved Maven dependency

   ::

      <dependency>
        <groupId>org.apache.spark</groupId>
        <artifactId>spark-sql_2.11</artifactId>
        <version>2.3.2</version>
      </dependency>

   Import dependency packages.

   ::

      import java.util.Properties
      import org.apache.spark.sql.{Row,SparkSession}
      import org.apache.spark.sql.SaveMode

#. Create a session.

   ::

      val sparkSession = SparkSession.builder().getOrCreate()

Accessing a Data Source Using a SQL API
---------------------------------------

#. Create a table to connect to a GaussDB(DWS) data source.

   ::

      sparkSession.sql(
        "CREATE TABLE IF NOT EXISTS dli_to_dws USING JDBC OPTIONS (
           'url'='jdbc:postgresql://to-dws-1174404209-cA37siB6.datasource.com:8000/postgres',
           'dbtable'='customer',
           'user'='dbadmin',
           'passwdauth'='######'// Name of the datasource authentication of the password type created on DLI. If datasource authentication is used, you do not need to set the username and password for the job.
      )"
      )

   .. _dli_09_0069__table193741955203417:

   .. table:: **Table 1** Parameters for creating a table

      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
      +===================================+=============================================================================================================================================================================================================================================================================================================================================================================================================================================================+
      | url                               | To obtain a GaussDB(DWS) IP address, you need to create a datasource connection first. Refer to *Data Lake Insight User Guide* for more information.                                                                                                                                                                                                                                                                                                        |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
      |                                   | After an enhanced datasource connection is created, you can use the JDBC connection string (intranet) provided by GaussDB(DWS) or the intranet IP address and port number to connect to GaussDB(DWS). The format is **protocol header://internal IP address:internal network port number/database name**, for example: **jdbc:postgresql://192.168.0.77:8000/postgres**. For details about how to obtain the value, see *GaussDB(DWS) cluster information*. |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
      |                                   | .. note::                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
      |                                   |    The GaussDB(DWS) IP address is in the following format: **protocol header://IP address:port number/database name**                                                                                                                                                                                                                                                                                                                                       |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
      |                                   |    Example:                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
      |                                   |    jdbc:postgresql://to-dws-1174405119-ihlUr78j.datasource.com:8000/postgres                                                                                                                                                                                                                                                                                                                                                                                |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
      |                                   |    If you want to connect to a database created in GaussDB(DWS), change **postgres** to the corresponding database name in this connection.                                                                                                                                                                                                                                                                                                                 |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | passwdauth                        | Name of datasource authentication of the password type created on DLI. If datasource authentication is used, you do not need to set the username and password for jobs.                                                                                                                                                                                                                                                                                     |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | dbtable                           | Tables in the PostgreSQL database.                                                                                                                                                                                                                                                                                                                                                                                                                          |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | partitionColumn                   | This parameter is used to set the numeric field used concurrently when data is read.                                                                                                                                                                                                                                                                                                                                                                        |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
      |                                   | .. note::                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
      |                                   |    -  The **partitionColumn**, **lowerBound**, **upperBound**, and **numPartitions** parameters must be set at the same time.                                                                                                                                                                                                                                                                                                                               |
      |                                   |    -  To improve the concurrent read performance, you are advised to use auto-increment columns.                                                                                                                                                                                                                                                                                                                                                            |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | lowerBound                        | Minimum value of a column specified by **partitionColumn**. The value is contained in the returned result.                                                                                                                                                                                                                                                                                                                                                  |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | upperBound                        | Maximum value of a column specified by **partitionColumn**. The value is not contained in the returned result.                                                                                                                                                                                                                                                                                                                                              |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | numPartitions                     | Number of concurrent read operations.                                                                                                                                                                                                                                                                                                                                                                                                                       |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
      |                                   | .. note::                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
      |                                   |    When data is read, **lowerBound** and **upperBound** are evenly allocated to each task to obtain data. Example:                                                                                                                                                                                                                                                                                                                                          |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
      |                                   |    'partitionColumn'='id',                                                                                                                                                                                                                                                                                                                                                                                                                                  |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
      |                                   |    'lowerBound'='0',                                                                                                                                                                                                                                                                                                                                                                                                                                        |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
      |                                   |    'upperBound'='100',                                                                                                                                                                                                                                                                                                                                                                                                                                      |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
      |                                   |    'numPartitions'='2'                                                                                                                                                                                                                                                                                                                                                                                                                                      |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
      |                                   |    Two concurrent tasks are started in DLI. The execution ID of one task is greater than or equal to **0** and the ID is less than **50**, and the execution ID of the other task is greater than or equal to **50** and the ID is less than **100**.                                                                                                                                                                                                       |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | fetchsize                         | Number of data records obtained in each batch during data reading. The default value is **1000**. If this parameter is set to a large value, the performance is good but more memory is occupied. If this parameter is set to a large value, memory overflow may occur.                                                                                                                                                                                     |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | batchsize                         | Number of data records written in each batch. The default value is **1000**. If this parameter is set to a large value, the performance is good but more memory is occupied. If this parameter is set to a large value, memory overflow may occur.                                                                                                                                                                                                          |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | truncate                          | Indicates whether to clear the table without deleting the original table when **overwrite** is executed. The options are as follows:                                                                                                                                                                                                                                                                                                                        |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
      |                                   | -  true                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
      |                                   | -  false                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
      |                                   | The default value is **false**, indicating that the original table is deleted and then a new table is created when the **overwrite** operation is performed.                                                                                                                                                                                                                                                                                                |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | isolationLevel                    | Transaction isolation level. The options are as follows:                                                                                                                                                                                                                                                                                                                                                                                                    |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
      |                                   | -  NONE                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
      |                                   | -  READ_UNCOMMITTED                                                                                                                                                                                                                                                                                                                                                                                                                                         |
      |                                   | -  READ_COMMITTED                                                                                                                                                                                                                                                                                                                                                                                                                                           |
      |                                   | -  REPEATABLE_READ                                                                                                                                                                                                                                                                                                                                                                                                                                          |
      |                                   | -  SERIALIZABLE                                                                                                                                                                                                                                                                                                                                                                                                                                             |
      |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
      |                                   | The default value is **READ_UNCOMMITTED**.                                                                                                                                                                                                                                                                                                                                                                                                                  |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Insert data

   ::

      sparkSession.sql("insert into dli_to_dws values(1, 'John',24),(2, 'Bob',32)")

#. Query data

   ::

      val dataFrame = sparkSession.sql("select * from dli_to_dws")
      dataFrame.show()

   Before data is inserted:

   |image1|

   Response:

   |image2|

#. Delete the datasource connection table.

   ::

      sparkSession.sql("drop table dli_to_dws")

.. _dli_09_0069__section519052144120:

Accessing a Data Source Using a DataFrame API
---------------------------------------------

#. Set connection parameters.

   ::

      val url = "jdbc:postgresql://to-dws-1174405057-EA1Kgo8H.datasource.com:8000/postgres"
      val username = "dbadmin"
      val password = "######"
      val dbtable = "customer"

#. Create a DataFrame, add data, and rename fields

   ::

      var dataFrame_1 = sparkSession.createDataFrame(List((8, "Jack_1", 18)))
      val df = dataFrame_1.withColumnRenamed("_1", "id")
                          .withColumnRenamed("_2", "name")
                          .withColumnRenamed("_3", "age")

#. Import data to GaussDB(DWS).

   ::

      df.write.format("jdbc")
        .option("url", url)
        .option("dbtable", dbtable)
        .option("user", username)
        .option("password", password)
        .mode(SaveMode.Append)
        .save()

   .. note::

      The options of **SaveMode** can be one of the following:

      -  **ErrorIfExis**: If the data already exists, the system throws an exception.
      -  **Overwrite**: If the data already exists, the original data will be overwritten.
      -  **Append**: If the data already exists, the system saves the new data.
      -  **Ignore**: If the data already exists, no operation is required. This is similar to the SQL statement **CREATE TABLE IF NOT EXISTS**.

#. Read data from GaussDB(DWS).

   -  Method 1: read.format()

      ::

         val jdbcDF = sparkSession.read.format("jdbc")
                          .option("url", url)
                          .option("dbtable", dbtable)
                          .option("user", username)
                          .option("password", password)
                          .load()

   -  Method 2: read.jdbc()

      ::

         val properties = new Properties()
          properties.put("user", username)
          properties.put("password", password)
          val jdbcDF2 = sparkSession.read.jdbc(url, dbtable, properties)

   Before data is inserted:

   |image3|

   Response:

   |image4|

   The dateFrame read by the **read.format()** or **read.jdbc()** method is registered as a temporary table. Then, you can use SQL statements to query data.

   ::

      jdbcDF.registerTempTable("customer_test")
       sparkSession.sql("select * from customer_test where id = 1").show()

   Query results

   |image5|

DataFrame-Related Operations
----------------------------

The data created by the **createDataFrame()** method and the data queried by the **read.format()** method and the **read.jdbc()** method are all DataFrame objects. You can directly query a single record. (In :ref:`Accessing a Data Source Using a DataFrame API <dli_09_0069__section519052144120>`, the DataFrame data is registered as a temporary table.)

-  where

   The **where** statement can be combined with filter expressions such as AND and OR. The DataFrame object after filtering is returned. The following is an example:

   ::

      jdbcDF.where("id = 1 or age <=10").show()

   |image6|

-  filter

   The **filter** statement can be used in the same way as **where**. The DataFrame object after filtering is returned. The following is an example:

   ::

      jdbcDF.filter("id = 1 or age <=10").show()

   |image7|

-  select

   The **select** statement is used to query the DataFrame object of the specified field. Multiple fields can be queried.

   -  Example 1:

      ::

         jdbcDF.select("id").show()

      |image8|

   -  Example 2:

      ::

         jdbcDF.select("id", "name").show()

      |image9|

   -  Example 3:

      ::

         jdbcDF.select("id","name").where("id<4").show()

      |image10|

-  selectExpr

   The **selectExpr** statement is used to perform special processing on a field. For example, it can be used to change the field name. The following is an example:

   If you want to set the **name** field to **name_test** and add 1 to the value of **age**, run the following statement:

   ::

      jdbcDF.selectExpr("id", "name as name_test", "age+1").show()

-  col

   **col** is used to obtain a specified field. Different from **select**, **col** can only be used to query the column type and one field can be returned at a time. The following is an example:

   ::

      val idCol = jdbcDF.col("id")

-  drop

   **drop** is used to delete a specified field. Specify a field you need to delete (only one field can be deleted at a time), the DataFrame object that does not contain the field is returned. The following is an example:

   ::

      jdbcDF.drop("id").show()

   |image11|

Submitting a Job
----------------

#. Generate a JAR file based on the code and upload the file to DLI.

#. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

Complete Example Code
---------------------

-  Maven dependency

   ::

      <dependency>
        <groupId>org.apache.spark</groupId>
        <artifactId>spark-sql_2.11</artifactId>
        <version>2.3.2</version>
      </dependency>

-  Connecting to data sources through SQL APIs

   .. note::

      Hard-coded or plaintext passwords pose significant security risks. To ensure security, encrypt your passwords, store them in configuration files or environment variables, and decrypt them when needed.

   ::

      import java.util.Properties
      import org.apache.spark.sql.SparkSession

      object Test_SQL_DWS {
        def main(args: Array[String]): Unit = {
          // Create a SparkSession session.
          val sparkSession = SparkSession.builder().getOrCreate()
          // Create a data table for DLI-associated DWS
          sparkSession.sql("CREATE TABLE IF NOT EXISTS dli_to_dws USING JDBC OPTIONS (
            'url'='jdbc:postgresql://to-dws-1174405057-EA1Kgo8H.datasource.com:8000/postgres',
            'dbtable'='customer',
            'user'='dbadmin',
            'password'='######')")

          //*****************************SQL model***********************************
          //Insert data into the DLI data table
          sparkSession.sql("insert into dli_to_dws values(1,'John',24),(2,'Bob',32)")

          //Read data from DLI data table
          val dataFrame = sparkSession.sql("select * from dli_to_dws")
          dataFrame.show()

          //drop table
          sparkSession.sql("drop table dli_to_dws")

          sparkSession.close()
        }
      }

-  Connecting to data sources through DataFrame APIs

   .. note::

      Hard-coded or plaintext passwords pose significant security risks. To ensure security, encrypt your passwords, store them in configuration files or environment variables, and decrypt them when needed.

   ::

      import java.util.Properties
      import org.apache.spark.sql.SparkSession
      import org.apache.spark.sql.SaveMode

      object Test_SQL_DWS {
        def main(args: Array[String]): Unit = {
          // Create a SparkSession session.
          val sparkSession = SparkSession.builder().getOrCreate()

          //*****************************DataFrame model***********************************
          // Set the connection configuration parameters. Contains url, username, password, dbtable.
          val url = "jdbc:postgresql://to-dws-1174405057-EA1Kgo8H.datasource.com:8000/postgres"
          val username = "dbadmin"
          val password = "######"
          val dbtable = "customer"

          //Create a DataFrame and initialize the DataFrame data.
          var dataFrame_1 = sparkSession.createDataFrame(List((1, "Jack", 18)))

          //Rename the fields set by the createDataFrame() method.
          val df = dataFrame_1.withColumnRenamed("_1", "id")
                          .withColumnRenamed("_2", "name")
                          .withColumnRenamed("_3", "age")

          //Write data to the dws_table_1 table
          df.write.format("jdbc")
            .option("url", url)
            .option("dbtable", dbtable)
            .option("user", username)
            .option("password", password)
            .mode(SaveMode.Append)
            .save()

          // DataFrame object for data manipulation
          //Filter users with id=1
          var newDF = df.filter("id!=1")
          newDF.show()

          // Filter the id column data
          var newDF_1 = df.drop("id")
          newDF_1.show()

          // Read the data of the customer table in the RDS database
          //Way one: Read data from GaussDB(DWS) using read.format()
          val jdbcDF = sparkSession.read.format("jdbc")
                          .option("url", url)
                          .option("dbtable", dbtable)
                          .option("user", username)
                          .option("password", password)
                          .option("driver", "org.postgresql.Driver")
                          .load()
          //Way two: Read data from GaussDB(DWS) using read.jdbc()
          val properties = new Properties()
          properties.put("user", username)
          properties.put("password", password)
          val jdbcDF2 = sparkSession.read.jdbc(url, dbtable, properties)

          /**
           * Register the dateFrame read by read.format() or read.jdbc() as a temporary table, and query the data
           * using the sql statement.
           */
          jdbcDF.registerTempTable("customer_test")
          val result = sparkSession.sql("select * from customer_test where id = 1")
          result.show()

          sparkSession.close()
        }
      }

.. |image1| image:: /_static/images/en-us_image_0223997003.png
.. |image2| image:: /_static/images/en-us_image_0223997004.png
.. |image3| image:: /_static/images/en-us_image_0000001757887441.png
.. |image4| image:: /_static/images/en-us_image_0000001710007784.png
.. |image5| image:: /_static/images/en-us_image_0000001757807269.png
.. |image6| image:: /_static/images/en-us_image_0000001709848312.png
.. |image7| image:: /_static/images/en-us_image_0000001757887457.png
.. |image8| image:: /_static/images/en-us_image_0000001710007804.png
.. |image9| image:: /_static/images/en-us_image_0000001757807293.png
.. |image10| image:: /_static/images/en-us_image_0000001709848328.png
.. |image11| image:: /_static/images/en-us_image_0000001757887477.png
