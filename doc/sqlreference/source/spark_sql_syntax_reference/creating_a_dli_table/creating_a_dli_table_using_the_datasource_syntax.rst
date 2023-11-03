:original_name: dli_08_0098.html

.. _dli_08_0098:

Creating a DLI Table Using the DataSource Syntax
================================================

Function
--------

This DataSource syntax can be used to create a DLI table. The main differences between the DataSource and the Hive syntax lie in the supported data formats and the number of supported partitions. For details, see syntax and precautions.

Syntax
------

::

   CREATE TABLE [IF NOT EXISTS] [db_name.]table_name
     [(col_name1 col_type1 [COMMENT col_comment1], ...)]
     USING file_format
     [OPTIONS (key1=val1, key2=val2, ...)]
     [PARTITIONED BY (col_name1, col_name2, ...)]
     [COMMENT table_comment]
     [AS select_statement];

Keyword
-------

-  IF NOT EXISTS: Prevents system errors when the created table exists.
-  USING: Specifies the storage format.
-  OPTIONS: Specifies the attribute name and attribute value when a table is created.
-  COMMENT: Field or table description.
-  PARTITIONED BY: Partition field.
-  AS: Run the CREATE TABLE AS statement to create a table.

Parameter Description
---------------------

.. table:: **Table 1** Parameter description

   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter        | Description                                                                                                                                                                                                                                                                                                        |
   +==================+====================================================================================================================================================================================================================================================================================================================+
   | db_name          | Database name that contains letters, digits, and underscores (_). The value cannot contain only digits and cannot start with a digit or underscore (_).                                                                                                                                                            |
   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name       | Table name of a database that contains letters, digits, and underscores (_). The value cannot contain only digits and cannot start with a digit or underscore (_). The matching rule is **^(?!_)(?![0-9]+$)[A-Za-z0-9_$]*$**. If special characters are required, use single quotation marks ('') to enclose them. |
   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | col_name         | Column names with data types separated by commas (,). The column name contains letters, digits, and underscores (_). It cannot contain only digits and must contain at least one letter.                                                                                                                           |
   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | col_type         | Field type                                                                                                                                                                                                                                                                                                         |
   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | col_comment      | Field description                                                                                                                                                                                                                                                                                                  |
   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | file_format      | Data storage format of DLI tables. The value can be **parquet** only.                                                                                                                                                                                                                                              |
   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_comment    | Table description                                                                                                                                                                                                                                                                                                  |
   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | select_statement | The CREATE TABLE AS statement is used to insert the SELECT query result of the source table or a data record to a newly created DLI table.                                                                                                                                                                         |
   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. table:: **Table 2** OPTIONS parameter description

   +---------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
   | Parameter           | Description                                                                                                                                                                                                | Default Value |
   +=====================+============================================================================================================================================================================================================+===============+
   | multiLevelDirEnable | Whether to iteratively query data in subdirectories. When this parameter is set to **true**, all files in the table path, including files in subdirectories, are iteratively read when a table is queried. | false         |
   +---------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
   | compression         | Specified compression format. Generally, you need to set this parameter to **zstd** for parquet files.                                                                                                     | ``-``         |
   +---------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+

Precautions
-----------

-  If no delimiter is specified, the comma (,) is used by default.

-  When a partitioned table is created, the column specified in PARTITIONED BY must be a column in the table, and the partition type must be specified. The partition column supports only the **string**, **boolean**, **tinyint**, **smallint**, **short**, **int**, **bigint**, **long**, **decimal**, **float**, **double**, **date**, and **timestamp** type.
-  When a partitioned table is created, the partition field must be the last one or several fields of the table field, and the sequence of the partition fields must be the same. Otherwise, an error occurs.
-  A maximum of 7,000 partitions can be created in a single table.
-  The CREATE TABLE AS statement cannot specify table attributes or create partitioned tables.

Example
-------

-  Create a **src** table that has two columns **key** and **value** in INT and STRING types respectively, and set the compression format to **zstd**.

   ::

      CREATE TABLE src(key INT, value STRING) USING PARQUET OPTIONS(compression = 'zstd');

-  Create a **student** table that has **name**, **score**, and **classNo** columns and stores data in **Parquet** format. Partition the table by **classNo**.

   ::

      CREATE TABLE student(name STRING, score INT, classNo INT) USING PARQUET OPTIONS('key1' = 'value1') PARTITIONED BY(classNo) ;

   .. note::

      **classNo** is the partition field, which must be placed at the end of the table field, that is, **student(name STRING, score INT, classNo INT)**.

-  Create table **t1** and insert **t2** data into table **t1**.

   ::

      CREATE TABLE t1 USING parquet AS select * from t2;
