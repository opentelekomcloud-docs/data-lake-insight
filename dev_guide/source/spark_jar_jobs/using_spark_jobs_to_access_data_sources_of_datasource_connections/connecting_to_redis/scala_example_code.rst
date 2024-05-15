:original_name: dli_09_0094.html

.. _dli_09_0094:

Scala Example Code
==================

Development Description
-----------------------

Redis supports only enhanced datasource connections.

-  Prerequisites

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
         <dependency>
           <groupId>redis.clients</groupId>
           <artifactId>jedis</artifactId>
           <version>3.1.0</version>
         </dependency>
         <dependency>
           <groupId>com.redislabs</groupId>
           <artifactId>spark-redis</artifactId>
           <version>2.4.0</version>
         </dependency>

      Import dependency packages.

      ::

         import org.apache.spark.sql.{Row, SaveMode, SparkSession}
         import org.apache.spark.sql.types._
         import com.redislabs.provider.redis._
         import scala.reflect.runtime.universe._
         import org.apache.spark.{SparkConf, SparkContext}

-  Connecting to data sources through DataFrame APIs

   #. Create a session.

      ::

         val sparkSession = SparkSession.builder().appName("datasource_redis").getOrCreate()

   #. Construct a schema.

      ::

         //method one
         var schema = StructType(Seq(StructField("name", StringType, false), StructField("age", IntegerType, false)))
         var rdd = sparkSession.sparkContext.parallelize(Seq(Row("abc",34),Row("Bob",19)))
         var dataFrame = sparkSession.createDataFrame(rdd, schema)
         // //method two
         // var jdbcDF= sparkSession.createDataFrame(Seq(("Jack",23)))
         // val dataFrame = jdbcDF.withColumnRenamed("_1", "name").withColumnRenamed("_2", "age")
         // //method three
         // case class Person(name: String, age: Int)
         // val dataFrame = sparkSession.createDataFrame(Seq(Person("John", 30), Person("Peter", 45)))

      .. note::

         **case class Person(name: String, age: Int)** must be written outside the object. For details, see :ref:`Connecting to data sources through DataFrame APIs <dli_09_0094__li1640095910911>`.

   #. Import data to Redis.

      ::

         dataFrame .write
           .format("redis")
           .option("host","192.168.4.199")
           .option("port","6379")
           .option("table","person")
           .option("password","******")
           .option("key.column","name")
           .mode(SaveMode.Overwrite)
           .save()

      .. table:: **Table 1** Redis operation parameters

         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                         | Description                                                                                                                                                                                                                                                                                       |
         +===================================+===================================================================================================================================================================================================================================================================================================+
         | host                              | IP address of the Redis cluster to be connected.                                                                                                                                                                                                                                                  |
         |                                   |                                                                                                                                                                                                                                                                                                   |
         |                                   | To obtain the IP address, log in to the official website, search for **redis**, go to the console of Distributed Cache Service for Redis, and choose **Cache Manager**. Select an IP address (including the port information) based on the IP address required by the host name to copy the data. |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | port                              | Access port.                                                                                                                                                                                                                                                                                      |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | password                          | Password for the connection. This parameter is optional if no password is required.                                                                                                                                                                                                               |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | table                             | Key or hash key in Redis.                                                                                                                                                                                                                                                                         |
         |                                   |                                                                                                                                                                                                                                                                                                   |
         |                                   | -  This parameter is mandatory when Redis data is inserted.                                                                                                                                                                                                                                       |
         |                                   | -  Either this parameter or the **keys.pattern** parameter when Redis data is queried.                                                                                                                                                                                                            |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | keys.pattern                      | Use a regular expression to match multiple keys or hash keys. This parameter is used only for query. Either this parameter or **table** is used to query Redis data.                                                                                                                              |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | key.column                        | Key value of a column. This parameter is optional. If a key is specified when data is written, the key must be specified during query. Otherwise, the key will be abnormally loaded during query.                                                                                                 |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | partitions.number                 | Number of concurrent tasks during data reading.                                                                                                                                                                                                                                                   |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | scan.count                        | Number of data records read in each batch. The default value is **100**. If the CPU usage of the Redis cluster still needs to be improved during data reading, increase the value of this parameter.                                                                                              |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | iterator.grouping.size            | Number of data records inserted in each batch. The default value is **100**. If the CPU usage of the Redis cluster still needs to be improved during the insertion, increase the value of this parameter.                                                                                         |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | timeout                           | Timeout interval for connecting to the Redis, in milliseconds. The default value is **2000** (2 seconds).                                                                                                                                                                                         |
         +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

      .. note::

         -  The options of **mode** are **Overwrite**, **Append**, **ErrorIfExis**, and **Ignore**.
         -  To save nested DataFrames, use **.option("model", "binary")**.
         -  Specify the data expiration time by **.option("ttl", 1000)**. The unit is second.

   #. Read data from Redis.

      ::

         sparkSession.read
           .format("redis")
           .option("host","192.168.4.199")
           .option("port","6379")
           .option("table", "person")
           .option("password","######")
           .option("key.column","name")
           .load()
           .show()

