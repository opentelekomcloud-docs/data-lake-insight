:original_name: dli_08_0091.html

.. _dli_08_0091:

Checking Table Creation Statements
==================================

Function
--------

This statement is used to show the statements for creating a table.

Syntax
------

::

   SHOW CREATE TABLE table_name;

Use the following syntax to view table creation statements when using Spark 3.3.1 (this applies only to querying table creation statements for Hive tables):

::

   SHOW CREATE TABLE table_name AS SERDE;

Keywords
--------

CREATE TABLE: statement for creating a table

Parameters
----------

.. table:: **Table 1** Parameter

   ========== ===========
   Parameter  Description
   ========== ===========
   table_name Table name
   ========== ===========

Precautions
-----------

The table specified in this statement must exist. Otherwise, an error will occur.

Example
-------

**Example of Spark 2.4.5:**

-  Run the following command to return the statement for creating the **testDB01.testTable5** table:

   **SHOW CREATE TABLE testDB01.testTable5**

-  Return the statement for creating the **test** table.

   .. code-block::

      createtab_stmt
       CREATE TABLE `testDB01`.`testTable5`(`id` INT, `age` INT, `money` DOUBLE)
      COMMENT 'test'
      ROW FORMAT SERDE 'org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe'
      WITH SERDEPROPERTIES (
        'serialization.format' = '1'
      )
      STORED AS
        INPUTFORMAT 'org.apache.hadoop.hive.ql.io.parquet.MapredParquetInputFormat'
        OUTPUTFORMAT 'org.apache.hadoop.hive.ql.io.parquet.MapredParquetOutputFormat'
      TBLPROPERTIES (
        'hive.serialization.extend.nesting.levels' = 'true',
        'ddlUpdateTime' = '1707202585460'
      )

**Example of Spark 3.3.1:**

-  Run the following command to return the statement for creating the **testDB02.testTable5** table:

   **SHOW CREATE TABLE testDB02.testTable5 AS SERDE**

-  Return the statement for creating the **test** table.

   .. code-block::

      createtab_stmt
       CREATE TABLE testDB02.testTable5 (
        id INT,
        age INT,
        money DOUBLE)
      ROW FORMAT SERDE 'org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe'
      WITH SERDEPROPERTIES (
        'serialization.format' = '1')
      STORED AS
        INPUTFORMAT 'org.apache.hadoop.hive.ql.io.parquet.MapredParquetInputFormat'
        OUTPUTFORMAT 'org.apache.hadoop.hive.ql.io.parquet.MapredParquetOutputFormat'
      TBLPROPERTIES (
        'hive.serialization.extend.nesting.levels' = 'true',
        'transient_lastDdlTime' = '1707201874')
