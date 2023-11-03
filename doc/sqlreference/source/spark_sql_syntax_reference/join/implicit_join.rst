:original_name: dli_08_0170.html

.. _dli_08_0170:

IMPLICIT JOIN
=============

Function
--------

This statement has the same function as INNER JOIN, that is, the result set that meet the WHERE condition is returned. However, IMPLICIT JOIN does not use the condition specified by JOIN.

.. _dli_08_0170__en-us_topic_0093946955_s87ab3fc6445b48e79412f8030e9d6d36:

Syntax
------

::

   SELECT table_reference.col_name, table_reference.col_name, ... FROM table_reference, table_reference
     WHERE table_reference.col_name = table_reference.col_name;

Keyword
-------

The keyword WHERE achieves the same function as JOIN...ON... and the mapped records will be returned. :ref:`Syntax <dli_08_0170__en-us_topic_0093946955_s87ab3fc6445b48e79412f8030e9d6d36>` shows the WHERE filtering according to an equation. The WHERE filtering according to an inequation is also supported.

Precautions
-----------

-  The to-be-joined table must exist. Otherwise, an error is reported.
-  The statement of IMPLICIT JOIN does not contain keywords JOIN...ON.... Instead, the WHERE clause is used as the condition to join two tables.

Example
-------

To return the student names and course names that match **courseId**, run the following statement:

::

   SELECT student_info.name, course_info.courseName FROM student_info,course_info
     WHERE student_info.courseId = course_info.courseId;
