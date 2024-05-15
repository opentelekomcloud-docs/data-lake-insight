:original_name: dli_09_0078.html

.. _dli_09_0078:

PySpark Example Code
====================

Development Description
-----------------------

The CloudTable HBase and MRS HBase can be connected to DLI as data sources.

-  Prerequisites

   A datasource connection has been created on the DLI management console.

   .. note::

      Hard-coded or plaintext passwords pose significant security risks. To ensure security, encrypt your passwords, store them in configuration files or environment variables, and decrypt them when needed.

-  Code implementation

   #. Import dependency packages.

      ::

         from __future__ import print_function
         from pyspark.sql.types import StructType, StructField, IntegerType, StringType, BooleanType, ShortType, LongType, FloatType, DoubleType
         from pyspark.sql import SparkSession

   #. Create a session.

      ::

         sparkSession = SparkSession.builder.appName("datasource-hbase").getOrCreate()

-  Connecting to data sources through SQL APIs

   #. Create a table to connect to an HBase data source.

      -  The sample code is applicable, if Kerberos authentication **is disabled** for the interconnected HBase cluster:

         .. code-block::

            sparkSession.sql(
                "CREATE TABLE testhbase(id STRING, location STRING, city STRING) using hbase OPTIONS (\
                'ZKHost' = '192.168.0.189:2181',\
                'TableName' = 'hbtest',\
                'RowKey' = 'id:5',\
                'Cols' = 'location:info.location,city:detail.city')")

      -  The sample code is applicable, if Kerberos authentication **is enabled** for the interconnected HBase cluster:

         .. code-block::

            sparkSession.sql(
                "CREATE TABLE testhbase(id STRING, location STRING, city STRING) using hbase OPTIONS (\
                'ZKHost' = '192.168.0.189:2181',\
                'TableName' = 'hbtest',\
                'RowKey' = 'id:5',\
                'Cols' = 'location:info.location,city:detail.city',\
                'krb5conf' = './krb5.conf',\
                'keytab'='./user.keytab',\
                'principal' ='krbtest')")

         If Kerberos authentication is enabled, you need to set three more parameters, as listed in :ref:`Table 1 <dli_09_0078__table8162174602419>`.

         .. _dli_09_0078__table8162174602419:

         .. table:: **Table 1** Description

            ========================== ===============================
            Parameter and Value        Description
            ========================== ===============================
            'krb5conf' = './krb5.conf' Path of the **krb5.conf** file.
            'keytab'='./user.keytab'   Path of the **keytab** file.
            'principal' ='krbtest'     Authentication username.
            ========================== ===============================

         For details about how to obtain the **krb5.conf** and **keytab** files, see :ref:`Completing Configurations for Enabling Kerberos Authentication <dli_09_0196__section12676527182715>`.

         .. note::

            For details about parameters in the table, see :ref:`Table 1 <dli_09_0063__table15979164115531>`.

   #. Import data to HBase.

      .. code-block::

         sparkSession.sql("insert into testhbase values('95274','abc','Jinan')")

   #. Read data from HBase.

      .. code-block::

         sparkSession.sql("select * from testhbase").show()

