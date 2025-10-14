:original_name: dli_09_0190.html

.. _dli_09_0190:

Java Example Code
=================

Prerequisites
-------------

A datasource connection has been created on the DLI management console.

CSS Non-Security Cluster
------------------------

-  Development description

   -  Code implementation

      -  Constructing dependency information and creating a Spark session

         #. Import dependencies.

            Maven dependency

            .. code-block::

               <dependency>
                           <groupId>org.apache.spark</groupId>
                           <artifactId>spark-sql_2.11</artifactId>
                           <version>2.3.2</version>
               </dependency>

            Import dependency packages.

            ::

               import org.apache.spark.sql.SparkSession;

         #. Create a session.

            ::

               SparkSession sparkSession = SparkSession.builder().appName("datasource-css").getOrCreate();

   -  Connecting to data sources through SQL APIs

      #. Create a table to connect to a CSS data source.

         .. code-block::

            sparkSession.sql("create table css_table(id long, name string) using css options( 'es.nodes' = '192.168.9.213:9200', 'es.nodes.wan.only' = 'true','resource' ='/mytest')");

      #. Insert data.

         .. code-block::

            sparkSession.sql("insert into css_table values(18, 'John'),(28, 'Bob')");

      #. Query data.

         .. code-block::

            sparkSession.sql("select * from css_table").show();

      #. Delete the datasource connection table.

         .. code-block::

            sparkSession.sql("drop table css_table");

   -  Submitting a Spark job

      #. Generate a JAR package based on the code file and upload the package to DLI.

      #. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

         .. note::

            -  If the Spark version is 2.3.2 (will be offline soon) or 2.4.5, specify the **Module** to **sys.datasource.css** when you submit a job.

            -  If the Spark version is 3.1.1, you do not need to select a module. Configure **Spark parameters (--conf)**.

               spark.driver.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/css/\*

               spark.executor.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/css/\*

-  Complete example code

   -  Maven dependency

      .. code-block::

         <dependency>
                     <groupId>org.apache.spark</groupId>
                     <artifactId>spark-sql_2.11</artifactId>
                     <version>2.3.2</version>
         </dependency>

   -  Connecting to data sources through SQL APIs

      ::

         import org.apache.spark.sql.*;

         public class java_css_unsecurity {

             public static void main(String[] args) {
                 SparkSession sparkSession = SparkSession.builder().appName("datasource-css-unsecurity").getOrCreate();

                 // Create a DLI data table for DLI-associated CSS
                 sparkSession.sql("create table css_table(id long, name string) using css options( 'es.nodes' = '192.168.15.34:9200', 'es.nodes.wan.only' = 'true', 'resource' = '/mytest')");

                 //*****************************SQL model***********************************
                 // Insert data into the DLI data table
                 sparkSession.sql("insert into css_table values(18, 'John'),(28, 'Bob')");

                 // Read data from DLI data table
                 sparkSession.sql("select * from css_table").show();

                 // drop table
                 sparkSession.sql("drop table css_table");

                 sparkSession.close();
             }
         }

CSS Security Cluster
--------------------

-  Preparations

   Generate the **keystore.jks** and **truststore.jks** files and upload them to the OBS bucket. For details, see :ref:`CSS Security Cluster Configuration <dli_09_0189>`.

-  Description of the development with HTTPS disabled

   If HTTPS is disabled, **keystore.jks** and **truststore.jks** files are not required. You only need to set SSL access parameters and credentials.

   -  Constructing dependency information and creating a Spark session

      #. Import dependencies.

         Maven dependency

         .. code-block::

            <dependency>
                        <groupId>org.apache.spark</groupId>
                        <artifactId>spark-sql_2.11</artifactId>
                        <version>2.3.2</version>
            </dependency>

         Import dependency packages.

         ::

            import org.apache.spark.sql.SparkSession;

      #. Create a session.

         ::

            SparkSession sparkSession = SparkSession.builder().appName("datasource-css").getOrCreate();

   -  Connecting to data sources through SQL APIs

      #. Create a table to connect to a CSS data source.

         ::

            sparkSession.sql("create table css_table(id long, name string) using css options( 'es.nodes' = '192.168.9.213:9200', 'es.nodes.wan.only' = 'true', 'resource' = '/mytest','es.net.ssl'='false','es.net.http.auth.user'='admin','es.net.http.auth.pass'='*******')");

         .. note::

            -  For details about the parameters for creating a CSS datasource connection table, see :ref:`Table 1 <dli_09_0061__en-us_topic_0190067468_table569314388144>`.
            -  In the preceding example, HTTPS access is disabled for the CSS security cluster. Therefore, you need to set **es.net.ssl** to **false**. **es.net.http.auth.user** and **es.net.http.auth.pass** are the username and password set during cluster creation, respectively.

      #. Insert data.

         ::

            sparkSession.sql("insert into css_table values(18, 'John'),(28, 'Bob')");

      #. Query data.

         ::

            sparkSession.sql("select * from css_table").show();

      #. Delete the datasource connection table.

         .. code-block::

            sparkSession.sql("drop table css_table");

   -  Submitting a Spark job

      #. Generate a JAR package based on the code file and upload the package to DLI.

      #. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

         .. note::

            -  When submitting a job, you need to specify a dependency module named **sys.datasource.css**.
            -  For details about how to submit a job on the console, see
            -  For details about how to submit a job through an API, see the **modules** parameter in

   -  Complete example code

      -  Maven dependency

         .. code-block::

            <dependency>
                        <groupId>org.apache.spark</groupId>
                        <artifactId>spark-sql_2.11</artifactId>
                        <version>2.3.2</version>
            </dependency>

