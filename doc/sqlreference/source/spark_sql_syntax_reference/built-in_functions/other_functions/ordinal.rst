:original_name: dli_spark_ordinal.html

.. _dli_spark_ordinal:

ordinal
=======

This function is used to sort input variables in ascending order and return the value at the position specified by **nth**.

Syntax
------

.. code-block::

   ordinal(bigint <nth>, <var1>, <var2>[,...])

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------+-----------+-------------------------------------+-------------------------------+
   | Parameter | Mandatory | Type                                | Description                   |
   +===========+===========+=====================================+===============================+
   | nth       | Yes       | BIGINT                              | Position value to be returned |
   +-----------+-----------+-------------------------------------+-------------------------------+
   | var       | Yes       | BIGINT, DOUBLE, DATETIME, or STRING | Value to be sorted            |
   +-----------+-----------+-------------------------------------+-------------------------------+

Return Values
-------------

The return value is of the DOUBLE or DECIMAL type.

.. note::

   -  Value of the nth bit. If there are no implicit conversions, the return value has the same data type as the input parameter.
   -  If there are type conversions, **DOUBLE** is returned for the conversion between **DOUBLE**, **BIGINT**, and **STRING**, and **DATETIME** is returned for the conversion between **STRING** and **DATETIME**. Other implicit conversions are not allowed.
   -  **NULL** indicates the minimum value.

Example Code
------------

The value **2** is returned.

.. code-block::

   select ordinal(3, 1, 3, 2, 5, 2, 4, 9);
