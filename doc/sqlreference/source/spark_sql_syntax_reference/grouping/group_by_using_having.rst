:original_name: dli_08_0162.html

.. _dli_08_0162:

GROUP BY Using HAVING
=====================

Function
--------

This statement filters a table after grouping it using the HAVING clause.

Syntax
------

::

   SELECT attr_expr_list FROM table_reference
     GROUP BY groupby_expression [, groupby_expression...]
     HAVING having_expression;

Keyword
-------

The groupby_expression can contain a single field or multiple fields, and can also call aggregate functions or string functions.

Precautions
-----------

-  The to-be-grouped table must exist. Otherwise, an error is reported.
-  If the filtering condition is subject to the query results of GROUP BY, the HAVING clause, rather than the WHERE clause, must be used for filtering. If HAVING and GROUP BY are used together, GROUP BY applies first for grouping and HAVING then applies for filtering. The arithmetic operation and aggregate function are supported by the HAVING clause.

Example
-------

Group the **transactions** according to **num**, use the HAVING clause to filter the records in which the maximum value derived from multiplying **price** with **amount** is higher than 5000, and return the filtered results.

::

   SELECT num, max(price*amount) FROM transactions
     WHERE time > '2016-06-01'
     GROUP BY num
     HAVING max(price*amount)>5000;
