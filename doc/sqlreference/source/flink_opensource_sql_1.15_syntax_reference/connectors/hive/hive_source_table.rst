:original_name: dli_08_15049.html

.. _dli_08_15049:

Hive Source Table
=================

Introduction
------------

`Apache Hive <https://hive.apache.org/>`__ has established itself as a focal point of the data warehousing ecosystem. It serves as not only a SQL engine for big data analytics and ETL, but also a data management platform, where data is discovered, defined, and evolved.

Flink offers a two-fold integration with Hive. The first is to leverage Hive's Metastore as a persistent catalog. The second is to offer Flink as an alternative engine for reading and writing Hive tables. `Overview \| Apache Flink <https://nightlies.apache.org/flink/flink-docs-release-1.15/zh/docs/connectors/table/hive/overview/>`__

Starting from 1.11.0, Flink allows users to write SQL statements in Hive syntax when Hive dialect is used. By providing compatibility with Hive syntax, we aim to improve the interoperability with Hive and reduce the scenarios when users need to switch between Flink and Hive in order to execute different statements. For details, see `Apache Flink Hive Dialect <https://nightlies.apache.org/flink/flink-docs-release-1.11/dev/table/hive/hive_dialect.html>`__.

Using the HiveCatalog, Apache Flink can be used for unified BATCH and STREAM processing of Apache Hive Tables. This means Flink can be used as a more performant alternative to Hive's batch engine, or to continuously read and write data into and out of Hive tables to power real-time data warehousing applications. `Apache Flink Hive Read & Write <https://nightlies.apache.org/flink/flink-docs-release-1.15/zh/docs/connectors/table/hive/hive_read_write/>`__

Function
--------

This section describes how to use Flink to read and write Hive tables, the definition of the Hive source table, parameters used for creating the source table, and sample code. For details, see `Apache Flink Hive Read & Write <https://nightlies.apache.org/flink/flink-docs-release-1.15/zh/docs/connectors/table/hive/hive_read_write/>`__.

Flink supports reading data from Hive in both **BATCH** and **STREAMING** modes. When running as a **BATCH** application, Flink will execute its query over the state of the table at the point in time when the query is executed. **STREAMING** reads will continuously monitor the table and incrementally fetch new data as it is made available. Flink will read tables as bounded by default.

**STREAMING** reads support consuming both partitioned and non-partitioned tables. For partitioned tables, Flink will monitor the generation of new partitions, and read them incrementally when available. For non-partitioned tables, Flink will monitor the generation of new files in the folder and read new files incrementally.

Prerequisites
-------------

To create a FileSystem source table, an enhanced datasource connection is required. You can set security group rules as required when you configure the connection.

Caveats
-------

-  When you create a Flink OpenSource SQL job, set **Flink Version** to **1.15** in the **Running Parameters** tab. Select **Save Job Log**, and specify the OBS bucket for saving job logs.
-  For details about how to use data types, see :ref:`Format <dli_08_15014>`.
-  Flink 1.15 currently only supports creating OBS tables and DLI lakehouse tables using Hive syntax, which is supported by Hive dialect DDL statements.

   -  To create an OBS table using Hive syntax:

      -  For the default dialect, set **hive.is-external** to **true** in the with properties.
      -  For the Hive dialect, use the **EXTERNAL** keyword in the create table statement.

   -  To create a DLI lakehouse table using Hive syntax:

      -  For the Hive dialect, add **'is_lakehouse'='true'** to the table properties.

-  Enable checkpointing.
-  You are advised to switch to Hive dialect to create Hive-compatible tables. If you want to create Hive-compatible tables with default dialect, make sure to set **'connector'='hive'** in your table properties, otherwise a table is considered generic by default in HiveCatalog. Note that the connector property is not required if you use Hive dialect.
-  Monitor strategy is to scan all directories/files currently in the location path. Many partitions may cause performance degradation.
-  Streaming reads for non-partitioned tables requires that each file be written atomically into the target directory.
-  Streaming reading for partitioned tables requires that each partition should be added atomically in the view of hive metastore. If not, new data added to an existing partition will be consumed.
-  Streaming reads do not support watermark grammar in Flink DDL. These tables cannot be used for window operators.

