:original_name: dli_09_0063.html

.. _dli_09_0063:

Scala Example Code
==================

Development Description
-----------------------

The CloudTable HBase and MRS HBase can be connected to DLI as data sources.

-  Prerequisites

   A datasource connection has been created on the DLI management console.

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

      Import dependency packages.

      ::

         import scala.collection.mutable
         import org.apache.spark.sql.{Row, SparkSession}
         import org.apache.spark.rdd.RDD
         import org.apache.spark.sql.types._

   #. Create a session.

      ::

         val sparkSession = SparkSession.builder().getOrCreate()

   #. Create a table to connect to an HBase data source.

      -  The sample code is applicable, if Kerberos authentication **is disabled** for the interconnected HBase cluster:

         ::

            sparkSession.sql("CREATE TABLE test_hbase('id' STRING, 'location' STRING, 'city' STRING, 'booleanf' BOOLEAN,
                    'shortf' SHORT, 'intf' INT, 'longf' LONG, 'floatf' FLOAT,'doublef' DOUBLE) using hbase OPTIONS (
                'ZKHost'='cloudtable-cf82-zk3-pa6HnHpf.cloudtable.com:2181,
                              cloudtable-cf82-zk2-weBkIrjI.cloudtable.com:2181,
                              cloudtable-cf82-zk1-WY09px9l.cloudtable.com:2181',
                'TableName'='table_DupRowkey1',
                'RowKey'='id:5,location:6,city:7',
                'Cols'='booleanf:CF1.booleanf,shortf:CF1.shortf,intf:CF1.intf,longf:CF1.longf,floatf:CF1.floatf,doublef:CF1.doublef')"
            )

      -  The sample code is applicable, if Kerberos authentication **is enabled** for the interconnected HBase cluster:

         ::

            sparkSession.sql("CREATE TABLE test_hbase('id' STRING, 'location' STRING, 'city' STRING, 'booleanf' BOOLEAN,
                    'shortf' SHORT, 'intf' INT, 'longf' LONG, 'floatf' FLOAT,'doublef' DOUBLE) using hbase OPTIONS (
                'ZKHost'='cloudtable-cf82-zk3-pa6HnHpf.cloudtable.com:2181,
                              cloudtable-cf82-zk2-weBkIrjI.cloudtable.com:2181,
                              cloudtable-cf82-zk1-WY09px9l.cloudtable.com:2181',
                'TableName'='table_DupRowkey1',
                'RowKey'='id:5,location:6,city:7',
                'Cols'='booleanf:CF1.booleanf,shortf:CF1.shortf,intf:CF1.intf,longf:CF1.longf,floatf:CF1.floatf,doublef:CF1.doublef',
                'krb5conf'='./krb5.conf',
                'keytab' = './user.keytab',
                'principal' = 'krbtest')")

      .. _dli_09_0063__table15979164115531:

      .. table:: **Table 1** Parameters for creating a table

         +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Parameter                         | Description                                                                                                                                                                                                                                                                                                                                  |
         +===================================+==============================================================================================================================================================================================================================================================================================================================================+
         | ZKHost                            | ZooKeeper IP address of the HBase cluster.                                                                                                                                                                                                                                                                                                   |
         |                                   |                                                                                                                                                                                                                                                                                                                                              |
         |                                   | You need to create a datasource connection first.                                                                                                                                                                                                                                                                                            |
         |                                   |                                                                                                                                                                                                                                                                                                                                              |
         |                                   | -  To access the CloudTable cluster, specify the ZooKeeper connection address in the internal network.                                                                                                                                                                                                                                       |
         |                                   | -  To access the MRS cluster, specify the IP addresses and port numbers of the ZooKeeper nodes. The format is as follows: ZK_IP1:ZK_PORT1,ZK_IP2:ZK_PORT2                                                                                                                                                                                    |
         +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | RowKey                            | Row key field of the table connected to DLI. The single and composite row keys are supported. A single row key can be of the numeric or string type. The length does not need to be specified. The composite row key supports only fixed-length data of the string type. The format is **attribute name 1:Length, attribute name 2:Length**. |
         +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | Cols                              | Mapping between the fields in the DLI table and the CloudTable table. In this mapping, the DLI table field is placed before the colon (:) and the CloudTable table field is placed after the colon (:). The period (.) is used to separate the column family and column name of the CloudTable table.                                        |
         |                                   |                                                                                                                                                                                                                                                                                                                                              |
         |                                   | For example: **DLI table field 1:CloudTable table.CloudTable table field 1**, **DLI table field 2:CloudTable table.CloudTable table field 2**, **DLI table field 3:CLoudTable table.CloudTable table field 3**                                                                                                                               |
         +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | krb5conf                          | Path of the **krb5.conf** file. This parameter is required when Kerberos authentication is enabled. The format is './krb5.conf'. For details, see :ref:`Completing Configurations for Enabling Kerberos Authentication <dli_09_0196__section12676527182715>`.                                                                                |
         +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | keytab                            | Path of the **keytab** file. This parameter is required when Kerberos authentication is enabled. The format is './user.keytab.'. For details, see :ref:`Completing Configurations for Enabling Kerberos Authentication <dli_09_0196__section12676527182715>`.                                                                                |
         +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
         | principal                         | Username created for Kerberos authentication.                                                                                                                                                                                                                                                                                                |
         +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Accessing a Data Source Using a SQL API
