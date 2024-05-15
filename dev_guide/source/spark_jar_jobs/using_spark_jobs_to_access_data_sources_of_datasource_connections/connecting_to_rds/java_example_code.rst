:original_name: dli_09_0187.html

.. _dli_09_0187:

Java Example Code
=================

Development Description
-----------------------

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

         SparkSession sparkSession = SparkSession.builder().appName("datasource-rds").getOrCreate();

-  Connecting to data sources through SQL APIs

   -  Create a table to connect to an RDS data source and set connection parameters.

      ::

         sparkSession.sql(
           "CREATE TABLE IF NOT EXISTS dli_to_rds USING JDBC OPTIONS (
              'url'='jdbc:mysql://to-rds-1174404209-cA37siB6.datasource.com:3306',  // Set this parameter to the actual URL.
              'dbtable'='test.customer',
              'user'='root',  // Set this parameter to the actual user.
              'password'='######',  // Set this parameter to the actual password.
              'driver'='com.mysql.jdbc.Driver')")

      For details about the parameters for creating a table, see :ref:`Table 1 <dli_09_0067__en-us_topic_0190647826_table127421320141311>`.

   -  Insert data.

      ::

         sparkSession.sql("insert into dli_to_rds values (1,'John',24)");

   -  Query data.

      ::

         sparkSession.sql("select * from dli_to_rd").show();

      Response

      |image1|

-  Submitting a Spark job

   #. Generate a JAR package based on the code and upload the package to DLI.

   #. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

   #. After the Spark job is created, click **Execute** in the upper right corner of the console to submit the job. If the message "Spark job submitted successfully." is displayed, the Spark job is successfully submitted. You can view the status and logs of the submitted job on the **Spark Jobs** page.

      .. note::

         -  If the Spark version is 2.3.2 (will be offline soon) or 2.4.5, specify the **Module** to **sys.datasource.rds** when you submit a job.

         -  If the Spark version is 3.1.1, you do not need to select a module. Configure **Spark parameters (--conf)**.

            spark.driver.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/rds/\*

            spark.executor.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/rds/\*

Complete Example Code
---------------------

Connecting to data sources through SQL APIs

::

   import org.apache.spark.sql.SparkSession;

   public class java_rds {

       public static void main(String[] args) {
           SparkSession sparkSession = SparkSession.builder().appName("datasource-rds").getOrCreate();

           // Create a data table for DLI-associated RDS
           sparkSession.sql("CREATE TABLE IF NOT EXISTS dli_to_rds USING JDBC OPTIONS ('url'='jdbc:mysql://192.168.6.150:3306','dbtable'='test.customer','user'='root','password'='**','driver'='com.mysql.jdbc.Driver')");

           //*****************************SQL model***********************************
           //Insert data into the DLI data table
           sparkSession.sql("insert into dli_to_rds values(3,'Liu',21),(4,'Joey',34)");

           //Read data from DLI data table
           sparkSession.sql("select * from dli_to_rds");

           //drop table
           sparkSession.sql("drop table dli_to_rds");

           sparkSession.close();
       }
   }

.. |image1| image:: /_static/images/en-us_image_0000001129442286.png
