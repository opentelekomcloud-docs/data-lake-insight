:original_name: dli_08_0436.html

.. _dli_08_0436:

Aggregate Functions
===================

An aggregate function performs a calculation operation on a set of input values and returns a value. For example, the COUNT function counts the number of rows retrieved by an SQL statement. :ref:`Table 1 <dli_08_0436__en-us_topic_0000001310215809_dli_08_0104_tb90f8c1797b346ca8ac5fcb1eb5d85df>` lists aggregate functions.

.. _dli_08_0436__en-us_topic_0000001310215809_dli_08_0104_tb90f8c1797b346ca8ac5fcb1eb5d85df:

.. table:: **Table 1** Aggregate functions

   +--------------------------------------------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------+
   | Function                                                           | Return Type           | Description                                                                                                                |
   +====================================================================+=======================+============================================================================================================================+
   | COUNT([ ALL ] expression \| DISTINCT expression1 [, expression2]*) | BIGINT                | Returns the number of input rows for which the expression is not NULL. Use DISTINCT for one unique instance of each value. |
   +--------------------------------------------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------+
   | COUNT(``*``)                                                       | BIGINT                | Returns the number of input rows.                                                                                          |
   |                                                                    |                       |                                                                                                                            |
   | COUNT(1)                                                           |                       |                                                                                                                            |
   +--------------------------------------------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------+
   | AVG([ ALL \| DISTINCT ] expression)                                | DOUBLE                | Returns the average (arithmetic mean) of expression across all input rows.                                                 |
   |                                                                    |                       |                                                                                                                            |
   |                                                                    |                       | Use DISTINCT for one unique instance of each value.                                                                        |
   +--------------------------------------------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------+
   | SUM([ ALL \| DISTINCT ] expression)                                | DOUBLE                | Returns the sum of expression across all input rows.                                                                       |
   |                                                                    |                       |                                                                                                                            |
   |                                                                    |                       | Use DISTINCT for one unique instance of each value.                                                                        |
   +--------------------------------------------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------+
   | MAX([ ALL \| DISTINCT ] expression)                                | DOUBLE                | Returns the maximum value of expression across all input rows.                                                             |
   +--------------------------------------------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------+
   | MIN([ ALL \| DISTINCT ] expression)                                | DOUBLE                | Returns the minimum value of expression across all input rows.                                                             |
   +--------------------------------------------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------+
   | STDDEV_POP([ ALL \| DISTINCT ] expression)                         | DOUBLE                | Returns the population standard deviation of expression across all input rows.                                             |
   +--------------------------------------------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------+
   | STDDEV_SAMP([ ALL \| DISTINCT ] expression)                        | DOUBLE                | Returns the sample standard deviation of expression across all input rows.                                                 |
   +--------------------------------------------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------+
   | VAR_POP([ ALL \| DISTINCT ] expression)                            | DOUBLE                | Returns the population variance (square of the population standard deviation) of expression across all input rows.         |
   +--------------------------------------------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------+
   | VAR_SAMP([ ALL \| DISTINCT ] expression)                           | DOUBLE                | Returns the sample variance (square of the sample standard deviation) of expression across all input rows.                 |
   +--------------------------------------------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------+
   | COLLECT([ ALL \| DISTINCT ] expression)                            | MULTISET              | Returns a multiset of expression across all input rows.                                                                    |
   +--------------------------------------------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------+
   | VARIANCE([ ALL \| DISTINCT ] expression)                           | DOUBLE                | Returns the sample variance (square of the sample standard deviation) of expression across all input rows.                 |
   +--------------------------------------------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------+
   | FIRST_VALUE(expression)                                            | Actual type           | Returns the first value in an ordered set of values.                                                                       |
   +--------------------------------------------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------+
   | LAST_VALUE(expression)                                             | Actual type           | Returns the last value in an ordered set of values.                                                                        |
   +--------------------------------------------------------------------+-----------------------+----------------------------------------------------------------------------------------------------------------------------+
