:original_name: dli_spark_round.html

.. _dli_spark_round:

round
=====

This function is used to calculate the rounded value of **a** up to **d** decimal places.

Syntax
------

.. code-block::

   round(DOUBLE a, INT d)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type                               | Description                                                                                                 |
   +=================+=================+====================================+=============================================================================================================+
   | a               | Yes             | DOUBLE, BIGINT, DECIMAL, or STRING | It indicates the value to be rounded off.                                                                   |
   |                 |                 |                                    |                                                                                                             |
   |                 |                 |                                    | The value can be a float, integer, or string.                                                               |
   +-----------------+-----------------+------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | d               | No              | INT                                | The default value is **0**.                                                                                 |
   |                 |                 |                                    |                                                                                                             |
   |                 |                 |                                    | It indicates the number of decimal places to which the value needs to be rounded.                           |
   |                 |                 |                                    |                                                                                                             |
   |                 |                 |                                    | If the value is not of the INT type, the system will implicitly convert it to the INT type for calculation. |
   +-----------------+-----------------+------------------------------------+-------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the DOUBLE type.

.. note::

   -  If the value of **d** is negative, an error is reported.
   -  If the value of **a** or **d** is **NULL**, **NULL** is returned.

Example Code
------------

The value **123.0** is returned.

.. code-block::

   select round(123.321);

The value **123.4** is returned.

.. code-block::

   select round(123.396, 1);

The value **NULL** is returned.

.. code-block::

   select round(null);

The value **123.321** is returned.

.. code-block::

   select round(123.321, 4);

The value **123.3** is returned.

.. code-block::

   select round(123.321,1.33333);

The value **123.3** is returned.

.. code-block::

   select round(123.321,1.33333);
