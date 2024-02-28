:original_name: dli_spark_atan.html

.. _dli_spark_atan:

atan
====

This function is used to return the arc tangent value of a given angle **a**.

Syntax
------

.. code-block::

   atan(DOUBLE a)

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

The return value is of the DOUBLE type. The value ranges from -Pi/2 to Pi/2.

.. note::

   -  If the value of **a** is not within the range [-1,1], **NaN** is returned.
   -  If the value of **a** is **NULL**, **NULL** is returned.

Example Code
------------

The value **0.7853981633974483** is returned.

.. code-block::

   select atan(1);

The value **0.5404195002705842** is returned.

.. code-block::

   select atan(0.6);

The value **NULL** is returned.

.. code-block::

   select atan(null);
