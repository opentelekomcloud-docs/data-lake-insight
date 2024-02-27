:original_name: dli_spark_regexp_extract.html

.. _dli_spark_regexp_extract:

regexp_extract
==============

This function is used to match the string **source** based on the **pattern** grouping rule and return the string content that matches **groupid**.

Syntax
------

regexp_extract(string <source>, string <pattern>[, bigint <groupid>])

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------+-----------+--------+----------------------------------------------------------------------------+
   | Parameter | Mandatory | Type   | Description                                                                |
   +===========+===========+========+============================================================================+
   | source    | Yes       | STRING | String to be split                                                         |
   +-----------+-----------+--------+----------------------------------------------------------------------------+
   | pattern   | Yes       | STRING | Constant or regular expression of the STRING type. Pattern to be matched.  |
   +-----------+-----------+--------+----------------------------------------------------------------------------+
   | groupid   | No        | BIGINT | Constant of the BIGINT type. The value must be greater than or equal to 0. |
   +-----------+-----------+--------+----------------------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If the value of **pattern** is an empty string or there is no group in **pattern**, an error is reported.
   -  If the value of **groupid** is not of the BIGINT type or is less than 0, an error is reported.
   -  If this parameter is not specified, the default value **1** is used, indicating that the first group is returned.
   -  If the value of **groupid** is **0**, the substring that meets the entire pattern is returned.
   -  If the value of **source**, **pattern**, or **groupid** is **NULL**, **NULL** is returned.

Example Code
------------

Splits **basketball** by **bas(.*?)(ball)**. The value **ket** is returned.

.. code-block::

   select regexp_extract('basketball', 'bas(.*?)(ball)');

The value **basketball** is returned.

.. code-block::

   select regexp_extract('basketball', 'bas(.*?)(ball)',0);

The value **99** is returned. When submitting SQL statements for regular expression calculation on DLI, two backslashes (\\) are used as escape characters.

.. code-block::

   select regexp_extract('8d99d8', '8d(\\d+)d8');

The value **[Hello]** is returned.

.. code-block::

   select regexp_extract('[Hello] hello', '([^\\x{00}-\\x{ff}]+)');

The value **Hello** is returned.

.. code-block::

   select regexp_extract('[Hello] hello', '([\\x{4e00}-\\x{9fa5}]+)');