Syntax
------

::

   CREATE EXTERNAL TABLE [IF NOT EXISTS] table_name
     [(col_name data_type [column_constraint] [COMMENT col_comment], ... [table_constraint])]
     [COMMENT table_comment]
     [PARTITIONED BY (col_name data_type [COMMENT col_comment], ...)]
     [
       [ROW FORMAT row_format]
       [STORED AS file_format]
     ]
     [LOCATION obs_path]
     [TBLPROPERTIES (property_name=property_value, ...)]

   row_format:
     : DELIMITED [FIELDS TERMINATED BY char [ESCAPED BY char]] [COLLECTION ITEMS TERMINATED BY char]
         [MAP KEYS TERMINATED BY char] [LINES TERMINATED BY char]
         [NULL DEFINED AS char]
     | SERDE serde_name [WITH SERDEPROPERTIES (property_name=property_value, ...)]

   file_format:
     : SEQUENCEFILE
     | TEXTFILE
     | RCFILE
     | ORC
     | PARQUET
     | AVRO
     | INPUTFORMAT input_format_classname OUTPUTFORMAT output_format_classname

   column_constraint:
     : NOT NULL [[ENABLE|DISABLE] [VALIDATE|NOVALIDATE] [RELY|NORELY]]

   table_constraint:
     : [CONSTRAINT constraint_name] PRIMARY KEY (col_name, ...) [[ENABLE|DISABLE] [VALIDATE|NOVALIDATE] [RELY|NORELY]]

Parameter Description
---------------------

