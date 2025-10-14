:original_name: dli_09_0061.html

.. _dli_09_0061:

Scala Example Code
==================

Prerequisites
-------------

A datasource connection has been created on the DLI management console.

CSS Non-Security Cluster
------------------------

-  Development description

   -  Constructing dependency information and creating a Spark session

      #. Import dependencies.

         Maven dependency

         ::

            <dependency>
              <groupId>org.apache.spark</groupId>
              <artifactId>spark-sql_2.11</artifactId>
              <version>2.3.2</version>
            </dependency>

         Import dependency packages.

         ::

            import org.apache.spark.sql.{Row, SaveMode, SparkSession}
            import org.apache.spark.sql.types.{IntegerType, StringType, StructField, StructType}

      #. Create a session.

         ::

            val sparkSession = SparkSession.builder().getOrCreate()

   -  Connecting to data sources through SQL APIs

      #. Create a table to connect to a CSS data source.

         ::

            sparkSession.sql("create table css_table(id int, name string) using css options(
                'es.nodes' 'to-css-1174404221-Y2bKVIqY.datasource.com:9200',
                'es.nodes.wan.only'='true',
                'resource' '/mytest/css')")

         .. _dli_09_0061__en-us_topic_0190067468_table569314388144:

         .. table:: **Table 1** Parameters for creating a table

            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | Parameter                         | Description                                                                                                                                                                                                                                                                                                                                                 |
            +===================================+=============================================================================================================================================================================================================================================================================================================================================================+
            | es.nodes                          | CSS connection address. You need to create a datasource connection first.                                                                                                                                                                                                                                                                                   |
            |                                   |                                                                                                                                                                                                                                                                                                                                                             |
            |                                   | If you have created an enhanced datasource connection, use the intranet IP address provided by CSS. The address format is **IP1:PORT1,\ IP2:PORT2**.                                                                                                                                                                                                        |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | resource                          | Name of the resource for the CSS datasource connection name. You can use **/index/type** to specify the resource location (for easier understanding, the **index** may be seen as **database** and **type** as **table**).                                                                                                                                  |
            |                                   |                                                                                                                                                                                                                                                                                                                                                             |
            |                                   | .. note::                                                                                                                                                                                                                                                                                                                                                   |
            |                                   |                                                                                                                                                                                                                                                                                                                                                             |
            |                                   |    -  In Elasticsearch 6.X, a single index supports only one type, and the type name can be customized.                                                                                                                                                                                                                                                     |
            |                                   |    -  In Elasticsearch 7.X, a single index uses **\_doc** as the type name and cannot be customized. To access Elasticsearch 7.X, set this parameter to **index**.                                                                                                                                                                                          |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | pushdown                          | Whether to enable the pushdown function of CSS. The default value is **true**. For tables with a large number of I/O requests, the pushdown function help reduce I/O pressure when the **where** condition is specified.                                                                                                                                    |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | strict                            | Whether the CSS pushdown is strict. The default value is **false**. The exact match function can reduce more I/O requests than pushdown.                                                                                                                                                                                                                    |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | batch.size.entries                | Maximum number of entries that can be inserted in a batch. The default value is **1000**. If the size of a single data record is so large that the number of data records in the bulk storage reaches the upper limit of the data amount in a single batch, the system stops storing data and submits the data based on the **batch.size.bytes** parameter. |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | batch.size.bytes                  | Maximum amount of data in a single batch. The default value is **1 MB**. If the size of a single data record is so small that the number of data records in the bulk storage reaches the upper limit of the data amount of a single batch, the system stops storing data and submits the data based on the **batch.size.entries** parameter.                |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | es.nodes.wan.only                 | Whether to access the Elasticsearch node using only the domain name. The default value is **false**. If the original internal IP address provided by CSS is used as the **es.nodes**, you do not need to set this parameter or set it to **false**.                                                                                                         |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | es.mapping.id                     | Document field name that contains the document ID in the Elasticsearch node.                                                                                                                                                                                                                                                                                |
            |                                   |                                                                                                                                                                                                                                                                                                                                                             |
            |                                   | .. note::                                                                                                                                                                                                                                                                                                                                                   |
            |                                   |                                                                                                                                                                                                                                                                                                                                                             |
            |                                   |    -  The document ID in the same **/index/type** is unique. If a field that contains a document ID has duplicate values, the document with the duplicate ID will be overwritten when the ES is inserted.                                                                                                                                                   |
            |                                   |    -  This feature can be used as a fault tolerance solution. When data is being inserted, the DLI job fails and some data has been inserted into Elasticsearch. The data is redundant. If the document ID is set, the previous data will be overwritten when the DLI job is executed again.                                                                |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

         .. note::

            **batch.size.entries** and **batch.size.bytes** limit the number of data records and data volume respectively.

      #. Insert data.

         ::

            sparkSession.sql("insert into css_table values(13, 'John'),(22, 'Bob')")

      #. Query data.

         ::

            val dataFrame = sparkSession.sql("select * from css_table")
            dataFrame.show()

         Before data is inserted:

         |image1|

         Response:

         |image2|

      #. Delete the datasource connection table.

         ::

            sparkSession.sql("drop table css_table")

   -  Connecting to data sources through DataFrame APIs

      #. Set connection parameters.

         ::

            val resource = "/mytest/css"
            val nodes = "to-css-1174405013-Ht7O1tYf.datasource.com:9200"

      #. Create a schema and add data to it.

         ::

            val schema = StructType(Seq(StructField("id", IntegerType, false), StructField("name", StringType, false)))
            val rdd = sparkSession.sparkContext.parallelize(Seq(Row(12, "John"),Row(21,"Bob")))

      #. Import data to CSS.

         ::

            val dataFrame_1 = sparkSession.createDataFrame(rdd, schema)
            dataFrame_1.write
              .format("css")
              .option("resource", resource)
              .option("es.nodes", nodes)
              .mode(SaveMode.Append)
              .save()

         .. note::

            The value of **SaveMode** can be one of the following:

            -  **ErrorIfExis**: If the data already exists, the system throws an exception.
            -  **Overwrite**: If the data already exists, the original data will be overwritten.
            -  **Append**: If the data already exists, the system saves the new data.
            -  **Ignore**: If the data already exists, no operation is required. This is similar to the SQL statement **CREATE TABLE IF NOT EXISTS**.

      #. Read data from CSS.

         ::

            val dataFrameR = sparkSession.read.format("css").option("resource",resource).option("es.nodes", nodes).load()
            dataFrameR.show()

         Before data is inserted:

         |image3|

         Response:

         |image4|

   -  Submitting a Spark job

      #. Generate a JAR package based on the code and upload the package to DLI.

      #. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

         .. note::

            -  If the Spark version is 2.3.2 (will be offline soon) or 2.4.5, specify the **Module** to **sys.datasource.css** when you submit a job.

            -  If the Spark version is 3.1.1, you do not need to select a module. Configure **Spark parameters (--conf)**.

               spark.driver.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/css/\*

               spark.executor.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/css/\*

