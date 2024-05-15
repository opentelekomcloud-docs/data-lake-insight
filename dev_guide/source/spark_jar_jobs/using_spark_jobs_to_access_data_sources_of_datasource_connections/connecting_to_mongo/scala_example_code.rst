:original_name: dli_09_0114.html

.. _dli_09_0114:

Scala Example Code
==================

Development Description
-----------------------

Mongo can be connected only through enhanced datasource connections.

.. note::

   DDS is compatible with the MongoDB protocol.

An enhanced datasource connection has been created on the DLI management console and bound to a queue in packages.

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

      .. code-block::

         import org.apache.spark.sql.SparkSession
         import org.apache.spark.sql.types.{IntegerType, StringType, StructField, StructType}

      Create a session.

      .. code-block::

         val sparkSession = SparkSession.builder().appName("datasource-mongo").getOrCreate()

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

      .. _dli_09_0114__en-us_topic_0204096844_table2072415395012:

      .. table:: **Table 1** Parameters for creating a table

         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                         | Description                                                                                                                                                                                    |
         +===================================+================================================================================================================================================================================================+
         | url                               | -  URL format:                                                                                                                                                                                 |
         |                                   |                                                                                                                                                                                                |
         |                                   |    "IP:PORT[,IP:PORT]/[DATABASE][.COLLECTION][AUTH_PROPERTIES]"                                                                                                                                |
         |                                   |                                                                                                                                                                                                |
         |                                   |    Example:                                                                                                                                                                                    |
         |                                   |                                                                                                                                                                                                |
         |                                   |    .. code-block::                                                                                                                                                                             |
         |                                   |                                                                                                                                                                                                |
         |                                   |       "192.168.4.62:8635/test?authSource=admin"                                                                                                                                                |
         |                                   |                                                                                                                                                                                                |
         |                                   | -  The URL needs to be obtained from the Mongo (DDS) connection address..                                                                                                                      |
         |                                   |                                                                                                                                                                                                |
         |                                   |    The obtained Mongo connection address is in the following format: **Protocol header://Username:Password\ @\ Connection address:Port number/Database name?authSource=admin**                 |
         |                                   |                                                                                                                                                                                                |
         |                                   |    Example:                                                                                                                                                                                    |
         |                                   |                                                                                                                                                                                                |
         |                                   |    .. code-block::                                                                                                                                                                             |
         |                                   |                                                                                                                                                                                                |
         |                                   |       mongodb://rwuser:****@192.168.4.62:8635,192.168.5.134:8635/test?authSource=admin                                                                                                         |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | uri                               | URI format: **mongodb://username:pwd@host:8635/db**                                                                                                                                            |
         |                                   |                                                                                                                                                                                                |
         |                                   | Set the following parameters to the actual values:                                                                                                                                             |
         |                                   |                                                                                                                                                                                                |
         |                                   | -  **username**: username used for creating the Mongo (DDS) database                                                                                                                           |
         |                                   | -  **pwd**: password of the username for the Mongo (DDS) database                                                                                                                              |
         |                                   | -  **host**: IP address of the Mongo (DDS) database instance                                                                                                                                   |
         |                                   | -  **db**: name of the created Mongo (DDS) database                                                                                                                                            |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | database                          | DDS database name. If the database name is specified in the URL, the database name in the URL does not take effect.                                                                            |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | collection                        | Collection name in the DDS. If the collection is specified in the URL, the collection in the URL does not take effect.                                                                         |
         |                                   |                                                                                                                                                                                                |
         |                                   | .. note::                                                                                                                                                                                      |
         |                                   |                                                                                                                                                                                                |
         |                                   |    If a collection already exists in DDS, you do not need to specify schema information when creating a table. DLI automatically generates schema information based on data in the collection. |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | user                              | Username for accessing the DDS cluster.                                                                                                                                                        |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | password                          | Password for accessing the DDS cluster.                                                                                                                                                        |
         +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   #. Insert data.

      .. code-block::

         sparkSession.sql("insert into test_dds values('3', 'Ann',23)")

   #. Query data.

      .. code-block::

         sparkSession.sql("select * from test_dds").show()

