:original_name: dli_09_0067.html

.. _dli_09_0067:

Scala Example Code
==================

Development Description
-----------------------

-  Prerequisites

   A datasource connection has been created and bound to a queue on the DLI management console.

   .. note::

      Hard-coded or plaintext passwords pose significant security risks. To ensure security, encrypt your passwords, store them in configuration files or environment variables, and decrypt them when needed.

-  Constructing dependency information and creating a Spark session

   #. Import dependencies.

      Maven dependency involved

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

-  Connecting to data sources through SQL APIs

   #. Create a table to connect to an RDS data source and set connection parameters.

      ::

         sparkSession.sql(
           "CREATE TABLE IF NOT EXISTS dli_to_rds USING JDBC OPTIONS (
              'url'='jdbc:mysql://to-rds-1174404209-cA37siB6.datasource.com:3306',  // Set this parameter to the actual URL.
              'dbtable'='test.customer',
              'user'='root',  // Set this parameter to the actual user.
              'password'='######',  // Set this parameter to the actual password.
              'driver'='com.mysql.jdbc.Driver')")

      .. _dli_09_0067__en-us_topic_0190647826_table127421320141311:

      .. table:: **Table 1** Parameters for creating a table

         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                         | Description                                                                                                                                                                                                                                                                                                                                                                                                                       |
         +===================================+===================================================================================================================================================================================================================================================================================================================================================================================================================================+
         | url                               | To obtain an RDS IP address, you need to create a datasource connection first. Refer to the *Data Lake Insight User Guide* for more information.                                                                                                                                                                                                                                                                                  |
         |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                   |
         |                                   | If you have created an enhanced datasource connection, use the internal network domain name or internal network address and the database port number provided by RDS to set up the connection. If MySQL is used, the format is **Protocol header://Internal IP address:Internal network port number**. If PostgreSQL is used, the format is **Protocol header://Internal IP address:Internal network port number/Database name**. |
         |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                   |
         |                                   | For example: **jdbc:mysql://192.168.0.193:3306** or **jdbc:postgresql://192.168.0.193:3306/postgres**.                                                                                                                                                                                                                                                                                                                            |
         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | dbtable                           | To connect to a MySQL cluster, enter **Database name.Table name**. To connect to a PostgreSQL cluster, enter **Mode name.Table name**.                                                                                                                                                                                                                                                                                            |
         |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                   |
         |                                   | .. note::                                                                                                                                                                                                                                                                                                                                                                                                                         |
         |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                   |
         |                                   |    If the database and table do not exist, create them first. Otherwise, the system reports an error and fails to run.                                                                                                                                                                                                                                                                                                            |
         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | user                              | RDS database username.                                                                                                                                                                                                                                                                                                                                                                                                            |
         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | password                          | RDS database password.                                                                                                                                                                                                                                                                                                                                                                                                            |
         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | driver                            | JDBC driver class name. To connect to a MySQL cluster, enter **com.mysql.jdbc.Driver**. To connect to a PostgreSQL cluster, enter **org.postgresql.Driver**.                                                                                                                                                                                                                                                                      |
         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | partitionColumn                   | One of the numeric fields that are required for concurrently reading data.                                                                                                                                                                                                                                                                                                                                                        |
         |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                   |
         |                                   | .. note::                                                                                                                                                                                                                                                                                                                                                                                                                         |
         |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                   |
         |                                   |    -  The **partitionColumn**, **lowerBound**, **upperBound**, and **numPartitions** parameters must be set at the same time.                                                                                                                                                                                                                                                                                                     |
         |                                   |    -  To improve the concurrent read performance, you are advised to use auto-increment columns.                                                                                                                                                                                                                                                                                                                                  |
         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | lowerBound                        | Minimum value of a column specified by **partitionColumn**. The value is contained in the returned result.                                                                                                                                                                                                                                                                                                                        |
         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | upperBound                        | Maximum value of a column specified by **partitionColumn**. The value is not contained in the returned result.                                                                                                                                                                                                                                                                                                                    |
         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | numPartitions                     | Number of concurrent read operations.                                                                                                                                                                                                                                                                                                                                                                                             |
         |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                   |
         |                                   | .. note::                                                                                                                                                                                                                                                                                                                                                                                                                         |
         |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                   |
         |                                   |    When data is read, **lowerBound** and **upperBound** are evenly allocated to each task to obtain data. Example:                                                                                                                                                                                                                                                                                                                |
         |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                   |
         |                                   |    'partitionColumn'='id',                                                                                                                                                                                                                                                                                                                                                                                                        |
         |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                   |
         |                                   |    'lowerBound'='0',                                                                                                                                                                                                                                                                                                                                                                                                              |
         |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                   |
         |                                   |    'upperBound'='100',                                                                                                                                                                                                                                                                                                                                                                                                            |
         |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                   |
         |                                   |    'numPartitions'='2'                                                                                                                                                                                                                                                                                                                                                                                                            |
         |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                   |
         |                                   |    Two concurrent tasks are started in DLI. The execution ID of one task is greater than or equal to **0** and the ID is smaller than **50**; the execution ID of the other task is greater than or equal to **50** and the ID is smaller than **100**.                                                                                                                                                                           |
         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | fetchsize                         | Number of data records obtained in each batch during data reading. The default value is **1000**. If this parameter is set to a large value, the performance is good but more memory is occupied, causing memory overflow as a result.                                                                                                                                                                                            |
         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | batchsize                         | Number of data records written in each batch. The default value is **1000**. If this parameter is set to a large value, the performance is good but more memory is occupied, causing memory overflow as a result.                                                                                                                                                                                                                 |
         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | truncate                          | Whether to clear the table without deleting the original table when **overwrite** is executed. The options are as follows:                                                                                                                                                                                                                                                                                                        |
         |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                   |
         |                                   | -  true                                                                                                                                                                                                                                                                                                                                                                                                                           |
         |                                   | -  false                                                                                                                                                                                                                                                                                                                                                                                                                          |
         |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                   |
         |                                   | The default value is **false**, indicating that the original table is deleted and then a new table is created when the **overwrite** operation is performed.                                                                                                                                                                                                                                                                      |
         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | isolationLevel                    | Transaction isolation level. The options are as follows:                                                                                                                                                                                                                                                                                                                                                                          |
         |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                   |
         |                                   | -  NONE                                                                                                                                                                                                                                                                                                                                                                                                                           |
         |                                   | -  READ_UNCOMMITTED                                                                                                                                                                                                                                                                                                                                                                                                               |
         |                                   | -  READ_COMMITTED                                                                                                                                                                                                                                                                                                                                                                                                                 |
         |                                   | -  REPEATABLE_READ                                                                                                                                                                                                                                                                                                                                                                                                                |
         |                                   | -  SERIALIZABLE                                                                                                                                                                                                                                                                                                                                                                                                                   |
         |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                   |
         |                                   | The default value is **READ_UNCOMMITTED**.                                                                                                                                                                                                                                                                                                                                                                                        |
         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   #. Insert data.

      ::

         sparkSession.sql("insert into dli_to_rds values(1, 'John',24),(2, 'Bob',32)")

   #. Query data.

      ::

         val dataFrame = sparkSession.sql("select * from dli_to_rds")
         dataFrame.show()

      Before data is inserted

      |image1|

      After data is inserted

      |image2|

   #. Delete the datasource connection table.

      ::

         sparkSession.sql("drop table dli_to_rds")

