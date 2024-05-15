:original_name: dli_09_0110.html

.. _dli_09_0110:

Java Example Code
=================

Development Description
-----------------------

Mongo can be connected only through enhanced datasource connections.

.. note::

   DDS is compatible with the MongoDB protocol.

-  Prerequisites

   An enhanced datasource connection has been created on the DLI management console and bound to a queue in packages.

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

         .. code-block::

            import org.apache.spark.SparkConf;
            import org.apache.spark.SparkContext;
            import org.apache.spark.api.java.JavaRDD;
            import org.apache.spark.api.java.JavaSparkContext;
            import org.apache.spark.sql.Dataset;
            import org.apache.spark.sql.Row;
            import org.apache.spark.sql.SQLContext;
            import org.apache.spark.sql.SaveMode;

   #. Create a session.

      ::

         SparkContext sparkContext = new SparkContext(new SparkConf().setAppName("datasource-mongo"));
         JavaSparkContext javaSparkContext = new JavaSparkContext(sparkContext);
         SQLContext sqlContext = new SQLContext(javaSparkContext);

-  Connecting to data sources through DataFrame APIs

   #. Read JSON data as DataFrames.

      .. code-block::

         JavaRDD<String> javaRDD = javaSparkContext.parallelize(Arrays.asList("{\"id\":\"5\",\"name\":\"Ann\",\"age\":\"23\"}"));
         Dataset<Row> dataFrame = sqlContext.read().json(javaRDD);

   #. Set connection parameters.

      .. code-block::

         String url = "192.168.4.62:8635,192.168.5.134:8635/test?authSource=admin";
         String uri = "mongodb://username:pwd@host:8635/db";
         String user = "rwuser";
         String database = "test";
         String collection = "test";
         String password = "######";

      .. note::

         For details about the parameters, see :ref:`Table 1 <dli_09_0114__en-us_topic_0204096844_table2072415395012>`.

   #. Import data to Mongo.

      .. code-block::

         dataFrame.write().format("mongo")
              .option("url",url)
              .option("uri",uri)
              .option("database",database)
              .option("collection",collection)
              .option("user",user)
              .option("password",password)
              .mode(SaveMode.Overwrite)
              .save();

   #. Read data from Mongo.

      ::

         sqlContext.read().format("mongo")
             .option("url",url)
             .option("uri",uri)
             .option("database",database)
             .option("collection",collection)
             .option("user",user)
             .option("password",password)
             .load().show();

-  Submitting a Spark job

   #. Upload the Java code file to DLI.

   #. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

      .. note::

         -  If the Spark version is 2.3.2 (will be offline soon) or 2.4.5, specify the **Module** to **sys.datasource.mongo** when you submit a job.

         -  If the Spark version is 3.1.1, you do not need to select a module. Configure **Spark parameters (--conf)**.

            spark.driver.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/mongo/\*

            spark.executor.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/mongo/\*

Complete Example Code
---------------------

::

   import org.apache.spark.SparkConf;
   import org.apache.spark.SparkContext;
   import org.apache.spark.api.java.JavaRDD;
   import org.apache.spark.api.java.JavaSparkContext;
   import org.apache.spark.sql.Dataset;
   import org.apache.spark.sql.Row;
   import org.apache.spark.sql.SQLContext;
   import org.apache.spark.sql.SaveMode;
   import java.util.Arrays;

   public class TestMongoSparkSql {
     public static void main(String[] args) {
       SparkContext sparkContext = new SparkContext(new SparkConf().setAppName("datasource-mongo"));
       JavaSparkContext javaSparkContext = new JavaSparkContext(sparkContext);
       SQLContext sqlContext = new SQLContext(javaSparkContext);

   //    // Read json file as DataFrame, read csv / parquet file, same as json file distribution
   //    DataFrame dataFrame = sqlContext.read().format("json").load("filepath");

       // Read RDD in JSON format to create DataFrame
       JavaRDD<String> javaRDD = javaSparkContext.parallelize(Arrays.asList("{\"id\":\"5\",\"name\":\"Ann\",\"age\":\"23\"}"));
       Dataset<Row> dataFrame = sqlContext.read().json(javaRDD);

       String url = "192.168.4.62:8635,192.168.5.134:8635/test?authSource=admin";
       String uri = "mongodb://username:pwd@host:8635/db";
       String user = "rwuser";
       String database = "test";
       String collection = "test";
       String password = "######";

       dataFrame.write().format("mongo")
               .option("url",url)
               .option("uri",uri)
               .option("database",database)
               .option("collection",collection)
               .option("user",user)
               .option("password",password)
               .mode(SaveMode.Overwrite)
               .save();

       sqlContext.read().format("mongo")
               .option("url",url)
               .option("uri",uri)
               .option("database",database)
               .option("collection",collection)
               .option("user",user)
               .option("password",password)
               .load().show();
       sparkContext.stop();
       javaSparkContext.close();
     }
   }
