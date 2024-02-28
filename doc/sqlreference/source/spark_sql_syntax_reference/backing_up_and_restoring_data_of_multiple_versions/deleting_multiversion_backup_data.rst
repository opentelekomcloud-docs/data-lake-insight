:original_name: dli_08_0355.html

.. _dli_08_0355:

Deleting Multiversion Backup Data
=================================

Function
--------

The retention period of multiversion backup data takes effect each time the **insert overwrite** or **truncate** statement is executed. If neither statement is executed for the table, multiversion backup data out of the retention period will not be automatically deleted. You can run the SQL commands described in this section to manually delete multiversion backup data.

Syntax
------

Delete multiversion backup data out of the retention period.

.. code-block::

   clear history for table [db_name.]table_name older_than 'timestamp';

Keywords
--------

-  clear history for table: Used to delete multiversion backup data
-  older_than: Used to specify the time range for deleting multiversion backup data

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
   | Timestamp  | Multiversion backup data generated before the timestamp will be deleted. Timestamp format: yyyy-MM-dd HH:mm:ss                                       |
   +------------+------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

-  Currently, the multiversion function supports only OBS tables created using the Hive syntax. For details about the syntax for creating a table, see :ref:`Creating an OBS Table Using the Hive Syntax <dli_08_0077>`.
-  This statement does not delete the backup data of the current version.

Example
-------

Delete the multiversion backup data generated before 2021-09-25 23:59:59 in the **dliTable** table. When the multiversion backup data is generated, a timestamp is generated.

.. code-block::

   clear history for table dliTable older_than '2021-09-25 23:59:59';
