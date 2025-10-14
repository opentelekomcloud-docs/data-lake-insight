:original_name: dli_09_0197.html

.. _dli_09_0197:

Java Example Code
=================

Development Description
-----------------------

This example applies only to MRS HBase.

-  Prerequisites

   A datasource connection has been created and bound to a queue on the DLI management console.

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

            import org.apache.spark.sql.SparkSession;

   #. Create a session.

      ::

         parkSession = SparkSession.builder().appName("datasource-HBase-MRS").getOrCreate();

-  Connecting to data sources through SQL APIs

   -  For clusters with Kerberos authentication disabled

      #. Create a table to connect to an MRS HBase data source and set connection parameters.

         ::

            sparkSession.sql("CREATE TABLE testhbase(id STRING, location STRING, city STRING) using hbase OPTIONS('ZKHost'='10.0.0.63:2181','TableName'='hbtest','RowKey'='id:5','Cols'='location:info.location,city:detail.city') ");

      #. Insert data.

         ::

            sparkSession.sql("insert into testhbase values('12345','abc','xxx')");

      #. Query data.

         ::

            sparkSession.sql("select * from testhbase").show();

   -  For clusters with Kerberos authentication enabled

      #. Create a table to connect to an MRS HBase data source and set connection parameters.

         ::

            sparkSession.sql("CREATE TABLE testhbase(id STRING, location STRING, city STRING) using hbase OPTIONS('ZKHost'='10.0.0.63:2181','TableName'='hbtest','RowKey'='id:5','Cols'='location:info.location,city:detail.city,'krb5conf'='./krb5.conf','keytab'='./user.keytab','principal'='krbtest') ");

         If Kerberos authentication is enabled, you need to set three more parameters, as listed in :ref:`Table 1 <dli_09_0197__table19673142717615>`.

         .. _dli_09_0197__table19673142717615:

         .. table:: **Table 1** Parameter description

            ========================== ===============================
            Parameter and Value        Description
            ========================== ===============================
            'krb5conf' = './krb5.conf' Path of the **krb5.conf** file.
            'keytab'='./user.keytab'   Path of the **keytab** file.
            'principal' ='krbtest'     Authentication username.
            ========================== ===============================

         For details about how to obtain the **krb5.conf** and **keytab** files, see :ref:`Completing Configurations for Enabling Kerberos Authentication <dli_09_0196__section12676527182715>`.

      #. Insert data.

         ::

            sparkSession.sql("insert into testhbase values('95274','abc','Hongkong')");

      #. Query data.

         ::

            sparkSession.sql("select * from testhbase").show();

-  Submitting a Spark job

   #. Generate a JAR package based on the code file and upload the package to DLI.

   #. (Optional) Add the **krb5.conf** and **user.keytab** files to the dependency files of the job when creating a Spark job in an MRS cluster with Kerberos authentication enabled. Skip this step if Kerberos authentication is not enabled for the cluster.

   #. In the Spark job editor, select the corresponding dependency module and execute the Spark job.

      .. note::

         -  If the Spark version is 2.3.2 (will be offline soon) or 2.4.5, specify the **Module** to **sys.datasource.hbase** when you submit a job.

         -  If the Spark version is 3.1.1, you do not need to select a module. Configure **Spark parameters (--conf)**.

            spark.driver.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/hbase/\*

            spark.executor.extraClassPath=/usr/share/extension/dli/spark-jar/datasource/hbase/\*

Complete Example Code
---------------------

-  Connecting to data sources through SQL APIs

   -  Complete example code for the cluster with Kerberos authentication **disabled**

      ::

         import org.apache.spark.sql.SparkSession;

         public class java_mrs_hbase {

             public static void main(String[] args) {
                 //create a SparkSession session
                 SparkSession sparkSession = SparkSession.builder().appName("datasource-HBase-MRS").getOrCreate();

                 sparkSession.sql("CREATE TABLE testhbase(id STRING, location STRING, city STRING) using hbase OPTIONS('ZKHost'='10.0.0.63:2181','TableName'='hbtest','RowKey'='id:5','Cols'='location:info.location,city:detail.city') ");

                 //*****************************SQL model***********************************
                 sparkSession.sql("insert into testhbase values('95274','abc','Hongkong')");
                 sparkSession.sql("select * from testhbase").show();

                 sparkSession.close();
             }
         }

   -  Complete example code for the cluster with Kerberos authentication **enabled**

      ::

         import org.apache.spark.SparkContext;
         import org.apache.spark.SparkFiles;
         import org.apache.spark.sql.SparkSession;
         import java.io.File;
         import java.io.FileInputStream;
         import java.io.FileOutputStream;
         import java.io.IOException;
         import java.io.InputStream;
         import java.io.OutputStream;

         public class Test_HBase_SparkSql_Kerberos {

             private static void copyFile(File src,File dst) throws IOException {
                 InputStream input  = null;
                 OutputStream output = null;
                 try {
                     input = new FileInputStream(src);
                     output = new FileOutputStream(dst);
                     byte[] buf = new byte[1024];
                     int bytesRead;
                     while ((bytesRead = input.read(buf)) > 0) {
                         output.write(buf, 0, bytesRead);
                     }
                 } finally {
                     input.close();
                     output.close();
                 }
             }

             public static void main(String[] args) throws InterruptedException, IOException {
                 SparkSession sparkSession = SparkSession.builder().appName("Test_HBase_SparkSql_Kerberos").getOrCreate();
                 SparkContext sc = sparkSession.sparkContext();
                 sc.addFile("obs://xietest1/lzq/krb5.conf");
                 sc.addFile("obs://xietest1/lzq/user.keytab");
                 Thread.sleep(20);

                 File krb5_startfile = new File(SparkFiles.get("krb5.conf"));
                 File keytab_startfile = new File(SparkFiles.get("user.keytab"));
                 String path_user = System.getProperty("user.dir");
                 File keytab_endfile = new File(path_user + "/" + keytab_startfile.getName());
                 File krb5_endfile = new File(path_user + "/" + krb5_startfile.getName());
                 copyFile(krb5_startfile,krb5_endfile);
                 copyFile(keytab_startfile,keytab_endfile);
                 Thread.sleep(20);

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
                         "'principal'='krbtest') ");

                 //*****************************SQL model***********************************
                 sparkSession.sql("insert into testhbase values('newtest',true,1,2,3,4,5)");
                 sparkSession.sql("select * from testhbase").show();
                 sparkSession.close();
             }
         }
