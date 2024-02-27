:original_name: dli_spark_negative.html

.. _dli_spark_negative:

negative
========

This function is used to return the additive inverse of **a**.

Syntax
------

.. code-block::

   negative(INT a)

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

The return value is of the DECIMAL or INT type.

.. note::

   If the value of **a** is **NULL**, **NULL** is returned.

Example Code
------------

The value **-1** is returned.

.. code-block::

   SELECT negative(1);

The value **3** is returned.

.. code-block::

   SELECT negative(-3);
