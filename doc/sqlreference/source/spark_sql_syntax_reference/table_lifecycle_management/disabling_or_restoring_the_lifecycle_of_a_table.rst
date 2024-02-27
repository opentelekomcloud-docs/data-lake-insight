:original_name: en-us_topic_0000001621542965.html

.. _en-us_topic_0000001621542965:

Disabling or Restoring the Lifecycle of a Table
===============================================

Function
--------

This section describes how to disable or restore the lifecycle of a specified table or partition.

You can disable or restore the lifecycle of a table in either of the following scenarios:

#. If the lifecycle function has been enabled for a table or partitioned table, the system allows you to disable or restore the lifecycle of the table by changing the value of **dli.table.lifecycle.status**.
#. If the lifecycle function is not enabled for a table or partitioned table, the system will add the **dli.table.lifecycle.status** property to allow you to disable or restore the lifecycle function of the table.

Constraints and Limitations
---------------------------

-  The table lifecycle function currently only supports creating tables and versioning tables using Hive and Datasource syntax.
-  The unit of the lifecycle is in days. The value should be a positive integer.
-  The lifecycle can be set only at the table level. The lifecycle specified for a partitioned table applies to all partitions of the table.

Syntax
------

-  This syntax can be used to disable or restore the lifecycle of a table at the table level.

   ::

      ALTER TABLE table_name SET TBLPROPERTIES("dli.table.lifecycle.status"={enable|disable});

-  This syntax can be used to disable or restore the lifecycle of a specified table at the table or partition table level.

   ::

      ALTER TABLE table_name [pt_spec] LIFECYCLE {enable|disable};

Keywords
--------

**TBLPROPERTIES**: Table properties, which can be used to extend the lifecycle of a table.

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Mandatory             | Description                                                                                                                                                                                                                                                                                                                                            |
   +=======================+=======================+========================================================================================================================================================================================================================================================================================================================================================+
   | table_name            | Yes                   | Name of the table whose lifecycle is to be disabled or restored                                                                                                                                                                                                                                                                                        |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | pt_spec               | No                    | Partition information of the table whose lifecycle is to be disabled or restored. The format is **partition_col1=col1_value1, partition_col2=col2_value1...**. For a table with multi-level partitions, all partition values must be specified.                                                                                                        |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | enable                | No                    | Restores the lifecycle function of a table or a specified partition.                                                                                                                                                                                                                                                                                   |
   |                       |                       |                                                                                                                                                                                                                                                                                                                                                        |
   |                       |                       | -  The table and its partitions participate in lifecycle reclamation again. By default, the lifecycle configuration of the current table and its partitions is used.                                                                                                                                                                                   |
   |                       |                       | -  Before enabling the table lifecycle function, it is recommended to modify the lifecycle configuration of the table and its partitions. This will help prevent any accidental data reclamation caused by the previous configuration once the table lifecycle function is enabled.                                                                    |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | disable               | No                    | Disables the lifecycle function of a table or a specified partition.                                                                                                                                                                                                                                                                                   |
   |                       |                       |                                                                                                                                                                                                                                                                                                                                                        |
   |                       |                       | -  Prevents a table and all its partitions from being reclaimed by the lifecycle. It takes priority over restoring the lifecycle of a table and its partitions. That is, when the lifecycle function of a table or a specified partition is disabled, the partition information of the table whose lifecycle is to be disabled or restored is invalid. |
   |                       |                       | -  After the lifecycle function of a table is disabled, the lifecycle configuration of the table and the enable and disable flags of its partitions are retained.                                                                                                                                                                                      |
   |                       |                       | -  After the lifecycle function of a table is disabled, the lifecycle configuration of the table and partitioned table can still be modified.                                                                                                                                                                                                          |
   +-----------------------+-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

-  Example 1: Disable the lifecycle function of the **test_lifecycle** table.

   ::

      alter table test_lifecycle SET TBLPROPERTIES("dli.table.lifecycle.status"='disable');

-  Example 2: Disable the lifecycle function for the partition whose time is **20230520** in the **test_lifecycle** table.

   ::

      alter table test_lifecycle partition (dt='20230520') LIFECYCLE 'disable';

   .. note::

      -  After the lifecycle function of a partitioned table is disabled, the lifecycle function of all partitions within the table will also be disabled.
