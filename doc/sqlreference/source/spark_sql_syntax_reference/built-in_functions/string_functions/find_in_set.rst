:original_name: dli_spark_find_in_set.html

.. _dli_spark_find_in_set:

find_in_set
===========

This function is used to return the position (stating from 1) of str1 in str2 separated by commas (,).

Syntax
------

.. code-block::

   find_in_set(string <str1>, string <str2>)

Parameters
----------

.. table:: **Table 1** Parameters

   ========= ========= ====== ===============================
   Parameter Mandatory Type   Description
   ========= ========= ====== ===============================
   str1      Yes       STRING String to be searched for
   str2      Yes       STRING String separated by a comma (,)
   ========= ========= ====== ===============================

Return Values
-------------

The return value is of the BIGINT type.

.. note::

   -  If str1 cannot be matched in str2 or str1 contains commas (,), **0** is returned.
   -  If the value of **str1** or **str2** is **NULL**, **NULL** is returned.

Example Code
------------

-  Search for the position of string **ab** in string **abc,123,ab,c**. An example command is as follows:

   The value **3** is returned.

   .. code-block::

      select find_in_set('ab', 'abc,123,ab,c');

-  Search for the position of string **hi** in string **abc,123,ab,c**. An example command is as follows:

   The value **0** is returned.

   .. code-block::

      select find_in_set('hi', 'abc,123,ab,c');

-  The value of any input parameter is **NULL**. An example command is as follows:

   The value **NULL** is returned.

   .. code-block::

      select find_in_set(null, 'abc,123,ab,c');