-  Complete example code

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

         object Test_SQL_CSS {
           def main(args: Array[String]): Unit = {
             // Create a SparkSession session.
             val sparkSession = SparkSession.builder().getOrCreate()

             // Create a DLI data table for DLI-associated CSS
             sparkSession.sql("create table css_table(id long, name string) using css options(
             'es.nodes' = 'to-css-1174404217-QG2SwbVV.datasource.com:9200',
             'es.nodes.wan.only' = 'true',
             'resource' = '/mytest/css')")

             //*****************************SQL model***********************************
             // Insert data into the DLI data table
             sparkSession.sql("insert into css_table values(13, 'John'),(22, 'Bob')")

             // Read data from DLI data table
             val dataFrame = sparkSession.sql("select * from css_table")
             dataFrame.show()

             // drop table
             sparkSession.sql("drop table css_table")

             sparkSession.close()
           }
         }

   -  Connecting to data sources through DataFrame APIs

      ::

         import org.apache.spark.sql.{Row, SaveMode, SparkSession};
         import org.apache.spark.sql.types.{IntegerType, StringType, StructField, StructType};

         object Test_SQL_CSS {
           def main(args: Array[String]): Unit = {
             //Create a SparkSession session.
             val sparkSession = SparkSession.builder().getOrCreate()

             //*****************************DataFrame model***********************************
             // Setting the /index/type of CSS
             val resource = "/mytest/css"

             // Define the cross-origin connection address of the CSS cluster
             val nodes = "to-css-1174405013-Ht7O1tYf.datasource.com:9200"

             //Setting schema
             val schema = StructType(Seq(StructField("id", IntegerType, false), StructField("name", StringType, false)))

             // Construction data
             val rdd = sparkSession.sparkContext.parallelize(Seq(Row(12, "John"),Row(21,"Bob")))

             // Create a DataFrame from RDD and schema
             val dataFrame_1 = sparkSession.createDataFrame(rdd, schema)

            //Write data to the CSS
            dataFrame_1.write.format("css")
             .option("resource", resource)
             .option("es.nodes", nodes)
             .mode(SaveMode.Append)
             .save()

             //Read data
             val dataFrameR = sparkSession.read.format("css").option("resource", resource).option("es.nodes", nodes).load()
             dataFrameR.show()

             spardSession.close()
           }
         }

