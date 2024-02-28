:original_name: dli_spark_sign.html

.. _dli_spark_sign:

sign
====

This function is used to return the positive and negative signs corresponding to **a**.

Syntax
------

.. code-block::

   sign(DOUBLE a)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------+-----------+------------------------------------+-----------------------------------------------+
   | Parameter | Mandatory | Type                               | Description                                   |
   +===========+===========+====================================+===============================================+
   | a         | Yes       | DOUBLE, BIGINT, DECIMAL, or STRING | The value can be a float, integer, or string. |
   +-----------+-----------+------------------------------------+-----------------------------------------------+

Return Values
-------------

The return value is of the DOUBLE type.

.. note::

   -  If the value of **a** is a positive number, **1** is returned.
   -  If the value of **a** is a negative number, **-1** is returned.
   -  If the value of **a** is **0**, **0** is returned.
   -  If the value of **a** is **NULL**, **NULL** is returned.

Example Code
------------

The value **-1** is returned.

.. code-block::

   select sign(-3);

The value **1** is returned.

.. code-block::

   select sign(3);

The value **0** is returned.

.. code-block::

   select sign(0);

The value **1** is returned.

.. code-block::

   select sign(3.1415926);

The value **NULL** is returned.

.. code-block::

   select sign(null);