-  Connecting to data sources through DataFrame APIs

   #. Set connection parameters.

      .. code-block::

         val url = "192.168.4.62:8635,192.168.5.134:8635/test?authSource=admin"
         val uri = "mongodb://username:pwd@host:8635/db"
         val user = "rwuser"
         val database = "test"
         val collection = "test"
         val password = "######"

   #. Construct a schema.

      ::

         val schema = StructType(List(StructField("id", StringType), StructField("name", StringType), StructField("age", IntegerType)))

   #. Construct a DataFrame.

      .. code-block::

         val rdd = spark.sparkContext.parallelize(Seq(Row("1", "John", 23), Row("2", "Bob", 32)))
         val dataFrame = spark.createDataFrame(rdd, schema)

   #. Import data to Mongo.

      ::

         dataFrame.write.format("mongo")
           .option("url", url)
           .option("uri", uri)
           .option("database", database)
           .option("collection", collection)
           .option("user", user)
           .option("password", password)
           .mode(SaveMode.Overwrite)
           .save()

      .. note::

         The options of **mode** are **Overwrite**, **Append**, **ErrorIfExis**, and **Ignore**.

   #. Read data from Mongo.

      ::

         val jdbcDF = spark.read.format("mongo").schema(schema)
           .option("url", url)
           .option("uri", uri)
           .option("database", database)
           .option("collection", collection)
           .option("user", user)
           .option("password", password)
           .load()

      Operation result

      |image1|

-  Submitting a Spark job

   #. Generate a JAR package based on the code and upload the package to DLI.

   #. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

      .. note::

         -  If the Spark version is 2.3.2 (will be offline soon) or 2.4.5, specify the **Module** to **sys.datasource.mongo** when you submit a job.

         -  If the Spark version is 3.1.1, you do not need to select a module. Configure **Spark parameters (--conf)**.

            spark.driver.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/mongo/\*

            spark.executor.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/mongo/\*

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

      import org.apache.spark.sql.SparkSession

      object TestMongoSql {
        def main(args: Array[String]): Unit = {
          val sparkSession = SparkSession.builder().getOrCreate()
          sparkSession.sql(
            "create table test_dds(id string, name string, age int) using mongo options(
              'url' = '192.168.4.62:8635,192.168.5.134:8635/test?authSource=admin',
              'uri' = 'mongodb://username:pwd@host:8635/db',
              'database' = 'test',
              'collection' = 'test',
              'user' = 'rwuser',
              'password' = '######')")
          sparkSession.sql("insert into test_dds values('3', 'Ann',23)")
          sparkSession.sql("select * from test_dds").show()
          sparkSession.close()
        }
      }

-  Connecting to data sources through DataFrame APIs

   .. code-block::

      import org.apache.spark.sql.{Row, SaveMode, SparkSession}
      import org.apache.spark.sql.types.{IntegerType, StringType, StructField, StructType}

      object Test_Mongo_SparkSql {
        def main(args: Array[String]): Unit = {
        //  Create a SparkSession session.
        val spark = SparkSession.builder().appName("mongodbTest").getOrCreate()

        // Set the connection configuration parameters.
        val url = "192.168.4.62:8635,192.168.5.134:8635/test?authSource=admin"
        val uri = "mongodb://username:pwd@host:8635/db"
        val user = "rwuser"
        val database = "test"
        val collection = "test"
        val password = "######"

        // Setting up the schema
        val schema = StructType(List(StructField("id", StringType), StructField("name", StringType), StructField("age", IntegerType)))

        // Setting up the DataFrame
        val rdd = spark.sparkContext.parallelize(Seq(Row("1", "John", 23), Row("2", "Bob", 32)))
        val dataFrame = spark.createDataFrame(rdd, schema)


        // Write data to mongo
        dataFrame.write.format("mongo")
          .option("url", url)
          .option("uri", uri)
          .option("database", database)
          .option("collection", collection)
          .option("user", user)
          .option("password", password)
          .mode(SaveMode.Overwrite)
          .save()

        // Reading data from mongo
        val jdbcDF = spark.read.format("mongo").schema(schema)
          .option("url", url)
          .option("uri", uri)
          .option("database", database)
          .option("collection", collection)
          .option("user", user)
          .option("password", password)
          .load()
        jdbcDF.show()

        spark.close()
       }
      }

.. |image1| image:: /_static/images/en-us_image_0223996997.png