CSS Security Cluster
--------------------

-  Development description

   -  Constructing dependency information and creating a Spark session

      #. Import dependencies.

         Maven dependency

         ::

            <dependency>
              <groupId>org.apache.spark</groupId>
              <artifactId>spark-sql_2.11</artifactId>
              <version>2.3.2</version>
            </dependency>

         Import dependency packages.

         ::

            import org.apache.spark.sql.{Row, SaveMode, SparkSession}
            import org.apache.spark.sql.types.{IntegerType, StringType, StructField, StructType}

      #. Create a session and set the AKs and SKs.

         .. note::

            Hard-coded or plaintext AK and SK pose significant security risks. To ensure security, encrypt your AK and SK, store them in configuration files or environment variables, and decrypt them when needed.

         ::

            val sparkSession = SparkSession.builder().getOrCreate()
            sparkSession.conf.set("fs.obs.access.key", ak)
            sparkSession.conf.set("fs.obs.secret.key", sk)
            sparkSession.conf.set("fs.obs.endpoint", enpoint)
            sparkSession.conf.set("fs.obs.connecton.ssl.enabled", "false")

   -  Connecting to data sources through SQL APIs

      #. Create a table to connect to a CSS data source.

         ::

            sparkSession.sql("create table css_table(id int, name string) using css options(
                'es.nodes' 'to-css-1174404221-Y2bKVIqY.datasource.com:9200',
                'es.nodes.wan.only'='true',
                'resource'='/mytest/css',
                'es.net.ssl'='true',
                'es.net.ssl.keystore.location'='obs://Bucket name/path/transport-keystore.jks',
                'es.net.ssl.keystore.pass'='***',
                'es.net.ssl.truststore.location'='obs://Bucket name/path/truststore.jks',
                'es.net.ssl.truststore.pass'='***',
                'es.net.http.auth.user'='admin',
                'es.net.http.auth.pass'='***')")

         .. table:: **Table 2** Parameters for creating a table

            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | Parameter                         | Description                                                                                                                                                                                                                                                                                                                                                 |
            +===================================+=============================================================================================================================================================================================================================================================================================================================================================+
            | es.nodes                          | CSS connection address. You need to create a datasource connection first.                                                                                                                                                                                                                                                                                   |
            |                                   |                                                                                                                                                                                                                                                                                                                                                             |
            |                                   | If you have created an enhanced datasource connection, use the intranet IP address provided by CSS. The address format is **IP1:PORT1,\ IP2:PORT2**.                                                                                                                                                                                                        |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | resource                          | Name of the resource for the CSS datasource connection name. You can use **/index/type** to specify the resource location (for easier understanding, the **index** may be seen as **database** and **type** as **table**).                                                                                                                                  |
            |                                   |                                                                                                                                                                                                                                                                                                                                                             |
            |                                   | .. note::                                                                                                                                                                                                                                                                                                                                                   |
            |                                   |                                                                                                                                                                                                                                                                                                                                                             |
            |                                   |    1. In Elasticsearch 6.\ *X*, a single index supports only one type, and the type name can be customized.                                                                                                                                                                                                                                                 |
            |                                   |                                                                                                                                                                                                                                                                                                                                                             |
            |                                   |    2. In Elasticsearch 7.\ *X*, a single index uses **\_doc** as the type name and cannot be customized. To access Elasticsearch 7.\ *X*, set this parameter to **index**.                                                                                                                                                                                  |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | pushdown                          | Whether to enable the pushdown function of CSS. The default value is **true**. For tables with a large number of I/O requests, the pushdown function help reduce I/O pressure when the **where** condition is specified.                                                                                                                                    |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | strict                            | Whether the CSS pushdown is strict. The default value is **false**. The exact match function can reduce more I/O requests than pushdown.                                                                                                                                                                                                                    |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | batch.size.entries                | Maximum number of entries that can be inserted in a batch. The default value is **1000**. If the size of a single data record is so large that the number of data records in the bulk storage reaches the upper limit of the data amount in a single batch, the system stops storing data and submits the data based on the **batch.size.bytes** parameter. |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | batch.size.bytes                  | Maximum amount of data in a single batch. The default value is **1 MB**. If the size of a single data record is so small that the number of data records in the bulk storage reaches the upper limit of the data amount of a single batch, the system stops storing data and submits the data based on the **batch.size.entries** parameter.                |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | es.nodes.wan.only                 | Whether to access the Elasticsearch node using only the domain name. The default value is **false**. If the original internal IP address provided by CSS is used as the **es.nodes**, you do not need to set this parameter or set it to **false**.                                                                                                         |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | es.mapping.id                     | Document field name that contains the document ID in the Elasticsearch node.                                                                                                                                                                                                                                                                                |
            |                                   |                                                                                                                                                                                                                                                                                                                                                             |
            |                                   | .. note::                                                                                                                                                                                                                                                                                                                                                   |
            |                                   |                                                                                                                                                                                                                                                                                                                                                             |
            |                                   |    -  The document ID in the same **/index/type** is unique. If a field that contains a document ID has duplicate values, the document with the duplicate ID will be overwritten when the ES is inserted.                                                                                                                                                   |
            |                                   |    -  This feature can be used as a fault tolerance solution. When data is being inserted, the DLI job fails and some data has been inserted into Elasticsearch. The data is redundant. If the document ID is set, the previous data will be overwritten when the DLI job is executed again.                                                                |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | es.net.ssl                        | Whether to connect to the security CSS cluster. The default value is **false**.                                                                                                                                                                                                                                                                             |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | es.net.ssl.keystore.location      | OBS bucket location of the **keystore** file generated by the security CSS cluster certificate.                                                                                                                                                                                                                                                             |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | es.net.ssl.keystore.pass          | Password of the **keystore** file generated by the security CSS cluster certificate.                                                                                                                                                                                                                                                                        |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | es.net.ssl.truststore.location    | OBS bucket location of the **truststore** file generated by the security CSS cluster certificate.                                                                                                                                                                                                                                                           |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | es.net.ssl.truststore.pass        | Password of the **truststore** file generated by the security CSS cluster certificate.                                                                                                                                                                                                                                                                      |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | es.net.http.auth.user             | Username of the security CSS cluster.                                                                                                                                                                                                                                                                                                                       |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
            | es.net.http.auth.pass             | Password of the security CSS cluster.                                                                                                                                                                                                                                                                                                                       |
            +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

         .. note::

            **batch.size.entries** and **batch.size.bytes** limit the number of data records and data volume respectively.

      #. Insert data.

         ::

            sparkSession.sql("insert into css_table values(13, 'John'),(22, 'Bob')")

      #. Query data.

         ::

            val dataFrame = sparkSession.sql("select * from css_table")
            dataFrame.show()

         Before data is inserted:

         |image5|

         Response:

         |image6|

      #. Delete the datasource connection table.

         ::

            sparkSession.sql("drop table css_table")

   -  Connecting to data sources through DataFrame APIs

      #. Set connection parameters.

         ::

            val resource = "/mytest/css"
            val nodes = "to-css-1174405013-Ht7O1tYf.datasource.com:9200"

      #. Create a schema and add data to it.

         ::

            val schema = StructType(Seq(StructField("id", IntegerType, false), StructField("name", StringType, false)))
            val rdd = sparkSession.sparkContext.parallelize(Seq(Row(12, "John"),Row(21,"Bob")))

      #. Import data to CSS.

         ::

            val dataFrame_1 = sparkSession.createDataFrame(rdd, schema)
            dataFrame_1.write
              .format("css")
              .option("resource", resource)
              .option("es.nodes", nodes)
              .option("es.net.ssl", "true")
              .option("es.net.ssl.keystore.location", "obs://Bucket name/path/transport-keystore.jks")
              .option("es.net.ssl.keystore.pass", "***")
              .option("es.net.ssl.truststore.location", "obs://Bucket name/path/truststore.jks")
              .option("es.net.ssl.truststore.pass", "***")
              .option("es.net.http.auth.user", "admin")
              .option("es.net.http.auth.pass", "***")
              .mode(SaveMode.Append)
              .save()

         .. note::

            The value of **Mode** can be one of the following:

            -  **ErrorIfExis**: If the data already exists, the system throws an exception.
            -  **Overwrite**: If the data already exists, the original data will be overwritten.
            -  **Append**: If the data already exists, the system saves the new data.
            -  **Ignore**: If the data already exists, no operation is required. This is similar to the SQL statement **CREATE TABLE IF NOT EXISTS**.

      #. Read data from CSS.

         ::

            val dataFrameR = sparkSession.read.format("css")
                    .option("resource",resource)
                    .option("es.nodes", nodes)
                    .option("es.net.ssl", "true")
                    .option("es.net.ssl.keystore.location", "obs://Bucket name/path/transport-keystore.jks")
                    .option("es.net.ssl.keystore.pass", "***")
                    .option("es.net.ssl.truststore.location", "obs://Bucket name/path/truststore.jks")
                    .option("es.net.ssl.truststore.pass", "***")
                    .option("es.net.http.auth.user", "admin")
                    .option("es.net.http.auth.pass", "***")
                    .load()
            dataFrameR.show()

         Before data is inserted:

         |image7|

         Response:

         |image8|

   -  Submitting a Spark job

      #. Generate a JAR package based on the code and upload the package to DLI.

      #. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

         .. note::

            -  When submitting a job, you need to specify a dependency module named **sys.datasource.css**.
            -  For details about how to submit a job on the DLI console, see
            -  For details about how to submit a job through an API, see the **modules** parameter in

