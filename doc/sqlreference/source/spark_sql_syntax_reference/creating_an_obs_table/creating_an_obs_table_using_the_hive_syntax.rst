:original_name: dli_08_0077.html

.. _dli_08_0077:

Creating an OBS Table Using the Hive Syntax
===========================================

Function
--------

This statement is used to create an OBS table using the Hive syntax. The main differences between the DataSource and the Hive syntax lie in the supported data formats and the number of supported partitions. For details, see syntax and precautions.

.. note::

   You are advised to use the OBS parallel file system for storage. A parallel file system is a high-performance file system that provides latency in milliseconds, TB/s-level bandwidth, and millions of IOPS. It applies to interactive big data analysis scenarios.

Precautions
-----------

-  The size of a table is calculated when the table is created.
-  When data is added, the table size will not be changed.
-  You can check the table size on OBS.
-  Table properties cannot be specified using CTAS table creation statements.
-  **Instructions on using partitioned tables:**

   -  When you create a partitioned table, ensure that the specified column in **PARTITIONED BY** is not a column in the table and the data type is specified. The partition column supports only the open-source Hive table types including **string**, **boolean**, **tinyint**, **smallint**, **short**, **int**, **bigint**, **long**, **decimal**, **float**, **double**, **date**, and **timestamp**.
   -  Multiple partition fields can be specified. The partition fields need to be specified after the **PARTITIONED BY** keyword, instead of the table name. Otherwise, an error occurs.
   -  A maximum of 200,000 partitions can be created in a single table.
   -  CTAS table creation statements cannot be used to create partitioned tables.

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
     [AS select_statement]
   row_format:
     : SERDE serde_cls [WITH SERDEPROPERTIES (key1=val1, key2=val2, ...)]
     | DELIMITED [FIELDS TERMINATED BY char [ESCAPED BY char]]
         [COLLECTION ITEMS TERMINATED BY char]
         [MAP KEYS TERMINATED BY char]
         [LINES TERMINATED BY char]
         [NULL DEFINED AS char]

Keywords
--------

-  EXTERNAL: Creates an OBS table.
-  IF NOT EXISTS: Prevents system errors when the created table exists.
-  COMMENT: Field or table description.
-  PARTITIONED BY: Partition field.
-  ROW FORMAT: Row data format.
-  STORED AS: Specifies the format of the file to be stored. Currently, only the TEXTFILE, AVRO, ORC, SEQUENCEFILE, RCFILE, and PARQUET format are supported.
-  LOCATION: Specifies the path of OBS. This keyword is mandatory when you create OBS tables.
-  TBLPROPERTIES: Allows you to add the **key/value** properties to a table.

   -  You can use this statement to enable the multiversion function to back up and restore table data. After the multiversion function is enabled, the system automatically backs up table data when you delete or modify the data using **insert overwrite** or **truncate**, and retains the data for a certain period. You can quickly restore data within the retention period. For details about the SQL syntax for the multiversion function, see :ref:`Enabling or Disabling Multiversion Backup <dli_08_0354>` and :ref:`Backing Up and Restoring Data of Multiple Versions <dli_08_0349>`.

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
         |                                   | -  PARQUET                                                                                                                  |
         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------+
         | auto.purge                        | If this parameter is set to **true**, the deleted or overwritten data is removed and will not be dumped to the recycle bin. |
         +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------+

-  AS: You can run the CREATE TABLE AS statement to create a table.

Parameters
----------

