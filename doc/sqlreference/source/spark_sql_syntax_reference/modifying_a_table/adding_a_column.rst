:original_name: dli_08_0263.html

.. _dli_08_0263:

Adding a Column
===============

Function
--------

This statement is used to add one or more new columns to a table.

Syntax
------

::

   ALTER TABLE [db_name.]table_name ADD COLUMNS (col_name1 col_type1 [COMMENT col_comment1], ...);

Keyword
-------

-  ADD COLUMNS: columns to add
-  COMMENT: column description

Parameters
----------

.. table:: **Table 1** Parameter description

   +-------------+----------------------------------------------------------------------------------------------------------------------------------+
   | Parameter   | Description                                                                                                                      |
   +=============+==================================================================================================================================+
   | db_name     | Database name that contains letters, digits, and underscores (_). It cannot contain only digits or start with an underscore (_). |
   +-------------+----------------------------------------------------------------------------------------------------------------------------------+
   | table_name  | Table name                                                                                                                       |
   +-------------+----------------------------------------------------------------------------------------------------------------------------------+
   | col_name    | Column name                                                                                                                      |
   +-------------+----------------------------------------------------------------------------------------------------------------------------------+
   | col_type    | Field type                                                                                                                       |
   +-------------+----------------------------------------------------------------------------------------------------------------------------------+
   | col_comment | Column description                                                                                                               |
   +-------------+----------------------------------------------------------------------------------------------------------------------------------+

Precautions
-----------

Do not run this SQL statement concurrently. Otherwise, columns may be overwritten.

Example
-------

::

   ALTER TABLE t1 ADD COLUMNS (column2 int, column3 string);
