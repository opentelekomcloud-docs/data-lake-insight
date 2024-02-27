:original_name: dli_spark_pow.html

.. _dli_spark_pow:

pow
===

This function is used to calculate and return the pth power of **a**.

Syntax
------

.. code-block::

   pow(DOUBLE a, DOUBLE p),
   power(DOUBLE a, DOUBLE p)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type                               | Description                                                                                                       |
   +=================+=================+====================================+===================================================================================================================+
   | a               | Yes             | DOUBLE, BIGINT, DECIMAL, or STRING | The value can be a float, integer, or string.                                                                     |
   |                 |                 |                                    |                                                                                                                   |
   |                 |                 |                                    | If the value is not of the DOUBLE type, the system will implicitly convert it to the DOUBLE type for calculation. |
   +-----------------+-----------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------+
   | p               | Yes             | DOUBLE, BIGINT, DECIMAL, or STRING | The value can be a float, integer, or string.                                                                     |
   |                 |                 |                                    |                                                                                                                   |
   |                 |                 |                                    | If the value is not of the DOUBLE type, the system will implicitly convert it to the DOUBLE type for calculation. |
   +-----------------+-----------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the DOUBLE type.

.. note::

   If the value of **a** or **p** is **NULL**, **NULL** is returned.

Example Code
------------

The value **16** returned.

.. code-block::

   select pow(2, 4);

The value **NULL** is returned.

.. code-block::

   select pow(2, null);

The value **17.429460393524256** is returned.

.. code-block::

   select pow(2, 4.123456);
