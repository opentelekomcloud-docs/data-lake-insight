:original_name: dli_08_0189.html

.. _dli_08_0189:

CASE Query Statement
====================

Function
--------

This statement is used to obtain the value of **boolean_expression** for each WHEN statement in a specified order. Then return the first **result_expression** with the value **TRUE** of **boolean_expression**.

Syntax
------

::

   CASE WHEN boolean_expression THEN result_expression [...n] [ELSE else_result_expression] END;

Keyword
-------

boolean_expression: can include subquery. However, the return value of boolean_expression can only be of Boolean type.

Precautions
-----------

If there is no Boolean_expression with the TRUE value, else_result_expression will be returned when the ELSE clause is specified. If the ELSE clause is not specified, NULL will be returned.

Example
-------

To query the student table and return the related results for the name and score fields: EXCELLENT if the score is higher than 90, GOOD if the score ranges from 80 to 90, and BAD if the score is lower than 80, run the following statement:

::

   SELECT name, CASE WHEN score >= 90 THEN 'EXCELLENT' WHEN 80 < score AND score < 90 THEN 'GOOD' ELSE 'BAD' END AS level FROM student;
