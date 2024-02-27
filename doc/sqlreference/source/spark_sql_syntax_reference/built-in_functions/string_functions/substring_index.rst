:original_name: dli_spark_substring_index.html

.. _dli_spark_substring_index:

substring_index
===============

This function is used to truncate the string before the **count** separator of **str**. If the value of **count** is positive, the string is truncated from the left. If the value of **count** is negative, the string is truncated from the right.

Syntax
------

.. code-block::

   substring_index(string <str>, string <separator>, int <count>)

Parameters
----------

.. table:: **Table 1** Parameters

   ========= ========= ====== ============================
   Parameter Mandatory Type   Description
   ========= ========= ====== ============================
   str       Yes       STRING String to be truncated
   separator Yes       STRING Separator of the STRING type
   count     No        INT    Position of the delimiter
   ========= ========= ====== ============================

Return Values
-------------

The return value is of the STRING type.

.. note::

   If the value of any input parameter is **NULL**, **NULL** is returned.

Example Code
------------

The value **hello.world** is returned.

.. code-block::

   SELECT substring_index('hello.world.people', '.', 2);

The value **world.people** is returned.

.. code-block::

   select substring_index('hello.world.people', '.', -2);