-  Connecting to data sources using Spark RDDs

   #. Create a datasource connection.

      ::

         val sparkContext = new SparkContext(new SparkConf()
            .setAppName("datasource_redis")
            .set("spark.redis.host", "192.168.4.199")
            .set("spark.redis.port", "6379")
            .set("spark.redis.auth", "######")
            .set("spark.driver.allowMultipleContexts","true"))

      .. note::

         If **spark.driver.allowMultipleContexts** is set to **true**, only the current context is used when multiple contexts are started, to prevent context invoking conflicts.

   #. Insert data.

      a. Save data in strings.

         ::

            val stringRedisData:RDD[(String,String)] = sparkContext.parallelize(Seq[(String,String)](("high","111"), ("together","333")))
            sparkContext.toRedisKV(stringRedisData)

      b. Save data in hashes.

         ::

            val hashRedisData:RDD[(String,String)] = sparkContext.parallelize(Seq[(String,String)](("saprk","123"), ("data","222")))
            sparkContext.toRedisHASH(hashRedisData, "hashRDD")

      c. Save data in lists.

         ::

            val data = List(("school","112"), ("tom","333"))
            val listRedisData:RDD[String] = sparkContext.parallelize(Seq[(String)](data.toString()))
            sparkContext.toRedisLIST(listRedisData, "listRDD")

      d. Save data in sets.

         ::

            val setData = Set(("bob","133"),("kity","322"))
            val setRedisData:RDD[(String)] = sparkContext.parallelize(Seq[(String)](setData.mkString))
            sparkContext.toRedisSET(setRedisData, "setRDD")

      e. Save data in zsets.

         ::

            val zsetRedisData:RDD[(String,String)] = sparkContext.parallelize(Seq[(String,String)](("whight","234"), ("bobo","343")))
            sparkContext.toRedisZSET(zsetRedisData, "zsetRDD")

   #. Query data.

      a. Query data by traversing keys.

         ::

            val keysRDD = sparkContext.fromRedisKeys(Array("high","together", "hashRDD", "listRDD", "setRDD","zsetRDD"), 6)
            keysRDD.getKV().collect().foreach(println)
            keysRDD.getHash().collect().foreach(println)
            keysRDD.getList().collect().foreach(println)
            keysRDD.getSet().collect().foreach(println)
            keysRDD.getZSet().collect().foreach(println)

      b. Query data by string.

         ::

            sparkContext.fromRedisKV(Array( "high","together")).collect().foreach{println}

      c. Query data by hash.

         ::

            sparkContext.fromRedisHash(Array("hashRDD")).collect().foreach{println}

      d. Query data by list.

         ::

            sparkContext.fromRedisList(Array("listRDD")).collect().foreach{println}

      e. Query data by set.

         ::

            sparkContext.fromRedisSet(Array("setRDD")).collect().foreach{println}

      f. Query data by zset.

         ::

            sparkContext.fromRedisZSet(Array("zsetRDD")).collect().foreach{println}

