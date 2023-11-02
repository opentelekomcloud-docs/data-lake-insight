:original_name: dli_08_0166.html

.. _dli_08_0166:

INNER JOIN
==========

Function
--------

This statement is used to join and return the rows that meet the JOIN conditions from two tables as the result set.

Syntax
------

::

   SELECT attr_expr_list FROM table_reference
     {JOIN | INNER JOIN} table_reference ON join_condition;

Keyword
-------

JOIN/INNER JOIN: Only the records that meet the JOIN conditions in joined tables will be displayed.

Precautions
-----------

-  The to-be-joined table must exist. Otherwise, an error is reported.
-  INNER JOIN can join more than two tables at one query.

Example
-------

To join the course IDs from the **student_info** and **course_info** tables and check the mapping between student names and courses, run the following statement:

::

   SELECT student_info.name, course_info.courseName FROM student_info
     JOIN course_info ON (student_info.courseId = course_info.courseId);
