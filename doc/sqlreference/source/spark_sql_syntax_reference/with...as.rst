:original_name: dli_08_0186.html

.. _dli_08_0186:

WITH...AS
=========

Function
--------

This statement is used to define the common table expression (CTE) using WITH...AS to simplify the query and make the result easier to read and maintain.

Syntax
------

::

   WITH cte_name AS (select_statement) sql_containing_cte_name;

Keyword
-------

-  cte_name: Name of a public expression. The name must be unique.
-  select_statement: complete SELECT clause.
-  sql_containing_cte_name: SQL statement containing the defined common expression.

Precautions
-----------

-  A CTE must be used immediately after it is defined. Otherwise, the definition becomes invalid.
-  Multiple CTEs can be defined by WITH at a time. The CTEs are separated by commas and the CTEs defined later can quote the CTEs defined earlier.

Example
-------

Define **SELECT courseId FROM course_info WHERE courseName = 'Biology'** as CTE **nv** and use **nv** as the SELECT statement in future queries.

::

   WITH nv AS (SELECT courseId FROM course_info WHERE courseName = 'Biology') SELECT DISTINCT courseId FROM nv;
