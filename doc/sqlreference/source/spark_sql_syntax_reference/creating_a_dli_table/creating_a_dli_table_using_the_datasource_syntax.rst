:original_name: dli_08_0098.html

.. _dli_08_0098:

Creating a DLI Table Using the DataSource Syntax
================================================

Function
--------

This DataSource syntax can be used to create a DLI table. The main differences between the DataSource and the Hive syntax lie in the supported data formats and the number of supported partitions. For details, see syntax and precautions.

Precautions
-----------

-  Table properties cannot be specified using CTAS table creation statements.
-  If no separator is specified, a comma (,) is used by default.
-  **Instructions on using partitioned tables:**

   -  When a partitioned table is created, the column specified in PARTITIONED BY must be a column in the table, and the partition type must be specified. Partition columns can only be in the **string**, **boolean**, **tinyint**, **smallint**, **short**, **int**, **bigint**, **long**, **decimal**, **float**, **double**, **date**, or **timestamp** format.
   -  When a partitioned table is created, the partition field must be the last one or several fields of the table field, and the sequence of the partition fields must be the same. Otherwise, an error occurs.
   -  A maximum of 200,000 partitions can be created in a single table.
   -  CTAS table creation statements cannot be used to create partitioned tables.

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

Keywords
--------

-  **IF NOT EXISTS**: Prevents system errors when the created table exists.
-  **USING**: Storage format.
-  **OPTIONS**: Property name and property value when a table is created.
-  **COMMENT**: Field or table description.
-  **PARTITIONED BY**: Partition field.
-  **AS**: Run the **CREATE TABLE AS** statement to create a table.

Parameters
----------

.. table:: **Table 1** Parameters

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
   | col_name              | Yes                   | Column names with data types separated by commas (,)                                                                                                                                                 |
   |                       |                       |                                                                                                                                                                                                      |
   |                       |                       | The column name can contain letters, numbers, and underscores (_), but it cannot contain only numbers and must contain at least one letter.                                                          |
   |                       |                       |                                                                                                                                                                                                      |
   |                       |                       | The column name is case insensitive.                                                                                                                                                                 |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | col_type              | Yes                   | Data type of a column field, which is primitive.                                                                                                                                                     |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | col_comment           | No                    | Column field description, which can only be string constants.                                                                                                                                        |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | file_format           | Yes                   | Data storage format of DLI tables. The value can be **parquet** or **orc**.                                                                                                                          |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_comment         | No                    | Table description, which can only be string constants.                                                                                                                                               |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | select_statement      | No                    | The **CREATE TABLE AS** statement is used to insert the **SELECT** query result of the source table or a data record to a newly created DLI table.                                                   |
   +-----------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_08_0098__en-us_topic_0241764534_dli_08_0098_en-us_topic_0114776192_table16713182975016:

.. table:: **Table 2** OPTIONS parameters

   +---------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
   | Parameter           | Mandatory | Description                                                                                                                                                                                                | Default Value |
   +=====================+===========+============================================================================================================================================================================================================+===============+
   | multiLevelDirEnable | No        | Whether to iteratively query data in subdirectories. When this parameter is set to **true**, all files in the table path, including files in subdirectories, are iteratively read when a table is queried. | false         |
   +---------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+
   | compression         | No        | Compression format. Generally, you need to set this parameter to **zstd** for parquet files.                                                                                                               | ``-``         |
   +---------------------+-----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------+

.. _dli_08_0098__section175482343414:

Example 1: Creating a DLI Non-Partitioned Table
-----------------------------------------------

Example description: Create a DLI non-partitioned table named **table1** and use the **USING** keyword to set the storage format of the table to **orc**.

You can save DLI tables in the **parquet** format.

::

   CREATE TABLE IF NOT EXISTS table1 (
       col_1 STRING,
       col_2 INT)
   USING orc;

Example 2: Creating a DLI Partitioned Table
-------------------------------------------

Example description: Create a partitioned table named **student**, which is partitioned using **facultyNo** and **classNo**.

In practice, you can select a proper partitioning field and add it to the end of the **PARTITIONED BY** keyword.

::

   CREATE TABLE IF NOT EXISTS student (
       Name        STRING,
       facultyNo   INT,
       classNo     INT
   )
   USING orc
   PARTITIONED BY (facultyNo, classNo);

Example 3: Using CTAS to Create a DLI Table Using All or Part of the Data in the Source Table
---------------------------------------------------------------------------------------------

Example description: Based on the DLI table **table1** created in :ref:`Example 1: Creating a DLI Non-Partitioned Table <dli_08_0098__section175482343414>`, use the CTAS syntax to copy data from **table1** to **table1_ctas**.

When using CTAS to create a table, you can ignore the syntax used to create the table being copied. This means that regardless of the syntax used to create **table1**, you can use the DataSource syntax to create **table1_ctas**.

In addition, in this example, the storage format of **table1** is **orc**, and the storage format of **table1_ctas** may be **orc** or **parquet**. This means that the storage format of the table created by CTAS may be different from that of the original table.

Use the **SELECT** statement following the **AS** keyword to select required data and insert the data to **table1_ctas**.

The **SELECT** syntax is as follows: **SELECT <**\ *Column name* **> FROM <**\ *Table name* **> WHERE <**\ *Related filter criteria*\ **>**.

-  In this example, **SELECT \* FROM table1** is used. **\*** indicates that all columns are selected from **table1** and all data in **table1** is inserted into **table1_ctas**.

   ::

      CREATE TABLE IF NOT EXISTS table1_ctas
      USING parquet
      AS
      SELECT  *
      FROM    table1;

-  To filter and insert data into **table1_ctas** in a customized way, you can use the following **SELECT** statement: **SELECT col_1 FROM table1 WHERE col_1 = 'Ann'**. This will allow you to select only **col_1** from **table1** and insert data into **table1_ctas** where the value equals **'Ann'**.

   ::

      CREATE TABLE IF NOT EXISTS table1_ctas
      USING parquet
      AS
      SELECT  col_1
      FROM    table1
      WHERE   col_1 = 'Ann';

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
   USING parquet;

Example 5: Creating a DLI Partitioned Table and Customizing OPTIONS Parameters
------------------------------------------------------------------------------

Example description: When creating a DLI table, you can customize property names and values. For details about OPTIONS parameters, see :ref:`Table 2 <dli_08_0098__en-us_topic_0241764534_dli_08_0098_en-us_topic_0114776192_table16713182975016>`.

In this example, a DLI partitioned table named **table3** is created and partitioned based on **col_2**. Set **pmultiLevelDirEnable** and **compression** in **OPTIONS**.

-  **multiLevelDirEnable**: In this example, this parameter is set to **true**, indicating that all files and subdirectories in the table path are read iteratively when the table is queried. If this parameter is not required, set it to **false** or leave it blank (the default value is **false**).

-  **compression**: If the created OBS table needs to be compressed, you can use the keyword **compression** to configure the compression format. In this example, the **zstd** compression format is used.

   ::

      CREATE TABLE IF NOT EXISTs table3 (
          col_1   STRING,
          col_2   int
      )
      USING parquet
      PARTITIONED BY (col_2)
      OPTIONS (
          multiLeveldirenable    = true,
          compression            = 'zstd'
      );
