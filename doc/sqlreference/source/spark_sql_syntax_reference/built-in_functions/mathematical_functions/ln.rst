:original_name: dli_spark_ln.html

.. _dli_spark_ln:

ln
==

This function is used to return the natural logarithm of a given value.

Syntax
------

.. code-block::

   ln(DOUBLE a)

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

   -  If the value of **a** is negative or **0**, **NULL** is returned.
   -  If the value of **a** is **NULL**, **NULL** is returned.

Example Code
------------

The value **1.144729868791239** is returned.

.. code-block::

   select ln(3.1415926);

The value **1** is returned.

.. code-block::

   select ln(2.718281828459045);

The value **NULL** is returned.

.. code-block::

   select ln(null);