.. table:: **Table 2** Parameters

   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                                                                                                                                                          |
   +=======================+=======================+======================================================================================================================================================================================================+
   | db_name               | No                    | Database name                                                                                                                                                                                        |
   |                       |                       |                                                                                                                                                                                                      |
   |                       |                       | The value can contain letters, numbers, and underscores (_), but it cannot contain only numbers or start with a number or underscore (_).                                                            |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name            | Yes                   | Table name in the database                                                                                                                                                                           |
   |                       |                       |                                                                                                                                                                                                      |
   |                       |                       | The value can contain letters, numbers, and underscores (_), but it cannot contain only numbers or start with a number or underscore (_). The matching rule is **^(?!_)(?![0-9]+$)[A-Za-z0-9_$]*$**. |
   |                       |                       |                                                                                                                                                                                                      |
   |                       |                       | Special characters must be enclosed in single quotation marks ('').                                                                                                                                  |
   |                       |                       |                                                                                                                                                                                                      |
   |                       |                       | The table name is case insensitive.                                                                                                                                                                  |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | col_name              | Yes                   | Name of a column field                                                                                                                                                                               |
   |                       |                       |                                                                                                                                                                                                      |
   |                       |                       | The column field can contain letters, numbers, and underscores (_), but it cannot contain only numbers and must contain at least one letter.                                                         |
   |                       |                       |                                                                                                                                                                                                      |
   |                       |                       | The column name is case insensitive.                                                                                                                                                                 |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | col_type              | Yes                   | Data type of a column field, which is primitive.                                                                                                                                                     |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | col_comment           | No                    | Column field description, which can only be string constants.                                                                                                                                        |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | row_format            | Yes                   | Row data format This function is available only for textfile tables.                                                                                                                                 |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | file_format           | Yes                   | OBS table storage format, which can be **TEXTFILE**, **AVRO**, **ORC**, **SEQUENCEFILE**, **RCFILE**, or **PARQUET**.                                                                                |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_comment         | No                    | Table description, which can only be string constants.                                                                                                                                               |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | obs_path              | Yes                   | OBS storage path where data files are stored. You are advised to use an OBS parallel file system for storage.                                                                                        |
   |                       |                       |                                                                                                                                                                                                      |
   |                       |                       | Format: **obs://bucketName/tblPath**                                                                                                                                                                 |
   |                       |                       |                                                                                                                                                                                                      |
   |                       |                       | *bucketName*: bucket name                                                                                                                                                                            |
   |                       |                       |                                                                                                                                                                                                      |
   |                       |                       | *tblPath*: directory name. You do not need to specify the file name following the directory.                                                                                                         |
   |                       |                       |                                                                                                                                                                                                      |
   |                       |                       | If there is a folder and a file with the same name in the OBS directory, the path pointed to by the OBS table will prioritize the file over the folder.                                              |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key = value           | No                    | Set table properties and values.                                                                                                                                                                     |
   |                       |                       |                                                                                                                                                                                                      |
   |                       |                       | For example, if you want to enable multiversion, you can set **"dli.multi.version.enable"="true"**.                                                                                                  |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | select_statement      | No                    | Used in the **CREATE TABLE AS** statement to insert the **SELECT** query results of the source table or a data record to a table newly created in the OBS bucket.                                    |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_08_0077__section139223276592:

Example 1: Creating an OBS Non-Partitioned Table
------------------------------------------------

Example description: Create an OBS non-partitioned table named **table1** and use the **STORED AS** keyword to set the storage format of the table to **orc**.

In practice, you can store OBS tables in **textfile**, **avro**, **orc**, **sequencefile**, **rcfile**, or **parquet** format.

::

   CREATE TABLE IF NOT EXISTS table1 (
       col_1   STRING,
       col_2   INT
   )
   STORED AS orc
   LOCATION 'obs://bucketName/filePath';

Example 2: Creating an OBS Partitioned Table
--------------------------------------------

Example description: Create a partitioned table named **student**, which is partitioned using **facultyNo** and **classNo**.

In practice, you can select a proper partitioning field and add it to the end of the **PARTITIONED BY** keyword.

::

   CREATE TABLE IF NOT EXISTS student(
       id      INT,
       name    STRING
   )
   STORED AS avro
   LOCATION 'obs://bucketName/filePath'
   PARTITIONED BY (
       facultyNo   INT,
       classNo     INT
   );

Example 3: Using CTAS to Create an OBS Table Using All or Part of the Data in the Source Table
----------------------------------------------------------------------------------------------

Example description: Based on the OBS table **table1** created in :ref:`Example 1: Creating an OBS Non-Partitioned Table <dli_08_0077__section139223276592>`, use the CTAS syntax to copy data from **table1** to **table1_ctas**.

When using CTAS to create a table, you can ignore the syntax used to create the table being copied. This means that regardless of the syntax used to create **table1**, you can use the DataSource syntax to create **table1_ctas**.

In addition, in this example, the storage format of **table1** is **orc**, and the storage format of **table1_ctas** may be **sequencefile** or **parquet**. This means that the storage format of the table created by CTAS may be different from that of the original table.

Use the **SELECT** statement following the **AS** keyword to select required data and insert the data to **table1_ctas**.

The **SELECT** syntax is as follows: **SELECT <**\ *Column name* **> FROM <**\ *Table name* **> WHERE <**\ *Related filter criteria*\ **>**.

-  In this example, **SELECT \* FROM table1** is used. **\*** indicates that all columns are selected from **table1** and all data in **table1** is inserted into **table1_ctas**.

   ::

      CREATE TABLE IF NOT EXISTS table1_ctas
      STORED AS sequencefile
      LOCATION 'obs://bucketName/filePath'
      AS
      SELECT  *
      FROM    table1;

