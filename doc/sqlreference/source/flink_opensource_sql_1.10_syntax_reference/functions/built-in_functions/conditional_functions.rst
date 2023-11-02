:original_name: dli_08_0335.html

.. _dli_08_0335:

Conditional Functions
=====================

Description
-----------

.. table:: **Table 1** Conditional functions

   +--------------------------------------------------+-------------------------------------------------------------------------------------------------+
   | Function                                         | Description                                                                                     |
   +==================================================+=================================================================================================+
   | CASE value                                       | Returns **resultX** when the value is contained in (valueX_1, valueX_2, …).                     |
   |                                                  |                                                                                                 |
   | WHEN value1_1 [, value1_2 ]\* THEN result1       | Only the first matched value is returned.                                                       |
   |                                                  |                                                                                                 |
   | [ WHEN value2_1 [, value2_2 ]\* THEN result2 ]\* | When no value matches, returns **resultZ** if it is provided and returns **NULL** otherwise.    |
   |                                                  |                                                                                                 |
   | [ ELSE resultZ ]                                 |                                                                                                 |
   |                                                  |                                                                                                 |
   | END                                              |                                                                                                 |
   +--------------------------------------------------+-------------------------------------------------------------------------------------------------+
   | CASE                                             | Returns **resultX** when the first **conditionX** is met.                                       |
   |                                                  |                                                                                                 |
   | WHEN condition1 THEN result1                     | Only the first matched value is returned.                                                       |
   |                                                  |                                                                                                 |
   | [ WHEN condition2 THEN result2 ]\*               | When no condition is met, returns **resultZ** if it is provided and returns **NULL** otherwise. |
   |                                                  |                                                                                                 |
   | [ ELSE resultZ ]                                 |                                                                                                 |
   |                                                  |                                                                                                 |
   | END                                              |                                                                                                 |
   +--------------------------------------------------+-------------------------------------------------------------------------------------------------+
   | NULLIF(value1, value2)                           | Returns NULL if value1 is equal to value2; returns value1 otherwise.                            |
   |                                                  |                                                                                                 |
   |                                                  | For example, **NullIF (5, 5)** returns **NULL**.                                                |
   |                                                  |                                                                                                 |
   |                                                  | **NULLIF(5, 0)** returns **5**.                                                                 |
   +--------------------------------------------------+-------------------------------------------------------------------------------------------------+
   | COALESCE(value1, value2 [, value3 ]\* )          | Returns the first value (from left to right) that is not NULL from value1, value2, ….           |
   |                                                  |                                                                                                 |
   |                                                  | For example, **COALESCE(NULL, 5)** returns **5**.                                               |
   +--------------------------------------------------+-------------------------------------------------------------------------------------------------+
   | IF(condition, true_value, false_value)           | Returns the **true_value** if condition is met, otherwise **false_value**.                      |
   |                                                  |                                                                                                 |
   |                                                  | For example, **IF(5 > 3, 5, 3)** returns **5**.                                                 |
   +--------------------------------------------------+-------------------------------------------------------------------------------------------------+
   | IS_ALPHA(string)                                 | Returns **TRUE** if all characters in the string are letters, otherwise **FALSE**.              |
   +--------------------------------------------------+-------------------------------------------------------------------------------------------------+
   | IS_DECIMAL(string)                               | Returns **TRUE** if string can be parsed to a valid numeric, otherwise **FALSE**.               |
   +--------------------------------------------------+-------------------------------------------------------------------------------------------------+
   | IS_DIGIT(string)                                 | Returns **TRUE** if all characters in the string are digits, otherwise **FALSE**.               |
   +--------------------------------------------------+-------------------------------------------------------------------------------------------------+
