:original_name: dli_08_0155.html

.. _dli_08_0155:

ORDER BY
========

Function
--------

This statement is used to order the result set of a query by the specified field.

Syntax
------

::

   SELECT attr_expr_list FROM table_reference
     ORDER BY col_name
     [ASC | DESC] [,col_name [ASC | DESC],...];

Keyword
-------

-  **ASC/DESC**: ASC sorts from the lowest value to the highest value. DESC sorts from the highest value to the lowest value. **ASC** is the default sort order.
-  **ORDER BY**: specifies that the values in one or more columns should be sorted globally. When **ORDER BY** is used with **GROUP BY**, **ORDER BY** can be followed by the aggregate function.

Precautions
-----------

The to-be-sorted table must exist. If this statement is used to sort a table that does not exist, an error is reported.

Example
-------

To sort table **student** in ascending order according to field **score** and return the sorting result, run the following statement:

::

   SELECT * FROM student
     ORDER BY score;
