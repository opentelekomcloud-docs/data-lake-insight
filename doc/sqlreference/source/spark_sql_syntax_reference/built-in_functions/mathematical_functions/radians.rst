:original_name: dli_spark_radians.html

.. _dli_spark_radians:

radians
=======

This function is used to return the radian corresponding to an angle.

Syntax
------

.. code-block::

   radians(DOUBLE a)

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

The value **1.0471975511965976** is returned.

.. code-block::

   select radians(60);

The value **0** is returned.

.. code-block::

   select radians(0);

The value **NULL** is returned.

.. code-block::

   select radians(null);
