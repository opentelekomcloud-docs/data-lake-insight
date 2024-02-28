:original_name: dli_spark_cos.html

.. _dli_spark_cos:

cos
===

This function is used to calculate the cosine value of **a**, with input in radians.

Syntax
------

.. code-block::

   cos(DOUBLE a)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------------+-----------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type                               | Description                                                                                                       |
   +=================+=================+====================================+===================================================================================================================+
   | a               | Yes             | DOUBLE, BIGINT, DECIMAL, or STRING | The value can be a float, integer, or string.                                                                     |
   |                 |                 |                                    |                                                                                                                   |
   |                 |                 |                                    | If the value is not of the DOUBLE type, the system will implicitly convert it to the DOUBLE type for calculation. |
   +-----------------+-----------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the DOUBLE type.

.. note::

   If the value of **a** is **NULL**, **NULL** is returned.

Example Code
------------

The value **-0.99999999999999999986** is returned.

.. code-block::

   select cos(3.1415926);

The value **NULL** is returned.

.. code-block::

   select cos(null);
