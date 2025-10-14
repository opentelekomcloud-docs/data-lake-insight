:original_name: dli_09_0090.html

.. _dli_09_0090:

PySpark Example Code
====================

Prerequisites
-------------

A datasource connection has been created on the DLI management console.

CSS Non-Security Cluster
------------------------

-  Development description

   -  Code implementation

      #. Import dependency packages.

         ::

            from __future__ import print_function
            from pyspark.sql.types import StructType, StructField, IntegerType, StringType, Row
            from pyspark.sql import SparkSession

      #. Create a session.

         ::

            sparkSession = SparkSession.builder.appName("datasource-css").getOrCreate()

   -  Connecting to data sources through DataFrame APIs

      #. Set connection parameters.

         ::

            resource = "/mytest"
            nodes = "to-css-1174404953-hDTx3UPK.datasource.com:9200"

         .. note::

            **resource** indicates the name of the resource associated with the CSS. You can specify the resource location in */index/type* format. (The **index** can be the database and **type** the table.)

            -  In Elasticsearch 6.X, a single index supports only one type, and the type name can be customized.
            -  In Elasticsearch 7.X, a single index uses **\_doc** as the type name and cannot be customized. To access Elasticsearch 7.X, set this parameter to **index**.

      #. Create a schema and add data to it.

         ::

            schema = StructType([StructField("id", IntegerType(), False),
                                 StructField("name", StringType(), False)])
            rdd = sparkSession.sparkContext.parallelize([Row(1, "John"), Row(2, "Bob")])

      #. Construct a DataFrame.

         ::

            dataFrame = sparkSession.createDataFrame(rdd, schema)

      #. Save data to CSS.

         ::

            dataFrame.write.format("css").option("resource", resource).option("es.nodes", nodes).mode("Overwrite").save()

         .. note::

            The options of **mode** can be one of the following:

            -  **ErrorIfExis**: If the data already exists, the system throws an exception.
            -  **Overwrite**: If the data already exists, the original data will be overwritten.
            -  **Append**: If the data already exists, the system saves the new data.
            -  **Ignore**: If the data already exists, no operation is required. This is similar to the SQL statement **CREATE TABLE IF NOT EXISTS**.

      #. Read data from CSS.

         ::

            jdbcDF = sparkSession.read.format("css").option("resource", resource).option("es.nodes", nodes).load()
            jdbcDF.show()

      #. View the operation result.

         |image1|

   -  Connecting to data sources through SQL APIs

      #. Create a table to connect to a CSS data source.

         ::

            sparkSession.sql(
                "create table css_table(id long, name string) using css options(
                'es.nodes'='to-css-1174404953-hDTx3UPK.datasource.com:9200',
                'es.nodes.wan.only'='true',
                'resource'='/mytest')")

         .. note::

            For details about the parameters for creating a CSS datasource connection table, see :ref:`Table 1 <dli_09_0061__en-us_topic_0190067468_table569314388144>`.

      #. Insert data.

         ::

            sparkSession.sql("insert into css_table values(3,'tom')")

      #. Query data.

         ::

            jdbcDF = sparkSession.sql("select * from css_table")
            jdbcDF.show()

      #. View the operation result.

         |image2|

   -  Submitting a Spark job

      #. Upload the Python code file to DLI.

      #. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

         .. note::

            -  If the Spark version is 2.3.2 (will be offline soon) or 2.4.5, specify the **Module** to **sys.datasource.css** when you submit a job.

            -  If the Spark version is 3.1.1, you do not need to select a module. Configure **Spark parameters (--conf)**.

               spark.driver.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/css/\*

               spark.executor.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/css/\*

-  Complete example code

   -  Connecting to data sources through DataFrame APIs

      ::

         # _*_ coding: utf-8 _*_
         from __future__ import print_function
         from pyspark.sql.types import Row, StructType, StructField, IntegerType, StringType
         from pyspark.sql import SparkSession

         if __name__ == "__main__":
           # Create a SparkSession session.
           sparkSession = SparkSession.builder.appName("datasource-css").getOrCreate()

           # Setting cross-source connection parameters
           resource = "/mytest"
           nodes = "to-css-1174404953-hDTx3UPK.datasource.com:9200"

           # Setting schema
           schema = StructType([StructField("id", IntegerType(), False),
                                StructField("name", StringType(), False)])

           # Construction data
           rdd = sparkSession.sparkContext.parallelize([Row(1, "John"), Row(2, "Bob")])

           # Create a DataFrame from RDD and schema
           dataFrame = sparkSession.createDataFrame(rdd, schema)

           # Write data to the CSS
           dataFrame.write.format("css").option("resource", resource).option("es.nodes", nodes).mode("Overwrite").save()

           # Read data
           jdbcDF = sparkSession.read.format("css").option("resource", resource).option("es.nodes", nodes).load()
           jdbcDF.show()

           # close session
           sparkSession.stop()

   -  Connecting to data sources through SQL APIs

      ::

         # _*_ coding: utf-8 _*_
         from __future__ import print_function
         from pyspark.sql import SparkSession

         if __name__ == "__main__":
           # Create a SparkSession session.
           sparkSession = SparkSession.builder.appName("datasource-css").getOrCreate()

           # Create a DLI data table for DLI-associated CSS
           sparkSession.sql(
               "create table css_table(id long, name string) using css options( \
               'es.nodes'='to-css-1174404953-hDTx3UPK.datasource.com:9200',\
               'es.nodes.wan.only'='true',\
               'resource'='/mytest')")

           # Insert data into the DLI data table
           sparkSession.sql("insert into css_table values(3,'tom')")

           # Read data from DLI data table
           jdbcDF = sparkSession.sql("select * from css_table")
           jdbcDF.show()

           # close session
           sparkSession.stop()

