:original_name: dli_08_0169.html

.. _dli_08_0169:

FULL OUTER JOIN
===============

Function
--------

Join all records from the right table and the left table and return all joined records. If no joined record is found, NULL will be returned.

Syntax
------

::

   SELECT attr_expr_list FROM table_reference
     FULL OUTER JOIN table_reference ON join_condition;

Keyword
-------

FULL OUTER JOIN: Matches all records in the left and right tables. If no record is matched, NULL is returned.

Precautions
-----------

The to-be-joined table must exist. Otherwise, an error is reported.

Example
-------

To join all records from the right table and the left table and return all joined records, run the following statement. If no joined record is found, NULL will be returned.

::

   SELECT student_info.name, course_info.courseName FROM student_info
     FULL OUTER JOIN course_info ON (student_info.courseId = course_info.courseId);
