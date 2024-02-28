:original_name: dli_spark_cot1.html

.. _dli_spark_cot1:

cot1
====

This function is used to calculate the cotangent value of **a**, with input in radians.

Syntax
------

.. code-block::

   cot1(DOUBLE a)

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

The return value is of the DOUBLE or DECIMAL type.

.. note::

   If the value of **a** is **NULL**, **NULL** is returned.

Example Code
------------

The value **1.0000000000000002** is returned.

.. code-block::

   select cot1(pi()/4);

The value **NULL** is returned.

.. code-block::

   select cot1(null);
