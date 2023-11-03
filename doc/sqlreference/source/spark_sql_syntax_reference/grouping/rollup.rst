:original_name: dli_08_0163.html

.. _dli_08_0163:

ROLLUP
======

Function
--------

This statement is used to generate the aggregate row, super-aggregate row, and the total row. The statement can achieve multi-layer statistics from right to left and display the aggregation of a certain layer.

Syntax
------

::

   SELECT attr_expr_list FROM table_reference
     GROUP BY col_name_list
     WITH ROLLUP;

Keyword
-------

ROLLUP is the expansion of GROUP BY. For example, **SELECT a, b, c, SUM(expression) FROM table GROUP BY a, b, c WITH ROLLUP;** can be transformed into the following query statements:

-  Counting the (a, b, c) combinations

   ::

      SELECT a, b, c, sum(expression) FROM table
        GROUP BY a, b, c;

-  Counting the (a, b) combinations

   ::

      SELECT a, b, NULL, sum(expression) FROM table
        GROUP BY a, b;

-  Counting the (a) combinations

   ::

      SELECT a, NULL, NULL, sum(expression) FROM table
        GROUP BY a;

-  Total

   ::

      SELECT NULL, NULL, NULL, sum(expression) FROM table;

Precautions
-----------

The to-be-grouped table must exist. If this statement is used to group a table that does not exist, an error is reported.

Example
-------

To generate the aggregate row, super-aggregate row, and total row according to the group_id and job fields and return the total salary on each aggregation condition, run the following statement:

::

   SELECT group_id, job, SUM(salary) FROM group_test
     GROUP BY group_id, job
     WITH ROLLUP;
