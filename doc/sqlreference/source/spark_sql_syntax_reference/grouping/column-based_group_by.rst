:original_name: dli_08_0160.html

.. _dli_08_0160:

Column-Based GROUP BY
=====================

Function
--------

This statement is used to group a table based on columns.

Syntax
------

::

   SELECT attr_expr_list FROM table_reference
     GROUP BY col_name_list;

Keyword
-------

Column-based GROUP BY can be categorized into single-column GROUP BY and multi-column GROUP BY.

-  Single-column GROUP BY indicates that the GROUP BY clause contains only one column. The fields in **col_name_list** must exist in **attr_expr_list**. The aggregate function, **count()** and **sum()** for example, is supported in **attr_expr_list**. The aggregate function can contain other fields.
-  Multi-column GROUP BY indicates that there is more than one column in the GROUP BY clause. The query statement is grouped according to all the fields in the GROUP BY clause. The records with the same fields are put in the same group. Similarly, the fields in the GROUP BY clause must be in the fields in **attr_expr_list**. The **attr_expr_list** field can also use the aggregate function.

Precautions
-----------

The to-be-grouped table must exist. Otherwise, an error is reported.

Example
-------

Group the **student** table according to the score and name fields and return the grouping results.

::

   SELECT score, count(name) FROM student
     GROUP BY score,name;
