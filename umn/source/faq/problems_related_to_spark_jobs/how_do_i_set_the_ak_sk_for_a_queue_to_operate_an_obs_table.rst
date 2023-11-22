:original_name: dli_03_0017.html

.. _dli_03_0017:

How Do I Set the AK/SK for a Queue to Operate an OBS Table?
===========================================================

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

.. note::

   For security purposes, you are advised not to include the AK and SK information in the OBS path. In addition, if a table is created in the OBS directory, the OBS path specified by the **Path** field cannot contain the AK and SK information.