---------------------------------------

#. Insert data.

   ::

      sparkSession.sql("insert into test_hbase values('12345','abc','guiyang',false,null,3,23,2.3,2.34)")

#. Query data.

   ::

      sparkSession.sql("select * from test_hbase").show ()

Accessing a Data Source Using a DataFrame API
---------------------------------------------

#. Construct a schema.

   ::

      val attrId = new StructField("id",StringType)
      val location = new StructField("location",StringType)
      val city = new StructField("city",StringType)
      val booleanf = new StructField("booleanf",BooleanType)
      val shortf = new StructField("shortf",ShortType)
      val intf = new StructField("intf",IntegerType)
      val longf = new StructField("longf",LongType)
      val floatf = new StructField("floatf",FloatType)
      val doublef = new StructField("doublef",DoubleType)
      val attrs = Array(attrId, location,city,booleanf,shortf,intf,longf,floatf,doublef)

#. Construct data based on the schema type.

   ::

      val mutableRow: Seq[Any] = Seq("12345","abc","city1",false,null,3,23,2.3,2.34)
      val rddData: RDD[Row] = sparkSession.sparkContext.parallelize(Array(Row.fromSeq(mutableRow)), 1)

#. Import data to HBase.

   ::

      sparkSession.createDataFrame(rddData, new StructType(attrs)).write.insertInto("test_hbase")

#. Read data from HBase.

   ::

      val map = new mutable.HashMap[String, String]()
      map("TableName") = "table_DupRowkey1"
      map("RowKey") = "id:5,location:6,city:7"
      map("Cols") = "booleanf:CF1.booleanf,shortf:CF1.shortf,intf:CF1.intf,longf:CF1.longf,floatf:CF1.floatf,doublef:CF1.doublef"
      map("ZKHost")="cloudtable-cf82-zk3-pa6HnHpf.cloudtable.com:2181,
                     cloudtable-cf82-zk2-weBkIrjI.cloudtable.com:2181,
                     cloudtable-cf82-zk1-WY09px9l.cloudtable.com:2181"
      sparkSession.read.schema(new StructType(attrs)).format("hbase").options(map.toMap).load().show()

Submitting a Spark Job
----------------------

#. Generate a JAR package based on the code and upload the package to DLI.

#. (Optional) Add the **krb5.conf** and **user.keytab** files to other dependency files of the job when creating a Spark job in an MRS cluster with Kerberos authentication enabled. Skip this step if Kerberos authentication is not enabled for the cluster.

#. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

   .. note::

      -  If the Spark version is 2.3.2 (will be offline soon) or 2.4.5, set **Module** to **sys.datasource.hbase** when you submit a job.

      -  If the Spark version is 3.1.1, you do not need to select a module. Set **Spark parameters (--conf)**.

         spark.driver.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/hbase/\*

         spark.executor.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/hbase/\*

Complete Example Code
---------------------

-  Maven dependency

   ::

      <dependency>
        <groupId>org.apache.spark</groupId>
        <artifactId>spark-sql_2.11</artifactId>
        <version>2.3.2</version>
      </dependency>

-  Connecting to data sources through SQL APIs

   -  Sample code when Kerberos authentication is disabled

      ::

         import org.apache.spark.sql.SparkSession

         object Test_SparkSql_HBase {
           def main(args: Array[String]): Unit = {
             // Create a SparkSession session.
             val sparkSession = SparkSession.builder().getOrCreate()

             /**
              * Create an association table for the DLI association Hbase table
              */
             sparkSession.sql("CREATE TABLE test_hbase('id' STRING, 'location' STRING, 'city' STRING, 'booleanf' BOOLEAN,
                 'shortf' SHORT, 'intf' INT, 'longf' LONG, 'floatf' FLOAT,'doublef' DOUBLE) using hbase OPTIONS (
             'ZKHost'='cloudtable-cf82-zk3-pa6HnHpf.cloudtable.com:2181,
                       cloudtable-cf82-zk2-weBkIrjI.cloudtable.com:2181,
                       cloudtable-cf82-zk1-WY09px9l.cloudtable.com:2181',
             'TableName'='table_DupRowkey1',
             'RowKey'='id:5,location:6,city:7',
             'Cols'='booleanf:CF1.booleanf,shortf:CF1.shortf,intf:CF1.intf,
                 longf:CF1.longf,floatf:CF1.floatf,doublef:CF1.doublef')")

             //*****************************SQL model***********************************
             sparkSession.sql("insert into test_hbase values('12345','abc','city1',false,null,3,23,2.3,2.34)")
             sparkSession.sql("select * from test_hbase").collect()

             sparkSession.close()
           }
         }

   -  Sample code when Kerberos authentication is enabled

      .. code-block::

         import org.apache.spark.SparkFiles
         import org.apache.spark.sql.SparkSession

         import java.io.{File, FileInputStream, FileOutputStream}

         object Test_SparkSql_HBase_Kerberos {

           def copyFile2(Input:String)(OutPut:String): Unit ={
             val fis = new FileInputStream(Input)
             val fos = new FileOutputStream(OutPut)
             val buf = new Array[Byte](1024)
             var len = 0
             while ({len = fis.read(buf);len} != -1){
               fos.write(buf,0,len)
             }
             fos.close()
             fis.close()
           }

           def main(args: Array[String]): Unit = {
             // Create a SparkSession session.
             val sparkSession = SparkSession.builder().getOrCreate()
             val sc = sparkSession.sparkContext
             sc.addFile("OBS address of krb5.conf")
             sc.addFile("OBS address of user.keytab")
             Thread.sleep(10)

             val krb5_startfile = new File(SparkFiles.get("krb5.conf"))
             val keytab_startfile = new File(SparkFiles.get("user.keytab"))
             val path_user = System.getProperty("user.dir")
             val keytab_endfile = new File(path_user + "/" + keytab_startfile.getName)
             val krb5_endfile = new File(path_user + "/" + krb5_startfile.getName)
             println(keytab_endfile)
             println(krb5_endfile)

             var krbinput = SparkFiles.get("krb5.conf")
             var krboutput = path_user+"/krb5.conf"
             copyFile2(krbinput)(krboutput)

             var keytabinput = SparkFiles.get("user.keytab")
             var keytaboutput = path_user+"/user.keytab"
             copyFile2(keytabinput)(keytaboutput)
             Thread.sleep(10)
             /**
              * Create an association table for the DLI association Hbase table
              */
             sparkSession.sql("CREATE TABLE testhbase(id string,booleanf boolean,shortf short,intf int,longf long,floatf float,doublef double) " +
               "using hbase OPTIONS(" +
               "'ZKHost'='10.0.0.146:2181'," +
               "'TableName'='hbtest'," +
               "'RowKey'='id:100'," +
               "'Cols'='booleanf:CF1.booleanf,shortf:CF1.shortf,intf:CF1.intf,longf:CF2.longf,floatf:CF1.floatf,doublef:CF2.doublef'," +
               "'krb5conf'='" + path_user + "/krb5.conf'," +
               "'keytab'='" + path_user+ "/user.keytab'," +
               "'principal'='krbtest') ")

           //*****************************SQL model***********************************
           sparkSession.sql("insert into testhbase values('newtest',true,1,2,3,4,5)")
           val result = sparkSession.sql("select * from testhbase")
           result.show()

           sparkSession.close()
           }
         }

-  Connecting to data sources through DataFrame APIs

   ::

      import scala.collection.mutable

      import org.apache.spark.sql.{Row, SparkSession}
      import org.apache.spark.rdd.RDD
      import org.apache.spark.sql.types._

      object Test_SparkSql_HBase {
        def main(args: Array[String]): Unit = {
          // Create a SparkSession session.
          val sparkSession = SparkSession.builder().getOrCreate()

          // Create an association table for the DLI association Hbase table
          sparkSession.sql("CREATE TABLE test_hbase('id' STRING, 'location' STRING, 'city' STRING, 'booleanf' BOOLEAN,
              'shortf' SHORT, 'intf' INT, 'longf' LONG, 'floatf' FLOAT,'doublef' DOUBLE) using hbase OPTIONS (
          'ZKHost'='cloudtable-cf82-zk3-pa6HnHpf.cloudtable.com:2181,
                    cloudtable-cf82-zk2-weBkIrjI.cloudtable.com:2181,
                    cloudtable-cf82-zk1-WY09px9l.cloudtable.com:2181',
          'TableName'='table_DupRowkey1',
          'RowKey'='id:5,location:6,city:7',
          'Cols'='booleanf:CF1.booleanf,shortf:CF1.shortf,intf:CF1.intf,longf:CF1.longf,floatf:CF1.floatf,doublef:CF1.doublef')")

          //*****************************DataFrame model***********************************
          // Setting schema
          val attrId = new StructField("id",StringType)
          val location = new StructField("location",StringType)
          val city = new StructField("city",StringType)
          val booleanf = new StructField("booleanf",BooleanType)
          val shortf = new StructField("shortf",ShortType)
          val intf = new StructField("intf",IntegerType)
          val longf = new StructField("longf",LongType)
          val floatf = new StructField("floatf",FloatType)
          val doublef = new StructField("doublef",DoubleType)
          val attrs = Array(attrId, location,city,booleanf,shortf,intf,longf,floatf,doublef)

          // Populate data according to the type of schema
          val mutableRow: Seq[Any] = Seq("12345","abc","city1",false,null,3,23,2.3,2.34)
          val rddData: RDD[Row] = sparkSession.sparkContext.parallelize(Array(Row.fromSeq(mutableRow)), 1)

          // Import the constructed data into Hbase
          sparkSession.createDataFrame(rddData, new StructType(attrs)).write.insertInto("test_hbase")

          // Read data on Hbase
          val map = new mutable.HashMap[String, String]()
          map("TableName") = "table_DupRowkey1"
          map("RowKey") = "id:5,location:6,city:7"
          map("Cols") = "booleanf:CF1.booleanf,shortf:CF1.shortf,intf:CF1.intf,longf:CF1.longf,floatf:CF1.floatf,doublef:CF1.doublef"
          map("ZKHost")="cloudtable-cf82-zk3-pa6HnHpf.cloudtable.com:2181,
                         cloudtable-cf82-zk2-weBkIrjI.cloudtable.com:2181,
                         cloudtable-cf82-zk1-WY09px9l.cloudtable.com:2181"
          sparkSession.read.schema(new StructType(attrs)).format("hbase").options(map.toMap).load().collect()

          sparkSession.close()
        }
      }
