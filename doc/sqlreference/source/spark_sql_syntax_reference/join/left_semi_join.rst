:original_name: dli_08_0172.html

.. _dli_08_0172:

LEFT SEMI JOIN
==============

Function
--------

This statement is used to query the records that meet the JOIN condition from the left table.

Syntax
------

::

   SELECT attr_expr_list FROM table_reference
     LEFT SEMI JOIN table_reference ON join_condition;

Keyword
-------

LEFT SEMI JOIN: Indicates to only return the records from the left table. LEFT SEMI JOIN can be achieved by nesting subqueries in LEFT SEMI JOIN, WHERE...IN, or WHERE EXISTS. LEFT SEMI JOIN returns the records that meet the JOIN condition from the left table, while LEFT OUTER JOIN returns all the records from the left table or NULL if no records that meet the JOIN condition are found.

Precautions
-----------

-  The to-be-joined table must exist. Otherwise, an error is reported.
-  he fields in attr_expr_list must be the fields in the left table. Otherwise, an error is reported.

Example
-------

To return the names of students who select the courses and the course IDs, run the following statement:

::

   SELECT student_info.name, student_info.courseId FROM student_info
     LEFT SEMI JOIN course_info ON (student_info.courseId = course_info.courseId);