-  Complete example code

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

         object csshttpstest {
           def main(args: Array[String]): Unit = {
             //Create a SparkSession session.
             val sparkSession = SparkSession.builder().getOrCreate()
             // Create a DLI data table for DLI-associated CSS
             sparkSession.sql("create table css_table(id long, name string) using css options('es.nodes' = '192.168.6.204:9200','es.nodes.wan.only' = 'false','resource' = '/mytest','es.net.ssl'='true','es.net.ssl.keystore.location' = 'obs://xietest1/lzq/keystore.jks','es.net.ssl.keystore.pass' = '**','es.net.ssl.truststore.location'='obs://xietest1/lzq/truststore.jks','es.net.ssl.truststore.pass'='**','es.net.http.auth.user'='admin','es.net.http.auth.pass'='**')")

             //*****************************SQL model***********************************
             // Insert data into the DLI data table
             sparkSession.sql("insert into css_table values(13, 'John'),(22, 'Bob')")

             // Read data from DLI data table
             val dataFrame = sparkSession.sql("select * from css_table")
             dataFrame.show()

             // drop table
             sparkSession.sql("drop table css_table")

             sparkSession.close()
           }
         }

   -  Connecting to data sources through DataFrame APIs

      .. note::

         Hard-coded or plaintext AK and SK pose significant security risks. To ensure security, encrypt your AK and SK, store them in configuration files or environment variables, and decrypt them when needed.

      ::

         import org.apache.spark.sql.{Row, SaveMode, SparkSession};
         import org.apache.spark.sql.types.{IntegerType, StringType, StructField, StructType};

         object Test_SQL_CSS {
           def main(args: Array[String]): Unit = {
             //Create a SparkSession session.
             val sparkSession = SparkSession.builder().getOrCreate()
             sparkSession.conf.set("fs.obs.access.key", ak)
             sparkSession.conf.set("fs.obs.secret.key", sk)

             //*****************************DataFrame model***********************************
             // Setting the /index/type of CSS
             val resource = "/mytest/css"

             // Define the cross-origin connection address of the CSS cluster
             val nodes = "to-css-1174405013-Ht7O1tYf.datasource.com:9200"

             //Setting schema
             val schema = StructType(Seq(StructField("id", IntegerType, false), StructField("name", StringType, false)))

             // Construction data
             val rdd = sparkSession.sparkContext.parallelize(Seq(Row(12, "John"),Row(21,"Bob")))

             // Create a DataFrame from RDD and schema
             val dataFrame_1 = sparkSession.createDataFrame(rdd, schema)

            //Write data to the CSS
            dataFrame_1.write .format("css")
             .option("resource", resource)
             .option("es.nodes", nodes)
             .option("es.net.ssl", "true")
             .option("es.net.ssl.keystore.location", "obs://Bucket name/path/transport-keystore.jks")
             .option("es.net.ssl.keystore.pass", "***")
             .option("es.net.ssl.truststore.location", "obs://Bucket name/path/truststore.jks")
             .option("es.net.ssl.truststore.pass", "***")
             .option("es.net.http.auth.user", "admin")
             .option("es.net.http.auth.pass", "***")
             .mode(SaveMode.Append)
             .save();

             //Read data
             val dataFrameR = sparkSession.read.format("css")
             .option("resource", resource)
             .option("es.nodes", nodes)
             .option("es.net.ssl", "true")
             .option("es.net.ssl.keystore.location", "obs://Bucket name/path/transport-keystore.jks")
             .option("es.net.ssl.keystore.pass", "***")
             .option("es.net.ssl.truststore.location", "obs://Bucket name/path/truststore.jks")
             .option("es.net.ssl.truststore.pass", "***")
             .option("es.net.http.auth.user", "admin")
             .option("es.net.http.auth.pass", "***")
             .load()
             dataFrameR.show()

             spardSession.close()
           }
         }

.. |image1| image:: /_static/images/en-us_image_0223997302.png
.. |image2| image:: /_static/images/en-us_image_0223997303.png
.. |image3| image:: /_static/images/en-us_image_0223997304.png
.. |image4| image:: /_static/images/en-us_image_0223997305.png
.. |image5| image:: /_static/images/en-us_image_0266325813.png
.. |image6| image:: /_static/images/en-us_image_0266325814.png
.. |image7| image:: /_static/images/en-us_image_0266325815.png
.. |image8| image:: /_static/images/en-us_image_0266325816.png
