:original_name: dli_09_0100.html

.. _dli_09_0100:

Java Example Code
=================

Development Description
-----------------------

Redis supports only enhanced datasource connections.

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

         ::

            import org.apache.spark.SparkConf;
            import org.apache.spark.api.java.JavaRDD;
            import org.apache.spark.api.java.JavaSparkContext;
            import org.apache.spark.sql.*;
            import org.apache.spark.sql.types.DataTypes;
            import org.apache.spark.sql.types.StructField;
            import org.apache.spark.sql.types.StructType;
            import java.util.*;

   #. Create a session.

      ::

         SparkConf sparkConf = new SparkConf();
         sparkConf.setAppName("datasource-redis")
                 .set("spark.redis.host", "192.168.4.199")
                 .set("spark.redis.port", "6379")
                 .set("spark.redis.auth", "******")
                 .set("spark.driver.allowMultipleContexts","true");
         JavaSparkContext javaSparkContext = new JavaSparkContext(sparkConf);
         SQLContext sqlContext = new SQLContext(javaSparkContext);

-  Connecting to data sources through DataFrame APIs

   #. Read JSON data as DataFrames.

      ::

         JavaRDD<String> javaRDD = javaSparkContext.parallelize(Arrays.asList(
                 "{\"id\":\"1\",\"name\":\"Ann\",\"age\":\"18\"}",
                 "{\"id\":\"2\",\"name\":\"lisi\",\"age\":\"21\"}"));
         Dataset dataFrame = sqlContext.read().json(javaRDD);

   #. Construct the Redis connection parameters.

      ::

         Map map = new HashMap<String, String>();
         map.put("table","person");
         map.put("key.column","id");

   #. Save data to Redis.

      ::

         dataFrame.write().format("redis").options(map).mode(SaveMode.Overwrite).save();

   #. Read data from Redis.

      ::

         sqlContext.read().format("redis").options(map).load().show();

-  Submitting a Spark job

   #. Upload the Java code file to DLI.

   #. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

      .. note::

         -  If the Spark version is 2.3.2 (will be offline soon) or 2.4.5, specify the **Module** to **sys.datasource.redis** when you submit a job.

         -  If the Spark version is 3.1.1, you do not need to select the Module module. You need to configure the 'Spark parameter (--conf) '.

            spark.driver.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/redis/\*

            spark.executor.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/redis/\*

Complete Example Code
---------------------

::

   public class Test_Redis_DaraFrame {
     public static void main(String[] args) {
       //create a SparkSession session
       SparkConf sparkConf = new SparkConf();
       sparkConf.setAppName("datasource-redis")
                .set("spark.redis.host", "192.168.4.199")
                .set("spark.redis.port", "6379")
                .set("spark.redis.auth", "******")
                .set("spark.driver.allowMultipleContexts","true");
       JavaSparkContext javaSparkContext = new JavaSparkContext(sparkConf);
       SQLContext sqlContext = new SQLContext(javaSparkContext);

       //Read RDD in JSON format to create DataFrame
       JavaRDD<String> javaRDD = javaSparkContext.parallelize(Arrays.asList(
               "{\"id\":\"1\",\"name\":\"Ann\",\"age\":\"18\"}",
               "{\"id\":\"2\",\"name\":\"lisi\",\"age\":\"21\"}"));
       Dataset dataFrame = sqlContext.read().json(javaRDD);

       Map map = new HashMap<String, String>();
       map.put("table","person");
       map.put("key.column","id");
       dataFrame.write().format("redis").options(map).mode(SaveMode.Overwrite).save();
       sqlContext.read().format("redis").options(map).load().show();

     }
   }
