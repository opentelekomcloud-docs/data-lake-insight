:original_name: dli_08_0184.html

.. _dli_08_0184:

INTERSECT
=========

Function
--------

This statement is used to return the intersection set of multiple query results.

Syntax
------

::

   select_statement INTERSECT select_statement;

Keyword
-------

INTERSECT returns the intersection of multiple query results. The number of columns returned by each SELECT statement must be the same. The column type and column name may not be the same. By default, INTERSECT deduplication is used.

Precautions
-----------

Do not add brackets between multiple set operations, such as UNION, INTERSECT, and EXCEPT. Otherwise, an error is reported.

Example
-------

To return the intersection set of the query results of the **SELECT \* FROM student \_1** and **SELECT \* FROM student \_2** commands with the repeated records removed, run the following statement:

::

   SELECT * FROM student _1 INTERSECT SELECT * FROM student _2;
