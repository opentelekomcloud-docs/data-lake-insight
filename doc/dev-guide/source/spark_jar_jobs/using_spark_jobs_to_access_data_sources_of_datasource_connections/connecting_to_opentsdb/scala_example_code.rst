:original_name: dli_09_0065.html

.. _dli_09_0065:

Scala Example Code
==================

Development Description
-----------------------

The CloudTable OpenTSDB and MRS OpenTSDB can be connected to DLI as data sources.

-  Prerequisites

   A datasource connection has been created on the DLI management console.

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

         import scala.collection.mutable
         import org.apache.spark.sql.{Row, SparkSession}
         import org.apache.spark.rdd.RDD
         import org.apache.spark.sql.types._

   #. Create a session.

      ::

         val sparkSession = SparkSession.builder().getOrCreate()

   #. Create a table to connect to an OpenTSDB data source.

      ::

         sparkSession.sql("create table opentsdb_test using opentsdb options(
             'Host'='opentsdb-3xcl8dir15m58z3.cloudtable.com:4242',
                 'metric'='ctopentsdb',
             'tags'='city,location')")

      .. _dli_09_0065__en-us_topic_0190597601_table463015581831:

      .. table:: **Table 1** Parameters for creating a table

         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                         | Description                                                                                                                                                                                                                                                                                   |
         +===================================+===============================================================================================================================================================================================================================================================================================+
         | host                              | OpenTSDB IP address.                                                                                                                                                                                                                                                                          |
         |                                   |                                                                                                                                                                                                                                                                                               |
         |                                   | -  To access CloudTable OpenTSDB, specify the OpenTSDB connection address. You can log in to the CloudTable console, choose **Cluster Mode** and click the target cluster name, and obtain the OpenTSDB connection address from the cluster information.                                      |
         |                                   | -  You can also access the MRS OpenTSDB. If you have created an enhanced datasource connection, enter the IP address and port number of the node where the OpenTSDB is located. The format is **IP:PORT**. If the OpenTSDB has multiple nodes, separate their IP addresses by semicolons (;). |
         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | metric                            | Name of the metric in OpenTSDB corresponding to the DLI table to be created.                                                                                                                                                                                                                  |
         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | tags                              | Tags corresponding to the metric, used for operations such as classification, filtering, and quick search. A maximum of 8 tags, including all **tagk** values under the metric, can be added and are separated by commas (,).                                                                 |
         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

-  Connecting to data sources through SQL APIs

   #. Insert data.

      ::

         sparkSession.sql("insert into opentsdb_test values('futian', 'abc', '1970-01-02 18:17:36', 30.0)")

   #. Query data.

      ::

         sparkSession.sql("select * from opentsdb_test").show()

-  Connecting to data sources through DataFrame APIs

   #. Construct a schema.

      ::

         val attrTag1Location = new StructField("location", StringType)
         val attrTag2Name = new StructField("name", StringType)
         val attrTimestamp = new StructField("timestamp", LongType)
         val attrValue = new StructField("value", DoubleType)
         val attrs = Array(attrTag1Location, attrTag2Name, attrTimestamp, attrValue)

   #. Construct data based on the schema type.

      ::

         val mutableRow: Seq[Any] = Seq("aaa", "abc", 123456L, 30.0)
         val rddData: RDD[Row] = sparkSession.sparkContext.parallelize(Array(Row.fromSeq(mutableRow)), 1)

   #. Import data to OpenTSDB.

      ::

         sparkSession.createDataFrame(rddData, new StructType(attrs)).write.insertInto("opentsdb_test")

   #. Read data from OpenTSDB.

      ::

         val map = new mutable.HashMap[String, String]()
         map("metric") = "ctopentsdb"
         map("tags") = "city,location"
         map("Host") = "opentsdb-3xcl8dir15m58z3.cloudtable.com:4242"
         sparkSession.read.format("opentsdb").options(map.toMap).load().show()

-  Submitting a Spark job

   #. Generate a JAR package based on the code and upload the package to DLI.

   #. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

      .. note::

         -  If the Spark version is 2.3.2 (will be offline soon) or 2.4.5, specify the **Module** to **sys.datasource.opentsdb** when you submit a job.

         -  If the Spark version is 3.1.1, you do not need to select a module. Configure **Spark parameters (--conf)**.

            spark.driver.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/opentsdb/\*

            spark.executor.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/opentsdb/\*

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

      object Test_OpenTSDB_CT {
        def main(args: Array[String]): Unit = {
          // Create a SparkSession session.
          val sparkSession = SparkSession.builder().getOrCreate()

          // Create a data table for DLI association OpenTSDB
          sparkSession.sql("create table opentsdb_test using opentsdb options(
          'Host'='opentsdb-3xcl8dir15m58z3.cloudtable.com:4242',
          'metric'='ctopentsdb',
          'tags'='city,location')")

          //*****************************SQL module***********************************
          sparkSession.sql("insert into opentsdb_test values('futian', 'abc', '1970-01-02 18:17:36', 30.0)")
          sparkSession.sql("select * from opentsdb_test").show()

          sparkSession.close()
        }
      }

-  Connecting to data sources through DataFrame APIs

   ::

      import scala.collection.mutable
      import org.apache.spark.sql.{Row, SparkSession}
      import org.apache.spark.rdd.RDD
      import org.apache.spark.sql.types._

      object Test_OpenTSDB_CT {
        def main(args: Array[String]): Unit = {
          // Create a SparkSession session.
          val sparkSession = SparkSession.builder().getOrCreate()

          // Create a data table for DLI association OpenTSDB
          sparkSession.sql("create table opentsdb_test using opentsdb options(
          'Host'='opentsdb-3xcl8dir15m58z3.cloudtable.com:4242',
          'metric'='ctopentsdb',
          'tags'='city,location')")

          //*****************************DataFrame model***********************************
          // Setting schema
          val attrTag1Location = new StructField("location", StringType)
          val attrTag2Name = new StructField("name", StringType)
          val attrTimestamp = new StructField("timestamp", LongType)
          val attrValue = new StructField("value", DoubleType)
          val attrs = Array(attrTag1Location, attrTag2Name, attrTimestamp,attrValue)

          // Populate data according to the type of schema
          val mutableRow: Seq[Any] = Seq("aaa", "abc", 123456L, 30.0)
          val rddData: RDD[Row] = sparkSession.sparkContext.parallelize(Array(Row.fromSeq(mutableRow)), 1)

          //Import the constructed data into OpenTSDB
          sparkSession.createDataFrame(rddData, new StructType(attrs)).write.insertInto("opentsdb_test")

          //Read data on OpenTSDB
          val map = new mutable.HashMap[String, String]()
          map("metric") = "ctopentsdb"
          map("tags") = "city,location"
          map("Host") = "opentsdb-3xcl8dir15m58z3.cloudtable.com:4242"
          sparkSession.read.format("opentsdb").options(map.toMap).load().show()

          sparkSession.close()
        }
      }
