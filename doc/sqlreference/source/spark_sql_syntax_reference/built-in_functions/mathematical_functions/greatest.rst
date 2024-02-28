:original_name: dli_spark_greatest.html

.. _dli_spark_greatest:

greatest
========

This function is used to return the greatest value in a list of values.

Syntax
------

.. code-block::

   greatest(T v1, T v2, ...)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------+-----------+----------------------------+--------------------------------------+
   | Parameter | Mandatory | Type                       | Description                          |
   +===========+===========+============================+======================================+
   | v1        | Yes       | DOUBLE, BIGINT, or DECIMAL | The value can be a float or integer. |
   +-----------+-----------+----------------------------+--------------------------------------+
   | v2        | Yes       | DOUBLE, BIGINT, or DECIMAL | The value can be a float or integer. |
   +-----------+-----------+----------------------------+--------------------------------------+

Return Values
-------------

The return value is of the DOUBLE type.

.. note::

   If the value of **a** is **NULL**, **NULL** is returned.

Example Code
------------

The value **4.0** is returned.

.. code-block::

   select greatest(1,2.0,3,4.0);

The value **NULL** is returned.

.. code-block::

   select greatest(null);
