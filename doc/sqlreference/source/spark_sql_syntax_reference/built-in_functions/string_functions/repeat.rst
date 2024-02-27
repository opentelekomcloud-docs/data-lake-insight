:original_name: dli_spark_repeat.html

.. _dli_spark_repeat:

repeat
======

This function is used to return the string after **str** is repeated for **n** times.

Syntax
------

.. code-block::

   repeat(string <str>, bigint <n>)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Type   | Description                                                                                                                             |
   +===========+===========+========+=========================================================================================================================================+
   | str       | Yes       | STRING | If the value is of the BIGINT, DOUBLE, DECIMAL, or DATETIME type, the value is implicitly converted to the STRING type for calculation. |
   +-----------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | n         | Yes       | BIGINT | Number used for repetition                                                                                                              |
   +-----------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If the value of **str** is not of the STRING, BIGINT, DOUBLE, DECIMAL, or DATETIME type, an error is reported.
   -  If the value of **n** is empty, an error is reported.
   -  If the value of **str** or **n** is **NULL**, **NULL** is returned.

Example Code
------------

The value **123123** is returned after the string **123** is repeated twice.

.. code-block::

   SELECT repeat('123', 2);
