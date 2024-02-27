:original_name: dli_spark_ltrim.html

.. _dli_spark_ltrim:

ltrim
=====

This function is used to remove characters from the left of **str**.

-  If **trimChars** is not specified, spaces are removed by default.
-  If **trimChars** is specified, the function removes the longest possible substring from the left end of **str** that consists of characters in the **trimChars** set.

Similar functions:

-  :ref:`rtrim <dli_spark_rtrim>`. This function is used to remove characters from the right of **str**.
-  :ref:`trim <dli_spark_trim>`. This function is used to remove characters from the left and right of **str**.

Syntax
------

.. code-block::

   ltrim([<trimChars>,] string <str>)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Type   | Description                                                                                                                                                                                         |
   +===========+===========+========+=====================================================================================================================================================================================================+
   | str       | Yes       | STRING | String from which characters on the left are to be removed. If the value is of the BIGINT, DECIMAL, DOUBLE, or DATETIME type, the value is implicitly converted to the STRING type for calculation. |
   +-----------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | trimChars | Yes       | STRING | Characters to be removed                                                                                                                                                                            |
   +-----------+-----------+--------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If the value of **str** is not of the STRING, BIGINT, DOUBLE, DECIMAL, or DATETIME type, an error is reported.
   -  If the value of **str** or **trimChars** is **NULL**, **NULL** is returned.

Example Code
------------

-  Removes spaces on the left of string **abc**. An example command is as follows:

   The value **stringabc** is returned.

   .. code-block::

      select ltrim('     abc');

   It is equivalent to the following statement:

   .. code-block::

      select trim(leading from '     abc');

   **leading** indicates that the leading spaces in a string are removed.

-  The value of the input parameter is **NULL**. An example command is as follows:

   The value **NULL** is returned.

   .. code-block::

      select ltrim(null);
      select ltrim('xy', null);
      select ltrim(null, 'xy');

-  Removes all substrings from the left end of the string **yxlucyxx** that consist of characters in the set **xy**.

   The function returns **lucyxx**, as any substring starting with **x** or **y** from the left end is removed.

   .. code-block::

      select ltrim( 'xy','yxlucyxx');

   It is equivalent to the following statement:

   .. code-block::

      select trim(leading 'xy' from 'yxlucyxx');
