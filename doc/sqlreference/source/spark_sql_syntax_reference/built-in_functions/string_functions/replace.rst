:original_name: dli_spark_replace.html

.. _dli_spark_replace:

replace
=======

This function is used to replace the part in a specified string that is the same as the string **old** with the string new and return the result.

If the string has no same characters as the string **old**, **str** is returned.

Syntax
------

.. code-block::

   replace(string <str>, string <old>, string <new>)

Parameters
----------

.. table:: **Table 1** Parameters

   ========= ========= ====== ========================
   Parameter Mandatory Type   Description
   ========= ========= ====== ========================
   str       Yes       STRING String to be replaced
   old       Yes       STRING String to be compared
   new       Yes       STRING String after replacement
   ========= ========= ====== ========================

Return Values
-------------

The return value is of the STRING type.

.. note::

   If the value of any input parameter is **NULL**, **NULL** is returned.

Example Code
------------

The value **AA123AA** is returned.

.. code-block::

   select replace('abc123abc','abc','AA');

The value **NULL** is returned.

.. code-block::

   select replace('abc123abc',null,'AA');
