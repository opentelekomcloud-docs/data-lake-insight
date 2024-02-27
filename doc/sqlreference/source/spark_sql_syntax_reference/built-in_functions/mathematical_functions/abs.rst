:original_name: dli_spark_abs.html

.. _dli_spark_abs:

abs
===

This function is used to calculate the absolute value of an input parameter.

Syntax
------

.. code-block::

   abs(DOUBLE a)

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

The return value is of the DOUBLE or INT type.

.. note::

   If the value of **a** is **NULL**, **NULL** is returned.

Example Code
------------

The value **NULL** is returned.

.. code-block::

   select abs(null);

The value **1** is returned.

.. code-block::

   select abs(-1);

The value **3.1415926** is returned.

.. code-block::

   select abs(-3.1415926);
