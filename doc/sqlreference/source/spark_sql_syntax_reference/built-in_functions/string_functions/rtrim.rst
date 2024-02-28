:original_name: dli_spark_rtrim.html

.. _dli_spark_rtrim:

rtrim
=====

This function is used to remove characters from the right of **str**.

-  If **trimChars** is not specified, spaces are removed by default.
-  If **trimChars** is specified, the function removes the longest possible substring from the right end of **str** that consists of characters in the **trimChars** set.

Similar functions:

-  :ref:`ltrim <dli_spark_ltrim>`. This function is used to remove characters from the left of **str**.
-  :ref:`trim <dli_spark_trim>`. This function is used to remove characters from the left and right of **str**.

Syntax
------

.. code-block::

   rtrim([<trimChars>, ]string <str>)

or

.. code-block::

   rtrim(trailing [<trimChars>] from <str>)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------+-----------+--------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter | Mandatory | Type   | Description                                                                                                                                                                                          |
   +===========+===========+========+======================================================================================================================================================================================================+
   | str       | Yes       | STRING | String from which characters on the right are to be removed. If the value is of the BIGINT, DECIMAL, DOUBLE, or DATETIME type, the value is implicitly converted to the STRING type for calculation. |
   +-----------+-----------+--------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | trimChars | Yes       | STRING | Characters to be removed                                                                                                                                                                             |
   +-----------+-----------+--------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If the value of **str** is not of the STRING, BIGINT, DOUBLE, DECIMAL, or DATETIME type, an error is reported.
   -  If the value of **str** or **trimChars** is **NULL**, **NULL** is returned.

Example Code
------------

-  Removes spaces on the right of the string **yxabcxx**. An example command is as follows:

   The value **yxabcxx** is returned.

   .. code-block::

      select rtrim('yxabcxx     ');

   It is equivalent to the following statement:

   .. code-block::

      select trim(trailing from ' yxabcxx ');

-  Removes all substrings from the right end of the string **yxabcxx** that consist of characters in the set **xy**.

   The function returns **yxabc**, as any substring starting with **x** or **y** from the right end is removed.

   .. code-block::

      select rtrim('xy', 'yxabcxx');

   It is equivalent to the following statement:

   .. code-block::

      select trim(trailing 'xy' from 'yxabcxx');

-  The value of the input parameter is **NULL**. An example command is as follows:

   The value **NULL** is returned.

   .. code-block::

      select rtrim(null);
      select ltrim('yxabcxx', 'null');
