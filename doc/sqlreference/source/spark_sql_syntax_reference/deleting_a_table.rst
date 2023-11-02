:original_name: dli_08_0087.html

.. _dli_08_0087:

Deleting a Table
================

Function
--------

This statement is used to delete tables.

Syntax
------

::

   DROP TABLE [IF EXISTS] [db_name.]table_name;

Keyword
-------

-  If the table is stored in OBS, only the metadata is deleted. The data stored on OBS is not deleted.
-  If the table is stored in DLI, the data and the corresponding metadata are all deleted.

Parameters
----------

.. table:: **Table 1** Parameter description

   +------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Description                                                                                                                                          |
   +============+======================================================================================================================================================+
   | db_name    | Database name, which consists of letters, digits, and underscores (_). The value cannot contain only digits or start with a digit or underscore (_). |
   +------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name | Table name                                                                                                                                           |
   +------------+------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

The to-be-deleted table must exist in the current database. Otherwise, an error is reported. To avoid this error, add **IF EXISTS** in this statement.

Example
-------

#. Create a table. For details, see :ref:`Creating an OBS Table <dli_08_0223>` or :ref:`Creating a DLI Table <dli_08_0224>`.

#. Run the following statement to delete table **test** from the current database:

   ::

      DROP TABLE IF EXISTS test;
