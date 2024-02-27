:original_name: dli_spark_instr1.html

.. _dli_spark_instr1:

instr1
======

This function is used to return the position of substring str2 in string str1.

Similar function: :ref:`instr <dli_spark_instr>`. The **instr** function is used to return the index of substr that appears earliest in **str**. However, instr does not support specifying the start search position and matching times.

Syntax
------

.. code-block::

   instr1(string <str1>, string <str2>[, bigint <start_position>[, bigint <nth_appearance>]])

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                              |
   +=================+=================+=================+==========================================================================================================================================================================================+
   | str1            | Yes             | STRING          | Target string to be searched for                                                                                                                                                         |
   |                 |                 |                 |                                                                                                                                                                                          |
   |                 |                 |                 | If the value is of the BIGINT, DOUBLE, DECIMAL, or DATETIME type, the value is implicitly converted to the STRING type for calculation. For other types of values, an error is reported. |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | str2            | Yes             | STRING          | Substring to be matched                                                                                                                                                                  |
   |                 |                 |                 |                                                                                                                                                                                          |
   |                 |                 |                 | If the value is of the BIGINT, DOUBLE, DECIMAL, or DATETIME type, the value is implicitly converted to the STRING type for calculation. For other types of values, an error is reported. |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | start_position  | No              | BIGINT          | Sequence number of the character in str1 the search starts from. The default start position is position 1 (position of the first character).                                             |
   |                 |                 |                 |                                                                                                                                                                                          |
   |                 |                 |                 | If this parameter is set to a negative number, the search starts from the end to the beginning of the string, and the last character is **-1**.                                          |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | nth_appearance  | No              | BIGINT          | Position of str2 that is matched for the nth_appearance time in str1.                                                                                                                    |
   |                 |                 |                 |                                                                                                                                                                                          |
   |                 |                 |                 | If the value is of another type or is less than or equal to 0, an error is reported.                                                                                                     |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the BIGINT type.

.. note::

   -  If **str2** is not found in **str1**, **0** is returned.
   -  If str2 is an empty string, the matching is always successful.
   -  If the value of **str1**, **str2**, **start_position**, or **nth_appearance** is **NULL**, **NULL** is returned.

Example Code
------------

The value **10** is returned.

.. code-block::

   select instr1('Tech on the net', 'h', 5, 1);

The value **2** is returned.

.. code-block::

   select instr1('abc', 'b');

The value **NULL** is returned.

.. code-block::

   select instr('abc', null);
