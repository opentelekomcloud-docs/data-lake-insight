:original_name: dli_spark_initcap.html

.. _dli_spark_initcap:

initcap
=======

This function is used to convert the first letter of each word of a string to upper case and all other letters to lower case.

Syntax
------

.. code-block::

   initcap(string A)

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

The return value is of the STRING type. In the string, the first letter of each word is capitalized, and the other letters are lowercased.

Example Code
------------

The value **Dli Sql** is returned.

.. code-block::

   SELECT initcap("dLI sql");
