:original_name: dli_08_0095.html

.. _dli_08_0095:

Inserting Data
==============

Function
--------

This statement is used to insert the SELECT query result or a certain data record into a table.

Notes and Constraints
---------------------

-  The **INSERT OVERWRITE** syntax is not suitable for "read-write" scenarios, where data is continuously processed and updated. Using this syntax in such scenarios may result in data loss.

   "Read-write" refers to the ability to read data while generating new data or modifying existing data during data processing.

-  When using Hive and Datasource tables (excluding Hudi), executing data modification commands (such as **insert into** and **load data**) may result in data duplication or inconsistency if the data source does not support transactions and there is a system failure or queue restart.

   To avoid this situation, you are advised to prioritize data sources that support transactions, such as Hudi data sources. This type of data source has Atomicity, Consistency, Isolation, Durability (ACID) capabilities, which helps ensure data consistency and accuracy.

   To learn more, refer to :ref:`How Do I Handle Duplicate Records After Executing the INSERT INTO Statement? <dli_08_0095__section1516329541>`

Syntax
------

-  Insert the SELECT query result into a table.

   ::

      INSERT INTO [TABLE] [db_name.]table_name
        [PARTITION part_spec] select_statement;

   ::

      INSERT OVERWRITE TABLE [db_name.]table_name
        [PARTITION part_spec] select_statement;

   .. code-block::

      part_spec:
        : (part_col_name1=val1 [, part_col_name2=val2, ...])

-  Insert a data record into a table.

   ::

      INSERT INTO [TABLE] [db_name.]table_name
        [PARTITION part_spec] VALUES values_row [, values_row ...];

   ::

      INSERT OVERWRITE TABLE [db_name.]table_name
        [PARTITION part_spec] VALUES values_row [, values_row ...];

   .. code-block::

      values_row:
        : (val1 [, val2, ...])

Keywords
--------

.. table:: **Table 1** INSERT keywords

   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter        | Description                                                                                                                                                                                                                                                        |
   +==================+====================================================================================================================================================================================================================================================================+
   | db_name          | Name of the database where the target table resides.                                                                                                                                                                                                               |
   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name       | Name of the target table.                                                                                                                                                                                                                                          |
   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | part_spec        | Detailed partition information. If there are multiple partition fields, all fields must be contained, but the corresponding values are optional. The system matches the corresponding partition. A maximum of 100,000 partitions can be created in a single table. |
   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | select_statement | SELECT query on the source table (DLI and OBS tables).                                                                                                                                                                                                             |
   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | values_row       | Value to be inserted to a table. Use commas (,) to separate columns.                                                                                                                                                                                               |
   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

-  The target DLI table must exist.

-  If no partition needs to be specified for dynamic partitioning, place **part_spec** in the SELECT statement as a common field.

-  During creation of the target OBS table, only the folder path can be specified.

-  The source table and the target table must have the same data types and column field quantity. Otherwise, data insertion fails.

-  You are not advised to insert data concurrently into the same table as it may result in abnormal data insertion due to concurrency conflicts.

-  The **INSERT INTO** statement is used to add the query result to the target table.

-  The **INSERT OVERWRITE** statement is used to overwrite existing data in the source table.

-  The **INSERT INTO** statement can be batch executed, but the **INSERT OVERWRITE** statement can be batch executed only when data of different partitioned tables is inserted to different static partitions.

-  The **INSERT INTO** and **INSERT OVERWRITE** statements can be executed at the same time. However, the result is unknown.

-  When you insert data of the source table to the target table, you cannot import or update data of the source table.

-  The dynamic INSERT OVERWRITE statement of Hive partitioned tables can overwrite the involved partition data but cannot overwrite the entire table data.

-  To overwrite data in a specified partition of the datasource table, set **dli.sql.dynamicPartitionOverwrite.enabled** to **true** and run the **insert overwrite** statement. The default value of **dli.sql.dynamicPartitionOverwrite.enabled** is **false**, indicating that data in the entire table is overwritten. The following is an example:

   ::

      insert overwrite table tb1 partition(part1='v1', part2='v2') select * from ...

   .. note::

      On the DLI management console, click **SQL Editor**. In the upper right corner of the editing window, click **Settings** to configure parameters.

-  You can configure the **spark.sql.shuffle.partitions** parameter to set the number of files to be inserted into the OBS bucket in the non-DLI table. In addition, to avoid data skew, you can add **distribute by rand()** to the end of the INSERT statement to increase the number of concurrent jobs. The following is an example:

   .. code-block::

      insert into table table_target select * from table_source distribute by cast(rand() * N as int);

Example
-------

.. note::

   Before importing data, you must create a table. For details, see :ref:`Creating an OBS Table <dli_08_0223>` or :ref:`Creating a DLI Table <dli_08_0224>`.

-  Insert the SELECT query result into a table.

   -  Use the DataSource syntax to create a parquet partitioned table.

      .. code-block::

         CREATE TABLE data_source_tab1 (col1 INT, p1 INT, p2 INT)
           USING PARQUET PARTITIONED BY (p1, p2);

   -  Insert the query result to the partition (p1 = 3, p2 = 4).

      .. code-block::

         INSERT INTO data_source_tab1 PARTITION (p1 = 3, p2 = 4)
           SELECT id FROM RANGE(1, 3);

   -  Insert the new query result to the partition (p1 = 3, p2 = 4).

      .. code-block::

         INSERT OVERWRITE TABLE data_source_tab1 PARTITION (p1 = 3, p2 = 4)
           SELECT id FROM RANGE(3, 5);

-  Insert a data record into a table.

   -  Create a Parquet partitioned table with Hive format

      .. code-block::

         CREATE TABLE hive_serde_tab1 (col1 INT, p1 INT, p2 INT)
           USING HIVE OPTIONS(fileFormat 'PARQUET') PARTITIONED BY (p1, p2);

   -  Insert two data records into the partition (p1 = 3, p2 = 4).

      .. code-block::

         INSERT INTO hive_serde_tab1 PARTITION (p1 = 3, p2 = 4)
           VALUES (1), (2);

   -  Insert new data to the partition (p1 = 3, p2 = 4).

      .. code-block::

         INSERT OVERWRITE TABLE hive_serde_tab1 PARTITION (p1 = 3, p2 = 4)
           VALUES (3), (4);

.. _dli_08_0095__section1516329541:

How Do I Handle Duplicate Records After Executing the INSERT INTO Statement?
----------------------------------------------------------------------------

-  **Symptom**

   When using Hive and Datasource tables (excluding Hudi), executing data modification commands (such as **insert into** and **load data**) may result in data duplication or inconsistency if the data source does not support transactions and there is a system failure or queue restart.

-  **Possible causes**

   If queue resources are restarted in the data commit phase, data may have been restored to a formal directory. If an **insert into** statement is executed and a retry is triggered after a resource restart, there is a possibility that data will be repeatedly written.

-  **Solution**

   #. Hudi data sources that support ACID properties are recommended.
   #. Use idempotent syntax such as **insert overwrite** instead of non-idempotent syntax such as **insert into** to insert data.
   #. If it is strictly required that data cannot be duplicated, you are advised to perform deduplication on the table data after executing the **insert into** statement to prevent duplicate data.
