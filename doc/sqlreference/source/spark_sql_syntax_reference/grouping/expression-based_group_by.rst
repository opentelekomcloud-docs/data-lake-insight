:original_name: dli_08_0161.html

.. _dli_08_0161:

Expression-Based GROUP BY
=========================

Function
--------

This statement is used to group a table according to expressions.

Syntax
------

::

   SELECT attr_expr_list FROM table_reference
     GROUP BY groupby_expression [, groupby_expression, ...];

Keywords
--------

The **groupby_expression** can contain a single field or multiple fields, and also can call aggregate functions or string functions.

Precautions
-----------

-  The to-be-grouped table must exist. Otherwise, an error is reported.
-  In the same single-column group, built-in functions and self-defined functions are supported in the expression in the GRUOP BY fields that must exit in **attr_expr_list**.

Example
-------

To use the **substr** function to obtain the string from the **name** field, group the student table according to the obtained string, and return each sub string and the number of records, run the following statement:

::

   SELECT substr(name,6),count(name) FROM student
     GROUP BY substr(name,6);
