:original_name: dli_spark_keyvalue.html

.. _dli_spark_keyvalue:

keyvalue
========

This function is used to split str by split1, convert each group into a key-value pair by split2, and return the value corresponding to the key.

Syntax
------

.. code-block::

   keyvalue(string <str>,[string <split1>,string <split2>,] string <key>)

Parameters
----------

.. table:: **Table 1** Parameters

   +----------------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter      | Mandatory | Type   | Description                                                                                                                                                                                                                                                                                                   |
   +================+===========+========+===============================================================================================================================================================================================================================================================================================================+
   | str            | Yes       | STRING | String to be split                                                                                                                                                                                                                                                                                            |
   +----------------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | split1, split2 | No        | STRING | Strings used as separators to split the source string. If these two separators are not specified in the expression, the default values are **;** for **split1** and **:** for **split2**. If there are multiple occurrences of split2 within a string that has been split by split1, the result is undefined. |
   +----------------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | key            | No        | BIGINT | The corresponding value when the string is split using **split1** and **split2**.                                                                                                                                                                                                                             |
   +----------------+-----------+--------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If the value of **split1** or **split2** is **NULL**, **NULL** is returned.
   -  If the value of **str** or **key** is **NULL** or no matching key is found, **NULL** is returned.
   -  If multiple key-value pairs are matched, the value corresponding to the first matched key is returned.

Example Code
------------

The value **2** is returned.

.. code-block::

   select keyvalue('a:1;b:2', 'b');

The value **2** is returned.

.. code-block::

   select keyvalue("\;abc:1\;def:2","\;",":","def");
