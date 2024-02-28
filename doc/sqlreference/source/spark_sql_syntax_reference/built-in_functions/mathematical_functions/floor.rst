:original_name: dli_spark_floor.html

.. _dli_spark_floor:

floor
=====

This function is used to round down **a** to the nearest integer.

Syntax
------

.. code-block::

   floor(DOUBLE a)

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

The return value is of the BIGINT type.

.. note::

   If the value of **a** is **NULL**, **NULL** is returned.

Example Code
------------

The value **1** is returned.

.. code-block::

   select floor(1.2);

The value **-2** is returned.

.. code-block::

   select floor(-1.2);

The value **NULL** is returned.

.. code-block::

   select floor(null);