-  Connecting to data sources through DataFrame APIs

   #. Create a table to connect to an HBase data source.

      ::

         sparkSession.sql(\
           "CREATE TABLE test_hbase(id STRING, location STRING, city STRING, booleanf BOOLEAN, shortf SHORT, intf INT, longf LONG,
              floatf FLOAT, doublef DOUBLE) using hbase OPTIONS (\
            'ZKHost' = 'cloudtable-cf82-zk3-pa6HnHpf.cloudtable.com:2181,\
                        cloudtable-cf82-zk2-weBkIrjI.cloudtable.com:2181,\
                        cloudtable-cf82-zk1-WY09px9l.cloudtable.com:2181',\
            'TableName' = 'table_DupRowkey1',\
            'RowKey' = 'id:5,location:6,city:7',\
            'Cols' = 'booleanf:CF1.booleanf, shortf:CF1.shortf, intf:CF1.intf, \  longf:CF1.longf, floatf:CF1.floatf, doublef:CF1.doublef')")

      .. note::

         -  For details about the **ZKHost**, **RowKey**, and **Cols** parameters, see :ref:`Table 1 <dli_09_0063__table15979164115531>`.
         -  **TableName**: Name of a table in the CloudTable file. If no table name exists, the system automatically creates one.

   #. Construct a schema.

      ::

         schema = StructType([StructField("id", StringType()),\
                              StructField("location", StringType()),\
                              StructField("city", StringType()),\
                              StructField("booleanf", BooleanType()),\
                              StructField("shortf", ShortType()),\
                              StructField("intf", IntegerType()),\
                              StructField("longf", LongType()),\
                              StructField("floatf", FloatType()),\
                              StructField("doublef", DoubleType())])

   #. Set data.

      ::

         dataList = sparkSession.sparkContext.parallelize([("11111", "aaa", "aaa", False, 4, 3, 23, 2.3, 2.34)])

   #. Create a DataFrame.

      ::

         dataFrame = sparkSession.createDataFrame(dataList, schema)

   #. Import data to HBase.

      ::

         dataFrame.write.insertInto("test_hbase")

   #. Read data from HBase.

      ::

         // Set cross-source connection parameters
         TableName = "table_DupRowkey1"
         RowKey = "id:5,location:6,city:7"
         Cols = "booleanf:CF1.booleanf,shortf:CF1.shortf,intf:CF1.intf,longf:CF1.longf,floatf:CF1.floatf,doublef:CF1.doublef"
         ZKHost = "cloudtable-cf82-zk3-pa6HnHpf.cloudtable.com:2181,cloudtable-cf82-zk2-weBkIrjI.cloudtable.com:2181,
                   cloudtable-cf82-zk1- WY09px9l.cloudtable.com:2181"

         // select
         jdbcDF = sparkSession.read.schema(schema)\
                          .format("hbase")\
                          .option("ZKHost",ZKHost)\
                          .option("TableName",TableName)\
                          .option("RowKey",RowKey)\
                          .option("Cols",Cols)\
                          .load()
         jdbcDF.filter("id = '12333' or id='11111'").show()

      .. note::

         The length of **id**, **location**, and **city** parameter is limited. When inserting data, you must set the data values based on the required length. Otherwise, an encoding format error occurs during query.

-  Submitting a Spark job

   #. Upload the Python code file to DLI.

   #. (Optional) Add the **krb5.conf** and **user.keytab** files to other dependency files of the job when creating a Spark job in an MRS cluster with Kerberos authentication enabled. Skip this step if Kerberos authentication is not enabled for the cluster.

   #. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

      .. note::

         -  If the Spark version is 2.3.2 (will be offline soon) or 2.4.5, specify the **Module** to **sys.datasource.hbase** when you submit a job.

         -  If the Spark version is 3.1.1, you do not need to select a module. Configure **Spark parameters (--conf)**.

            spark.driver.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/hbase/\*

            spark.executor.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/hbase/\*

Complete Example Code
---------------------

-  Connecting to MRS HBase through SQL APIs

   -  Sample code when Kerberos authentication is **disabled**

      .. code-block::

         # _*_ coding: utf-8 _*_
         from __future__ import print_function
         from pyspark.sql.types import StructType, StructField, IntegerType, StringType, BooleanType, ShortType, LongType, FloatType, DoubleType
         from pyspark.sql import SparkSession

         if __name__ == "__main__":
           # Create a SparkSession session.
           sparkSession = SparkSession.builder.appName("datasource-hbase").getOrCreate()

           sparkSession.sql(
             "CREATE TABLE testhbase(id STRING, location STRING, city STRING) using hbase OPTIONS (\
             'ZKHost' = '192.168.0.189:2181',\
             'TableName' = 'hbtest',\
             'RowKey' = 'id:5',\
             'Cols' = 'location:info.location,city:detail.city')")


           sparkSession.sql("insert into testhbase values('95274','abc','Jinan')")

           sparkSession.sql("select * from testhbase").show()
           # close session
           sparkSession.stop()

   -  Sample code when Kerberos authentication is **enabled**

      .. code-block::

         # _*_ coding: utf-8 _*_
         from __future__ import print_function
         from pyspark import SparkFiles
         from pyspark.sql import SparkSession
         import shutil
         import time
         import os

         if __name__ == "__main__":
             # Create a SparkSession session.
             sparkSession = SparkSession.builder.appName("Test_HBase_SparkSql_Kerberos").getOrCreate()
             sc = sparkSession.sparkContext
             time.sleep(10)

             krb5_startfile = SparkFiles.get("krb5.conf")
             keytab_startfile = SparkFiles.get("user.keytab")
             path_user = os.getcwd()
             krb5_endfile = path_user + "/" + "krb5.conf"
             keytab_endfile = path_user + "/" + "user.keytab"
             shutil.copy(krb5_startfile, krb5_endfile)
             shutil.copy(keytab_startfile, keytab_endfile)
             time.sleep(20)

             sparkSession.sql(
               "CREATE TABLE testhbase(id string,booleanf boolean,shortf short,intf int,longf long,floatf float,doublef double) " +
               "using hbase OPTIONS(" +
               "'ZKHost'='10.0.0.146:2181'," +
               "'TableName'='hbtest'," +
               "'RowKey'='id:100'," +
               "'Cols'='booleanf:CF1.booleanf,shortf:CF1.shortf,intf:CF1.intf,longf:CF2.longf,floatf:CF1.floatf,doublef:CF2.doublef'," +
               "'krb5conf'='" + path_user + "/krb5.conf'," +
               "'keytab'='" + path_user+ "/user.keytab'," +
               "'principal'='krbtest') ")

               sparkSession.sql("insert into testhbase values('95274','abc','Jinan')")

             sparkSession.sql("select * from testhbase").show()
             # close session
             sparkSession.stop()

