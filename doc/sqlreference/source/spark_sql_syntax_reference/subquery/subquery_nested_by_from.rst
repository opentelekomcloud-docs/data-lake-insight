:original_name: dli_08_0176.html

.. _dli_08_0176:

Subquery Nested by FROM
=======================

Function
--------

This statement is used to nest subquery by FROM and use the subquery results as the data source of the external SELECT statement.

Syntax
------

::

   SELECT [ALL | DISTINCT] attr_expr_list FROM (sub_query) [alias];

Keyword
-------

-  All is used to return repeated rows. By default, all repeated rows are returned. It is followed by asterisks (*) only. Otherwise, an error will occur.
-  DISTINCT is used to remove the repeated line from the result.

Precautions
-----------

-  The to-be-queried table must exist. If this statement is used to query a table that does not exist, an error is reported.
-  The subquery nested in FROM must have an alias. The alias must be specified before the running of the statement. Otherwise, an error is reported. It is advised to specify a unique alias.
-  The subquery results sequent to FROM must be followed by the specified alias. Otherwise, an error is reported.

Example
-------

To return the names of students who select the courses in the **course_info** table and remove the repeated records using DISTINCT, run the following statement:

::

   SELECT DISTINCT name FROM (SELECT name FROM student_info
     JOIN course_info ON student_info.courseId = course_info.courseId) temp;
