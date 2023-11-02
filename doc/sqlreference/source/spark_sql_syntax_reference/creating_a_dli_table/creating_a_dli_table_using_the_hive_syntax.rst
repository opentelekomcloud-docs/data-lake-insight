:original_name: dli_08_0204.html

.. _dli_08_0204:

Creating a DLI Table Using the Hive Syntax
==========================================

Function
--------

This Hive syntax is used to create a DLI table. The main differences between the DataSource and the Hive syntax lie in the supported data formats and the number of supported partitions. For details, see syntax and precautions.

Syntax
------

::

   CREATE TABLE [IF NOT EXISTS] [db_name.]table_name
     [(col_name1 col_type1 [COMMENT col_comment1], ...)]
     [COMMENT table_comment]
     [PARTITIONED BY (col_name2 col_type2, [COMMENT col_comment2], ...)]
     [ROW FORMAT row_format]
     STORED AS file_format
     [TBLPROPERTIES (key1=val1, key2=val2, ...)]
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

-  IF NOT EXISTS: Prevents system errors when the created table exists.

-  COMMENT: Field or table description.

-  PARTITIONED BY: Partition field.

-  ROW FORMAT: Row data format.

-  STORED AS: Specifies the format of the file to be stored. Currently, only the TEXTFILE, AVRO, ORC, SEQUENCEFILE, RCFILE, and PARQUET format are supported. This keyword is mandatory when you create DLI tables.

-  TBLPROPERTIES: The TBLPROPERTIES clause allows you to add the **key/value** attribute to a table.

   For example, if the table storage format is Parquet, you can use **TBLPROPERTIES(parquet.compression = 'zstd')** to set the table compression format to **zstd**.

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
   | row_format       | Line data format                                                                                                                                                                                                                                                                                                   |
   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | file_format      | Data storage format: TEXTFILE, AVRO, ORC, SEQUENCEFILE, RCFILE, PARQUET.                                                                                                                                                                                                                                           |
   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_comment    | Table description                                                                                                                                                                                                                                                                                                  |
   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | select_statement | The CREATE TABLE AS statement is used to insert the SELECT query result of the source table or a data record to a newly created DLI table.                                                                                                                                                                         |
   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

-  When you create a partitioned table, ensure that the specified column in **PARTITIONED BY** is not a column in the table and the data type is specified. The partition column supports only the open-source Hive table types including **string**, **boolean**, **tinyint**, **smallint**, **short**, **int**, **bigint**, **long**, **decimal**, **float**, **double**, **date**, and **timestamp**.
-  Multiple partition fields can be specified. The partition fields need to be specified after the **PARTITIONED BY** keyword, instead of the table name. Otherwise, an error occurs.
-  A maximum of 100,000 partitions can be created in a single table.
-  The CREATE TABLE AS statement cannot specify table attributes or create partitioned tables.

Example
-------

-  Create a **src** table that has **key** and **value** columns in INT and STRING types respectively, and specify a property as required.

   ::

      CREATE TABLE src
        (key INT, value STRING)
        STORED AS PARQUET
        TBLPROPERTIES('key1' = 'value1');

-  Create a **student** table that has **name**, **score**, and **classNo** columns, and partition the table by **classNo**.

   ::

      CREATE TABLE student
        (name STRING, score INT)
        STORED AS PARQUET
        TBLPROPERTIES(parquet.compression = 'zstd') PARTITIONED BY(classNo INT);

-  Create table **t1** and insert **t2** data into table **t1**.

   ::

      CREATE TABLE t1
        STORED AS PARQUET
        AS select * from t2;
