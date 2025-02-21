:original_name: dli_03_0017.html

.. _dli_03_0017:

How Do I Set Up AK/SK So That a General Queue Can Access Tables Stored in OBS?
==============================================================================

Setting Up a Spark Jar Job to Obtain the AK/SK
----------------------------------------------

-  **To obtain the AK/SK, set the parameters as follows:**

   -  Create a SparkContext using code.

      .. code-block::

         val sc: SparkContext = new SparkContext()
         sc.hadoopConfiguration.set("fs.obs.access.key", ak)
         sc.hadoopConfiguration.set("fs.obs.secret.key", sk)

   -  Create a SparkSession using code.

      .. code-block::

         val sparkSession: SparkSession = SparkSession
               .builder()
               .config("spark.hadoop.fs.obs.access.key", ak)
               .config("spark.hadoop.fs.obs.secret.key", sk)
               .enableHiveSupport()
               .getOrCreate()

-  **To obtain the AK/SK and security token and use them together for authentication, set the parameters as follows:**

   -  Create a SparkContext using code.

      .. code-block::

         val sc: SparkContext = new SparkContext()
         sc.hadoopConfiguration.set("fs.obs.access.key", ak)
         sc.hadoopConfiguration.set("fs.obs.secret.key", sk)
         sc.hadoopConfiguration.set("fs.obs.session.token", sts)

   -  Create a SparkSession using code.

      .. code-block::

         val sparkSession: SparkSession = SparkSession
               .builder()
               .config("spark.hadoop.fs.obs.access.key", ak)
               .config("spark.hadoop.fs.obs.secret.key", sk)
               .config("spark.hadoop.fs.obs.session.token", sts)
               .enableHiveSupport()
               .getOrCreate()
