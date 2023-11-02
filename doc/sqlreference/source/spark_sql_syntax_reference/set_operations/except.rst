:original_name: dli_08_0185.html

.. _dli_08_0185:

EXCEPT
======

Function
--------

This statement is used to return the difference set of two query results.

Syntax
------

::

   select_statement EXCEPT select_statement;

Keyword
-------

EXCEPT minus the sets. A EXCEPT B indicates to remove the records that exist in both A and B from A and return the results. The repeated records returned by EXCEPT are not removed by default. The number of columns returned by each SELECT statement must be the same. The types and names of columns do not have to be the same.

Precautions
-----------

Do not add brackets between multiple set operations, such as UNION, INTERSECT, and EXCEPT. Otherwise, an error is reported.

Example
-------

To remove the records that exist in both **SELECT \* FROM student_1** and **SELECT \* FROM student_2** from **SELECT \* FROM student_1** and return the results, run the following statement:

::

   SELECT * FROM student_1 EXCEPT SELECT * FROM student_2;