CSS Security Cluster
--------------------

-  Development description

   -  Code implementation

      #. Import dependency packages.

         ::

            from __future__ import print_function
            from pyspark.sql.types import StructType, StructField, IntegerType, StringType, Row
            from pyspark.sql import SparkSession

      #. Create a session and set the AKs and SKs.

         .. note::

            Hard-coded or plaintext AK and SK pose significant security risks. To ensure security, encrypt your AK and SK, store them in configuration files or environment variables, and decrypt them when needed.

         ::

            sparkSession = SparkSession.builder.appName("datasource-css").getOrCreate()
            sparkSession.conf.set("fs.obs.access.key", ak)
            sparkSession.conf.set("fs.obs.secret.key", sk)
            sparkSession.conf.set("fs.obs.endpoint", enpoint)
            sparkSession.conf.set("fs.obs.connecton.ssl.enabled", "false")

   -  Connecting to data sources through DataFrame APIs

      #. Set connection parameters.

         ::

            resource = "/mytest";
            nodes = "to-css-1174404953-hDTx3UPK.datasource.com:9200"

         .. note::

            **resource** indicates the name of the resource associated with the CSS. You can specify the resource location in */index/type* format. (The **index** can be the database and **type** the table.)

            -  In Elasticsearch 6.X, a single index supports only one type, and the type name can be customized.
            -  In Elasticsearch 7.X, a single index uses **\_doc** as the type name and cannot be customized. To access Elasticsearch 7.X, set this parameter to **index**.

      #. Create a schema and add data to it.

         ::

            schema = StructType([StructField("id", IntegerType(), False),
                                 StructField("name", StringType(), False)])
            rdd = sparkSession.sparkContext.parallelize([Row(1, "John"), Row(2, "Bob")])

      #. Construct a DataFrame.

         ::

            dataFrame = sparkSession.createDataFrame(rdd, schema)

      #. Save data to CSS.

         ::

            dataFrame.write.format("css")
              .option("resource", resource)
              .option("es.nodes", nodes)
              .option("es.net.ssl", "true")
              .option("es.net.ssl.keystore.location", "obs://Bucket name/path/transport-keystore.jks")
              .option("es.net.ssl.keystore.pass", "***")
              .option("es.net.ssl.truststore.location", "obs://Bucket name/path/truststore.jks")
              .option("es.net.ssl.truststore.pass", "***")
              .option("es.net.http.auth.user", "admin")
              .option("es.net.http.auth.pass", "***")
              .mode("Overwrite")
              .save()

         .. note::

            The options of **mode** can be one of the following:

            -  **ErrorIfExis**: If the data already exists, the system throws an exception.
            -  **Overwrite**: If the data already exists, the original data will be overwritten.
            -  **Append**: If the data already exists, the system saves the new data.
            -  **Ignore**: If the data already exists, no operation is required. This is similar to the SQL statement **CREATE TABLE IF NOT EXISTS**.

      #. Read data from CSS.

         ::

            jdbcDF = sparkSession.read.format("css")\
              .option("resource", resource)\
              .option("es.nodes", nodes)\
              .option("es.net.ssl", "true")\
              .option("es.net.ssl.keystore.location", "obs://Bucket name/path/transport-keystore.jks")\
              .option("es.net.ssl.keystore.pass", "***")\
              .option("es.net.ssl.truststore.location", "obs://Bucket name/path/truststore.jks")\
              .option("es.net.ssl.truststore.pass", "***")\
              .option("es.net.http.auth.user", "admin")\
              .option("es.net.http.auth.pass", "***")\
              .load()
            jdbcDF.show()

      #. View the operation result.

         |image3|

   -  Connecting to data sources through SQL APIs

      #. Create a table to connect to a CSS data source.

         ::

            sparkSession.sql(
                    "create table css_table(id long, name string) using css options(\
                    'es.nodes'='to-css-1174404953-hDTx3UPK.datasource.com:9200',\
                    'es.nodes.wan.only'='true',\
                    'resource'='/mytest',\
                'es.net.ssl'='true',\
                'es.net.ssl.keystore.location'='obs://Bucket name/path/transport-keystore.jks',\
                'es.net.ssl.keystore.pass'='***',\
                'es.net.ssl.truststore.location'='obs://Bucket name/path/truststore.jks',\
                'es.net.ssl.truststore.pass'='***',\
                'es.net.http.auth.user'='admin',\
                'es.net.http.auth.pass'='***')")

         .. note::

            For details about the parameters for creating a CSS datasource connection table, see :ref:`Table 1 <dli_09_0061__en-us_topic_0190067468_table569314388144>`.

      #. Insert data.

         ::

            sparkSession.sql("insert into css_table values(3,'tom')")

      #. Query data.

         ::

            jdbcDF = sparkSession.sql("select * from css_table")
            jdbcDF.show()

      #. View the operation result.

         |image4|

   -  Submitting a Spark job

      #. Upload the Python code file to DLI.
      #. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

         .. note::

            -  When submitting a job, you need to specify a dependency module named **sys.datasource.css**.
            -  For details about how to submit a job on the DLI console, see
            -  For details about how to submit a job through an API, see the **modules** parameter in

