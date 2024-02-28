:original_name: dli_spark_ceil.html

.. _dli_spark_ceil:

ceil
====

This function is used to round up **a** to the nearest integer.

Syntax
------

.. code-block::

   ceil(DOUBLE a)

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

The return value is of the DECIMAL type.

.. note::

   If the value of **a** is **NULL**, **NULL** is returned.

Example Code
------------

The value **2** is returned.

.. code-block::

   select ceil(1.3);

The value **-1** is returned.

.. code-block::

   select ceil(-1.3);

The value **NULL** is returned.

.. code-block::

   select ceil(null);
