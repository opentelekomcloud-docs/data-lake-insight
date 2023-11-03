:original_name: dli_08_0175.html

.. _dli_08_0175:

Subquery Nested by WHERE
========================

Function
--------

Subqueries are nested in the WHERE clause, and the subquery result is used as the filtering condition.

Syntax
------

::

   SELECT [ALL | DISTINCT] attr_expr_list FROM table_reference
     WHERE {col_name operator (sub_query) | [NOT] EXISTS sub_query};

Keyword
-------

-  All is used to return repeated rows. By default, all repeated rows are returned. It is followed by asterisks (*) only. Otherwise, an error will occur.
-  DISTINCT is used to remove the repeated line from the result.
-  The subquery results are used as the filter condition in the subquery nested by WHERE.
-  The operator includes the equation and inequation operators, and IN, NOT IN, EXISTS, and NOT EXISTS operators.

   -  If the operator is IN or NOT IN, the returned records are in a single column.
   -  If the operator is EXISTS or NOT EXISTS, the subquery must contain WHERE. If any a field in the subquery is the same as that in the external query, add the table name before the field in the subquery.

Precautions
-----------

The to-be-queried table must exist. If this statement is used to query a table that does not exist, an error is reported.

Example
-------

To query the courseId of Biology from the course_info table, and then query the student name matched the courseId from the student_info table, run the following statement:

::

   SELECT name FROM student_info
     WHERE courseId = (SELECT courseId FROM course_info WHERE courseName = 'Biology');