-  Complete example code

   -  Connecting to data sources through DataFrame APIs

      .. note::

         Hard-coded or plaintext AK and SK pose significant security risks. To ensure security, encrypt your AK and SK, store them in configuration files or environment variables, and decrypt them when needed.

      ::

         # _*_ coding: utf-8 _*_
         from __future__ import print_function
         from pyspark.sql.types import Row, StructType, StructField, IntegerType, StringType
         from pyspark.sql import SparkSession

         if __name__ == "__main__":
           # Create a SparkSession session.
           sparkSession = SparkSession.builder.appName("datasource-css").getOrCreate()
           sparkSession.conf.set("fs.obs.access.key", ak)
           sparkSession.conf.set("fs.obs.secret.key", sk)
           sparkSession.conf.set("fs.obs.endpoint", enpoint)
           sparkSession.conf.set("fs.obs.connecton.ssl.enabled", "false")

           # Setting cross-source connection parameters
           resource = "/mytest";
           nodes = "to-css-1174404953-hDTx3UPK.datasource.com:9200"

           # Setting schema
           schema = StructType([StructField("id", IntegerType(), False),
                                StructField("name", StringType(), False)])

           # Construction data
           rdd = sparkSession.sparkContext.parallelize([Row(1, "John"), Row(2, "Bob")])

           # Create a DataFrame from RDD and schema
           dataFrame = sparkSession.createDataFrame(rdd, schema)

           # Write data to the CSS
           dataFrame.write.format("css")
             .option("resource", resource)
             .option("es.nodes", nodes)
             .option("es.net.ssl", "true")
             .option("es.net.ssl.keystore.location", "obs://Bucket name/path/transport-keystore.jks")
             .option("es.net.ssl.keystore.pass", "***")
             .option("es.net.ssl.truststore.location", "obs://Bucket name/path/truststore.jks")
             .option("es.net.ssl.truststore.pass", "***")
             .option("es.net.http.auth.user", "admin")
             .option("es.net.http.auth.pass", "***")
             .mode("Overwrite")
             .save()

           # Read data
           jdbcDF = sparkSession.read.format("css")\
             .option("resource", resource)\
             .option("es.nodes", nodes)\
             .option("es.net.ssl", "true")\
             .option("es.net.ssl.keystore.location", "obs://Bucket name/path/transport-keystore.jks")\
             .option("es.net.ssl.keystore.pass", "***")\
             .option("es.net.ssl.truststore.location", "obs://Bucket name/path/truststore.jks")
             .option("es.net.ssl.truststore.pass", "***")\
             .option("es.net.http.auth.user", "admin")\
             .option("es.net.http.auth.pass", "***")\
             .load()
           jdbcDF.show()

           # close session
           sparkSession.stop()

   -  Connecting to data sources through SQL APIs

      ::

         # _*_ coding: utf-8 _*_
         from __future__ import print_function
         from pyspark.sql import SparkSession
         import os

         if __name__ == "__main__":

           # Create a SparkSession session.
           sparkSession = SparkSession.builder.appName("datasource-css").getOrCreate()
           # Create a DLI data table for DLI-associated CSS
           sparkSession.sql("create table css_table(id int, name string) using css options(\
                             'es.nodes'='192.168.6.204:9200',\
                             'es.nodes.wan.only'='true',\
                             'resource'='/mytest',\
                             'es.net.ssl'='true',\
                             'es.net.ssl.keystore.location' = 'obs://xietest1/lzq/keystore.jks',\
                             'es.net.ssl.keystore.pass' = '**',\
                             'es.net.ssl.truststore.location'='obs://xietest1/lzq/truststore.jks',\
                             'es.net.ssl.truststore.pass'='**',\
                             'es.net.http.auth.user'='admin',\
                             'es.net.http.auth.pass'='**')")

           # Insert data into the DLI data table
           sparkSession.sql("insert into css_table values(3,'tom')")

           # Read data from DLI data table
           jdbcDF = sparkSession.sql("select * from css_table")
           jdbcDF.show()

           # close session
           sparkSession.stop()

.. |image1| image:: /_static/images/en-us_image_0266332985.png
.. |image2| image:: /_static/images/en-us_image_0223997308.png
.. |image3| image:: /_static/images/en-us_image_0266332986.png
.. |image4| image:: /_static/images/en-us_image_0266332987.png
