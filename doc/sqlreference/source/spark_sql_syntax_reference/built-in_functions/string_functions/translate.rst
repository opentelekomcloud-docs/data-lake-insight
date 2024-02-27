:original_name: dli_spark_translate.html

.. _dli_spark_translate:

translate
=========

This function is used to translate the input string by replacing the characters or string specified by **from** with the characters or string specified by **to**.

For example, it replaces **bcd** in **abcde** with **BCD**.

.. code-block::

   translate("abcde", "bcd", "BCD")

Syntax
------

.. code-block::

   translate(string|char|varchar input, string|char|varchar from, string|char|varchar to)

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

The value **A1B2C3** is returned.

.. code-block::

   SELECT translate('AaBbCc', 'abc', '123');
