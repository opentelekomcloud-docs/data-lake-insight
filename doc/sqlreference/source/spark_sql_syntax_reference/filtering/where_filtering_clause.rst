:original_name: dli_08_0152.html

.. _dli_08_0152:

WHERE Filtering Clause
======================

Function
--------

This statement is used to filter the query results using the WHERE clause.

Syntax
------

::

   SELECT [ALL | DISTINCT] attr_expr_list FROM table_reference
     WHERE where_condition;

Keyword
-------

-  All is used to return repeated rows. By default, all repeated rows are returned. It is followed by asterisks (*) only. Otherwise, an error will occur.
-  DISTINCT is used to remove the repeated line from the result.
-  WHERE is used to filter out records that do not meet the condition and return records that meet the condition.

Precautions
-----------

The to-be-queried table must exist.

Example
-------

To filter the records in which the scores are higher than 90 and lower than 95 in the **student** table, run the following statement:

::

   SELECT * FROM student
     WHERE score > 90 AND score < 95;
