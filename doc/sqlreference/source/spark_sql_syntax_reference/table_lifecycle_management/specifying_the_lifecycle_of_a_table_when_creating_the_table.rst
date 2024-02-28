:original_name: en-us_topic_0000001621263317.html

.. _en-us_topic_0000001621263317:

Specifying the Lifecycle of a Table When Creating the Table
===========================================================

Function
--------

DLI provides table lifecycle management to allow you to specify the lifecycle of a table when creating the table. DLI determines whether to reclaim a table based on the table's last modification time and its lifecycle. By setting the lifecycle of a table, you can better manage a large number of tables, automatically delete data tables that are no longer used for a long time, and simplify the process of reclaiming data tables. Additionally, data restoration settings are supported to prevent data loss caused by misoperations.

Table Reclamation Rules
-----------------------

-  When creating a table, use **TBLPROPERTIES** to specify the lifecycle of the table.

   -  **Non-partitioned table**

      If the table is not a partitioned table, the system determines whether to reclaim the table after the lifecycle time based on the last modification time of the table.

   -  **Partitioned table**

      If the table is a partitioned table, the system determines whether the partition needs to be reclaimed based on the last modification time (LAST_ACCESS_TIME) of the partition. After the last partition of a partitioned table is reclaimed, the table is not deleted.

      Only table-level lifecycle management is supported for partitioned tables.

-  Lifecycle reclamation starts at a specified time every day to scan all partitions.

   Lifecycle reclamation starts at a specified time every day. Reclamation only occurs if the last modification time of the table data (**LAST_ACCESS_TIME**) detected when scanning complete partitions exceeds the time specified by the lifecycle.

   Assume that the lifecycle of a partitioned table is one day and the last modification time of the partitioned data is 15:00 on May 20, 2023. If the table is scanned before 15:00 on May 20, 2023 (less than one day), the partitions in the table will not be reclaimed. If the last data modification time (**LAST_ACCESS_TIME**) of a table partition exceeds the time specified by the lifecycle during reclamation scan on May 20, 2023, the partition will be reclaimed.

-  The lifecycle function periodically reclaims tables or partitions, which are reclaimed irregularly every day depending on the level of busyness of the service. It cannot ensure that a table or partition will be reclaimed immediately after its lifecycle expires.

-  After a table is deleted, all properties of the table, including the lifecycle, will be deleted. After a table with the same name is created again, the lifecycle of the table will be determined by the new property.

Constraints and Limitations
---------------------------

-  Before using the lifecycle function, log in to the DLI console, choose **Global Configuration** > **Service Authorization**, select **Tenant Administrator(Project-level)**, and click **Update** on the **Assign Agency Permissions** page.
-  The table lifecycle function currently only supports creating tables and versioning tables using Hive and Datasource syntax.
-  The unit of the lifecycle is in days. The value should be a positive integer.
-  The lifecycle can be set only at the table level. The lifecycle specified for a partitioned table applies to all partitions of the table.
-  After the lifecycle is set, DLI and OBS tables will support data backup. The backup directory for OBS tables needs to be set manually. The backup directory must be in the parallel file system and in the same bucket as the original table directory. It cannot have the same directory or subdirectory name as the original table.

Syntax
------

-  **Creating a DLI table using the Datasource syntax**

   .. code-block::

      CREATE TABLE table_name(name string, id int)
      USING parquet
      TBLPROPERTIES( "dli.lifecycle.days"=1 );

-  **Creating a DLI table using the Hive syntax**

   .. code-block::

      CREATE TABLE table_name(name string, id int)
      stored as parquet
      TBLPROPERTIES( "dli.lifecycle.days"=1 );

-  **Creating an OBS table using the Datasource syntax**

   .. code-block::

      CREATE TABLE table_name(name string, id int)
      USING parquet
      OPTIONS (path "obs://dli-test/table_name")
      TBLPROPERTIES( "dli.lifecycle.days"=1, "external.table.purge"='true', "dli.lifecycle.trash.dir"='obs://dli-test/Lifecycle-Trash' );

