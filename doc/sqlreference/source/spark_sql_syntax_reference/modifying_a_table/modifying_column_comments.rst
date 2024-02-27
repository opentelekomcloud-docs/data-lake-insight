:original_name: dli_08_0470.html

.. _dli_08_0470:

Modifying Column Comments
=========================

Function
--------

You can modify the column comments of non-partitioned or partitioned tables.

Syntax
------

.. code-block::

   ALTER TABLE [db_name.]table_name CHANGE COLUMN col_name col_name col_type COMMENT 'col_comment';

Keywords
--------

-  CHANGE COLUMN: Modify a column.
-  COMMENT: column description

Parameters
----------

.. table:: **Table 1** Parameters

   +-------------+-----------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter   | Mandatory | Description                                                                                                                                |
   +=============+===========+============================================================================================================================================+
   | db_name     | No        | Database name. Only letters, digits, and underscores (_) are allowed. The name cannot contain only digits or start with an underscore (_). |
   +-------------+-----------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | table_name  | Yes       | Table name                                                                                                                                 |
   +-------------+-----------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | col_name    | Yes       | Column name. The value must be the name of an existing column.                                                                             |
   +-------------+-----------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | col_type    | Yes       | Column data type specified when the table is created, which cannot be modified.                                                            |
   +-------------+-----------+--------------------------------------------------------------------------------------------------------------------------------------------+
   | col_comment | Yes       | Column comment after modification. The comment can contain a maximum of 1024 bytes.                                                        |
   +-------------+-----------+--------------------------------------------------------------------------------------------------------------------------------------------+

Example
-------

Change the comment of the **c1** column in the **t1** table to **the new comment**.

::

   ALTER TABLE t1 CHANGE COLUMN c1 c1 STRING COMMENT 'the new comment';
