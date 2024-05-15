:original_name: dli_09_0193.html

.. _dli_09_0193:

Java Example Code
=================

Development Description
-----------------------

This example applies only to MRS OpenTSDB.

-  Prerequisites

   A datasource connection has been created and bound to a queue on the DLI management console.

   .. note::

      Hard-coded or plaintext passwords pose significant security risks. To ensure security, encrypt your passwords, store them in configuration files or environment variables, and decrypt them when needed.

-  Code implementation

   #. Import dependencies.

      -  Maven dependency involved

         ::

            <dependency>
              <groupId>org.apache.spark</groupId>
              <artifactId>spark-sql_2.11</artifactId>
              <version>2.3.2</version>
            </dependency>

      -  Import dependency packages.

         ::

            import org.apache.spark.sql.SparkSession;

   #. Create a session.

      ::

         sparkSession = SparkSession.builder().appName("datasource-opentsdb").getOrCreate();

-  Connecting to data sources through SQL APIs

   -  Create a table to connect to an MRS OpenTSDB data source and set connection parameters.

      ::

         sparkSession.sql("create table opentsdb_new_test using opentsdb options('Host'='10.0.0.171:4242','metric'='ctopentsdb','tags'='city,location')");

      .. note::

         For details about the **Host**, **metric**, and **tags** parameters, see :ref:`Table 1 <dli_09_0065__en-us_topic_0190597601_table463015581831>`.

   -  Insert data.

      ::

         sparkSession.sql("insert into opentsdb_new_test values('Penglai', 'abc', '2021-06-30 18:00:00', 30.0)");

   -  Query data.

      ::

         sparkSession.sql("select * from opentsdb_new_test").show();

-  Submitting a Spark job

   #. Generate a JAR package based on the code file and upload the package to DLI.

   #. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

      .. note::

         -  If the Spark version is 2.3.2 (will be offline soon) or 2.4.5, specify the **Module** to **sys.datasource.opentsdb** when you submit a job.

         -  If the Spark version is 3.1.1, you do not need to select a module. Configure **Spark parameters (--conf)**.

            spark.driver.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/opentsdb/\*

            spark.executor.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/opentsdb/\*

Complete Example Code
---------------------

-  Maven dependency involved

   .. code-block::

      <dependency>
                  <groupId>org.apache.spark</groupId>
                  <artifactId>spark-sql_2.11</artifactId>
                  <version>2.3.2</version>
      </dependency>

-  Connecting to data sources through SQL APIs

   ::

      import org.apache.spark.sql.SparkSession;

      public class java_mrs_opentsdb {

          private static SparkSession sparkSession = null;

          public static void main(String[] args) {
              //create a SparkSession session
              sparkSession = SparkSession.builder().appName("datasource-opentsdb").getOrCreate();

              sparkSession.sql("create table opentsdb_new_test using opentsdb options('Host'='10.0.0.171:4242','metric'='ctopentsdb','tags'='city,location')");

              //*****************************SQL module***********************************
              sparkSession.sql("insert into opentsdb_new_test values('Penglai', 'abc', '2021-06-30 18:00:00', 30.0)");
              System.out.println("Penglai new timestamp");
              sparkSession.sql("select * from opentsdb_new_test").show();

             sparkSession.close();

          }
      }