-  Connecting to data sources through DataFrame APIs

   #. Configure datasource connection parameters.

      ::

         val url = "jdbc:mysql://to-rds-1174405057-EA1Kgo8H.datasource.com:3306"
         val username = "root"
         val password = "######"
         val dbtable = "test.customer"

   #. Create a DataFrame, add data, and rename fields.

      ::

         var dataFrame_1 = sparkSession.createDataFrame(List((8, "Jack_1", 18)))
         val df = dataFrame_1.withColumnRenamed("_1", "id")
                             .withColumnRenamed("_2", "name")
                             .withColumnRenamed("_3", "age")

   #. Import data to RDS.

      ::

         df.write.format("jdbc")
           .option("url", url)
           .option("dbtable", dbtable)
           .option("user", username)
           .option("password", password)
           .option("driver", "com.mysql.jdbc.Driver")
           .mode(SaveMode.Append)
           .save()

      .. note::

         The value of **SaveMode** can be one of the following:

         -  **ErrorIfExis**: If the data already exists, the system throws an exception.
         -  **Overwrite**: If the data already exists, the original data will be overwritten.
         -  **Append**: If the data already exists, the system saves the new data.
         -  **Ignore**: If the data already exists, no operation is required. This is similar to the SQL statement **CREATE TABLE IF NOT EXISTS**.

   #. .. _dli_09_0067__en-us_topic_0190647826_li146260357585:

      Read data from RDS.

      -  Method 1: read.format()

         ::

            val jdbcDF = sparkSession.read.format("jdbc")
                            .option("url", url)
                            .option("dbtable", dbtable)
                            .option("user", username)
                            .option("password", password)
                            .option("driver", "org.postgresql.Driver")
                            .load()

      -  Method 2: read.jdbc()

         ::

            val properties = new Properties()
            properties.put("user", username)
            properties.put("password", password)
            val jdbcDF2 = sparkSession.read.jdbc(url, dbtable, properties)

      Before data is inserted

      |image3|

      After data is inserted

      |image4|

      The DataFrame read by the **read.format()** or **read.jdbc()** method is registered as a temporary table. Then, you can use SQL statements to query data.

      ::

         jdbcDF.registerTempTable("customer_test")
         sparkSession.sql("select * from customer_test where id = 1").show()

      Query results

      |image5|

-  DataFrame-related operations

   The data created by the **createDataFrame()** method and the data queried by the **read.format()** method and the **read.jdbc()** method are all DataFrame objects. You can directly query a single record. (In :ref:`4 <dli_09_0067__en-us_topic_0190647826_li146260357585>`, the DataFrame data is registered as a temporary table.)

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

      **selectExpr** is used to perform special processing on a field. For example, the **selectExpr** function can be used to change the field name. The following is an example:

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

