:original_name: dli_08_0168.html

.. _dli_08_0168:

RIGHT OUTER JOIN
================

Function
--------

Match the right table with the left table and return all matched records of the right table. If no matched record is found, NULL will be returned.

Syntax
------

::

   SELECT attr_expr_list FROM table_reference
     RIGHT OUTER JOIN table_reference ON join_condition;

Keyword
-------

RIGHT OUTER JOIN: Return all matched records of the right table. If no record is matched, NULL is returned.

Precautions
-----------

The to-be-joined table must exist. Otherwise, an error is reported.

Example
-------

To join the courseId from the **course_info** table to the **courseId** from the **student_info** table for inner join and return the records in the **course_info** table, run the following statement. If no joined record is found, NULL will be returned.

::

   SELECT student_info.name, course_info.courseName FROM student_info
     RIGHT OUTER JOIN course_info ON (student_info.courseId = course_info.courseId);
