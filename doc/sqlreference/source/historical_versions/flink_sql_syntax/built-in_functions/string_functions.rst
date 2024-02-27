:original_name: dli_08_0096.html

.. _dli_08_0096:

String Functions
================

The common string functions of DLI are as follows:

.. table:: **Table 1** String operators

   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Operator                                                     | Returned Data Type    | Description                                                                                                                                                                                    |
   +==============================================================+=======================+================================================================================================================================================================================================+
   | :ref:`|| <dli_08_0096__section9727458134419>`                | VARCHAR               | Concatenates two strings.                                                                                                                                                                      |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`CHAR_LENGTH <dli_08_0096__section511503164915>`        | INT                   | Returns the number of characters in a string.                                                                                                                                                  |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`CHARACTER_LENGTH <dli_08_0096__section18626101155017>` | INT                   | Returns the number of characters in a string.                                                                                                                                                  |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`CONCAT <dli_08_0096__section1941511592278>`            | VARCHAR               | Concatenates two or more string values to form a new string. If the value of any parameter is **NULL**, skip this parameter.                                                                   |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`CONCAT_WS <dli_08_0096__section858291242912>`          | VARCHAR               | Concatenates each parameter value and the separator specified by the first parameter separator to form a new string. The length and type of the new string depend on the input value.          |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`HASH_CODE <dli_08_0096__section089042532319>`          | INT                   | Returns the absolute value of **HASH_CODE()** of a string. In addition to **string**, **int**, **bigint**, **float**, and **double** are also supported.                                       |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`INITCAP <dli_08_0096__section870614420584>`            | VARCHAR               | Returns a string whose first letter is in uppercase and the other letters in lowercase. Words are sequences of alphanumeric characters separated by non-alphanumeric characters.               |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`IS_ALPHA <dli_08_0096__section114504453324>`           | BOOLEAN               | Checks whether a string contains only letters.                                                                                                                                                 |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`IS_DIGITS <dli_08_0096__section11496210113613>`        | BOOLEAN               | Checks whether a string contains only digits.                                                                                                                                                  |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`IS_NUMBER <dli_08_0096__section35411640163419>`        | BOOLEAN               | Checks whether a string is numeric.                                                                                                                                                            |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`IS_URL <dli_08_0096__section229175583720>`             | BOOLEAN               | Checks whether a string is a valid URL.                                                                                                                                                        |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`JSON_VALUE <dli_08_0096__section10302139205814>`       | VARCHAR               | Obtains the value of a specified path in a JSON string.                                                                                                                                        |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`KEY_VALUE <dli_08_0096__section281262819297>`          | VARCHAR               | Obtains the value of a key in a key-value pair string.                                                                                                                                         |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`LOWER <dli_08_0096__section1558193715588>`             | VARCHAR               | Returns a string of lowercase characters.                                                                                                                                                      |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`LPAD <dli_08_0096__section46291824203214>`             | VARCHAR               | Concatenates the pad string to the left of the str string until the length of the new string reaches the specified length len.                                                                 |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`MD5 <dli_08_0096__section5579173518713>`               | VARCHAR               | Returns the MD5 value of a string. If the parameter is an empty string (that is, the parameter is **"**), an empty string is returned.                                                         |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`OVERLAY <dli_08_0096__section16107161311363>`          | VARCHAR               | Replaces the substring of **x** with **y**. Replace length+1 characters starting from **start_position**.                                                                                      |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`POSITION <dli_08_0096__section174871931132317>`        | INT                   | Returns the position of the first occurrence of the target string **x** in the queried string **y**. If the target string **x** does not exist in the queried string **y**, **0** is returned. |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`REPLACE <dli_08_0096__section1427510535106>`           | VARCHAR               | Replaces all **str2** in the **str1** string with **str3**.                                                                                                                                    |
   |                                                              |                       |                                                                                                                                                                                                |
   |                                                              |                       | -  **str1**: original character.                                                                                                                                                               |
   |                                                              |                       | -  **str2**: target character.                                                                                                                                                                 |
   |                                                              |                       | -  **str3**: replacement character.                                                                                                                                                            |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`RPAD <dli_08_0096__section24420324307>`                | VARCHAR               | Concatenates the pad string to the right of the str string until the length of the new string reaches the specified length len.                                                                |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`SHA1 <dli_08_0096__section595513321186>`               | STRING                | Returns the SHA1 value of the **expr** string.                                                                                                                                                 |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`SHA256 <dli_08_0096__section1742102714911>`            | STRING                | Returns the SHA256 value of the **expr** string.                                                                                                                                               |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`STRING_TO_ARRAY <dli_08_0096__section2011913586245>`   | ARRAY[STRING]         | Separates the **value** string as string arrays by using the delimiter.                                                                                                                        |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`SUBSTRING <dli_08_0096__section4366645154114>`         | VARCHAR               | Returns the substring starting from a fixed position of A. The start position starts from 1.                                                                                                   |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`TRIM <dli_08_0096__section84703469261>`                | STRING                | Removes A at the start position, or end position, or both the start and end positions from B. By default, string expressions A at both the start and end positions are removed.                |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`UPPER <dli_08_0096__section19635513115615>`            | VARCHAR               | Returns a string converted to uppercase characters.                                                                                                                                            |
   +--------------------------------------------------------------+-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_08_0096__section9727458134419:

\|\|
----

-  Function

   Concatenates two strings.

-  Syntax

   .. code-block::

      VARCHAR VARCHAR a || VARCHAR b

-  Parameters

   -  **a**: string.
   -  **b**: string.

-  Example

   -  Test statement

      .. code-block::

         SELECT "hello" || "world";

   -  Test result

      .. code-block::

         "helloworld"

.. _dli_08_0096__section511503164915:

CHAR_LENGTH
-----------

-  Function

   Returns the number of characters in a string.

-  Syntax

   .. code-block::

      INT CHAR_LENGTH(a)

-  Parameters

   -  **a**: string.

-  Example

   -  Test statement

      .. code-block::

         SELECT  CHAR_LENGTH(var1) as aa FROM T1;

   -  Test data and result

      .. table:: **Table 2** Test data and result

         ================ ================
         Test Data (var1) Test Result (aa)
         ================ ================
         abcde123         8
         ================ ================

.. _dli_08_0096__section18626101155017:

CHARACTER_LENGTH
----------------

-  Function

   Returns the number of characters in a string.

-  Syntax

   .. code-block::

      INT CHARACTER_LENGTH(a)

-  Parameters

   -  **a**: string.

-  Example

   -  Test statement

      .. code-block::

         SELECT  CHARACTER_LENGTH(var1) as aa FROM T1;

   -  Test data and result

      .. table:: **Table 3** Test data and result

         ================ ================
         Test Data (var1) Test Result (aa)
         ================ ================
         abcde123         8
         ================ ================

.. _dli_08_0096__section1941511592278:

CONCAT
------

-  Function

   Concatenates two or more string values to form a new string. If the value of any parameter is NULL, skip this parameter.

-  Syntax

   .. code-block::

      VARCHAR CONCAT(VARCHAR var1, VARCHAR var2, ...)

-  Parameters

   -  **var1**: string
   -  **var2**: string

-  Example

   -  Test statement

      .. code-block::

         SELECT CONCAT("abc", "def", "ghi", "jkl");

   -  Test result

      .. code-block::

         "abcdefghijkl"

.. _dli_08_0096__section858291242912:

CONCAT_WS
---------

-  Function

   Concatenates each parameter value and the separator specified by the first parameter separator to form a new string. The length and type of the new string depend on the input value.

   .. note::

      If the value of **separator** is **null**, **separator** is combined with an empty string. If other parameters are set to null, the parameters whose values are null are skipped during combination.

-  Syntax

   .. code-block::

      VARCHAR CONCAT_WS(VARCHAR separator, VARCHAR var1, VARCHAR var2, ...)

-  Parameters

   -  **separator**: separator.
   -  **var1**: string
   -  **var2**: string

-  Example

   -  Test statement

      .. code-block::

         SELECT CONCAT_WS("-", "abc", "def", "ghi", "jkl");

   -  Test result

      .. code-block::

         "abc-def-ghi-jkl"

.. _dli_08_0096__section089042532319:

HASH_CODE
---------

-  Function

   Returns the absolute value of **HASH_CODE()** of a string. In addition to **string**, **int**, **bigint**, **float**, and **double** are also supported.

-  Syntax

   .. code-block::

      INT HASH_CODE(VARCHAR str)

-  Parameters

   -  **str**: string.

-  Example

   -  Test statement

      .. code-block::

         SELECT HASH_CODE("abc");

   -  Test result

      .. code-block::

         96354

.. _dli_08_0096__section870614420584:

INITCAP
-------

-  Function

   Return the string whose first letter is in uppercase and the other letters in lowercase. Strings are sequences of alphanumeric characters separated by non-alphanumeric characters.

-  Syntax

   .. code-block::

      VARCHAR INITCAP(a)

-  Parameters

   -  **a**: string.

-  Example

   -  Test statement

      .. code-block::

         SELECT INITCAP(var1)as aa FROM T1;

   -  Test data and result

      .. table:: **Table 4** Test data and result

         ================ ================
         Test Data (var1) Test Result (aa)
         ================ ================
         aBCde            Abcde
         ================ ================

.. _dli_08_0096__section114504453324:

IS_ALPHA
--------

-  Function

   Checks whether a string contains only letters.

-  Syntax

   .. code-block::

      BOOLEAN IS_ALPHA(VARCHAR content)

-  Parameters

   -  **content**: Enter a string.

-  Example

   -  Test statement

      .. code-block::

         SELECT IS_ALPHA(content)  AS case_result FROM T1;

   -  Test data and results

      .. table:: **Table 5** Test data and results

         =================== =========================
         Test Data (content) Test Result (case_result)
         =================== =========================
         Abc                 true
         abc1#$              false
         null                false
         Empty string        false
         =================== =========================

.. _dli_08_0096__section11496210113613:

IS_DIGITS
---------

-  Function

   Checks whether a string contains only digits.

-  Syntax

   .. code-block::

      BOOLEAN IS_DIGITS(VARCHAR content)

-  Parameters

   -  **content**: Enter a string.

-  Example

   -  Test statement

      .. code-block::

         SELECT IS_DIGITS(content) AS case_result FROM T1;

   -  Test data and results

      .. table:: **Table 6** Test data and results

         =================== =========================
         Test Data (content) Test Result (case_result)
         =================== =========================
         78                  true
         78.0                false
         78a                 false
         null                false
         Empty string        false
         =================== =========================

.. _dli_08_0096__section35411640163419:

IS_NUMBER
---------

-  Function

   This function is used to check whether a string is a numeric one.

-  Syntax

   .. code-block::

      BOOLEAN IS_NUMBER(VARCHAR content)

-  Parameters

   -  **content**: Enter a string.

-  Example

   -  Test statement

      .. code-block::

         SELECT IS_NUMBER(content) AS case_result FROM T1;

   -  Test data and results

      .. table:: **Table 7** Test data and results

         =================== =========================
         Test Data (content) Test Result (case_result)
         =================== =========================
         78                  true
         78.0                true
         78a                 false
         null                false
         Empty string        false
         =================== =========================

.. _dli_08_0096__section229175583720:

IS_URL
------

-  Function

   This function is used to check whether a string is a valid URL.

-  Syntax

   .. code-block::

      BOOLEAN IS_URL(VARCHAR content)

-  Parameters

   -  **content**: Enter a string.

-  Example

   -  Test statement

      .. code-block::

         SELECT IS_URL(content) AS case_result FROM T1;

   -  Test data and results

      .. table:: **Table 8** Test data and results

         =========================== =========================
         Test Data (content)         Test Result (case_result)
         =========================== =========================
         https://www.testweb.com     true
         https://www.testweb.com:443 true
         www.testweb.com:443         false
         null                        false
         Empty string                false
         =========================== =========================

.. _dli_08_0096__section10302139205814:

JSON_VALUE
----------

-  Function

   Obtains the value of a specified path in a JSON string.

-  Syntax

   .. code-block::

      VARCHAR JSON_VALUE(VARCHAR content, VARCHAR path)

-  Parameters

   -  **content**: Enter a string.
   -  **path**: path to be obtained.

-  Example

   -  Test statement

      .. code-block::

         SELECT JSON_VALUE(content, path) AS case_result FROM T1;

   -  Test data and results

      .. table:: **Table 9** Test data and results

         +---------------------------------------------------------------------+-------------+---------------------------------------------------------------------+
         | Test Data (content and path)                                        |             | Test Result (case_result)                                           |
         +=====================================================================+=============+=====================================================================+
         | { "a1":"v1","a2":7,"a3":8.0,"a4": {"a41":"v41","a42": ["v1","v2"]}} | $           | { "a1":"v1","a2":7,"a3":8.0,"a4": {"a41":"v41","a42": ["v1","v2"]}} |
         +---------------------------------------------------------------------+-------------+---------------------------------------------------------------------+
         | { "a1":"v1","a2":7,"a3":8.0,"a4": {"a41":"v41","a42": ["v1","v2"]}} | $.a1        | v1                                                                  |
         +---------------------------------------------------------------------+-------------+---------------------------------------------------------------------+
         | { "a1":"v1","a2":7,"a3":8.0,"a4": {"a41":"v41","a42": ["v1","v2"]}} | $.a4        | {"a41":"v41","a42": ["v1","v2"]}                                    |
         +---------------------------------------------------------------------+-------------+---------------------------------------------------------------------+
         | { "a1":"v1","a2":7,"a3":8.0,"a4": {"a41":"v41","a42": ["v1","v2"]}} | $.a4.a42    | ["v1","v2"]                                                         |
         +---------------------------------------------------------------------+-------------+---------------------------------------------------------------------+
         | { "a1":"v1","a2":7,"a3":8.0,"a4": {"a41":"v41","a42": ["v1","v2"]}} | $.a4.a42[0] | v1                                                                  |
         +---------------------------------------------------------------------+-------------+---------------------------------------------------------------------+

.. _dli_08_0096__section281262819297:

KEY_VALUE
---------

-  Function

   This function is used to obtain the value of a key in a key-value pair string.

-  Syntax

   .. code-block::

      VARCHAR KEY_VALUE(VARCHAR content, VARCHAR split1, VARCHAR split2, VARCHAR key_name)

-  Parameters

   -  **content**: Enter a string.
   -  **split1**: separator of multiple key-value pairs.
   -  **split2**: separator between the key and value.
   -  **key_name**: name of the key to be obtained.

-  Example

   -  Test statement

      .. code-block::

         SELECT KEY_VALUE(content, split1, split2, key_name)  AS case_result FROM T1;

   -  Test data and results

      .. table:: **Table 10** Test data and results

         +---------------------------------------------------+------+---+----+---------------------------+
         | Test Data (content, split1, split2, and key_name) |      |   |    | Test Result (case_result) |
         +===================================================+======+===+====+===========================+
         | k1=v1;k2=v2                                       | ;    | = | k1 | v1                        |
         +---------------------------------------------------+------+---+----+---------------------------+
         | null                                              | ;    | = | k1 | null                      |
         +---------------------------------------------------+------+---+----+---------------------------+
         | k1=v1;k2=v2                                       | null | = | k1 | null                      |
         +---------------------------------------------------+------+---+----+---------------------------+

.. _dli_08_0096__section1558193715588:

LOWER
-----

-  Function

   Returns a string of lowercase characters.

-  Syntax

   .. code-block::

      VARCHAR LOWER(A)

-  Parameters

   -  **A**: string.

-  Example

   -  Test statement

      .. code-block::

         SELECT LOWER(var1) AS aa FROM T1;

   -  Test data and result

      .. table:: **Table 11** Test data and result

         ================ ================
         Test Data (var1) Test Result (aa)
         ================ ================
         ABc              abc
         ================ ================

.. _dli_08_0096__section46291824203214:

LPAD
----

-  Function

   Concatenates the pad string to the left of the str string until the length of the new string reaches the specified length len.

-  Syntax

   .. code-block::

      VARCHAR LPAD(VARCHAR str, INT len, VARCHAR pad)

-  Parameters

   -  **str**: string before concatenation.
   -  **len**: length of the concatenated string.
   -  **pad**: string to be concatenated.

   .. note::

      -  If any parameter is null, **null** is returned.
      -  If the value of len is a negative number, value **null** is returned.
      -  If the value of **len** is less than the length of **str**, the first chunk of **str** characters in **len** length is returned.

-  Example

   -  Test statement

      .. code-block::

         SELECT
           LPAD("adc", 2, "hello"),
           LPAD("adc", -1, "hello"),
           LPAD("adc", 10, "hello");

   -  Test result

      .. code-block::

         "ad",,"helloheadc"

.. _dli_08_0096__section5579173518713:

MD5
---

-  Function

   Returns the MD5 value of a string. If the parameter is an empty string (that is, the parameter is **"**), an empty string is returned.

-  Syntax

   .. code-block::

      VARCHAR MD5(VARCHAR str)

-  Parameters

   -  **str**: string

-  Example

   -  Test statement

      .. code-block::

         SELECT MD5("abc");

   -  Test result

      .. code-block::

         "900150983cd24fb0d6963f7d28e17f72"

.. _dli_08_0096__section16107161311363:

OVERLAY
-------

-  Function

   Replaces the substring of **x** with **y**. Replaces length+1 characters starting from **start_position**.

-  Syntax

   .. code-block::

      VARCHAR OVERLAY ( (VARCHAR x PLACING VARCHAR y FROM INT start_position [ FOR INT length ]) )

-  Parameters

   -  **x**: string.
   -  **y**: string.
   -  **start_position**: start position.
   -  **length (optional)**: indicates the character length.

-  Example

   -  Test statement

      .. code-block::

         OVERLAY('abcdefg' PLACING 'xyz' FROM 2 FOR 2) AS result FROM T1;

   -  Test result

      .. table:: **Table 12** Test result

         +----------+
         | result   |
         +==========+
         | axyzdefg |
         +----------+

.. _dli_08_0096__section174871931132317:

POSITION
--------

-  Function

   Returns the position of the first occurrence of the target string **x** in the queried string **y**. If the target string **x** does not exist in the queried string **y**, **0** is returned.

-  Syntax

   .. code-block::

      INTEGER POSITION(x IN y)

-  Parameters

   -  **x**: string
   -  **y**: string.

-  Example

   -  Test statement

      .. code-block::

         POSITION('in' IN 'chin') AS result FROM T1;

   -  Test result

      .. table:: **Table 13** Test result

         +--------+
         | result |
         +========+
         | 3      |
         +--------+

.. _dli_08_0096__section1427510535106:

REPLACE
-------

-  Function

   The string replacement function is used to replace all **str2** in the **str1** string with **str3**.

-  Syntax

   .. code-block::

      VARCHAR REPLACE(VARCHAR str1, VARCHAR str2, VARCHAR str3)

-  Parameters

   -  **str1**: original character.
   -  **str2**: target character.
   -  **str3**: replacement character.

-  Example

   -  Test statement

      .. code-block::

         SELECT
           replace(
             "hello world hello world hello world",
             "world",
             "hello"
           );

   -  Test result

      .. code-block::

         "hello hello hello hello hello hello"

.. _dli_08_0096__section24420324307:

RPAD
----

-  Function

   Concatenates the pad string to the right of the str string until the length of the new string reaches the specified length len.

   -  If any parameter is null, **null** is returned.
   -  If the value of len is a negative number, value **null** is returned.
   -  The value of **pad** is an empty string. If the value of **len** is less than the length of **str**, the string whose length is the same as the length of **str** is returned.

-  Syntax

   .. code-block::

      VARCHAR RPAD(VARCHAR str, INT len, VARCHAR pad)

-  Parameters

   -  **str**: start string.
   -  **len**: length of the new string.
   -  **pad**: string that needs to be added repeatedly.

-  Example

   -  Test statement

      .. code-block::

         SELECT
           RPAD("adc", 2, "hello"),
           RPAD("adc", -1, "hello"),
           RPAD("adc", 10, "hello");

   -  Test result

      .. code-block::

         "ad",,"adchellohe"

.. _dli_08_0096__section595513321186:

SHA1
----

-  Function

   Returns the SHA1 value of the **expr** string.

-  Syntax

   .. code-block::

      STRING SHA1(STRING expr)

-  Parameters

   -  **expr**: string.

-  Example

   -  Test statement

      .. code-block::

         SELECT SHA1("abc");

   -  Test result

      .. code-block::

         "a9993e364706816aba3e25717850c26c9cd0d89d"

.. _dli_08_0096__section1742102714911:

SHA256
------

-  Function

   Returns the SHA256 value of the expr string.

-  Syntax

   .. code-block::

      STRING SHA256(STRING expr)

-  Parameters

   -  **expr**: string.

-  Example

   -  Test statement

      .. code-block::

         SELECT SHA256("abc");

   -  Test result

      .. code-block::

         "ba7816bf8f01cfea414140de5dae2223b00361a396177a9cb410ff61f20015ad"

.. _dli_08_0096__section2011913586245:

STRING_TO_ARRAY
---------------

-  Function

   Separates the **value** string as string arrays by using the delimiter.

   .. note::

      **delimiter** uses the Java regular expression. If special characters are used, they need to be escaped.

-  Syntax

   .. code-block::

      ARRAY[String] STRING_TO_ARRAY(STRING value, VARCHAR delimiter)

-  Parameters

   -  **value**: string.
   -  **delimiter**: delimiter.

-  Example

   -  Test statement

      .. code-block::

         SELECT
           string_to_array("127.0.0.1", "\\."),
           string_to_array("red-black-white-blue", "-");

   -  Test result

      .. code-block::

         [127,0,0,1],[red,black,white,blue]

.. _dli_08_0096__section4366645154114:

SUBSTRING
---------

-  Function

   Returns the substring that starts from a fixed position of A. The start position starts from 1.

   -  If **len** is not specified, the substring from the start position to the end of the string is truncated.
   -  If **len** is specified, the substring starting from the position specified by **start** is truncated. The length is specified by **len**.

   .. note::

      The value of **start** starts from **1**. If the value is **0**, it is regarded as **1**. If the value of start is a negative number, the position is calculated from the end of the string in reverse order.

-  Syntax

   .. code-block::

      VARCHAR SUBSTRING(STRING A FROM INT start)

   Or

   .. code-block::

      VARCHAR SUBSTRING(STRING A FROM INT start FOR INT len)

-  Parameters

   -  **A**: specified string.
   -  **start**: start position for truncating the string **A**.
   -  **len**: intercepted length.

-  Example

   -  Test statement 1

      .. code-block::

         SELECT SUBSTRING("123456" FROM 2);

   -  Test result 1

      .. code-block::

         "23456"

   -  Test statement 2

      .. code-block::

         SELECT SUBSTRING("123456" FROM 2 FOR 4);

   -  Test result 2

      .. code-block::

         "2345"

.. _dli_08_0096__section84703469261:

TRIM
----

-  Function

   Remove A at the start position, or end position, or both the start and end positions from B. By default, string expressions A at both the start and end positions are removed.

-  Syntax

   .. code-block::

      STRING TRIM( { BOTH | LEADING | TRAILING } STRING a FROM STRING b)

-  Parameters

   -  **a**: string.
   -  **b**: string.

-  Example

   -  Test statement

      .. code-block::

         SELECT TRIM(BOTH " " FROM "  hello world  ");

   -  Test result

      .. code-block::

         "hello world"

.. _dli_08_0096__section19635513115615:

UPPER
-----

-  Function

   Returns a string converted to an uppercase character.

-  Syntax

   .. code-block::

      VARCHAR UPPER(A)

-  Parameters

   -  **A**: string.

-  Example

   -  Test statement

      .. code-block::

         SELECT UPPER("hello world");

   -  Test result

      .. code-block::

         "HELLO WORLD"
