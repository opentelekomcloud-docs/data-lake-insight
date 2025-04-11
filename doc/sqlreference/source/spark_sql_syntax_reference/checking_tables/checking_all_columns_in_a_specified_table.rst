:original_name: dli_08_0093.html

.. _dli_08_0093:

Checking All Columns in a Specified Table
=========================================

Function
--------

This statement is used to query all columns in a specified table.

Syntax
------

::

   SHOW COLUMNS {FROM | IN} table_name [{FROM | IN} db_name];

Keywords
--------

-  COLUMNS: columns in the current table
-  FROM/IN: followed by the name of a database whose tables and views will be displayed. Keyword FROM is equivalent to IN. You can use either of them in a statement.

Parameters
----------

.. table:: **Table 1** Parameters

   ========== =============
   Parameter  Description
   ========== =============
   table_name Table name
   db_name    Database name
   ========== =============

Precautions
-----------

The specified table must exist in the database. If the table does not exist, an error is reported.

Example
-------

Run the following statement to view all columns in the **student** table.

::

   SHOW COLUMNS IN student;
