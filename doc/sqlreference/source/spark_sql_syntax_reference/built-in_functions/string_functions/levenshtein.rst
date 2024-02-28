:original_name: dli_spark_levenshtein.html

.. _dli_spark_levenshtein:

levenshtein
===========

This function is used to returns the Levenshtein distance between two strings, for example, **levenshtein('kitten','sitting') = 3**.

.. note::

   Levenshtein distance is a type of edit distance. It indicates the minimum number of edit operations required to convert one string into another.

Syntax
------

.. code-block::

   levenshtein(string A, string B)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------+-----------+--------+---------------------------------------------------------------+
   | Parameter | Mandatory | Type   | Description                                                   |
   +===========+===========+========+===============================================================+
   | A, B      | Yes       | STRING | String to be entered for calculating the Levenshtein distance |
   +-----------+-----------+--------+---------------------------------------------------------------+

Return Values
-------------

The return value is of the INT type.

Example Code
------------

The value **3** is returned.

.. code-block::

   SELECT levenshtein('kitten','sitting');
