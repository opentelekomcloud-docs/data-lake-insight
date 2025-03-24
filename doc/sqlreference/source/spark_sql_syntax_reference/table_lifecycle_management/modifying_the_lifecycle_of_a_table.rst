:original_name: dli_08_0467.html

.. _dli_08_0467:

Modifying the Lifecycle of a Table
==================================

Function
--------

This section describes how to modify the lifecycle of an existing partitioned or non-partitioned table.

When the lifecycle function is enabled for the first time, the system scans tables or partitions, scans table data files in the path, and updates **LAST_ACCESS_TIME** of tables or partitions. The time required depends on the number of partitions and files.

Notes and Constraints
---------------------

-  The table lifecycle function currently only supports creating tables and versioning tables using Hive and Datasource syntax.
-  The unit of the lifecycle is in days. The value should be a positive integer.
-  The lifecycle can be set only at the table level. The lifecycle specified for a partitioned table applies to all partitions of the table.

Syntax
------

.. code-block::

   ALTER TABLE table_name
   SET TBLPROPERTIES("dli.lifecycle.days"='N')

Keywords
--------

**TBLPROPERTIES**: Table properties, which can be used to extend the lifecycle of a table.

Parameters
----------

.. table:: **Table 1** Parameters

   +--------------------+-----------+-------------------------------------------------------------------------------------------+
   | Parameter          | Mandatory | Description                                                                               |
   +====================+===========+===========================================================================================+
   | table_name         | Yes       | Name of the table whose lifecycle needs to be modified                                    |
   +--------------------+-----------+-------------------------------------------------------------------------------------------+
   | dli.lifecycle.days | Yes       | Lifecycle duration after the modification. The value must be a positive integer, in days. |
   +--------------------+-----------+-------------------------------------------------------------------------------------------+

Example
-------

-  Example 1: Enable the lifecycle function for the **test_lifecycle_exists** table and set the lifecycle to 50 days.

   .. code-block::

      alter table test_lifecycle_exists
      SET TBLPROPERTIES("dli.lifecycle.days"='50');

-  Example 2: Enable the lifecycle function for an existing partitioned or non-partitioned table for which lifecycle is not set, for example, for the **test_lifecycle_exists** table, and set the lifecycle to 50 days.

   .. code-block::

      alter table test_lifecycle_exists
      SET TBLPROPERTIES(
          "dli.lifecycle.days"='50',
          "dli.table.lifecycle.status"='enable'
      );
