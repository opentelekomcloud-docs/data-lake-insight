:original_name: dli_08_0177.html

.. _dli_08_0177:

Subquery Nested by HAVING
=========================

Function
--------

This statement is used to embed a subquery in the HAVING clause. The subquery result is used as a part of the HAVING clause.

Syntax
------

::

   SELECT [ALL | DISTINCT] attr_expr_list FROM table_reference
     GROUP BY groupby_expression
     HAVING aggregate_func(col_name) operator (sub_query);

Keyword
-------

-  All is used to return repeated rows. By default, all repeated rows are returned. It is followed by asterisks (*) only. Otherwise, an error will occur.
-  DISTINCT is used to remove the repeated line from the result.

-  The **groupby_expression** can contain a single field or multiple fields, and also can call aggregate functions or string functions.
-  The operator includes the equation and inequation operators, and IN and NOT IN operators.

Precautions
-----------

-  The to-be-queried table must exist. If this statement is used to query a table that does not exist, an error is reported.
-  The sequence of **sub_query** and the aggregate function cannot be changed.

Example
-------

To group the **student_info** table according to the name field, count the records of each group, and return the number of records in which the name fields in the **student_info** table equal to the name fields in the **course_info** table if the two tables have the same number of records, run the following statement:

::

   SELECT name FROM student_info
     GROUP BY name
     HAVING count(name) = (SELECT count(*) FROM course_info);
