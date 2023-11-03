:original_name: dli_08_0105.html

.. _dli_08_0105:

Viewing Table Statistics
========================

Function
--------

This statement is used to view the table statistics. The names and data types of all columns in a specified table will be returned.

Syntax
------

::

   DESCRIBE [EXTENDED|FORMATTED] [db_name.]table_name;

Keyword
-------

-  EXTENDED: displays all metadata of the specified table. It is used during debugging in general.
-  FORMATTED: displays all metadata of the specified table in a form.

Parameters
----------

.. table:: **Table 1** Parameter description

   +------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Description                                                                                                                                                                                                                                                                                 |
   +============+=============================================================================================================================================================================================================================================================================================+
   | db_name    | Database name that contains letters, digits, and underscores (_). It cannot contain only digits or start with an underscore (_).                                                                                                                                                            |
   +------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name | Table name of a database that contains letters, digits, and underscores (_). It cannot contain only digits or start with an underscore (_). The matching rule is **^(?!_)(?![0-9]+$)[A-Za-z0-9_$]*$**. If special characters are required, use single quotation marks ('') to enclose them. |
   +------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

The to-be-queried table must exist. If this statement is used to query the information about a table that does not exist, an error is reported.

Example
-------

To query the names and data types of all columns in the **student** table, run the following statement:

::

   DESCRIBE student;
