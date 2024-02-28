:original_name: dli_spark_regexp_instr.html

.. _dli_spark_regexp_instr:

regexp_instr
============

This function is used to return the start or end position of the substring that matches a specified pattern for the occurrence time, starting from **start_position** in the string **source**.

Syntax
------

.. code-block::

   regexp_instr(string <source>, string <pattern>[,bigint <start_position>[, bigint <occurrence>[, bigint <return_option>]]])

Parameters
----------

.. table:: **Table 1** Parameters

   +----------------+-----------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter      | Mandatory | Type   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
   +================+===========+========+======================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
   | source         | Yes       | STRING | Source string                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
   +----------------+-----------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | pattern        | Yes       | STRING | Constant or regular expression of the STRING type. Pattern to be matched. If the value of this parameter is an empty string, an error is reported.                                                                                                                                                                                                                                                                                                                   |
   +----------------+-----------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | start_position | No        | BIGINT | Constant of the BIGINT type. Start position of the search. If it is not specified, the default value **1** is used.                                                                                                                                                                                                                                                                                                                                                  |
   +----------------+-----------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | occurrence     | No        | BIGINT | Constant of the BIGINT type. It indicates the specified number of matching times. If this parameter is not specified, the default value **1** is used, indicating that the first occurrence position is searched.                                                                                                                                                                                                                                                    |
   +----------------+-----------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | return_option  | No        | BIGINT | Constant of the BIGINT type. This parameter indicates the location to be returned. The value can be **0** or **1**. If this parameter is not specified, the default value **0** is used. If this parameter is set to a value of another type or a value that is not allowed, an error message is returned. The value **0** indicates that the start position of the match is returned, and the value **1** indicates that the end position of the match is returned. |
   +----------------+-----------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the BIGINT type. **return_option** indicates the start or end position of the matched substring in **source**.

.. note::

   -  If the value of **pattern** is an empty string, an error is reported.
   -  If the value of **start_position** or **occurrence** is not of the BIGINT type or is less than or equal to 0, an error is reported.
   -  If the value of **source**, **pattern**, **start_position**, **occurrence**, or **return_option** is **NULL**, **NULL** is returned.

Example Code
------------

The value **6** is returned.

.. code-block::

   select regexp_instr('a1b2c3d4', '[0-9]', 3, 2);

The value **NULL** is returned.

.. code-block::

   select regexp_instr('a1b2c3d4', null, 3, 2);
