:original_name: dli_spark_cbrt.html

.. _dli_spark_cbrt:

cbrt
====

This function is used to return the cube root of **a**.

Syntax
------

.. code-block::

   cbrt(DOUBLE a)

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

The value **3** is returned.

.. code-block::

   select cbrt(27);

The value **3.3019272488946267** is returned.

.. code-block::

   select cbrt(36);

The value **NULL** is returned.

.. code-block::

   select cbrt(null);
