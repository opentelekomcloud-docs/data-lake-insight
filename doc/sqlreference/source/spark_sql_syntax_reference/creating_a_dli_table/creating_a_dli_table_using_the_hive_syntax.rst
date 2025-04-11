:original_name: dli_08_0204.html

.. _dli_08_0204:

Creating a DLI Table Using the Hive Syntax
==========================================

Function
--------

This Hive syntax is used to create a DLI table. The main differences between the DataSource and the Hive syntax lie in the supported data formats and the number of supported partitions. For details, see syntax and precautions.

Precautions
-----------

-  Table properties cannot be specified using CTAS table creation statements.
-  You cannot specify multi-character delimiters when creating Hive DLI tables.
-  **Instructions on using partitioned tables:**

   -  When you create a partitioned table, ensure that the specified column in **PARTITIONED BY** is not a column in the table and the data type is specified. The partition column supports only the open-source Hive table types including **string**, **boolean**, **tinyint**, **smallint**, **short**, **int**, **bigint**, **long**, **decimal**, **float**, **double**, **date**, and **timestamp**.
   -  Multiple partition fields can be specified. The partition fields need to be specified after the **PARTITIONED BY** keyword, instead of the table name. Otherwise, an error occurs.
   -  A maximum of 200,000 partitions can be created in a single table.
   -  CTAS table creation statements cannot be used to create partitioned tables.

Syntax
------

::

   CREATE TABLE [IF NOT EXISTS] [db_name.]table_name
     [(col_name1 col_type1 [COMMENT col_comment1], ...)]
     [COMMENT table_comment]
     [PARTITIONED BY (col_name2 col_type2, [COMMENT col_comment2], ...)]
     [ROW FORMAT row_format]
     STORED AS file_format
     [TBLPROPERTIES (key = value)]
     [AS select_statement];

   row_format:
     : SERDE serde_cls [WITH SERDEPROPERTIES (key1=val1, key2=val2, ...)]
     | DELIMITED [FIELDS TERMINATED BY char [ESCAPED BY char]]
         [COLLECTION ITEMS TERMINATED BY char]
         [MAP KEYS TERMINATED BY char]
         [LINES TERMINATED BY char]
         [NULL DEFINED AS char]

Keywords
--------

-  IF NOT EXISTS: Prevents system errors when the created table exists.
-  COMMENT: Field or table description.
-  PARTITIONED BY: Partition field.
-  ROW FORMAT: Row data format.
-  STORED AS: Specifies the format of the file to be stored. Currently, only the TEXTFILE, AVRO, ORC, SEQUENCEFILE, RCFILE, and PARQUET format are supported. This keyword is mandatory when you create DLI tables.
-  TBLPROPERTIES: This keyword is used to add a **key/value** property to a table.

   -  If the table storage format is Parquet, you can use **TBLPROPERTIES(parquet.compression = 'zstd')** to set the table compression format to **zstd**.

-  AS: Run the CREATE TABLE AS statement to create a table.

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                                                                                                                                                                                                                                               |
   +=======================+=======================+===========================================================================================================================================================================================================================================================================================+
   | db_name               | No                    | Database name                                                                                                                                                                                                                                                                             |
   |                       |                       |                                                                                                                                                                                                                                                                                           |
   |                       |                       | The value can contain letters, numbers, and underscores (_), but it cannot contain only numbers or start with a number or underscore (_).                                                                                                                                                 |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name            | Yes                   | Table name in the database                                                                                                                                                                                                                                                                |
   |                       |                       |                                                                                                                                                                                                                                                                                           |
   |                       |                       | The value can contain letters, numbers, and underscores (_), but it cannot contain only numbers or start with a number or underscore (_). The matching rule is **^(?!_)(?![0-9]+$)[A-Za-z0-9_$]*$**. If special characters are required, use single quotation marks ('') to enclose them. |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | col_name              | Yes                   | Column name                                                                                                                                                                                                                                                                               |
   |                       |                       |                                                                                                                                                                                                                                                                                           |
   |                       |                       | The column field can contain letters, numbers, and underscores (_), but it cannot contain only numbers and must contain at least one letter.                                                                                                                                              |
   |                       |                       |                                                                                                                                                                                                                                                                                           |
   |                       |                       | The column name is case insensitive.                                                                                                                                                                                                                                                      |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | col_type              | Yes                   | Data type of a column field, which is primitive.                                                                                                                                                                                                                                          |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | col_comment           | No                    | Column field description, which can only be string constants.                                                                                                                                                                                                                             |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | row_format            | Yes                   | Row data format The ROW FORMAT function is available only for textfile tables.                                                                                                                                                                                                            |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | file_format           | Yes                   | Data storage format of DLI tables. The options include **textfile**, **avro**, **orc**, **sequencefile**, **rcfile**, and **parquet**.                                                                                                                                                    |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_comment         | No                    | Table description, which can only be string constants.                                                                                                                                                                                                                                    |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key = value           | No                    | Set table properties and values.                                                                                                                                                                                                                                                          |
   |                       |                       |                                                                                                                                                                                                                                                                                           |
   |                       |                       | If the table storage format is Parquet, you can use **TBLPROPERTIES(parquet.compression = 'zstd')** to set the table compression format to **zstd**.                                                                                                                                      |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | select_statement      | No                    | The **CREATE TABLE AS** statement is used to insert the **SELECT** query result of the source table or a data record to a newly created DLI table.                                                                                                                                        |
   +-----------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_08_0204__section139223276592:

