:original_name: dli_08_0188.html

.. _dli_08_0188:

Basic CASE Statement
====================

Function
--------

This statement is used to display **result_expression** according to the joined results of **input_expression** and **when_expression**.

Syntax
------

::

   CASE input_expression WHEN when_expression THEN result_expression [...n] [ELSE else_result_expression] END;

Keyword
-------

CASE: Subquery is supported in basic CASE statement. However, input_expression and when_expression must be joinable.

Precautions
-----------

If there is no input_expression = when_expression with the TRUE value, else_result_expression will be returned when the ELSE clause is specified. If the ELSE clause is not specified, NULL will be returned.

Example
-------

To return the name field and the character that is matched to id from the student table with the following matching rules, run the following statement:

-  If id is 1, 'a' is returned.
-  If id is 2, 'b' is returned.
-  If id is 3, 'c' is returned.
-  Otherwise, **NULL** is returned.

::

   SELECT name, CASE id WHEN 1 THEN 'a' WHEN 2 THEN 'b' WHEN 3 THEN 'c' ELSE NULL END FROM student;
