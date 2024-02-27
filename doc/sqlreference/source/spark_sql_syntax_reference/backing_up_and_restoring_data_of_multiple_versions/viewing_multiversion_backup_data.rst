:original_name: dli_08_0351.html

.. _dli_08_0351:

Viewing Multiversion Backup Data
================================

Function
--------

After the multiversion function is enabled, you can run the **SHOW HISTORY** command to view the backup data of a table. For details about the syntax for enabling or disabling the multiversion function, see :ref:`Enabling or Disabling Multiversion Backup <dli_08_0354>`.

Currently, the multiversion function supports only OBS tables created using the Hive syntax. For details about the syntax for creating a table, see :ref:`Creating an OBS Table Using the Hive Syntax <dli_08_0077>`.

Syntax
------

-  View the backup data of a non-partitioned table.

   .. code-block::

      SHOW HISTORY FOR TABLE [db_name.]table_name;

-  View the backup data of a specified partition.

   .. code-block::

      SHOW HISTORY FOR TABLE [db_name.]table_name PARTITION (column = value, ...);

Keywords
--------

-  SHOW HISTORY FOR TABLE: Used to view backup data
-  PARTITION: Used to specify the partition column

Parameters
----------

.. table:: **Table 1** Parameters

   +------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Description                                                                                                                                          |
   +============+======================================================================================================================================================+
   | db_name    | Database name, which consists of letters, digits, and underscores (_). The value cannot contain only digits or start with a digit or underscore (_). |
   +------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name | Table name                                                                                                                                           |
   +------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | column     | Partition column name                                                                                                                                |
   +------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value      | Value corresponding to the partition column name                                                                                                     |
   +------------+------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

Currently, the multiversion function supports only OBS tables created using the Hive syntax. For details about the syntax for creating a table, see :ref:`Creating an OBS Table Using the Hive Syntax <dli_08_0077>`.

Example
-------

-  View multiversion backup data of the **test_table** table.

   ::

      SHOW HISTORY FOR TABLE test_table;

-  View multiversion backup data of the **dt** partition in the **test_table** partitioned table.

   ::

      SHOW HISTORY FOR TABLE test_table PARTITION (dt='2021-07-27');