-  Connecting to HBase through DataFrame APIs

   .. code-block::

      # _*_ coding: utf-8 _*_
      from __future__ import print_function
      from pyspark.sql.types import StructType, StructField, IntegerType, StringType, BooleanType, ShortType, LongType, FloatType, DoubleType
      from pyspark.sql import SparkSession

      if __name__ == "__main__":
        # Create a SparkSession session.
        sparkSession = SparkSession.builder.appName("datasource-hbase").getOrCreate()

        # Createa data table for DLI-associated ct
        sparkSession.sql(\
         "CREATE TABLE test_hbase(id STRING, location STRING, city STRING, booleanf BOOLEAN, shortf SHORT, intf INT, longf LONG,floatf FLOAT,doublef DOUBLE) using hbase OPTIONS ( \
          'ZKHost' = 'cloudtable-cf82-zk3-pa6HnHpf.cloudtable.com:2181,\
                      cloudtable-cf82-zk2-weBkIrjI.cloudtable.com:2181,\
                      cloudtable-cf82-zk1-WY09px9l.cloudtable.com:2181',\
          'TableName' = 'table_DupRowkey1',\
          'RowKey' = 'id:5,location:6,city:7',\
          'Cols' = 'booleanf:CF1.booleanf,shortf:CF1.shortf,intf:CF1.intf,longf:CF1.longf,floatf:CF1.floatf,doublef:CF1.doublef')")

        # Create a DataFrame and initialize the DataFrame data.
        dataList = sparkSession.sparkContext.parallelize([("11111", "aaa", "aaa", False, 4, 3, 23, 2.3, 2.34)])

        # Setting schema
        schema = StructType([StructField("id", StringType()),
                             StructField("location", StringType()),
                             StructField("city", StringType()),
                             StructField("booleanf", BooleanType()),
                             StructField("shortf", ShortType()),
                             StructField("intf", IntegerType()),
                             StructField("longf", LongType()),
                             StructField("floatf", FloatType()),
                             StructField("doublef", DoubleType())])

        # Create a DataFrame from RDD and schema
        dataFrame = sparkSession.createDataFrame(dataList, schema)

        # Write data to the cloudtable-hbase
        dataFrame.write.insertInto("test_hbase")

        # Set cross-source connection parameters
        TableName = "table_DupRowkey1"
        RowKey = "id:5,location:6,city:7"
        Cols = "booleanf:CF1.booleanf,shortf:CF1.shortf,intf:CF1.intf,longf:CF1.longf,floatf:CF1.floatf,doublef:CF1.doublef"
        ZKHost = "cloudtable-cf82-zk3-pa6HnHpf.cloudtable.com:2181,cloudtable-cf82-zk2-weBkIrjI.cloudtable.com:2181,
                  cloudtable-cf82-zk1-WY09px9l.cloudtable.com:2181"
        # Read data on CloudTable-HBase
        jdbcDF = sparkSession.read.schema(schema)\
                             .format("hbase")\
                             .option("ZKHost", ZKHost)\
                             .option("TableName",TableName)\
                             .option("RowKey", RowKey)\
                             .option("Cols", Cols)\
                             .load()
        jdbcDF.filter("id = '12333' or id='11111'").show()

        # close session
        sparkSession.stop()