-  Submitting a Spark job

   #. Generate a JAR package based on the code and upload the package to DLI.

   #. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

      .. note::

         -  If the Spark version is 2.3.2 (will be offline soon) or 2.4.5, specify the **Module** to **sys.datasource.rds** when you submit a job.

         -  If the Spark version is 3.1.1, you do not need to select a module. Configure **Spark parameters (--conf)**.

            spark.driver.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/rds/\*

            spark.executor.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/rds/\*

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

   ::

      import java.util.Properties
      import org.apache.spark.sql.SparkSession

      object Test_SQL_RDS {
        def main(args: Array[String]): Unit = {
          // Create a SparkSession session.
          val sparkSession = SparkSession.builder().getOrCreate()

          // Create a data table for DLI-associated RDS
          sparkSession.sql("CREATE TABLE IF NOT EXISTS dli_to_rds USING JDBC OPTIONS (
         'url'='jdbc:mysql://to-rds-1174404209-cA37siB6.datasource.com:3306,
            'dbtable'='test.customer',
            'user'='root',
            'password'='######',
                'driver'='com.mysql.jdbc.Driver')")

          //*****************************SQL model***********************************
          //Insert data into the DLI data table
          sparkSession.sql("insert into dli_to_rds values(1,'John',24),(2,'Bob',32)")

          //Read data from DLI data table
          val dataFrame = sparkSession.sql("select * from dli_to_rds")
          dataFrame.show()

          //drop table
          sparkSession.sql("drop table dli_to_rds")

          sparkSession.close()
        }
      }

-  Connecting to data sources through DataFrame APIs

   ::

      import java.util.Properties
      import org.apache.spark.sql.SparkSession
      import org.apache.spark.sql.SaveMode

      object Test_SQL_RDS {
        def main(args: Array[String]): Unit = {
          // Create a SparkSession session.
          val sparkSession = SparkSession.builder().getOrCreate()

          //*****************************DataFrame model***********************************
          // Set the connection configuration parameters. Contains url, username, password, dbtable.
          val url = "jdbc:mysql://to-rds-1174404209-cA37siB6.datasource.com:3306"
          val username = "root"
          val password = "######"
          val dbtable = "test.customer"

          // Create a DataFrame and initialize the DataFrame data.
          var dataFrame_1 = sparkSession.createDataFrame(List((1, "Jack", 18)))

          // Rename the fields set by the createDataFrame() method.
          val df = dataFrame_1.withColumnRenamed("_1", "id")
                          .withColumnRenamed("_2", "name")
                          .withColumnRenamed("_3", "age")

          // Write data to the rds_table_1 table
          df.write.format("jdbc")
            .option("url", url)
            .option("dbtable", dbtable)
            .option("user", username)
            .option("password", password)
            .option("driver", "com.mysql.jdbc.Driver")
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
          // Way one: Read data from RDS using read.format()
          val jdbcDF = sparkSession.read.format("jdbc")
                          .option("url", url)
                          .option("dbtable", dbtable)
                          .option("user", username)
                          .option("password", password)
                          .option("driver", "com.mysql.jdbc.Driver")
                          .load()
          // Way two: Read data from RDS using read.jdbc()
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

-  DataFrame-related operations

   ::

        // The where() method uses " and" and "or" for condition filters, returning filtered DataFrame objects
        jdbcDF.where("id = 1 or age <=10").show()

        // The filter() method is used in the same way as the where() method.
        jdbcDF.filter("id = 1 or age <=10").show()

        // The select() method passes multiple arguments and returns the DataFrame object of the specified field.
        jdbcDF.select("id").show()
        jdbcDF.select("id", "name").show()
        jdbcDF.select("id","name").where("id<4").show()

        /**
         * The selectExpr() method implements special handling of fields, such as renaming, increasing or
         * decreasing data values.
         */
        jdbcDF.selectExpr("id", "name as name_test", "age+1").show()

        // The col() method gets a specified field each time, and the return type is a Column type.
        val idCol = jdbcDF.col("id")

        /**
         * The drop() method returns a DataFrame object that does not contain deleted fields, and only one field
         * can be deleted at a time.
         */
        jdbcDF.drop("id").show()

.. |image1| image:: /_static/images/en-us_image_0223997410.png
.. |image2| image:: /_static/images/en-us_image_0223997411.png
.. |image3| image:: /_static/images/en-us_image_0223997412.png
.. |image4| image:: /_static/images/en-us_image_0223997413.png
.. |image5| image:: /_static/images/en-us_image_0223997414.png
.. |image6| image:: /_static/images/en-us_image_0223997415.png
.. |image7| image:: /_static/images/en-us_image_0223997416.png
.. |image8| image:: /_static/images/en-us_image_0223997417.png
.. |image9| image:: /_static/images/en-us_image_0223997418.png
.. |image10| image:: /_static/images/en-us_image_0223997419.png
.. |image11| image:: /_static/images/en-us_image_0223997420.png
