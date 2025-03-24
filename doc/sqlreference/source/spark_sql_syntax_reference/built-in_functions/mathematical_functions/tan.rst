:original_name: dli_spark_tan.html

.. _dli_spark_tan:

tan
===

This function is used to return the tangent value of **a**, with input in radians.

Syntax
------

.. code-block::

   tan(DOUBLE a)

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

The value **0.9999999999999999** is returned.

.. code-block::

   select tan(pi()/4);

The value **NULL** is returned.

.. code-block::

   select tan(null);
