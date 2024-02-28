:original_name: dli_spark_regexp_replace.html

.. _dli_spark_regexp_replace:

regexp_replace
==============

This function has slight variations in its functionality depending on the version of Spark being used.

-  For Spark 2.4.5 or earlier: Replaces the substring that matches **pattern** in the string **source** with the specified string **replace_string** and returns the result string.
-  For Spark 3.3.1: Replaces the substring that matches the pattern for the occurrence time in the source string and the substring that matches the pattern later with the specified string **replace_string** and returns the result string.

Similar function: :ref:`regexp_replace1 <dli_spark_regexp_replace1>`. The **regexp_replace1** function is used to replace the substring that matches pattern for the occurrence time in the source string with the specified string **replace_string** and return the result string. However, the egexp_replace1 function applies only to Spark 2.4.5 or earlier.

The regexp_replace1 function is applicable to Spark 2.4.5. It supports specifying the **occurrence** parameter, whereas the **regexp_replace** function does not support it.

Syntax
------

-  Spark 2.4.5 or earlier

   .. code-block::

      regexp_replace(string <source>, string <pattern>, string <replace_string>)

-  Spark 3.1.1

   .. code-block::

      regexp_replace(string <source>, string <pattern>, string <replace_string>[, bigint <occurrence>])

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                                                                                                                                                                                                              |
   +=================+=================+=================+==========================================================================================================================================================================================================================================================================================================================+
   | source          | Yes             | STRING          | String to be replaced                                                                                                                                                                                                                                                                                                    |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | pattern         | Yes             | STRING          | Constant or regular expression of the STRING type. Pattern to be matched. For more guidelines on writing regular expressions, refer to the regular expression specifications. If the value of this parameter is an empty string, an error is reported.                                                                   |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | replace_string  | Yes             | STRING          | String that replaces the one matching the **pattern** parameter                                                                                                                                                                                                                                                          |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | occurrence      | No              | BIGINT          | The value must be greater than or equal to 1, indicating that the string that is matched for the occurrence time is replaced with **replace_string**. When the value is **1**, all matched substrings are replaced. If the value is of another type or is less than 1, an error is reported. The default value is **1**. |
   |                 |                 |                 |                                                                                                                                                                                                                                                                                                                          |
   |                 |                 |                 | .. note::                                                                                                                                                                                                                                                                                                                |
   |                 |                 |                 |                                                                                                                                                                                                                                                                                                                          |
   |                 |                 |                 |    This parameter is available only when Spark 3.1.1 is used.                                                                                                                                                                                                                                                            |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If the value of **pattern** is an empty string or there is no group in **pattern**, an error is reported.
   -  If a group that does not exist is referenced, the group is not replaced.
   -  If the value of **replace_string** is **NULL** and the pattern is matched, **NULL** is returned.
   -  If the value of **replace_string** is **NULL** but the pattern is not matched, **NULL** is returned.
   -  If the value of **source**, **pattern**, or **occurrence** is **NULL**, **NULL** is returned.

Example Code
------------

-  For Spark 2.4.5 or earlier

   The value **num-num** is returned.

   .. code-block::

      SELECT regexp_replace('100-200', '(\\d+)', 'num');

-  For Spark 3.1.1

   The value **2222** is returned.

   .. code-block::

      select regexp_replace('abcd', '[a-z]', '2');

   The value **2222** is returned.

   .. code-block::

      select regexp_replace('abcd', '[a-z]', '2', 1);

   The value **a222** is returned.

   .. code-block::

      select regexp_replace('abcd', '[a-z]', '2', 2);

   The value **ab22** is returned.

   .. code-block::

      select regexp_replace('abcd', '[a-z]', '2', 3);

   The value **abc2** is returned.

   .. code-block::

      select regexp_replace('abcd', '[a-z]', '2', 4);