-  Connecting to data sources through SQL APIs

   #. Create a table to connect to a Redis data source.

      .. code-block::

         sparkSession.sql(
              "CREATE TEMPORARY VIEW person (name STRING, age INT) USING org.apache.spark.sql.redis OPTIONS (
              'host' = '192.168.4.199',
              'port' = '6379',
              'password' = '######',
              table  'person')".stripMargin)

   #. Insert data.

      ::

         sparkSession.sql("INSERT INTO TABLE person VALUES ('John', 30),('Peter', 45)".stripMargin)

   #. Query data.

      ::

         sparkSession.sql("SELECT * FROM person".stripMargin).collect().foreach(println)

-  Submitting a Spark job

   #. Generate a JAR package based on the code and upload the package to DLI.

   #. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

      .. note::

         -  If the Spark version is 2.3.2 (will be offline soon) or 2.4.5, specify the **Module** to **sys.datasource.redis** when you submit a job.

         -  If the Spark version is 3.1.1, you do not need to select a module. Configure **Spark parameters (--conf)**.

            spark.driver.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/redis/\*

            spark.executor.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/redis/\*

Complete Example Code
---------------------

-  Maven dependency

   ::

      <dependency>
        <groupId>org.apache.spark</groupId>
        <artifactId>spark-sql_2.11</artifactId>
        <version>2.3.2</version>
      </dependency>
      <dependency>
        <groupId>redis.clients</groupId>
        <artifactId>jedis</artifactId>
        <version>3.1.0</version>
      </dependency>
      <dependency>
        <groupId>com.redislabs</groupId>
        <artifactId>spark-redis</artifactId>
        <version>2.4.0</version>
      </dependency>

-  Connecting to data sources through SQL APIs

   ::

      import org.apache.spark.sql.{SparkSession};

      object Test_Redis_SQL {
        def main(args: Array[String]): Unit = {
          // Create a SparkSession session.
          val sparkSession = SparkSession.builder().appName("datasource_redis").getOrCreate();

          sparkSession.sql(
            "CREATE TEMPORARY VIEW person (name STRING, age INT) USING org.apache.spark.sql.redis OPTIONS (
               'host' = '192.168.4.199', 'port' = '6379', 'password' = '******',table 'person')".stripMargin)

          sparkSession.sql("INSERT INTO TABLE person VALUES ('John', 30),('Peter', 45)".stripMargin)

          sparkSession.sql("SELECT * FROM person".stripMargin).collect().foreach(println)

          sparkSession.close()
        }
      }

-  .. _dli_09_0094__li1640095910911:

   Connecting to data sources through DataFrame APIs

   ::

      import org.apache.spark.sql.{Row, SaveMode, SparkSession}
      import org.apache.spark.sql.types._

      object Test_Redis_SparkSql {
        def main(args: Array[String]): Unit = {
        // Create a SparkSession session.
        val sparkSession = SparkSession.builder().appName("datasource_redis").getOrCreate()

        // Set cross-source connection parameters.
        val host = "192.168.4.199"
        val port = "6379"
        val table = "person"
        val auth = "######"
        val key_column = "name"

        // ******** setting DataFrame ********
        // method one
        var schema = StructType(Seq(StructField("name", StringType, false),StructField("age", IntegerType, false)))
        var rdd = sparkSession.sparkContext.parallelize(Seq(Row("xxx",34),Row("Bob",19)))
        var dataFrame = sparkSession.createDataFrame(rdd, schema)

      // // method two
      // var jdbcDF= sparkSession.createDataFrame(Seq(("Jack",23)))
      // val dataFrame = jdbcDF.withColumnRenamed("_1", "name").withColumnRenamed("_2", "age")

      // // method three
      // val dataFrame = sparkSession.createDataFrame(Seq(Person("John", 30), Person("Peter", 45)))

        // Write data to redis
        dataFrame.write.format("redis").option("host",host).option("port",port).option("table", table).option("password",auth).mode(SaveMode.Overwrite).save()

        // Read data from redis
        sparkSession.read.format("redis").option("host",host).option("port",port).option("table", table).option("password",auth).load().show()

        // Close session
        sparkSession.close()
        }
      }
      // methoe two
      // case class Person(name: String, age: Int)

