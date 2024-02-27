:original_name: dli_spark_degress.html

.. _dli_spark_degress:

degrees
=======

This function is used to calculate the angle corresponding to the returned radian.

Syntax
------

.. code-block::

   degrees(DOUBLE a)

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

The value **90.0** is returned.

.. code-block::

   select degrees(1.5707963267948966);

The value **0** is returned.

.. code-block::

   select degrees(0);

The value **NULL** is returned.

.. code-block::

   select degrees(null);
