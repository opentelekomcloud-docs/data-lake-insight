:original_name: dli_08_0173.html

.. _dli_08_0173:

NON-EQUIJOIN
============

Function
--------

This statement is used to join multiple tables using unequal values and return the result set that meet the condition.

Syntax
------

::

   SELECT attr_expr_list FROM table_reference
     JOIN table reference ON non_equi_join_condition;

Keyword
-------

The **non_equi_join_condition** is similar to **join_condition**. The only difference is that the JOIN condition is inequation.

Precautions
-----------

The to-be-joined table must exist. Otherwise, an error is reported.

Example
-------

To return all the pairs of different student names from the **student_info_1** and **student_info_2** tables, run the following statement:

::

   SELECT student_info_1.name, student_info_2.name FROM student_info_1
     JOIN student_info_2 ON (student_info_1. name <> student_info_2. name);
