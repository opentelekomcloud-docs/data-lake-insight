:original_name: dli_spark_regexp_count.html

.. _dli_spark_regexp_count:

regexp_count
============

This function is used to return the number of substrings that match a specified pattern in the source, starting from the **start_position** position.

Syntax
------

.. code-block::

   regexp_count(string <source>, string <pattern>[, bigint <start_position>])

Parameters
----------

.. table:: **Table 1** Parameters

   +----------------+-----------+--------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter      | Mandatory | Type   | Description                                                                                                                                                                                                                                                                                          |
   +================+===========+========+======================================================================================================================================================================================================================================================================================================+
   | source         | Yes       | STRING | String to be searched for. For other types, an error is reported.                                                                                                                                                                                                                                    |
   +----------------+-----------+--------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | pattern        | Yes       | STRING | Constant or regular expression of the STRING type. Pattern to be matched. If the value of this parameter is an empty string or of other types, an error is reported.                                                                                                                                 |
   +----------------+-----------+--------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | start_position | No        | BIGINT | Constant of the BIGINT type. The value must be greater than 0. If the value is of another type or is less than or equal to 0, an error is reported. If this parameter is not specified, the default value **1** is used, indicating that the matching starts from the first character of **source**. |
   +----------------+-----------+--------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the BIGINT type.

.. note::

   -  If no match is found, **0** is returned.
   -  If the value of **source** or **pattern** is **NULL**, **NULL** is returned.

Example Code
------------

The value **4** is returned.

.. code-block::

   select regexp_count('ab0a1a2b3c', '[0-9]');

The value **3** is returned.

.. code-block::

   select regexp_count('ab0a1a2b3c', '[0-9]', 4);

The value **null** is returned.

.. code-block::

   select regexp_count('ab0a1a2b3c', null);
