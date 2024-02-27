:original_name: dli_spark_median.html

.. _dli_spark_median:

median
======

This function is used to calculate the median of input parameters.

Syntax
------

.. code-block::

   median(colname)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------------+-----------------+------------------------------------+--------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type                               | Description                                                                                                                    |
   +=================+=================+====================================+================================================================================================================================+
   | colname         | Yes             | DOUBLE, DECIMAL, STRING, or BIGINT | Name of the column to be sorted                                                                                                |
   |                 |                 |                                    |                                                                                                                                |
   |                 |                 |                                    | The elements in a column are of the DOUBLE type.                                                                               |
   |                 |                 |                                    |                                                                                                                                |
   |                 |                 |                                    | If an element in a column is not of the DOUBLE type, the system will implicitly convert it to the DOUBLE type for calculation. |
   +-----------------+-----------------+------------------------------------+--------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the DOUBLE or DECIMAL type.

.. note::

   If the column name does not exist, an error is reported.

Example Code
------------

Assume that the elements in the **int_test** column are 1, 2, 3, and 4 and they are of the INT type.

The value **2.5** is returned.

.. code-block::

   select median(int_test) FROM int_test;
