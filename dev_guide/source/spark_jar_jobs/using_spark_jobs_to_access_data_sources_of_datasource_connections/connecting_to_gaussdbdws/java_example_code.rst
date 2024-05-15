:original_name: dli_09_0199.html

.. _dli_09_0199:

Java Example Code
=================

Scenario
--------

This section provides Java example code that demonstrates how to use a Spark job to access data from the GaussDB(DWS) data source.

A datasource connection has been created and bound to a queue on the DLI management console.

.. note::

   Hard-coded or plaintext passwords pose significant security risks. To ensure security, encrypt your passwords, store them in configuration files or environment variables, and decrypt them when needed.

Preparations
------------

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

      SparkSession sparkSession = SparkSession.builder().appName("datasource-dws").getOrCreate();

Accessing a Data Source Through a SQL API
-----------------------------------------

#. Create a table to connect to a GaussDB(DWS) data source and set connection parameters.

   ::

      sparkSession.sql("CREATE TABLE IF NOT EXISTS dli_to_dws USING JDBC OPTIONS ('url'='jdbc:postgresql://10.0.0.233:8000/postgres','dbtable'='test','user'='dbadmin','password'='**')");

#. Insert data.

   ::

      sparkSession.sql("insert into dli_to_dws values(3,'L'),(4,'X')");

#. Query data.

   ::

      sparkSession.sql("select * from dli_to_dws").show();

Submitting a Spark Job
----------------------

#. Generate a JAR package based on the code file and upload the package to DLI.

#. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

   .. note::

      -  If the Spark version is 2.3.2 (will be offline soon) or 2.4.5, specify the **Module** to **sys.datasource.dws** when you submit a job.

      -  If the Spark version is 3.1.1, you do not need to select a module. Configure **Spark parameters (--conf)**.

         spark.driver.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/dws/\*

         spark.executor.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/dws/\*

Complete Example Code
---------------------

Accessing GaussDB(DWS) tables through SQL APIs

.. code-block::

   import org.apache.spark.sql.SparkSession;

   public class java_dws {
       public static void main(String[] args) {
           SparkSession sparkSession = SparkSession.builder().appName("datasource-dws").getOrCreate();

           sparkSession.sql("CREATE TABLE IF NOT EXISTS dli_to_dws USING JDBC OPTIONS ('url'='jdbc:postgresql://10.0.0.233:8000/postgres','dbtable'='test','user'='dbadmin','password'='**')");

           //*****************************SQL model***********************************
           //Insert data into the DLI data table
           sparkSession.sql("insert into dli_to_dws values(3,'Liu'),(4,'Xie')");

           //Read data from DLI data table
           sparkSession.sql("select * from dli_to_dws").show();

           //drop table
           sparkSession.sql("drop table dli_to_dws");

           sparkSession.close();
       }
   }
