:original_name: dli_spark_trim.html

.. _dli_spark_trim:

trim
====

This function is used to remove characters from the left and right of **str**.

-  If **trimChars** is not specified, spaces are removed by default.
-  If **trimChars** is specified, the function removes the longest possible substring from both the left and right ends of **str** that consists of characters in the **trimChars** set.

Similar functions:

-  :ref:`ltrim <dli_spark_ltrim>`. This function is used to remove characters from the left of **str**.
-  :ref:`rtrim <dli_spark_rtrim>`. This function is used to remove characters from the right of **str**.

Syntax
------

.. code-block::

   trim([<trimChars>,]string <str>)

or

.. code-block::

   trim([BOTH] [<trimChars>] from <str>)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                             |
   +=================+=================+=================+=========================================================================================================================================+
   | str             | Yes             | STRING          | String from which characters on both left and right are to be removed                                                                   |
   |                 |                 |                 |                                                                                                                                         |
   |                 |                 |                 | If the value is of the BIGINT, DECIMAL, DOUBLE, or DATETIME type, the value is implicitly converted to the STRING type for calculation. |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | trimChars       | No              | STRING          | Characters to be removed                                                                                                                |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If the value of **str** is not of the STRING, BIGINT, DOUBLE, DECIMAL, or DATETIME type, an error is reported.
   -  If the value of **str** or **trimChars** is **NULL**, **NULL** is returned.

Example Code
------------

-  Removes spaces on both left and right of the string **yxabcxx**. An example command is as follows:

   The value **yxabcxx** is returned.

   .. code-block::

      select trim(' yxabcxx ');

   It is equivalent to the following statement:

   .. code-block::

      select trim(both from ' yxabcxx ');
      select trim(from ' yxabcxx ');

-  Removes all substrings from both the left and right ends of the string **yxabcxx** that consist of characters in the set **xy**.

   The function returns **Txyom**, as any substring starting with **x** or **y** from both the left and right ends is removed.

   .. code-block::

      select trim('xy', 'yxabcxx');

   It is equivalent to the following statement:

   .. code-block::

      select trim(both 'xy' from 'yxabcxx');

-  The value of the input parameter is **NULL**. An example command is as follows:

   The value **NULL** is returned.

   .. code-block::

      select trim(null);
      select trim(null, 'yxabcxx');