-  **Creating an OBS table using the Hive syntax**

   ::

      CREATE TABLE table_name(name string, id int)
      STORED AS parquet
      LOCATION 'obs://dli-test/table_name'
      TBLPROPERTIES( "dli.lifecycle.days"=1, "external.table.purge"='true', "dli.lifecycle.trash.dir"='obs://dli-test/Lifecycle-Trash' );

Keywords
--------

-  **TBLPROPERTIES**: Table properties, which can be used to extend the lifecycle of a table.
-  **OPTIONS**: path of the new table, which is applicable to OBS tables created using the Datasource syntax.
-  **LOCATION**: path of the new table, which is applicable to OBS tables created using the Hive syntax.

Parameters
----------

.. table:: **Table 1** Parameters

   +-------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter               | Mandatory             | Description                                                                                                                                  |
   +=========================+=======================+==============================================================================================================================================+
   | table_name              | Yes                   | Name of the table whose lifecycle needs to be set                                                                                            |
   +-------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------+
   | dli.lifecycle.days      | Yes                   | Lifecycle duration. The value must be a positive integer, in days.                                                                           |
   +-------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------+
   | external.table.purge    | No                    | This parameter is available only for OBS tables.                                                                                             |
   |                         |                       |                                                                                                                                              |
   |                         |                       | Whether to clear data in the path when deleting a table or partition. The data is not cleared by default.                                    |
   |                         |                       |                                                                                                                                              |
   |                         |                       | When this parameter is set to **true**:                                                                                                      |
   |                         |                       |                                                                                                                                              |
   |                         |                       | -  After a file is deleted from a non-partitioned OBS table, the table directory is also deleted.                                            |
   |                         |                       | -  The custom partition data in the partitioned OBS table is also deleted.                                                                   |
   +-------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------+
   | dli.lifecycle.trash.dir | No                    | This parameter is available only for OBS tables.                                                                                             |
   |                         |                       |                                                                                                                                              |
   |                         |                       | When **external.table.purge** is set to **true**, the backup directory will be deleted. By default, backup data is deleted seven days later. |
   +-------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

-  **Create the test_datasource_lifecycle table using the Datasource syntax. The lifecycle is set to 100 days.**

   ::

      CREATE TABLE test_datasource_lifecycle(id int)
      USING parquet
      TBLPROPERTIES( "dli.lifecycle.days"=100);

-  **Create the test_hive_lifecycle table using the Hive syntax. The lifecycle is set to 100 days.**

   ::

      CREATE TABLE test_hive_lifecycle(id int)
      stored as parquet
      TBLPROPERTIES( "dli.lifecycle.days"=100);

-  **Create the test_datasource_lifecycle_obs table using the Datasource syntax. The lifecycle is set to 100 days. When the lifecycle expires, data is deleted by default and backed up to the obs://dli-test/ directory.**

   ::

      CREATE TABLE test_datasource_lifecycle_obs(name string, id int)
      USING parquet
      OPTIONS (path "obs://dli-test/xxx")
      TBLPROPERTIES( "dli.lifecycle.days"=100, "external.table.purge"='true', "dli.lifecycle.trash.dir"='obs://dli-test/Lifecycle-Trash' );

-  **Create the test_hive_lifecycle_obs table using the Hive syntax. The lifecycle is set to 100 days. When the lifecycle expires, data is deleted by default and backed up to the obs://dli-test/ directory.**

   ::

      CREATE TABLE test_hive_lifecycle_obs(name string, id int)
      STORED AS parquet
      LOCATION 'obs://dli-test/xxx'
      TBLPROPERTIES( "dli.lifecycle.days"=100, "external.table.purge"='true', "dli.lifecycle.trash.dir"='obs://dli-test/Lifecycle-Trash' );
