:original_name: dli_spark_regexp_replace1.html

.. _dli_spark_regexp_replace1:

regexp_replace1
===============

This function is used to replace the substring that matches pattern for the occurrence time in the source string with the specified string **replace_string** and return the result string.

.. note::

   This function applies only to Spark 2.4.5 or earlier.

Similar function: :ref:`regexp_replace <dli_spark_regexp_replace>`. The **regexp_replace** function has slightly different functionalities for different versions of Spark. For details, see :ref:`regexp_replace <dli_spark_regexp_replace>`.

Syntax
------

.. code-block::

   regexp_replace1(string <source>, string <pattern>, string <replace_string>[, bigint <occurrence>])

Parameters
----------

.. table:: **Table 1** Parameters

   +----------------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter      | Mandatory | Type   | Description                                                                                                                                                                                                                                                                                                              |
   +================+===========+========+==========================================================================================================================================================================================================================================================================================================================+
   | source         | Yes       | STRING | String to be replaced                                                                                                                                                                                                                                                                                                    |
   +----------------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | pattern        | Yes       | STRING | Constant or regular expression of the STRING type. Pattern to be matched. For more guidelines on writing regular expressions, refer to the regular expression specifications. If the value of this parameter is an empty string, an error is reported.                                                                   |
   +----------------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | replace_string | Yes       | STRING | String that replaces the one matching the **pattern** parameter                                                                                                                                                                                                                                                          |
   +----------------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | occurrence     | No        | BIGINT | The value must be greater than or equal to 1, indicating that the string that is matched for the occurrence time is replaced with **replace_string**. When the value is **1**, all matched substrings are replaced. If the value is of another type or is less than 1, an error is reported. The default value is **1**. |
   +----------------+-----------+--------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If a group that does not exist is referenced, the group is not replaced.
   -  If the value of **replace_string** is **NULL** and the pattern is matched, **NULL** is returned.
   -  If the value of **replace_string** is **NULL** but the pattern is not matched, **NULL** is returned.
   -  If the value of **source**, **pattern**, or **occurrence** is **NULL**, **NULL** is returned.

Example Code
------------

The value **2222** is returned.

.. code-block::

   select regexp_replace('abcd', '[a-z]', '2');

The value **2bcd** is returned.

.. code-block::

   select regexp_replace('abcd', '[a-z]', '2', 1);

The value **a2cd** is returned.

.. code-block::

   select regexp_replace('abcd', '[a-z]', '2', 2);

The value **ab2d** is returned.

.. code-block::

   select regexp_replace('abcd', '[a-z]', '2', 3);

The value **abc2** is returned.

.. code-block::

   select regexp_replace('abcd', '[a-z]', '2', 4);
