:original_name: dli_spark_shiftleft.html

.. _dli_spark_shiftleft:

shiftleft
=========

This function is used to perform a signed bitwise left shift. It takes the binary number **a** and shifts it **b** positions to the left.

Syntax
------

shiftleft(BIGINT a, BIGINT b)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type                               | Description                                                                                                       |
   +=================+=================+====================================+===================================================================================================================+
   | a               | Yes             | DOUBLE, BIGINT, DECIMAL, or STRING | The value can be a float, integer, or string.                                                                     |
   |                 |                 |                                    |                                                                                                                   |
   |                 |                 |                                    | If the value is not of the BIGINT type, the system will implicitly convert it to the BIGINT type for calculation. |
   +-----------------+-----------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------+
   | b               | Yes             | DOUBLE, BIGINT, DECIMAL, or STRING | The value can be a float, integer, or string.                                                                     |
   |                 |                 |                                    |                                                                                                                   |
   |                 |                 |                                    | If the value is not of the BIGINT type, the system will implicitly convert it to the BIGINT type for calculation. |
   +-----------------+-----------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the INT type.

.. note::

   If the value of **a** or **b** is **NULL**, **NULL** is returned.

Example Code
------------

The value **8** is returned.

.. code-block::

   select shiftleft(1,3);

The value **48** is returned.

.. code-block::

   select shiftleft(6,3);

The value **48** is returned.

.. code-block::

   select shiftleft(6.123456,3.123456);

The value **NULL** is returned.

.. code-block::

   select shiftleft(null,3);
