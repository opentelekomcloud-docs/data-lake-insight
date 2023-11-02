:original_name: dli_08_0156.html

.. _dli_08_0156:

SORT BY
=======

Function
--------

This statement is used to achieve the partial sorting of tables according to fields.

Syntax
------

::

   SELECT attr_expr_list FROM table_reference
     SORT BY col_name
     [ASC | DESC] [,col_name [ASC | DESC],...];

Keyword
-------

-  ASC/DESC: ASC sorts from the lowest value to the highest value. DESC sorts from the highest value to the lowest value. ASC is the default sort order.
-  SORT BY: Used together with GROUP BY to perform local sorting of a single column or multiple columns for PARTITION.

Precautions
-----------

The to-be-sorted table must exist. If this statement is used to sort a table that does not exist, an error is reported.

Example
-------

To sort the **student** table in ascending order of the **score** field in Reducer, run the following statement:

::

   SELECT * FROM student
     SORT BY score;
