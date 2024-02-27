:original_name: dli_spark_shiftright.html

.. _dli_spark_shiftright:

shiftright
==========

This function is used to perform a signed bitwise right shift. It takes the binary number **a** and shifts it **b** positions to the right.

Syntax
------

.. code-block::

   shiftright(BIGINT a, BIGINT b)

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

The value **2** is returned.

.. code-block::

   select shiftright(16,3);

The value **4** is returned.

.. code-block::

   select shiftright(36,3);

The value **4** is returned.

.. code-block::

   select shiftright(36.123456,3.123456);

The value **NULL** is returned.

.. code-block::

   select shiftright(null,3);
