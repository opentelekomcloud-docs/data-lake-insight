:original_name: dli_08_0346.html

.. _dli_08_0346:

File System Result Table
========================

Function
--------

You can create a file system result table to export data to a file system such as HDFS or OBS. After the data is generated, a non-DLI table can be created directly according to the generated directory. The table can be processed through DLI SQL, and the output data directory can be stored in partition tables. It is applicable to scenarios such as data dumping, big data analysis, data backup, and active, deep, or cold archiving.

Syntax
------

::

   create table filesystemSink (
     attr_name attr_type (',' attr_name attr_type) *
   ) with (
     'connector.type' = 'filesystem',
     'connector.file-path' = '',
     'format.type' = ''
   );

Important Notes
---------------

-  If the data output directory in the table creation syntax is OBS, the directory must be a parallel file system and cannot be an OBS bucket.
-  When using a file system table, you must enable checkpointing to ensure job consistency.
-  When **format.type** is **parquet**, the supported data type is string, boolean, tinyint, smallint, int, bigint, float, double, map<string, string>, timestamp(3), and time.
-  To avoid data loss or data coverage, you need to enable automatic restart upon job exceptions. Enable the **Restore Job from Checkpoint**.
-  Set the checkpoint interval after weighing between real-time output file, file size, and recovery time, such as 10 minutes.
-  When using HDFS, you need to bind the data source and enter the host information.
-  When using HDFS, you need to configure information about the node where the active NameNode locates.

Parameter
---------

.. table:: **Table 1** Parameter description

   +--------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                | Mandatory             | Description                                                                                                                                             |
   +==========================+=======================+=========================================================================================================================================================+
   | connector.type           | Yes                   | The value is fixed to **filesystem**.                                                                                                                   |
   +--------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.file-path      | Yes                   | Data output directory. The format is *schema*://*file.path*.                                                                                            |
   |                          |                       |                                                                                                                                                         |
   |                          |                       | .. note::                                                                                                                                               |
   |                          |                       |                                                                                                                                                         |
   |                          |                       |    Currently, Schema supports only OBS and HDFS.                                                                                                        |
   |                          |                       |                                                                                                                                                         |
   |                          |                       |    -  If **schema** is set to **obs**, data is stored to OBS. **Note that OBS directory must be a parallel file system and must not be an OBS bucket.** |
   |                          |                       |                                                                                                                                                         |
   |                          |                       |       For example, **obs://**\ *bucketName*/*fileName* indicates that data is exported to the **fileName** directory in the **bucketName** bucket.      |
   |                          |                       |                                                                                                                                                         |
   |                          |                       |    -  If **schema** is set to **hdfs**, data is exported to HDFS.                                                                                       |
   |                          |                       |                                                                                                                                                         |
   |                          |                       |       Example: **hdfs://node-master1sYAx:9820/user/car_infos**, where **node-master1sYAx:9820** is the name of the node where the NameNode locates.     |
   +--------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
   | format.type              | Yes                   | Output data encoding format. Only **parquet** and **csv** are supported.                                                                                |
   |                          |                       |                                                                                                                                                         |
   |                          |                       | -  When **schema** is set to **obs**, the encoding format of the output data can only be **parquet**.                                                   |
   |                          |                       | -  When **schema** is set to **hdfs**, the output data can be encoded in **Parquet** or **CSV** format.                                                 |
   +--------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
   | format.field-delimiter   | No                    | Delimiter used to separate every two attributes.                                                                                                        |
   |                          |                       |                                                                                                                                                         |
   |                          |                       | This parameter needs to be configured if the CSV encoding format is adopted. It can be user-defined, for example, a comma (**,**).                      |
   +--------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.ak             | No                    | Access key for accessing OBS                                                                                                                            |
   |                          |                       |                                                                                                                                                         |
   |                          |                       | This parameter is mandatory when data is written to OBS.                                                                                                |
   +--------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.sk             | No                    | Secret key for accessing OBS                                                                                                                            |
   |                          |                       |                                                                                                                                                         |
   |                          |                       | This parameter is mandatory when data is written to OBS.                                                                                                |
   +--------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+
   | connector.partitioned-by | No                    | Partitioning field. Use commas (,) to separate multiple fields.                                                                                         |
   +--------------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

Read data from Kafka and write the data in Parquet format to the **fileName** directory in the **bucketName** bucket.

.. code-block::

   create table kafkaSource(
     attr0 string,
     attr1 boolean,
     attr2 TINYINT,
     attr3 smallint,
     attr4 int,
     attr5 bigint,
     attr6 float,
     attr7 double,
     attr8 timestamp(3),
     attr9 time
   ) with (
     'connector.type' = 'kafka',
     'connector.version' = '0.11',
     'connector.topic' = 'test_json',
     'connector.properties.bootstrap.servers' = 'xx.xx.xx.xx:9092',
     'connector.properties.group.id' = 'test_filesystem',
     'connector.startup-mode' = 'latest-offset',
     'format.type' = 'csv'
   );

   create table filesystemSink(
     attr0 string,
     attr1 boolean,
     attr2 TINYINT,
     attr3 smallint,
     attr4 int,
     attr5 bigint,
     attr6 float,
     attr7 double,
     attr8 map < string,  string >,
     attr9 timestamp(3),
     attr10 time
   ) with (
     "connector.type" = "filesystem",
     "connector.file-path" = "obs://bucketName/fileName",
     "format.type" = "parquet",
     "connector.ak" = "xxxx",
     "connector.sk" = "xxxxxx"
   );

   insert into
     filesystemSink
   select
     attr0,
     attr1,
     attr2,
     attr3,
     attr4,
     attr5,
     attr6,
     attr7,
     map [attr0,attr0],
     attr8,
     attr9
   from
     kafkaSource;
