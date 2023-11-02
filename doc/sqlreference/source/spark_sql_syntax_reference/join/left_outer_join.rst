:original_name: dli_08_0167.html

.. _dli_08_0167:

LEFT OUTER JOIN
===============

Function
--------

Join the left table with the right table and return all joined records of the left table. If no joined record is found, NULL will be returned.

Syntax
------

::

   SELECT attr_expr_list FROM table_reference
     LEFT OUTER JOIN table_reference ON join_condition;

Keyword
-------

LEFT OUTER JOIN: Returns all joined records of the left table. If no record is matched, NULL is returned.

Precautions
-----------

The to-be-joined table must exist. Otherwise, an error is reported.

Example
-------

To join the courseId from the **student_info** table to the **courseId** from the **course_info** table for inner join and return the name of the students who have selected course, run the following statement. If no joined record is found, NULL will be returned.

::

   SELECT student_info.name, course_info.courseName FROM student_info
     LEFT OUTER JOIN course_info ON (student_info.courseId = course_info.courseId);
