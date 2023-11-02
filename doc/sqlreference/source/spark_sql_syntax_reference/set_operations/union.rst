:original_name: dli_08_0183.html

.. _dli_08_0183:

UNION
=====

Function
--------

This statement is used to return the union set of multiple query results.

Syntax
------

::

   select_statement UNION [ALL] select_statement;

Keyword
-------

UNION: The set operation is used to join the head and tail of a table based on certain conditions. The number of columns returned by each SELECT statement must be the same. The column type and column name may not be the same.

Precautions
-----------

-  By default, the repeated records returned by UNION are removed. The repeated records returned by UNION ALL are not removed.
-  Do not add brackets between multiple set operations, such as UNION, INTERSECT, and EXCEPT. Otherwise, an error is reported.

Example
-------

To return the union set of the query results of the **SELECT \* FROM student \_1** and **SELECT \* FROM student \_2** commands with the repeated records removed, run the following statement:

::

   SELECT * FROM student_1 UNION SELECT * FROM student_2;