-  Description of development with HTTPS enabled

   -  Constructing dependency information and creating a Spark session

      #. Import dependencies.

         Maven dependency

         .. code-block::

            <dependency>
                        <groupId>org.apache.spark</groupId>
                        <artifactId>spark-sql_2.11</artifactId>
                        <version>2.3.2</version>
            </dependency>

         Import dependency packages.

         ::

            import org.apache.spark.sql.SparkSession;

      #. Create a session.

         ::

            SparkSession sparkSession = SparkSession.builder().appName("datasource-css").getOrCreate();

   -  Connecting to data sources through SQL APIs

      #. Create a table to connect to a CSS data source.

         ::

            sparkSession.sql("create table css_table(id long, name string) using css options( 'es.nodes' = '192.168.13.189:9200', 'es.nodes.wan.only' = 'true', 'resource' = '/mytest','es.net.ssl'='true','es.net.ssl.keystore.location' = 'obs://Bucket name/Address/transport-keystore.jks','es.net.ssl.keystore.pass' = '**',
            'es.net.ssl.truststore.location'='obs://Bucket name/Address/truststore.jks,
            'es.net.ssl.truststore.pass'='***','es.net.http.auth.user'='admin','es.net.http.auth.pass'='**')");

         .. note::

            For details about the parameters for creating a CSS datasource connection table, see :ref:`Table 1 <dli_09_0061__en-us_topic_0190067468_table569314388144>`.

      #. Insert data.

         ::

            sparkSession.sql("insert into css_table values(18, 'John'),(28, 'Bob')");

      #. Query data.

         ::

            sparkSession.sql("select * from css_table").show();

      #. Delete the datasource connection table.

         .. code-block::

            sparkSession.sql("drop table css_table");

   -  Submitting a Spark job

      #. Generate a JAR package based on the code file and upload the package to DLI.

      #. If HTTPS access is enabled, you need to upload the dependency file **hadoop-site.xml** when creating a Spark job. The content of the **hadoop-site.xml** file is as follows:

         .. code-block::

            <?xml version="1.0" encoding="UTF-8"?>
            <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
            <!--
              Licensed under the Apache License, Version 2.0 (the "License");
              you may not use this file except in compliance with the License.
              You may obtain a copy of the License at

                http://www.apache.org/licenses/LICENSE-2.0

              Unless required by applicable law or agreed to in writing, software
              distributed under the License is distributed on an "AS IS" BASIS,
              WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
              See the License for the specific language governing permissions and
              limitations under the License. See accompanying LICENSE file.
            -->

            <!-- Put site-specific property overrides in this file. -->

            <configuration>
            <property>
                <name>fs.obs.bucket.Bucket name.access.key</name>
                <value>AK</value>
              </property>
            <property>
                <name>fs.obs.bucket.Bucket name.secret.key </name>
                <value>SK</value>
              </property>
            </configuration>

         .. note::

            **<name>fs.obs.bucket.\ Bucket name.access.key</name>** is used to better locate the bucket address. The bucket name is the name of the bucket where the **keystore.jks** and **truststore.jks** files are stored.

      #. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

         .. note::

            -  When submitting a job, you need to specify a dependency module named **sys.datasource.css**.
            -  For details about how to submit a job on the console, see
            -  For details about how to submit a job through an API, see the **modules** parameter in

   -  Complete example code

      -  Maven dependency

         .. code-block::

            <dependency>
                        <groupId>org.apache.spark</groupId>
                        <artifactId>spark-sql_2.11</artifactId>
                        <version>2.3.2</version>
            </dependency>

      -  Connecting to data sources through SQL APIs

         ::

            import org.apache.spark.sql.SparkSession;

            public class java_css_security_httpson {
                public static void main(String[] args) {
                    SparkSession sparkSession = SparkSession.builder().appName("datasource-css").getOrCreate();

                    // Create a DLI data table for DLI-associated CSS
                    sparkSession.sql("create table css_table(id long, name string) using css options( 'es.nodes' = '192.168.13.189:9200', 'es.nodes.wan.only' = 'true', 'resource' = '/mytest','es.net.ssl'='true','es.net.ssl.keystore.location' = 'obs://Bucket name/Address/transport-keystore.jks','es.net.ssl.keystore.pass' = '**','es.net.ssl.truststore.location'='obs://Bucket name/Address/truststore.jks','es.net.ssl.truststore.pass'='**','es.net.http.auth.user'='admin','es.net.http.auth.pass'='**')");

                    //*****************************SQL model***********************************
                    // Insert data into the DLI data table
                    sparkSession.sql("insert into css_table values(34, 'Yuan'),(28, 'Kids')");

                    // Read data from DLI data table
                    sparkSession.sql("select * from css_table").show();

                    // drop table
                    sparkSession.sql("drop table css_table");

                    sparkSession.close();
                }
            }
