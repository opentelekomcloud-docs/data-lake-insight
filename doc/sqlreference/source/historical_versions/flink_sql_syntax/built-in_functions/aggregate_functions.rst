:original_name: dli_08_0104.html

.. _dli_08_0104:

Aggregate Functions
===================

An aggregate function performs a calculation operation on a set of input values and returns a value. For example, the COUNT function counts the number of rows retrieved by an SQL statement. :ref:`Table 1 <dli_08_0104__tb90f8c1797b346ca8ac5fcb1eb5d85df>` lists aggregate functions.

Sample data: Table T1

.. code-block::

   |score|
   |81   |
   |100  |
   |60   |
   |95   |
   |86   |

Common Aggregate Functions
--------------------------

.. _dli_08_0104__tb90f8c1797b346ca8ac5fcb1eb5d85df:

.. table:: **Table 1** Common aggregation functions

   +------------------------------------------------------------------+------------------+--------------------------------------------------------------------------------------------------------------------------+
   | Function                                                         | Return Data Type | Description                                                                                                              |
   +==================================================================+==================+==========================================================================================================================+
   | :ref:`COUNT(*) <dli_08_0104__li164511853108>`                    | BIGINT           | Return count of tuples.                                                                                                  |
   +------------------------------------------------------------------+------------------+--------------------------------------------------------------------------------------------------------------------------+
   | :ref:`COUNT([ ALL ] expression... <dli_08_0104__li846217557716>` | BIGINT           | Returns the number of input rows for which the expression is not NULL. Use DISTINCT for a unique instance of each value. |
   +------------------------------------------------------------------+------------------+--------------------------------------------------------------------------------------------------------------------------+
   | :ref:`AVG(numeric) <dli_08_0104__li179515614471>`                | DOUBLE           | Return average (arithmetic mean) of all input values.                                                                    |
   +------------------------------------------------------------------+------------------+--------------------------------------------------------------------------------------------------------------------------+
   | :ref:`SUM(numeric) <dli_08_0104__li15283143616321>`              | DOUBLE           | Return the sum of all input numerical values.                                                                            |
   +------------------------------------------------------------------+------------------+--------------------------------------------------------------------------------------------------------------------------+
   | :ref:`MAX(value) <dli_08_0104__li16157133003419>`                | DOUBLE           | Return the maximum value of all input values.                                                                            |
   +------------------------------------------------------------------+------------------+--------------------------------------------------------------------------------------------------------------------------+
   | :ref:`MIN(value) <dli_08_0104__li35471713227>`                   | DOUBLE           | Return the minimum value of all input values.                                                                            |
   +------------------------------------------------------------------+------------------+--------------------------------------------------------------------------------------------------------------------------+
   | :ref:`STDDEV_POP(value) <dli_08_0104__li102928394313>`           | DOUBLE           | Return the population standard deviation of all numeric fields of all input values.                                      |
   +------------------------------------------------------------------+------------------+--------------------------------------------------------------------------------------------------------------------------+
   | :ref:`STDDEV_SAMP(value) <dli_08_0104__li129316477810>`          | DOUBLE           | Return the sample standard deviation of all numeric fields of all input values.                                          |
   +------------------------------------------------------------------+------------------+--------------------------------------------------------------------------------------------------------------------------+
   | :ref:`VAR_POP(value) <dli_08_0104__li18741144581010>`            | DOUBLE           | Return the population variance (square of population standard deviation) of numeral fields of all input values.          |
   +------------------------------------------------------------------+------------------+--------------------------------------------------------------------------------------------------------------------------+
   | :ref:`VAR_SAMP(value) <dli_08_0104__li1133489201213>`            | DOUBLE           | Return the sample variance (square of the sample standard deviation) of numeric fields of all input values.              |
   +------------------------------------------------------------------+------------------+--------------------------------------------------------------------------------------------------------------------------+

Example
-------

-  .. _dli_08_0104__li164511853108:

   COUNT(*)

   -  Test statement

      .. code-block::

         SELECT COUNT(score) FROM T1;

   -  Test data and results

      .. table:: **Table 2** T1

         ================= ===========
         Test Data (score) Test Result
         ================= ===========
         81                5
         100
         60
         95
         86
         ================= ===========

-  .. _dli_08_0104__li846217557716:

   COUNT([ ALL ] expression \| DISTINCT expression1 [, expression2]*)

   -  Test statement

      .. code-block::

         SELECT COUNT(DISTINCT content ) FROM T1;

   -  Test data and results

      .. table:: **Table 3** T1

         ================ ===========
         content (STRING) Test Result
         ================ ===========
         "hello1 "        2
         "hello2 "
         "hello2"
         null
         86
         ================ ===========

-  .. _dli_08_0104__li179515614471:

   AVG(numeric)

   -  Test statement

      .. code-block::

         SELECT AVG(score) FROM T1;

   -  Test data and results

      .. table:: **Table 4** T1

         ================= ===========
         Test Data (score) Test Result
         ================= ===========
         81                84.0
         100
         60
         95
         86
         ================= ===========

-  .. _dli_08_0104__li15283143616321:

   SUM(numeric)

   -  Test statement

      .. code-block::

         SELECT SUM(score) FROM T1;

   -  Test data and results

      .. table:: **Table 5** T1

         ================= ===========
         Test Data (score) Test Result
         ================= ===========
         81                422.0
         100
         60
         95
         86
         ================= ===========

-  .. _dli_08_0104__li16157133003419:

   MAX(value)

   -  Test statement

      .. code-block::

         SELECT MAX(score) FROM T1;

   -  Test data and results

      .. table:: **Table 6** T1

         ================= ===========
         Test Data (score) Test Result
         ================= ===========
         81                100.0
         100
         60
         95
         86
         ================= ===========

-  .. _dli_08_0104__li35471713227:

   MIN(value)

   -  Test statement

      .. code-block::

         SELECT MIN(score) FROM T1;

   -  Test data and results

      .. table:: **Table 7** T1

         ================= ===========
         Test Data (score) Test Result
         ================= ===========
         81                60.0
         100
         60
         95
         86
         ================= ===========

-  .. _dli_08_0104__li102928394313:

   STDDEV_POP(value)

   -  Test statement

      .. code-block::

         SELECT STDDEV_POP(score) FROM T1;

   -  Test data and results

      .. table:: **Table 8** T1

         ================= ===========
         Test Data (score) Test Result
         ================= ===========
         81                13.0
         100
         60
         95
         86
         ================= ===========

-  .. _dli_08_0104__li129316477810:

   STDDEV_SAMP(value)

   -  Test statement

      .. code-block::

         SELECT STDDEV_SAMP(score) FROM T1;

   -  Test data and results

      .. table:: **Table 9** T1

         ================= ===========
         Test Data (score) Test Result
         ================= ===========
         81                15.0
         100
         60
         95
         86
         ================= ===========

-  .. _dli_08_0104__li18741144581010:

   VAR_POP(value)

   -  Test statement

      .. code-block::

         SELECT VAR_POP(score) FROM T1;

   -  Test data and results

      .. table:: **Table 10** T1

         ================= ===========
         Test Data (score) Test Result
         ================= ===========
         81                193.0
         100
         60
         95
         86
         ================= ===========

-  .. _dli_08_0104__li1133489201213:

   VAR_SAMP(value)

   -  Test statement

      .. code-block::

         SELECT VAR_SAMP(score) FROM T1;

   -  Test data and results

      .. table:: **Table 11** T1

         ================= ===========
         Test Data (score) Test Result
         ================= ===========
         81                241.0
         100
         60
         95
         86
         ================= ===========
