:original_name: dli_08_0164.html

.. _dli_08_0164:

GROUPING SETS
=============

Function
--------

This statement is used to generate the cross-table row and achieve the cross-statistics of the GROUP BY field.

Syntax
------

::

   SELECT attr_expr_list FROM table_reference
     GROUP BY col_name_list
     GROUPING SETS(col_name_list);

Keyword
-------

GROUPING SETS is the expansion of GROUP BY. For example:

-  **SELECT a, b, sum(expression) FROM table GROUP BY a, b GROUPING SETS((a,b));**

   It can be converted to the following query:

   ::

      SELECT a, b, sum(expression) FROM table
        GROUP BY a, b;

-  **SELECT a, b, sum(expression) FROM table GROUP BY a, b GROUPING SETS(a,b);**

   It can be converted to the following two queries:

   ::

      SELECT a, NULL, sum(expression) FROM table GROUP BY a;
      UNION
      SELECT NULL, b, sum(expression) FROM table GROUP BY b;

-  **SELECT a, b, sum(expression) FROM table GROUP BY a, b GROUPING SETS((a,b), a);**

   It can be converted to the following two queries:

   ::

      SELECT a, b, sum(expression) FROM table GROUP BY a, b;
      UNION
      SELECT a, NULL, sum(expression) FROM table GROUP BY a;

-  **SELECT a, b, sum(expression) FROM table GROUP BY a, b GROUPING SETS((a,b), a, b, ());**

   It can be converted to the following four queries:

   ::

      SELECT a, b, sum(expression) FROM table GROUP BY a, b;
      UNION
      SELECT a, NULL, sum(expression) FROM table GROUP BY a, NULL;
      UNION
      SELECT NULL, b, sum(expression) FROM table GROUP BY NULL, b;
      UNION
      SELECT NULL, NULL, sum(expression) FROM table;

Precautions
-----------

-  The to-be-grouped table must exist. Otherwise, an error is reported.
-  Different from ROLLUP, there is only one syntax for GROUPING SETS.

Example
-------

To generate the cross-table row according to the **group_id** and **job** fields and return the total salary on each aggregation condition, run the following statement:

::

   SELECT group_id, job, SUM(salary) FROM group_test
     GROUP BY group_id, job
     GROUPING SETS (group_id, job);
