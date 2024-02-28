:original_name: dli_spark_asin.html

.. _dli_spark_asin:

asin
====

This function is used to return the arc sine value of a given angle **a**.

Syntax
------

.. code-block::

   asin(DOUBLE a)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------------+-----------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type                               | Description                                                                                                       |
   +=================+=================+====================================+===================================================================================================================+
   | a               | Yes             | DOUBLE, BIGINT, DECIMAL, or STRING | The value range is [-1,1]. The value can be a float, integer, or string.                                          |
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

The value **1.5707963267948966** is returned.

.. code-block::

   select asin(1);

The value **0.6435011087932844** is returned.

.. code-block::

   select asin(0.6);

The value **NULL** is returned.

.. code-block::

   select asin(null);

The value **NAN** is returned.

.. code-block::

   select asin(10);
