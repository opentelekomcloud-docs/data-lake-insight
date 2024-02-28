:original_name: dli_03_0017.html

.. _dli_03_0017:

How Do I Set the AK/SK for a Queue to Operate an OBS Table?
===========================================================

.. note::

   Hard-coded or plaintext AK and SK pose significant security risks. To ensure security, encrypt your AK and SK, store them in configuration files or environment variables, and decrypt them when needed.

-  If the AK and SK are obtained, set the parameters as follows:

   -  Create SparkContext using code

      .. code-block::

         val sc: SparkContext = new SparkContext()
         sc.hadoopConfiguration.set("fs.obs.access.key", ak)
         sc.hadoopConfiguration.set("fs.obs.secret.key", sk)

   -  Create SparkSession using code

      .. code-block::

         val sparkSession: SparkSession = SparkSession
               .builder()
               .config("spark.hadoop.fs.obs.access.key", ak)
               .config("spark.hadoop.fs.obs.secret.key", sk)
               .enableHiveSupport()
               .getOrCreate()

-  If **ak**, **sk**, and **securitytoken** are obtained, the temporary AK/SK and security token must be used at the same time during authentication. The setting is as follows:

   -  Create SparkContext using code

      .. code-block::

         val sc: SparkContext = new SparkContext()
         sc.hadoopConfiguration.set("fs.obs.access.key", ak)
         sc.hadoopConfiguration.set("fs.obs.secret.key", sk)
         sc.hadoopConfiguration.set("fs.obs.session.token", sts)

   -  Create SparkSession using code

      .. code-block::

         val sparkSession: SparkSession = SparkSession
               .builder()
               .config("spark.hadoop.fs.obs.access.key", ak)
               .config("spark.hadoop.fs.obs.secret.key", sk)
               .config("spark.hadoop.fs.obs.session.token", sts)
               .enableHiveSupport()
               .getOrCreate()
