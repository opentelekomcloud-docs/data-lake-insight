:original_name: dli_spark_instr.html

.. _dli_spark_instr:

instr
=====

This function is used to return the index of substr that appears earliest in str.

It returns **NULL** if either of the arguments are **NULL** and returns **0** if substr does not exist in str. Note that the first character in str has index 1.

Similar function: :ref:`instr1 <dli_spark_instr1>`. The **instr1** function is used to calculate the position of the substring str2 in the string str1. The **instr1** function allows you to specify the start search position and the number of matching times.

Syntax
------

.. code-block::

   instr(string <str>, string <substr>)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                              |
   +=================+=================+=================+==========================================================================================================================================================================================+
   | str             | Yes             | STRING          | Target string to be searched for                                                                                                                                                         |
   |                 |                 |                 |                                                                                                                                                                                          |
   |                 |                 |                 | If the value is of the BIGINT, DOUBLE, DECIMAL, or DATETIME type, the value is implicitly converted to the STRING type for calculation. For other types of values, an error is reported. |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | substr          | Yes             | STRING          | Substring to be matched                                                                                                                                                                  |
   |                 |                 |                 |                                                                                                                                                                                          |
   |                 |                 |                 | If the value is of the BIGINT, DOUBLE, DECIMAL, or DATETIME type, the value is implicitly converted to the STRING type for calculation. For other types of values, an error is reported. |
   +-----------------+-----------------+-----------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the BIGINT type.

.. note::

   -  If **str2** is not found in **str1**, **0** is returned.
   -  If **str2** is an empty string, the matching is always successful. For example, **select instr('abc','');** returns **1**.
   -  If the value of **str1** or **str2** is **NULL**, **NULL** is returned.

Example Code
------------

-  Returns the position of character b in string abc. An example command is as follows:

   The value **2** is returned.

   .. code-block::

      select instr('abc', 'b');

-  The value of any input parameter is **NULL**. An example command is as follows:

   The value **NULL** is returned.

   .. code-block::

      select instr('abc', null)