-  To filter and insert data into **table1_ctas** in a customized way, you can use the following **SELECT** statement: **SELECT col_1 FROM table1 WHERE col_1 = 'Ann'**. This will allow you to select only **col_1** from **table1** and insert data into **table1_ctas** where the value equals **'Ann'**.

   ::

      CREATE TABLE IF NOT EXISTS table1_ctas
      USING parquet
      OPTIONS (path 'obs:// bucketName/filePath')
      AS
      SELECT  col_1
      FROM    table1
      WHERE   col_1 = 'Ann';

Example 4: Creating an OBS Non-Partitioned Table and Customizing the Data Type of a Column Field
------------------------------------------------------------------------------------------------

Example description: Create an OBS non-partitioned table named **table2**. You can customize the native data types of column fields based on service requirements.

-  **STRING**, **CHAR**, or **VARCHAR** can be used for text characters.
-  **TIMESTAMP** or **DATE** can be used for time characters.
-  **INT**, **SMALLINT/SHORT**, **BIGINT/LONG**, or **TINYINT** can be used for integer characters.
-  **FLOAT**, **DOUBLE**, or **DECIMAL** can be used for decimal calculation.
-  **BOOLEAN** can be used if only logical switches are involved.

For details, see "Data Types" > "Primitive Data Types".

::

   CREATE TABLE IF NOT EXISTS table2 (
       col_01  STRING,
       col_02  CHAR (2),
       col_03  VARCHAR (32),
       col_04  TIMESTAMP,
       col_05  DATE,
       col_06  INT,
       col_07  SMALLINT,
       col_08  BIGINT,
       col_09  TINYINT,
       col_10  FLOAT,
       col_11  DOUBLE,
       col_12  DECIMAL (10, 3),
       col_13  BOOLEAN
   )
   STORED AS parquet
   LOCATION 'obs://bucketName/filePath';

Example 5: Creating an OBS Partitioned Table and Customizing TBLPROPERTIES Parameters
-------------------------------------------------------------------------------------

Example description: Create an OBS partitioned table named **table3** and partition the table based on **col_3**. Set **dli.multi.version.enable**, **comment**, **orc.compress**, and **auto.purge** in **TBLPROPERTIES**.

-  **dli.multi.version.enable**: In this example, set this parameter to **true**, indicating that the DLI data versioning function is enabled for table data backup and restoration.
-  **comment**: table description, which can be modified later.
-  **orc.compress**: compression mode of the **orc** format, which is **ZLIB** in this example.
-  **auto.purge**: In this example, set this parameter to **true**, indicating that data that is deleted or overwritten will bypass the recycle bin and be permanently deleted.

::

   CREATE TABLE IF NOT EXISTs table3 (
       col_1 STRING,
       col_2 STRING
   )
   PARTITIONED BY (col_3 DATE)
   STORED AS rcfile
   LOCATION 'obs://bucketName/filePath'
   TBLPROPERTIES (
       dli.multi.version.enable  = true,
       comment                   = 'Created by dli',
       orc.compress              = 'ZLIB',
       auto.purge                = true
   );

Example 6: Creating a Non-Partitioned Table in textfile Format and Setting ROW FORMAT
-------------------------------------------------------------------------------------

Example description: Create a non-partitioned table named **table4** in the **textfile** format and set **ROW FORMAT** (the ROW FORMAT function is available only for textfile tables).

-  **FIELDS**: columns in a table. Each field has a name and data type. Fields in a table are separated by slashes (/).
-  **COLLECTION ITEMS**: A collection item refers to an element in a group of data, which can be an array, a list, or a collection. Collection items in a table are separated by $.
-  **MAP KEYS**: A map key is a data structure of key-value pairs and is used to store a group of associated data. Map keys in a table are separated by number signs (#).
-  **LINES**: rows in a table. Each row contains a group of field values. Rows in a table end with **\\n**. (Note that only **\\n** can be used as the row separator.)
-  **NULL**: a special value that represents a missing or unknown value. In a table, **NULL** indicates that the field has no value or the value is unknown. When there is a null value in the data, it is represented by the string **null**.

::

   CREATE TABLE IF NOT EXISTS table4 (
       col_1   STRING,
       col_2   INT
   )
   STORED AS textfile
   LOCATION 'obs://bucketName/filePath'
   ROW FORMAT
   DELIMITED FIELDS TERMINATED   BY '/'
   COLLECTION ITEMS TERMINATED   BY '$'
   MAP KEYS TERMINATED           BY '#'
   LINES TERMINATED              BY '\n'
   NULL DEFINED                  AS 'null';