.. table:: **Table 1** TBLPROPERTIES parameters

   +---------------------------------------+-------------+----------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                             | Mandatory   | Default Value  | Data Type   | Description                                                                                                                                                                                                                                                                                                                                                                                                                |
   +=======================================+=============+================+=============+============================================================================================================================================================================================================================================================================================================================================================================================================================+
   | streaming-source.enable               | No          | false          | Boolean     | Enable streaming source or not. Note: Make sure that each partition/file should be written atomically, otherwise the reader may get incomplete data.                                                                                                                                                                                                                                                                       |
   +---------------------------------------+-------------+----------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | streaming-source.partition.include    | No          | all            | String      | Option to set the partitions to read, supported options are **all** and **latest**. By default, this parameter is set to **all**.                                                                                                                                                                                                                                                                                          |
   |                                       |             |                |             |                                                                                                                                                                                                                                                                                                                                                                                                                            |
   |                                       |             |                |             | **all** means read all partitions.                                                                                                                                                                                                                                                                                                                                                                                         |
   |                                       |             |                |             |                                                                                                                                                                                                                                                                                                                                                                                                                            |
   |                                       |             |                |             | **latest** only works when the streaming hive source table used as temporal table. **latest** means reading latest partition in order of **streaming-source.partition.order**.                                                                                                                                                                                                                                             |
   |                                       |             |                |             |                                                                                                                                                                                                                                                                                                                                                                                                                            |
   |                                       |             |                |             | Flink supports temporal join the latest hive partition by enabling **streaming-source.enable** and setting **streaming-source.partition.include** to **latest**. At the same time, user can assign the partition compare order and data update interval by configuring following partition-related options.                                                                                                                |
   +---------------------------------------+-------------+----------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | streaming-source.monitor-interval     | No          | None           | Duration    | Time interval for consecutively monitoring partition/file. Notes: The default interval for hive streaming reading is '1 m', the default interval for hive streaming temporal join is '60 m', this is because there's one framework limitation that every TM will visit the Hive metaStore in current hive streaming temporal join implementation which may produce pressure to metaStore, this will improve in the future. |
   +---------------------------------------+-------------+----------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | streaming-source.partition-order      | No          | partition-name | String      | The partition order of streaming source, supporting **create-time**, **partition-time**, and **partition-name**.                                                                                                                                                                                                                                                                                                           |
   |                                       |             |                |             |                                                                                                                                                                                                                                                                                                                                                                                                                            |
   |                                       |             |                |             | **create-time** compares partition/file creation time, this is not the partition create time in Hive metaStore, but the folder/file modification time in filesystem, if the partition folder somehow gets updated, e.g. add new file into folder, it can affect how the data is consumed.                                                                                                                                  |
   |                                       |             |                |             |                                                                                                                                                                                                                                                                                                                                                                                                                            |
   |                                       |             |                |             | **partition-time** compares the time extracted from partition name.                                                                                                                                                                                                                                                                                                                                                        |
   |                                       |             |                |             |                                                                                                                                                                                                                                                                                                                                                                                                                            |
   |                                       |             |                |             | **partition-name** compares partition name's alphabetical order.                                                                                                                                                                                                                                                                                                                                                           |
   |                                       |             |                |             |                                                                                                                                                                                                                                                                                                                                                                                                                            |
   |                                       |             |                |             | For a non-partition table, this value should always be **create-time**.                                                                                                                                                                                                                                                                                                                                                    |
   |                                       |             |                |             |                                                                                                                                                                                                                                                                                                                                                                                                                            |
   |                                       |             |                |             | By default the value is **partition-name**. The option is equality with deprecated option **streaming-source.consume-order**.                                                                                                                                                                                                                                                                                              |
   +---------------------------------------+-------------+----------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | streaming-source.consume-start-offset | No          | None           | String      | Start offset for streaming consuming. How to parse and compare offsets depends on your order. For **create-time** and **partition-time**, should be a timestamp string (yyyy-[m]m-[d]d [hh:mm:ss]).                                                                                                                                                                                                                        |
   |                                       |             |                |             |                                                                                                                                                                                                                                                                                                                                                                                                                            |
   |                                       |             |                |             | For **partition-time**, will use partition time extractor to extract time from partition. For **partition-name**, is the partition name string (e.g. pt_year=2020/pt_mon=10/pt_day=01).                                                                                                                                                                                                                                    |
   +---------------------------------------+-------------+----------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | is_lakehouse                          | No          | None           | Boolean     | If DLI lakehouse tables using Hive syntax are used, set this parameter to **true**.                                                                                                                                                                                                                                                                                                                                        |
   +---------------------------------------+-------------+----------------+-------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

-  **Source Parallelism Inference**

   By default, Flink infers the hive source parallelism based on the number of splits, and the number of splits is based on the number of files and the number of blocks in the files.

   Flink allows you to flexibly configure the policy of parallelism inference. You can configure the following parameters in TableConfig (note that these parameters affect all sources of the job):

   +----------------------------------------------+---------+---------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | Key                                          | Default | Type    | Description                                                                                                                                   |
   +==============================================+=========+=========+===============================================================================================================================================+
   | table.exec.hive.infer-source-parallelism     | true    | Boolean | If it is **true**, source parallelism is inferred according to splits number. If it is **false**, parallelism of source is set by **config**. |
   +----------------------------------------------+---------+---------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | table.exec.hive.infer-source-parallelism.max | 1000    | Integer | Sets max infer parallelism for source operator.                                                                                               |
   +----------------------------------------------+---------+---------+-----------------------------------------------------------------------------------------------------------------------------------------------+

