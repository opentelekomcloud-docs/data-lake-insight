:original_name: dli_spark_soundex.html

.. _dli_spark_soundex:

soundex
=======

This function is used to return the soundex string from **str**, for example, **soundex('Miller') = M460**.

Syntax
------

.. code-block::

   soundex(string <str>)

Parameters
----------

.. table:: **Table 1** Parameter

   ========= ========= ====== ======================
   Parameter Mandatory Type   Description
   ========= ========= ====== ======================
   str       Yes       STRING String to be converted
   ========= ========= ====== ======================

Return Values
-------------

The return value is of the STRING type.

.. note::

   If the value of **str** is **NULL**, **NULL** is returned.

Example Code
------------

The value **M460** is returned.

.. code-block::

   SELECT soundex('Miller');
