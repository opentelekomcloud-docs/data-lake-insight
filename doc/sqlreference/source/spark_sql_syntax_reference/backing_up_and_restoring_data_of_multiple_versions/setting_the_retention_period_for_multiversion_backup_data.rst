:original_name: dli_08_0350.html

.. _dli_08_0350:

Setting the Retention Period for Multiversion Backup Data
=========================================================

Function
--------

After multiversion is enabled, backup data is retained for seven days by default. You can change the retention period by setting system parameter **dli.multi.version.retention.days**. Multiversion data out of the retention period will be automatically deleted when the **insert overwrite** or **truncate** statement is executed. You can also set table attribute **dli.multi.version.retention.days** to adjust the retention period when adding a column or modifying a partitioned table.

For details about the syntax for enabling or disabling the multiversion function, see :ref:`Enabling or Disabling Multiversion Backup <dli_08_0354>`.

Currently, the multiversion function supports only OBS tables created using the Hive syntax. For details about the syntax for creating a table, see :ref:`Creating an OBS Table Using the Hive Syntax <dli_08_0077>`.

Syntax
------

::

   ALTER TABLE [db_name.]table_name
   SET TBLPROPERTIES ("dli.multi.version.retention.days"="days");

Keywords
--------

-  TBLPROPERTIES: This keyword is used to add a **key/value** property to a table.

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
   | days       | Date when the multiversion backup data is reserved. The default value is 7 days. The value ranges from 1 to 7 days.                                  |
   +------------+------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

Currently, the multiversion function supports only OBS tables created using the Hive syntax. For details about the syntax for creating a table, see :ref:`Creating an OBS Table Using the Hive Syntax <dli_08_0077>`.

Example
-------

Set the retention period of multiversion backup data to 5 days.

::

   ALTER TABLE test_table
   SET TBLPROPERTIES ("dli.multi.version.retention.days"="5");
