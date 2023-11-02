:original_name: dli_08_0077.html

.. _dli_08_0077:

Creating an OBS Table Using the Hive Syntax
===========================================

Function
--------

This statement is used to create an OBS table using the Hive syntax. The main differences between the DataSource and the Hive syntax lie in the supported data formats and the number of supported partitions. For details, see syntax and precautions.

Usage
-----

-  The size of the table will be calculated during creation.
-  When data is added, the table size will not be changed.
-  You can view the table size on OBS.

Syntax
------

::

   CREATE [EXTERNAL] TABLE [IF NOT EXISTS] [db_name.]table_name
     [(col_name1 col_type1 [COMMENT col_comment1], ...)]
     [COMMENT table_comment]
     [PARTITIONED BY (col_name2 col_type2, [COMMENT col_comment2], ...)]
     [ROW FORMAT row_format]
     [STORED AS file_format]
     LOCATION 'obs_path'
     [TBLPROPERTIES (key = value)]
     [AS select_statement];

   row_format:
     : SERDE serde_cls [WITH SERDEPROPERTIES (key1=val1, key2=val2, ...)]
     | DELIMITED [FIELDS TERMINATED BY char [ESCAPED BY char]]
         [COLLECTION ITEMS TERMINATED BY char]
         [MAP KEYS TERMINATED BY char]
         [LINES TERMINATED BY char]
         [NULL DEFINED AS char]

Keyword
-------

-  EXTERNAL: Creates an OBS table.

-  IF NOT EXISTS: Prevents system errors when the created table exists.

-  COMMENT: Field or table description.

-  PARTITIONED BY: Partition field.

-  ROW FORMAT: Row data format.

-  STORED AS: Specifies the format of the file to be stored. Currently, only the TEXTFILE, AVRO, ORC, SEQUENCEFILE, RCFILE, and PARQUET format are supported.

-  LOCATION: Specifies the path of OBS. This keyword is mandatory when you create OBS tables.

-  TBLPROPERTIES: Allows you to add the **key/value** properties to a table.

   For example, you can use this statement to enable the multiversion function to back up and restore table data. After the multiversion function is enabled, the system automatically backs up table data when you delete or modify the data using **insert overwrite** or **truncate**, and retains the data for a certain period. You can quickly restore data within the retention period. For details about SQL syntax related to the multiversion function, see :ref:`Enabling or Disabling Multiversion Backup <dli_08_0354>` and :ref:`Backing Up and Restoring Data of Multiple Versions <dli_08_0349>`.

   When creating an OBS table, you can use **TBLPROPERTIES ("dli.multi.version.enable"="true")** to enable multiversion. For details, see the following example.

   .. table:: **Table 1** TBLPROPERTIES parameters

      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
      | Key                               | Value                                                                                                                       |
      +===================================+=============================================================================================================================+
      | dli.multi.version.enable          | -  **true**: Enable the multiversion backup function.                                                                       |
      |                                   | -  **false**: Disable the multiversion backup function.                                                                     |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
      | comment                           | Description of the table                                                                                                    |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
      | orc.compress                      | An attribute of the ORC table, which specifies the compression mode of the ORC storage. Available values are as follows:    |
      |                                   |                                                                                                                             |
      |                                   | -  **ZLIB**                                                                                                                 |
      |                                   | -  **SNAPPY**                                                                                                               |
      |                                   | -  **NONE**                                                                                                                 |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
      | auto.purge                        | If this parameter is set to **true**, the deleted or overwritten data is removed and will not be dumped to the recycle bin. |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------+

-  AS: You can run the CREATE TABLE AS statement to create a table.

Parameter
---------

.. table:: **Table 2** Parameter description

   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                                                                                                                                                                        |
   +===================================+====================================================================================================================================================================================================================================================================================================================+
   | db_name                           | Database name that contains letters, digits, and underscores (_). The value cannot contain only digits and cannot start with a digit or underscore (_).                                                                                                                                                            |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name                        | Table name of a database that contains letters, digits, and underscores (_). The value cannot contain only digits and cannot start with a digit or underscore (_). The matching rule is **^(?!_)(?![0-9]+$)[A-Za-z0-9_$]*$**. If special characters are required, use single quotation marks ('') to enclose them. |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | col_name                          | Field name                                                                                                                                                                                                                                                                                                         |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | col_type                          | Field type                                                                                                                                                                                                                                                                                                         |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | col_comment                       | Field description                                                                                                                                                                                                                                                                                                  |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | row_format                        | Line data format                                                                                                                                                                                                                                                                                                   |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | file_format                       | OBS table storage format. TEXTFILE, AVRO, ORC, SEQUENCEFILE, RCFILE, and PARQUET are supported.                                                                                                                                                                                                                    |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_comment                     | Table description                                                                                                                                                                                                                                                                                                  |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | obs_path                          | OBS path                                                                                                                                                                                                                                                                                                           |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key = value                       | Set table properties and values.                                                                                                                                                                                                                                                                                   |
   |                                   |                                                                                                                                                                                                                                                                                                                    |
   |                                   | For example, if you want to enable multiversion, you can set **"dli.multi.version.enable"="true"**.                                                                                                                                                                                                                |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | select_statement                  | The CREATE TABLE AS statement is used to insert the SELECT query result of the source table or a data record to a new table in OBS bucket.                                                                                                                                                                         |
   +-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

-  The table and column names are case-insensitive.
-  Descriptions of table names and column names support only string constants.
-  During table creation, you need to specify the column name and corresponding data type. The data type is primitive type.
-  If a folder and a file have the same name in the OBS directory, the file is preferred as the path when creating an OBS table.
-  When you create a partitioned table, ensure that the specified column in **PARTITIONED BY** is not a column in the table and the data type is specified. The partition column supports only the open-source Hive table types including **string**, **boolean**, **tinyint**, **smallint**, **short**, **int**, **bigint**, **long**, **decimal**, **float**, **double**, **date**, and **timestamp**.
-  Multiple partition fields can be specified. The partition fields need to be specified after the **PARTITIONED BY** keyword, instead of the table name. Otherwise, an error occurs.
-  A maximum of 100,000 partitions can be created in a single table.
-  The CREATE TABLE AS statement cannot specify table attributes or create partitioned tables.

Example
-------

-  To create a Parquet table named **student**, in which the **id**, **name**, and **score** fields are contained and the data types of the respective fields are INT, STRING, and FLOAT, run the following statement:

   ::

      CREATE TABLE student (id INT, name STRING, score FLOAT) STORED AS PARQUET LOCATION 'obs://bucketName/filePath';

-  To create a table named **student**, for which **classNo** is the partition field and two fields **name** and **score** are specified, run the following statement:

   ::

      CREATE TABLE IF NOT EXISTS student(name STRING, score DOUBLE) PARTITIONED BY (classNo INT) STORED AS PARQUET LOCATION 'obs://bucketName/filePath';

   .. note::

      **classNo** is a partition field and must be specified after the PARTITIONED BY keyword, that is, **PARTITIONED BY (classNo INT)**. It cannot be specified after the table name as a table field.

-  To create table **t1** and insert data of table **t2** into table **t1** by using the Hive syntax, run the following statement:

   ::

      CREATE TABLE t1 STORED AS parquet LOCATION 'obs://bucketName/filePath' as select * from t2;

-  Create the **student** table and enable multiversion by using the Hive syntax.

   ::

      CREATE TABLE student (id INT, name STRING, score FLOAT) STORED AS PARQUET LOCATION 'obs://bucketName/filePath' TBLPROPERTIES ("dli.multi.version.enable"="true");
