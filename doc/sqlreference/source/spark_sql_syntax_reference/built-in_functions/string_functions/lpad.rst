:original_name: dli_spark_lpad.html

.. _dli_spark_lpad:

lpad
====

This function is used to return a string of a specified length. If the length of the given string (str1) is shorter than the specified length (length), the given string is left-padded with str2 to the specified length.

Syntax
------

.. code-block::

   lpad(string <str1>, int <length>, string <str2>)

Parameters
----------

.. table:: **Table 1** Parameters

   ========= ========= ====== =========================================
   Parameter Mandatory Type   Description
   ========= ========= ====== =========================================
   str1      Yes       STRING String to be left-padded
   length    Yes       STRING Number of digits to be padded to the left
   str2      No        STRING String used for padding
   ========= ========= ====== =========================================

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If the value of **length** is smaller than the number of digits in **str1**, the string whose length is truncated from the left of str1 is returned.
   -  If the value of **length** is **0**, an empty string is returned.
   -  If there is no input parameter or the value of any input parameter is **NULL**, **NULL** is returned.

Example Code
------------

-  Uses string **ZZ** to left pad string **abcdefgh** to 10 digits. An example command is as follows:

   The value **ZZabcdefgh** is returned.

   .. code-block::

      select lpad('abcdefgh', 10, 'ZZ');

-  Uses string **ZZ** to left pad string **abcdefgh** to 5 digits. An example command is as follows:

   The value **abcde** is returned.

   .. code-block::

      select lpad('abcdefgh', 5, 'ZZ');

-  The value of **length** is **0**. An example command is as follows:

   An empty string is returned.

   .. code-block::

      select lpad('abcdefgh', 0, 'ZZ');

-  The value of any input parameter is **NULL**. An example command is as follows:

   The value **NULL** is returned.

   .. code-block::

      select lpad(null ,0, 'ZZ');
