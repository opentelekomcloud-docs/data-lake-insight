:original_name: dli_08_0153.html

.. _dli_08_0153:

HAVING Filtering Clause
=======================

Function
--------

This statement is used to filter the query results using the HAVING clause.

Syntax
------

::

   SELECT [ALL | DISTINCT] attr_expr_list FROM table_reference
     [WHERE where_condition]
     [GROUP BY col_name_list]
     HAVING having_condition;

Keyword
-------

-  All is used to return repeated rows. By default, all repeated rows are returned. It is followed by asterisks (*) only. Otherwise, an error will occur.
-  DISTINCT is used to remove the repeated line from the result.
-  Generally, HAVING and GROUP BY are used together. GROUP BY applies first for grouping and HAVING then applies for filtering. The arithmetic operation and aggregate function are supported by the HAVING clause.

Precautions
-----------

-  The to-be-queried table must exist.
-  If the filtering condition is subject to the query results of GROUP BY, the HAVING clause, rather than the WHERE clause, must be used for filtering.

Example
-------

Group the **student** table according to the **name** field and filter the records in which the maximum score is higher than 95 based on groups.

::

   SELECT name, max(score) FROM student
     GROUP BY name
     HAVING max(score) >95;
