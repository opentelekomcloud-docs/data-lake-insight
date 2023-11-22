:original_name: dli_03_0234.html

.. _dli_03_0234:

Why Does the Flink Submission Fail Due to Hadoop JAR File Conflict?
===================================================================

Symptom
-------

Flink Job submission failed. The exception information is as follows:

.. code-block::

   Caused by: java.lang.RuntimeException: java.lang.ClassNotFoundException: Class org.apache.hadoop.fs.obs.metrics.OBSAMetricsProvider not found
    at org.apache.hadoop.conf.Configuration.getClass(Configuration.java:2664)
    at org.apache.hadoop.conf.Configuration.getClass(Configuration.java:2688)
    ... 31 common frames omitted
    Caused by: java.lang.ClassNotFoundException: Class org.apache.hadoop.fs.obs.metrics.OBSAMetricsProvider not found
    at org.apache.hadoop.conf.Configuration.getClassByName(Configuration.java:2568)
    at org.apache.hadoop.conf.Configuration.getClass(Configuration.java:2662)
    ... 32 common frames omitted

Cause Analysis
--------------

Flink JAR files conflicted. The submitted Flink JAR file conflicted with the HDFS JAR file of the DLI cluster.

Procedure
---------

#. Configure **hadoop-hdfs** in the POM file as follows:

   .. code-block::

       <dependency>
       <groupId>org.apache.hadoop</groupId>
       <artifactId>hadoop-hdfs</artifactId>
       <version>${hadoop.version}</version>
       <scope> provided </scope>
       </dependency>

   Alternatively, use the **exclusions** tag to exclude the association.

#. To use HDFS configuration files, change **core-site.xml**, **hdfs-site.xml**, and **yarn-site.xml** to **mrs-core-site.xml**, **mrs-hdfs-site.xml** and **mrs-hbase-site.xml**, respectively.

   .. code-block::

       conf.addResource(HBaseUtil.class.getClassLoader().getResourceAsStream("mrs-core-site.xml"), false);
       conf.addResource(HBaseUtil.class.getClassLoader().getResourceAsStream("mrs-hdfs-site.xml"), false);
       conf.addResource(HBaseUtil.class.getClassLoader().getResourceAsStream("mrs-hbase-site.xml"), false);
