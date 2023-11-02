:original_name: dli_08_0352.html

.. _dli_08_0352:

Restoring Multiversion Backup Data
==================================

Function
--------

After the multiversion function is enabled, you can run the **RESTORE TABLE** statement to restore a table or partition of a specified version. For details about the syntax for enabling or disabling the multiversion function, see :ref:`Enabling or Disabling Multiversion Backup <dli_08_0354>`.

Currently, the multiversion function supports only OBS tables created using the Hive syntax. For details about the syntax for creating a table, see :ref:`Creating an OBS Table Using the Hive Syntax <dli_08_0077>`.

Syntax
------

-  Restore the non-partitioned table data to the backup data of a specified version.

   .. code-block::

      RESTORE TABLE [db_name.]table_name TO VERSION 'version_id';

-  Restore the data of a single partition in a partitioned table to the backup data of a specified version.

   .. code-block::

      RESTORE TABLE [db_name.]table_name PARTITION (column = value, ...) TO VERSION 'version_id';

Keyword
-------

-  RESTORE TABLE: Used to restore backup data
-  PARTITION: Used to specify the partition column
-  TO VERSION: Used to specify the version number You can run the **SHOW HISTORY** command to obtain the version number. For details, see :ref:`Viewing Multiversion Backup Data <dli_08_0351>`.

Parameter
---------

.. table:: **Table 1** Parameter description

   +------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Description                                                                                                                                                                                     |
   +============+=================================================================================================================================================================================================+
   | db_name    | Database name, which consists of letters, digits, and underscores (_). The value cannot contain only digits or start with a digit or underscore (_).                                            |
   +------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name | Table name                                                                                                                                                                                      |
   +------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | column     | Partition column name                                                                                                                                                                           |
   +------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | value      | Value corresponding to the partition column name                                                                                                                                                |
   +------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | version_id | Target version of the backup data to be restored You can run the **SHOW HISTORY** command to obtain the version number. For details, see :ref:`Viewing Multiversion Backup Data <dli_08_0351>`. |
   +------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

Currently, the multiversion function supports only OBS tables created using the Hive syntax. For details about the syntax for creating a table, see :ref:`Creating an OBS Table Using the Hive Syntax <dli_08_0077>`.

Example
-------

-  Restore the data in non-partitioned table **test_table** to version 20210930.

   ::

      RESTORE TABLE test_table TO VERSION '20210930';

-  Restore the data of partition **dt** in partitioned table **test_table** to version 20210930.

   ::

      RESTORE TABLE test_table PARTITION (dt='2021-07-27') TO VERSION '20210930';