-  **Load Partition Splits**

   Multi-thread is used to split hive's partitions. You can use **table.exec.hive.load-partition-splits.thread-num** to configure the thread number. The default value is **3** and the configured value should be greater than 0.

   +--------------------------------------------------+---------+---------+------------------------------------------------+
   | Key                                              | Default | Type    | Description                                    |
   +==================================================+=========+=========+================================================+
   | table.exec.hive.load-partition-splits.thread-num | 3       | Integer | The configured value should be greater than 0. |
   +--------------------------------------------------+---------+---------+------------------------------------------------+

   SQL hints can be used to apply configurations to a Hive table without changing its definition in the Hive metastore. See `Hints \| Apache Flink <https://nightlies.apache.org/flink/flink-docs-release-1.15/zh/docs/dev/table/sql/queries/hints/>`__.

-  **Vectorized Optimization upon Read**

   Flink will automatically used vectorized reads of Hive tables when the following conditions are met:

   -  Format: ORC or Parquet.

   -  Columns without complex data type, like hive types: List, Map, Struct, Union.

      This feature is enabled by default. It may be disabled with the following configuration.

      .. code-block::

         table.exec.hive.fallback-mapred-reader=true

-  **Reading Hive Views**

   Flink is able to read from Hive defined views, but some limitations apply:

   -  The Hive catalog must be set as the current catalog before you can query the view. This can be done by either **tableEnv.useCatalog(...)** in Table API or **USE CATALOG ...** in SQL Client.
   -  Hive and Flink SQL have different syntax, e.g. different reserved keywords and literals. Make sure the view's query is compatible with Flink grammar.

Example
-------

#. Create an OBS table in Hive syntax using Spark SQL and insert 10 data records. Simulate the data source.

   .. code-block::

      CREATE TABLE IF NOT EXISTS demo.student(
        name STRING,
       score DOUBLE)
      PARTITIONED BY (classNo INT)
      STORED AS PARQUET
      LOCATION 'obs://demo/spark.db/student';

      INSERT INTO demo.student PARTITION(classNo=1) VALUES ('Alice', 90.0), ('Bob', 80.0), ('Charlie', 70.0), ('David', 60.0), ('Eve', 50.0), ('Frank', 40.0), ('Grace', 30.0), ('Hank', 20.0), ('Ivy', 10.0), ('Jack', 0.0);

#. Demonstrate batch processing using Flink SQL to read data from the Hive syntax OBS table demo.student in batch mode and print it out. Checkpointing is required.

   .. code-block::

      CREATE CATALOG myhive WITH (
          'type' = 'hive',
          'default-database' = 'demo',
           'hive-conf-dir' = '/opt/flink/conf'
      );

      USE CATALOG myhive;

      create table if not exists print (
          name STRING,
          score DOUBLE,
          classNo INT)
      with ('connector' = 'print');

      insert into print
      select * from student;

   Result (out log of TaskManager):

   .. code-block::

      +I[Alice, 90.0, 1]
      +I[Bob, 80.0, 1]
      +I[Charlie, 70.0, 1]
      +I[David, 60.0, 1]
      +I[Eve, 50.0, 1]
      +I[Frank, 40.0, 1]
      +I[Grace, 30.0, 1]
      +I[Hank, 20.0, 1]
      +I[Ivy, 10.0, 1]
      +I[Jack, 0.0, 1]

#. Demonstrate stream processing by using Flink SQL to read data from the Hive syntax OBS table demo.student in stream mode and print it out.

   .. code-block::

      CREATE CATALOG myhive WITH (
          'type' = 'hive' ,
          'default-database' = 'demo',
           'hive-conf-dir' = '/opt/flink/conf'
      );

      USE CATALOG myhive;

      create table if not exists print (
          name STRING,
          score DOUBLE,
          classNo INT)
      with ('connector' = 'print');

      insert into print
      select * from student /*+ OPTIONS('streaming-source.enable' = 'true', 'streaming-source.monitor-interval' = '3 m') */;

The SQL hints function is used. SQL hints can be used to apply configurations to a Hive table without changing its definition in the Hive metastore. For details, see `SQL Hints <https://nightlies.apache.org/flink/flink-docs-release-1.15/docs/dev/table/sql/queries/hints/>`__.
