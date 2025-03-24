:original_name: dli_08_15050.html

.. _dli_08_15050:

Result Table
============

Function
--------

This section describes how to use Flink to write Hive tables, the definition of the Hive result table, parameters used for creating the result table, and sample code. For details, see `Apache Flink Hive Read & Write <https://nightlies.apache.org/flink/flink-docs-release-1.15/docs/connectors/table/hive/hive_read_write/>`__.

Flink supports writing data to Hive in both **BATCH** and **STREAMING** modes.

-  When run as a BATCH application, Flink will write to a Hive table only making those records visible when the Job finishes. BATCH writes support both appending to and overwriting existing tables.
-  **STREAMING** writes continuously adding new data to Hive, committing records - making them visible - incrementally. Users control when/how to trigger commits with several properties. Insert overwrite is not supported for streaming write. Please see the `streaming sink <https://nightlies.apache.org/flink/flink-docs-release-1.15/docs/connectors/table/filesystem/>`__ for a full list of available configurations.

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

-  When creating a Flink OpenSource SQL job, enable checkpointing in the job editing interface.

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

Please see the `streaming sink <https://nightlies.apache.org/flink/flink-docs-release-1.15/docs/connectors/table/filesystem/>`__ for a full list of available configurations.

Example
-------

The following example demonstrates how to use Datagen to write to a Hive table with partition submission functionality.

.. code-block::

   CREATE CATALOG myhive WITH (
       'type' = 'hive' ,
       'default-database' = 'demo',
       'hive-conf-dir' = '/opt/flink/conf'
   );

   USE CATALOG myhive;

   SET table.sql-dialect=hive;

   -- drop table demo.student_hive_sink;
   CREATE EXTERNAL TABLE IF NOT EXISTS demo.student_hive_sink(
     name STRING,
    score DOUBLE)
   PARTITIONED BY (classNo INT)
   STORED AS PARQUET
   LOCATION 'obs://demo/spark.db/student_hive_sink'
   TBLPROPERTIES (
      'sink.partition-commit.policy.kind'='metastore,success-file'
    );

   SET table.sql-dialect=default;
   create table if not exists student_datagen_source(
     name STRING,
     score DOUBLE,
     classNo INT
   ) with (
     'connector' = 'datagen',
     'rows-per-second' = '1', --Generates a piece of data per second.
     'fields.name.kind' = 'random', --Specifies a random generator for the user_id field.
     'fields.name.length' = '7', --Limits the user_id length to 7.
     'fields.classNo.kind' ='random',
     'fields.classNo.min' = '1',
     'fields.classNo.max' = '10'
   );

   insert into student_hive_sink select * from student_datagen_source;

Query the result table using Spark SQL.

.. code-block::

   select * from  demo.student_hive_sink where classNo > 0 limit 10
