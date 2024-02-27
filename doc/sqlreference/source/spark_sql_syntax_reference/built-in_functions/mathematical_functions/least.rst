:original_name: dli_spark_least.html

.. _dli_spark_least:

least
=====

This function is used to return the smallest value in a list of values.

Syntax
------

.. code-block::

   least(T v1, T v2, ...)

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

   -  If the value of **v1** or **v2** is of the STRING type, an error is reported.
   -  If the values of all parameters are **NULL**, **NULL** is returned.

Example Code
------------

The value **1.0** is returned.

.. code-block::

   select least(1,2.0,3,4.0);

The value **NULL** is returned.

.. code-block::

   select least(null);