Example 1: Creating a DLI Non-Partitioned Table
-----------------------------------------------

Example description: Create a DLI non-partitioned table named **table1** and use the **STORED AS** keyword to set the storage format of the table to **orc**.

You can save DLI tables in the **textfile**, **avro**, **orc**, **sequencefile**, **rcfile**, or **parquet** format.

::

   CREATE TABLE IF NOT EXISTS table1 (
       col_1   STRING,
       col_2   INT
   )
   STORED AS orc;

Example 2: Creating a DLI Partitioned Table
-------------------------------------------

Example description: Create a partitioned table named **student**, which is partitioned using **facultyNo** and **classNo**.

In practice, you can select a proper partitioning field and add it to the end of the **PARTITIONED BY** keyword.

::

   CREATE TABLE IF NOT EXISTS student(
       id      int,
       name    STRING
   )
   STORED AS avro
   PARTITIONED BY (
       facultyNo   INT,
       classNo     INT
   );

Example 3: Using CTAS to Create a DLI Table Using All or Part of the Data in the Source Table
---------------------------------------------------------------------------------------------

Example description: Based on the DLI table **table1** created in :ref:`Example 1: Creating a DLI Non-Partitioned Table <dli_08_0204__section139223276592>`, use the CTAS syntax to copy data from **table1** to **table1_ctas**.

When using CTAS to create a table, you can ignore the syntax used to create the table being copied. This means that regardless of the syntax used to create **table1**, you can use the DataSource syntax to create **table1_ctas**.

In this example, the storage format of **table1** is **orc**, and the storage format of **table1_ctas** may be **parquet**. This means that the storage format of the table created by CTAS may be different from that of the original table.

Use the **SELECT** statement following the **AS** keyword to select required data and insert the data to **table1_ctas**.

The **SELECT** syntax is as follows: **SELECT <**\ *Column name* **> FROM <**\ *Table name* **> WHERE <**\ *Related filter criteria*\ **>**.

-  In the example, **select \* from table1** indicates that all statements are selected from **table1** and copied to **table1_ctas**.

   ::

      CREATE TABLE IF NOT EXISTS table1_ctas
      STORED AS sequencefile
      AS
      SELECT  *
      FROM table1;

-  If you do not need all data in **table1**, change **AS SELECT \* FROM table1** to **AS SELECT col_1 FROM table1 WHERE col_1 = Ann**. In this way, you can run the **SELECT** statement to insert all rows whose **col_1** column is **Ann** from **table1** to **table1_ctas**.

   ::

      CREATE TABLE IF NOT EXISTS table1_ctas
      USING parquet
      AS
      SELECT col_1
      FROM table1
      WHERE col_1 = 'Ann';

Example 4: Creating a DLI Non-Partitioned Table and Customizing the Data Type of a Column Field
-----------------------------------------------------------------------------------------------

Example description: Create a DLI non-partitioned table named **table2**. You can customize the native data types of column fields based on service requirements.

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
   STORED AS parquet;

Example 5: Creating a DLI Partitioned Table and Customizing TBLPROPERTIES Parameters
------------------------------------------------------------------------------------

Example description: Create a DLI partitioned table named **table3** and partition the table based on **col_3**. Set **dli.multi.version.enable**, **comment**, **orc.compress**, and **auto.purge** in **TBLPROPERTIES**.

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
   TBLPROPERTIES (
       dli.multi.version.enable    = true,
       comment                     = 'Created by dli',
       orc.compress                = 'ZLIB',
       auto.purge                  = true
   );

Example 6: Creating a Non-Partitioned Table in Textfile Format and Setting ROW FORMAT
-------------------------------------------------------------------------------------

Example description: In this example, create a non-partitioned table named **table4** in the **textfile** format and set **ROW FORMAT** (the ROW FORMAT function is available only for textfile tables).

-  **Fields**: columns in a table. Each field has a name and data type. Fields in a table are separated by slashes (/).
-  **COLLECTION ITEMS**: A collection item refers to an element in a group of data, which can be an array, a list, or a collection. Collection items in **table4** are separated by $.
-  **MAP KEYS**: A map key is a data structure of key-value pairs and is used to store a group of associated data. Map keys in a table are separated by number signs (#).
-  **Rows**: rows in a table. Each row contains a group of field values. Rows in a table end with **\\n**. (Note that only **\\n** can be used as the row separator.)
-  **NULL**: a special value that represents a missing or unknown value. In a table, **NULL** indicates that the field has no value or the value is unknown. When there is a null value in the data, it is represented by the string **null**.

::

   CREATE TABLE IF NOT EXISTS table4 (
       col_1   STRING,
       col_2   INT
   )
   STORED AS TEXTFILE
   ROW FORMAT
   DELIMITED FIELDS TERMINATED   BY '/'
   COLLECTION ITEMS TERMINATED   BY '$'
   MAP KEYS TERMINATED           BY '#'
   LINES TERMINATED              BY '\n'
   NULL DEFINED                  AS 'NULL';
