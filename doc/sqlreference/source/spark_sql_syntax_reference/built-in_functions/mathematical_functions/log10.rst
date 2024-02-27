:original_name: dli_spark_log10.html

.. _dli_spark_log10:

log10
=====

This function is used to return the natural logarithm of a given value with a base of 10.

Syntax
------

.. code-block::

   log10(DOUBLE a)

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

   If the value of **a** is negative, **0**, or **NULL**, **NULL** is returned.

Example Code
------------

The value **NULL** is returned.

.. code-block::

   select log10(null);

The value **NULL** is returned.

.. code-block::

   select log10(0);

The value **0.9542425094393249** is returned.

.. code-block::

   select log10(9);

The value **1** is returned.

.. code-block::

   select log10(10);
