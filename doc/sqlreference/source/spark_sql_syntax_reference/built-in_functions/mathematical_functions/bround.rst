:original_name: dli_spark_bround.html

.. _dli_spark_bround:

bround
======

This function is used to return a value that is rounded off to **d** decimal places.

Syntax
------

.. code-block::

   bround(DOUBLE a, INT d)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+------------------------------------+----------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type                               | Description                                                                                                                |
   +=================+=================+====================================+============================================================================================================================+
   | a               | Yes             | DOUBLE, BIGINT, DECIMAL, or STRING | The value can be a float, integer, or string.                                                                              |
   |                 |                 |                                    |                                                                                                                            |
   |                 |                 |                                    | It indicates the value that needs to be rounded.                                                                           |
   |                 |                 |                                    |                                                                                                                            |
   |                 |                 |                                    | The digit 5 is rounded up if the digit before 5 is an odd number and rounded down if the digit before 5 is an even number. |
   |                 |                 |                                    |                                                                                                                            |
   |                 |                 |                                    | If the value is not of the DOUBLE type, the system will implicitly convert it to the DOUBLE type for calculation.          |
   +-----------------+-----------------+------------------------------------+----------------------------------------------------------------------------------------------------------------------------+
   | d               | No              | DOUBLE, BIGINT, DECIMAL, or STRING | It indicates the number of decimal places to which the value needs to be rounded.                                          |
   |                 |                 |                                    |                                                                                                                            |
   |                 |                 |                                    | If the value is not of the INT type, the system will implicitly convert it to the INT type for calculation.                |
   +-----------------+-----------------+------------------------------------+----------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the DOUBLE type.

.. note::

   If the value of **a** or **d** is **NULL**, **NULL** is returned.

Example Code
------------

The value **123.4** is returned.

.. code-block::

   select bround(123.45,1);

The value **123.6** is returned.

.. code-block::

   select bround(123.55,1);

The value **NULL** is returned.

.. code-block::

   select bround(null);

The value **123.457** is returned.

.. code-block::

   select bround(123.456789,3.123456);
