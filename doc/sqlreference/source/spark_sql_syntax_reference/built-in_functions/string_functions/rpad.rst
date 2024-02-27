:original_name: dli_spark_rpad.html

.. _dli_spark_rpad:

rpad
====

This function is used to right pad **str1** with **str2** to the specified length.

Syntax
------

.. code-block::

   rpad(string <str1>, int <length>, string <str2>)

Parameters
----------

.. table:: **Table 1** Parameters

   ========= ========= ====== ==========================================
   Parameter Mandatory Type   Description
   ========= ========= ====== ==========================================
   str1      Yes       STRING String to be right-padded
   length    Yes       INT    Number of digits to be padded to the right
   str2      Yes       STRING String used for padding
   ========= ========= ====== ==========================================

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If the value of **length** is smaller than the number of digits in **str1**, the string whose length is truncated from the left of str1 is returned.
   -  If the value of **length** is **0**, an empty string is returned.
   -  If there is no input parameter or the value of any input parameter is **NULL**, **NULL** is returned.

Example Code
------------

The value **hi???** is returned.

.. code-block::

   SELECT rpad('hi', 5, '??');

The value **h** is returned.

.. code-block::

   SELECT rpad('hi', 1, '??');
