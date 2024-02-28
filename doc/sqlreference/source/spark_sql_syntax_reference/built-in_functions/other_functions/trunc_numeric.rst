:original_name: dli_spark_trunc_numeric.html

.. _dli_spark_trunc_numeric:

trunc_numeric
=============

This function is used to truncate the **number** value to a specified decimal place.

Syntax
------

.. code-block::

   trunc_numeric(<number>[, bigint<decimal_places>])

Parameters
----------

.. table:: **Table 1** Parameters

   +----------------+-----------+------------------------------------+-------------------------------------------------------------------------------------------------+
   | Parameter      | Mandatory | Type                               | Description                                                                                     |
   +================+===========+====================================+=================================================================================================+
   | number         | Yes       | DOUBLE, BIGINT, DECIMAL, or STRING | Data to be truncated                                                                            |
   +----------------+-----------+------------------------------------+-------------------------------------------------------------------------------------------------+
   | decimal_places | No        | BIGINT                             | The default value is **0**, indicating that the decimal place at which the number is truncated. |
   +----------------+-----------+------------------------------------+-------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the DOUBLE or DECIMAL type.

.. note::

   The return rules are as follows:

   -  If the **number** value is of the DOUBLE or DECIMAL type, the corresponding type is returned.
   -  If the **number** value is of the STRING or BIGINT type, **DOUBLE** is returned.
   -  If the **decimal_places** value is not of the BIGINT type, an error is reported.
   -  If the value of **number** is **NULL**, **NULL** is returned.

Example Code
------------

The value **3.141** is returned.

.. code-block::

   select trunc_numeric(3.1415926, 3);

The value **3** is returned.

.. code-block::

   select trunc_numeric(3.1415926);

An error is reported.

.. code-block::

   select trunc_numeric(3.1415926, 3.1);
