:original_name: dli_spark_log.html

.. _dli_spark_log:

log
===

This function is used to return the natural logarithm of a given base and exponent.

Syntax
------

.. code-block::

   log(DOUBLE base, DOUBLE a)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type                               | Description                                                                                                       |
   +=================+=================+====================================+===================================================================================================================+
   | base            | Yes             | DOUBLE, BIGINT, DECIMAL, or STRING | The value can be a float, integer, or string.                                                                     |
   |                 |                 |                                    |                                                                                                                   |
   |                 |                 |                                    | If the value is not of the DOUBLE type, the system will implicitly convert it to the DOUBLE type for calculation. |
   +-----------------+-----------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------+
   | a               | Yes             | DOUBLE, BIGINT, DECIMAL, or STRING | The value can be a float, integer, or string.                                                                     |
   |                 |                 |                                    |                                                                                                                   |
   |                 |                 |                                    | If the value is not of the DOUBLE type, the system will implicitly convert it to the DOUBLE type for calculation. |
   +-----------------+-----------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the DOUBLE type.

.. note::

   -  If the value of **base** or **a** is **NULL**, **NULL** is returned.
   -  If the value of **base** or **a** is negative or **0**, **NULL** is returned.
   -  If the value of **base** is **1** (would cause a division by zero), **NULL** is returned.

Example Code
------------

The value **2** is returned.

.. code-block::

   select log(2, 4);

The value **NULL** is returned.

.. code-block::

   select log(2, null);

The value **NULL** is returned.

.. code-block::

   select log(null, 4);
