:original_name: dli_spark_regexp_substr.html

.. _dli_spark_regexp_substr:

regexp_substr
=============

This function is used to return the substring that matches a specified pattern for the occurrence time, starting from **start_position** in the string **source**.

Syntax
------

.. code-block::

   regexp_substr(string <source>, string <pattern>[, bigint <start_position>[, bigint <occurrence>]])

Parameters
----------

.. table:: **Table 1** Parameters

   +----------------+-----------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter      | Mandatory | Type   | Description                                                                                                                                                                                              |
   +================+===========+========+==========================================================================================================================================================================================================+
   | source         | Yes       | STRING | String to be searched for                                                                                                                                                                                |
   +----------------+-----------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | pattern        | Yes       | STRING | Constant or regular expression of the STRING type. Pattern to be matched.                                                                                                                                |
   +----------------+-----------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | start_position | No        | BIGINT | Start position. The value must be greater than 0. If this parameter is not specified, the default value **1** is used, indicating that the matching starts from the first character of **source**.       |
   +----------------+-----------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | occurrence     | No        | BIGINT | The value is a constant of the BIGINT type, which must be greater than 0. If it is not specified, the default value **1** is used, indicating that the substring matched for the first time is returned. |
   +----------------+-----------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If the value of **pattern** is an empty string, an error is reported.
   -  If no match is found, **NULL** is returned.
   -  If the value of **start_position** or **occurrence** is not of the BIGINT type or is less than or equal to 0, an error is reported.
   -  If the value of **source**, **pattern**, **start_position**, **occurrence**, or **return_option** is **NULL**, **NULL** is returned.

Example Code
------------

The value **a** is returned.

.. code-block::

   select regexp_substr('a1b2c3', '[a-z]');

The value **b** is returned.

.. code-block::

   select regexp_substr('a1b2c3', '[a-z]', 2, 1);

The value **c** is returned.

.. code-block::

   select regexp_substr('a1b2c3', '[a-z]', 2, 2);

The value **NULL** is returned.

.. code-block::

   select regexp_substr('a1b2c3', null);
