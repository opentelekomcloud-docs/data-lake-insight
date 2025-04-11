:original_name: dli_08_15089.html

.. _dli_08_15089:

String Functions
================


String Functions
----------------

DLI offers a wide range of string functions for processing and transforming string data. These functions include concatenation, case conversion, substring extraction, replacement, regex matching, encoding and decoding, format conversion, and more. Additionally, it supports string length calculation, position searching, padding, reversing, and even extracting values from JSON strings using the **JSON_VAL** function. These features are widely used in data cleansing, text processing, and data analysis scenarios, providing developers with powerful tool support.

For detailed string functions, see :ref:`Table 1 <dli_08_15089__table157276446018>`. For more information, see `Apache Flink <https://nightlies.apache.org/flink>`__.

.. _dli_08_15089__table157276446018:

.. table:: **Table 1** String functions

   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SQL Function                                                    | Description                                                                                                                                                                                                                                       |
   +=================================================================+===================================================================================================================================================================================================================================================+
   | string1 \|\| string2                                            | Returns the concatenation of **STRING1** and **STRING2**.                                                                                                                                                                                         |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | CHAR_LENGTH(string) CHARACTER_LENGTH(string)                    | Returns the number of characters in a string.                                                                                                                                                                                                     |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | UPPER(string)                                                   | Returns a string in uppercase.                                                                                                                                                                                                                    |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | LOWER(string)                                                   | Returns a string in lowercase.                                                                                                                                                                                                                    |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | POSITION(string1 IN string2)                                    | Returns the position (starting from 1) of the first occurrence of **STRING1** in **STRING2**.                                                                                                                                                     |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | Returns **0** if **STRING1** is not found in **STRING2**.                                                                                                                                                                                         |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | TRIM([ BOTH \| LEADING \| TRAILING ] string1 FROM string2)      | Returns the result of removing the string that starts/ends/starts and ends with **STRING2** from **STRING1**. By default, both sides' spaces will be removed.                                                                                     |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | LTRIM(string)                                                   | Returns the string with left spaces removed from **STRING**.                                                                                                                                                                                      |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | For example, **' This is a test String.'.ltrim()** returns **'This is a test String.'**.                                                                                                                                                          |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | RTRIM(string)                                                   | Returns the string with right spaces removed from **STRING**.                                                                                                                                                                                     |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | For example, **'This is a test String. '.ltrim()** returns **'This is a test String.'**.                                                                                                                                                          |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | REPEAT(string, int)                                             | Returns a string that is the concatenation of INT number of strings.                                                                                                                                                                              |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | For example, **REPEAT('This is a test String.', 2)** returns **"This is a test String.This is a test String."**.                                                                                                                                  |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | REGEXP_REPLACE(string1, string2, string3)                       | Returns a string where all substrings in **STRING1** that match the regular expression **STRING2** are replaced with **STRING3**.                                                                                                                 |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | For example, **'foobar'.regexpReplace('oo|ar', '')** returns **"fb"**.                                                                                                                                                                            |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | OVERLAY(string1 PLACING string2 FROM integer1 [ FOR integer2 ]) | Returns a string that replaces **INT2** (STRING2's length by default) characters of **STRING1** with **STRING2** from position **INT1**.                                                                                                          |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | For example, **'xxxxxtest'.overlay('xxxx', 6)** returns **"xxxxxxxxx"**, and **'xxxxxtest'.overlay('xxxx', 6, 2)** returns **"xxxxxxxxxst"**.                                                                                                     |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SUBSTRING(string FROM integer1 [ FOR integer2 ])                | Returns a substring of **STRING** starting from position **INT1** with length **INT2** (default to the end).                                                                                                                                      |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | REPLACE(string1, string2, string3)                              | Returns a new string where all occurrences of **STRING2** in **STRING1** are replaced with **STRING3** (non-overlapping).                                                                                                                         |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | For example, **'hello world'.replace('world', 'flink')** returns **'hello flink'**; **'ababab'.replace('abab', 'z')** returns **'zab'**.                                                                                                          |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | REGEXP_EXTRACT(string1, string2[, integer])                     | Splits the string **STRING1** according to the regular expression rule **STRING2** and returns the string at the specified position **INTEGER1**.                                                                                                 |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | The regular expression match group index starts at 1, with 0 indicating the entire regular expression match. In addition, the regular expression match group index should not exceed the defined number of groups.                                |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | For example, **REGEXP_EXTRACT('foothebar', 'foo(.*?)(bar)', 2)** returns **"bar"**.                                                                                                                                                               |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | INITCAP(string)                                                 | Returns a new string where the first character of each word is capitalized and the rest are lowercase. Here, a word is defined as a sequence of alphanumeric characters.                                                                          |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | CONCAT(string1, string2, ...)                                   | Returns a string that concatenates string1, string2, ..., together. If any parameter is **NULL**, **NULL** is returned.                                                                                                                           |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | For example, **CONCAT('AA', 'BB', 'CC')** returns **"AABBCC"**.                                                                                                                                                                                   |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | CONCAT_WS(string1, string2, string3, ...)                       | Returns a string that concatenates **STRING2**, **STRING3**, ..., together with the separator **STRING1**.                                                                                                                                        |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | A separator is added between each string to be concatenated.                                                                                                                                                                                      |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | If **STRING1** is **NULL**, **NULL** is returned.                                                                                                                                                                                                 |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | Compared to **concat()**, **concat_ws()** automatically skips NULL parameters.                                                                                                                                                                    |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | For example, **concat_ws('~', 'AA', Null(STRING), 'BB', '', 'CC')** returns **"AA~BB~~CC"**.                                                                                                                                                      |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | LPAD(string1, integer, string2)                                 | Returns a new string where **string2** is left-padded to the length of **INT**.                                                                                                                                                                   |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | If the length of **string1** is less than the value of **INT**, **string1** is returned shortened to an integer character.                                                                                                                        |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | For example, **LPAD('hi', 4, '??')** returns **"??hi"**; **LPAD('hi', 1, '??')** returns **"h"**.                                                                                                                                                 |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | RPAD(string1, integer, string2)                                 | Returns a new string where **string2** is right-padded to the length of **INT**.                                                                                                                                                                  |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | If the length of **string1** is less than the value of **INT**, returns a new string where **string1** is shortened to a length of **INT**.                                                                                                       |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | For example, **RPAD('hi', 4, '??')** returns **"hi??"**; **RPAD('hi', 1, '??')** returns **"h"**.                                                                                                                                                 |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | FROM_BASE64(string)                                             | Returns the result of decoding the base64-encoded **string1**. If the string is **NULL**, **NULL** is returned.                                                                                                                                   |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | For example, **FROM_BASE64('aGVsbG8gd29ybGQ=')** returns **"hello world"**.                                                                                                                                                                       |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | TO_BASE64(string)                                               | Returns the result of encoding the string to base64. If the string is **NULL**, **NULL** is returned.                                                                                                                                             |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | For example, **TO_BASE64('hello world')** returns **"aGVsbG8gd29ybGQ="**.                                                                                                                                                                         |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ASCII(string)                                                   | Returns the numeric value of the first character in the string. If the string is **NULL**, **NULL** is returned.                                                                                                                                  |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | For example, **ascii('abc')** returns **97**, and **ascii(CAST(NULL AS VARCHAR))** returns **NULL**.                                                                                                                                              |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | CHR(integer)                                                    | Returns the ASCII character that corresponds to the binary value of the **integer**.                                                                                                                                                              |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | If the integer is greater than 255, we first take the modulo of the integer with 255 and return the CHR of the modulo.                                                                                                                            |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | If the integer is **NULL**, **NULL** is returned.                                                                                                                                                                                                 |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | For example, **chr(97) returns 'a', chr(353)** returns **'a'**, and **chr(CAST(NULL AS VARCHAR))** returns **NULL**.                                                                                                                              |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | DECODE(binary, string)                                          | Decodes using the provided character set ('US-ASCII', 'ISO-8859-1', 'UTF-8', 'UTF-16BE', 'UTF-16LE', 'UTF-16'). If any of the parameters are empty, the result will also be empty.                                                                |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ENCODE(string1, string2)                                        | Encodes using the provided character set ('US-ASCII', 'ISO-8859-1', 'UTF-8', 'UTF-16BE', 'UTF-16LE', 'UTF-16'). If any of the parameters are empty, the result will also be empty.                                                                |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | INSTR(string1, string2)                                         | Returns the position of the first occurrence of **string2** in **string1**. Returns **NULL** if the value of any parameter is **NULL**.                                                                                                           |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | LEFT(string, integer)                                           | Returns the leftmost substring of the string with a length equal to the **integer** value. If the **integer** is negative, an empty string is returned. Returns **NULL** if the value of any parameter is **NULL**.                               |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | RIGHT(string, integer)                                          | Returns the rightmost substring of the string with a length equal to the **integer** value. If the **integer** is negative, an empty string is returned. Returns **NULL** if the value of any parameter is **NULL**.                              |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | LOCATE(string1, string2[, integer])                             | Returns the position of the first occurrence of **string1** after position **integer** in **string2**. If not found, returns **0**. Returns **NULL** if either parameter is **NULL**.                                                             |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | PARSE_URL(string1, string2[, string3])                          | Returns a specified part from a URL. The valid values for **string2** include **"HOST"**, **"PATH"**, **"QUERY"**, **"REF"**, **"PROTOCOL"**, **"AUTHORITY"**, **"FILE"**, and **"USERINFO"**.                                                    |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | Returns **NULL** if the value of any parameter is **NULL**.                                                                                                                                                                                       |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | For example, **parse_url('http://facebook.com/path1/p.php?k1=v1&k2=v2#Ref1', 'HOST')** returns **'facebook.com'**.                                                                                                                                |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | You can also extract the value of a specific key in the QUERY by providing a keyword **string3** as the third parameter.                                                                                                                          |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | For example, **parse_url('http://facebook.com/path1/p.php?k1=v1&k2=v2#Ref1', 'QUERY', 'k1')** returns **'v1'**.                                                                                                                                   |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | REGEXP(string1, string2)                                        | Returns **TRUE** if any (possibly empty) substring of **string1** matches the Java regular expression **string2**, otherwise it returns **FALSE**. Returns **NULL** if the value of any parameter is **NULL**.                                    |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | REVERSE(string)                                                 | Returns the reversed string. If the string is **NULL**, returns **NULL**.                                                                                                                                                                         |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SPLIT_INDEX(string1, string2, integer1)                         | Splits **string1** by the delimiter **string2** and returns the integer-th (starting from zero) split string. If the integer is negative, returns **NULL**. Returns **NULL** if the value of any parameter is **NULL**.                           |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | STR_TO_MAP(string1[, string2, string3])                         | Splits **string1** into key-value pairs using a separator and returns a map. **string2** is the pair separator, and the default separator is a comma (,). **string3** is the key-value separator, and the default separator is an equal sign (=). |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | Both separators are regular expressions, so special characters should be escaped beforehand, such as **<([{\\^-=$!|]})?*+.>**.                                                                                                                    |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SUBSTR(string[, integer1[, integer2]])                          | Returns a substring of a string starting from position **integer1** with a length of **integer2** (default to the end).                                                                                                                           |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | JSON_VAL(STRING json_string, STRING json_path)                  | Returns the value of the specified **json_path** from the **json_string**. For details about how to use the functions, see :ref:`JSON_VAL Function <dli_08_15089__section624613301257>`.                                                          |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 | .. note::                                                                                                                                                                                                                                         |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 |    The following rules are listed in descending order of priority.                                                                                                                                                                                |
   |                                                                 |                                                                                                                                                                                                                                                   |
   |                                                                 |    #. The two arguments **json_string** and **json_path** cannot be **NULL**.                                                                                                                                                                     |
   |                                                                 |    #. The value of **json_string** must be a valid JSON string. Otherwise, the function returns **NULL**.                                                                                                                                         |
   |                                                                 |    #. If **json_string** is an empty string, the function returns an empty string.                                                                                                                                                                |
   |                                                                 |    #. If **json_path** is an empty string or the path does not exist, the function returns **NULL**.                                                                                                                                              |
   +-----------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _dli_08_15089__section624613301257:

