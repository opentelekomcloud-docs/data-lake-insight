:original_name: dli_spark_lower_lcase.html

.. _dli_spark_lower_lcase:

lower/lcase
===========

This function is used to convert all characters of a string to the lower case.

Syntax
------

.. code-block::

   lower(string A) / lcase(string A)

Parameters
----------

.. table:: **Table 1** Parameter

   ========= ========= ====== ===========================
   Parameter Mandatory Type   Description
   ========= ========= ====== ===========================
   A         Yes       STRING Text string to be converted
   ========= ========= ====== ===========================

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If the value of the input parameter is not of the STRING, BIGINT, DOUBLE, DECIMAL, or DATETIME type, an error is reported.
   -  If the value of the input parameter is **NULL**, **NULL** is returned.

Example Code
------------

Converts uppercase characters in a string to the lower case. An example command is as follows:

The value **abc** is returned.

.. code-block::

   select lower('ABC');

The value of the input parameter is **NULL**. An example command is as follows:

The value **NULL** is returned.

.. code-block::

   select lower(null);