-  Connecting to data sources using Spark RDDs

   ::

      import com.redislabs.provider.redis._
      import org.apache.spark.rdd.RDD
      import org.apache.spark.{SparkConf, SparkContext}

      object Test_Redis_RDD {
        def main(args: Array[String]): Unit = {
          // Create a SparkSession session.
          val sparkContext = new SparkContext(new SparkConf()
                .setAppName("datasource_redis")
                .set("spark.redis.host", "192.168.4.199")
                .set("spark.redis.port", "6379")
                .set("spark.redis.auth", "@@@@@@")
                .set("spark.driver.allowMultipleContexts","true"))

          //***************** Write data to redis **********************
          // Save String type data
          val stringRedisData:RDD[(String,String)] = sparkContext.parallelize(Seq[(String,String)](("high","111"), ("together","333")))
          sparkContext.toRedisKV(stringRedisData)

          // Save Hash type data
          val hashRedisData:RDD[(String,String)] = sparkContext.parallelize(Seq[(String,String)](("saprk","123"), ("data","222")))
          sparkContext.toRedisHASH(hashRedisData, "hashRDD")

          // Save List type data
          val data = List(("school","112"), ("tom","333"));
          val listRedisData:RDD[String] = sparkContext.parallelize(Seq[(String)](data.toString()))
          sparkContext.toRedisLIST(listRedisData, "listRDD")

          // Save Set type data
          val setData = Set(("bob","133"),("kity","322"))
          val setRedisData:RDD[(String)] = sparkContext.parallelize(Seq[(String)](setData.mkString))
          sparkContext.toRedisSET(setRedisData, "setRDD")

          // Save ZSet type data
          val zsetRedisData:RDD[(String,String)] = sparkContext.parallelize(Seq[(String,String)](("whight","234"), ("bobo","343")))
          sparkContext.toRedisZSET(zsetRedisData, "zsetRDD")

          // ***************************** Read data from redis *******************************************
          // Traverse the specified key and get the value
          val keysRDD = sparkContext.fromRedisKeys(Array("high","together", "hashRDD", "listRDD", "setRDD","zsetRDD"), 6)
          keysRDD.getKV().collect().foreach(println)
          keysRDD.getHash().collect().foreach(println)
          keysRDD.getList().collect().foreach(println)
          keysRDD.getSet().collect().foreach(println)
          keysRDD.getZSet().collect().foreach(println)

          // Read String type data//
          val stringRDD = sparkContext.fromRedisKV("keyPattern *")
          sparkContext.fromRedisKV(Array( "high","together")).collect().foreach{println}

          // Read Hash type data//
          val hashRDD = sparkContext.fromRedisHash("keyPattern *")
          sparkContext.fromRedisHash(Array("hashRDD")).collect().foreach{println}

          // Read List type data//
          val listRDD = sparkContext.fromRedisList("keyPattern *")
          sparkContext.fromRedisList(Array("listRDD")).collect().foreach{println}

          // Read Set type data//
          val setRDD = sparkContext.fromRedisSet("keyPattern *")
          sparkContext.fromRedisSet(Array("setRDD")).collect().foreach{println}

          // Read ZSet type data//
          val zsetRDD = sparkContext.fromRedisZSet("keyPattern *")
          sparkContext.fromRedisZSet(Array("zsetRDD")).collect().foreach{println}

          // close session
          sparkContext.stop()
        }
      }