JSON_VAL Function
-----------------

-  Syntax

.. code-block::

   STRING JSON_VAL(STRING json_string, STRING json_path)

.. table:: **Table 2** Parameters

   +-------------+------------+----------------------------------------------------------------------------------------------------------------------------------+
   | Parameter   | Data Types | Description                                                                                                                      |
   +=============+============+==================================================================================================================================+
   | json_string | STRING     | JSON object to be parsed                                                                                                         |
   +-------------+------------+----------------------------------------------------------------------------------------------------------------------------------+
   | json_path   | STRING     | Path expression for parsing the JSON string For the supported expressions, see :ref:`Table 3 <dli_08_15089__table147467547297>`. |
   +-------------+------------+----------------------------------------------------------------------------------------------------------------------------------+

.. _dli_08_15089__table147467547297:

.. table:: **Table 3** Expressions supported

   ========== =====================
   Expression Description
   ========== =====================
   $          Root node in the path
   []         Access array elements
   \*         Array wildcard
   .          Access child elements
   ========== =====================

-  Example

   #. Test input data.

      Test the data source kafka. The message content is as follows:

      .. code-block::

         {"name":"James","age":24,"gender":"male","grade":{"math":95,"science":[80,85],"english":100}}

   #. Use JSON_VAL in SQL statements.

      .. code-block::

         CREATE TABLE kafkaSource (
           message string
         ) WITH (
           'connector' = 'kafka',
           'topic-pattern' = '<yourSinkTopic>',
           'properties.bootstrap.servers' = '<yourKafkaAddress1>:<yourKafkaPort>,<yourKafkaAddress2>:<yourKafkaPort>',
           'properties.group.id' = '<yourGroupId>',
           'scan.startup.mode' = 'latest-offset',
           'format' = 'csv',
           'csv.field-delimiter' = '\u0001',
           'csv.quote-character' = ''''
         );


         CREATE TABLE printSink (
           message1 STRING,
           message2 STRING,
           message3 STRING,
           message4 STRING,
           message5 STRING,
           message6 STRING
         ) WITH (
           'connector' = 'print'
         );
         insert into printSink select
         JSON_VAL(message,''),
         JSON_VAL(message,'$.name'),
         JSON_VAL(message,'$.grade.science'),
         JSON_VAL(message,'$.grade.science[*]'),
         JSON_VAL(message,'$.grade.science[1]'),
         JSON_VAL(message,'$.grade.dddd')
         from kafkaSource;

   #. Check the output of the **out** file of the taskmanager.

      .. code-block::

         +I[null, James, [80,85], [80,85], 85, null]
