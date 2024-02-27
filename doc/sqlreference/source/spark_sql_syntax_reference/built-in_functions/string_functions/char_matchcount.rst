:original_name: dli_spark_char_matchcount.html

.. _dli_spark_char_matchcount:

char_matchcount
===============

This parameter is used to return the number of characters in str1 that appear in str2.

Syntax
------

.. code-block::

   char_matchcount(string <str1>, string <str2>)

Parameters
----------

.. table:: **Table 1** Parameter

   ========== ========= ====== ==============================
   Parameter  Mandatory Type   Description
   ========== ========= ====== ==============================
   str1, str2 Yes       STRING str1 and str2 to be calculated
   ========== ========= ====== ==============================

Return Values
-------------

The return value is of the BIGINT type.

.. note::

   If the value of **str1** or **str2** is **NULL**, **NULL** is returned.

Example Code
------------

The value **3** is returned.

.. code-block::

   select char_matchcount('abcz','abcde');

The value **NULL** is returned.

.. code-block::

   select char_matchcount(null,'abcde');
