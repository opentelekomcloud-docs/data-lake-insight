:original_name: dli_08_0058.html

.. _dli_08_0058:

Primitive Data Types
====================

:ref:`Table 1 <dli_08_0058__en-us_topic_0093946969_t8554599ebef94ea49cef6d24756f2cbf>` lists the primitive data types supported by DLI.

.. _dli_08_0058__en-us_topic_0093946969_t8554599ebef94ea49cef6d24756f2cbf:

.. table:: **Table 1** Primitive data types

   +--------------------------+---------------------------------------------------------------------------------------+---------------+-----------------------------------------------------------------------------------------------------+----------------------+----------------------+
   | Data Type                | Description                                                                           | Storage Space | Value Range                                                                                         | Support by OBS Table | Support by DLI Table |
   +==========================+=======================================================================================+===============+=====================================================================================================+======================+======================+
   | INT                      | Signed integer                                                                        | 4 bytes       | -2147483648 to 2147483647                                                                           | Yes                  | Yes                  |
   +--------------------------+---------------------------------------------------------------------------------------+---------------+-----------------------------------------------------------------------------------------------------+----------------------+----------------------+
   | STRING                   | String                                                                                | ``-``         | ``-``                                                                                               | Yes                  | Yes                  |
   +--------------------------+---------------------------------------------------------------------------------------+---------------+-----------------------------------------------------------------------------------------------------+----------------------+----------------------+
   | FLOAT                    | Single-precision floating point                                                       | 4 bytes       | ``-``                                                                                               | Yes                  | Yes                  |
   +--------------------------+---------------------------------------------------------------------------------------+---------------+-----------------------------------------------------------------------------------------------------+----------------------+----------------------+
   | DOUBLE                   | Double-precision floating-point                                                       | 8 bytes       | ``-``                                                                                               | Yes                  | Yes                  |
   +--------------------------+---------------------------------------------------------------------------------------+---------------+-----------------------------------------------------------------------------------------------------+----------------------+----------------------+
   | DECIMAL(precision,scale) | Decimal number. Data type of valid fixed places and decimal places, for example, 3.5. | ``-``         | 1<=precision<=38                                                                                    | Yes                  | Yes                  |
   |                          |                                                                                       |               |                                                                                                     |                      |                      |
   |                          | -  **precision**: indicates the maximum number of digits that can be displayed.       |               | 0<=scale<=38                                                                                        |                      |                      |
   |                          | -  **scale**: indicates the number of decimal places.                                 |               |                                                                                                     |                      |                      |
   |                          |                                                                                       |               | If **precision** and **scale** are not specified, **DECIMAL (38,38)** is used by default.           |                      |                      |
   +--------------------------+---------------------------------------------------------------------------------------+---------------+-----------------------------------------------------------------------------------------------------+----------------------+----------------------+
   | BOOLEAN                  | Boolean                                                                               | 1 byte        | TRUE/FALSE                                                                                          | Yes                  | Yes                  |
   +--------------------------+---------------------------------------------------------------------------------------+---------------+-----------------------------------------------------------------------------------------------------+----------------------+----------------------+
   | SMALLINT/SHORT           | Signed integer                                                                        | 2 bytes       | -32768~32767                                                                                        | Yes                  | Yes                  |
   +--------------------------+---------------------------------------------------------------------------------------+---------------+-----------------------------------------------------------------------------------------------------+----------------------+----------------------+
   | TINYINT                  | Signed integer                                                                        | 1 byte        | -128~127                                                                                            | Yes                  | No                   |
   +--------------------------+---------------------------------------------------------------------------------------+---------------+-----------------------------------------------------------------------------------------------------+----------------------+----------------------+
   | BIGINT/LONG              | Signed integer                                                                        | 8 bytes       | -9223372036854775808 to 9223372036854775807                                                         | Yes                  | Yes                  |
   +--------------------------+---------------------------------------------------------------------------------------+---------------+-----------------------------------------------------------------------------------------------------+----------------------+----------------------+
   | TIMESTAMP                | Timestamp in raw data format, indicating the date and time Example: 1621434131222     | ``-``         | ``-``                                                                                               | Yes                  | Yes                  |
   +--------------------------+---------------------------------------------------------------------------------------+---------------+-----------------------------------------------------------------------------------------------------+----------------------+----------------------+
   | CHAR                     | Fixed-length string                                                                   | ``-``         | ``-``                                                                                               | Yes                  | Yes                  |
   +--------------------------+---------------------------------------------------------------------------------------+---------------+-----------------------------------------------------------------------------------------------------+----------------------+----------------------+
   | VARCHAR                  | Variable-length string                                                                | ``-``         | ``-``                                                                                               | Yes                  | Yes                  |
   +--------------------------+---------------------------------------------------------------------------------------+---------------+-----------------------------------------------------------------------------------------------------+----------------------+----------------------+
   | DATE                     | Date type in the format of **yyyy-mm-dd**, for example, **2014-05-29**                | ``-``         | **DATE** does not contain time information. Its value ranges from **0000-01-01** to **9999-12-31**. | Yes                  | Yes                  |
   +--------------------------+---------------------------------------------------------------------------------------+---------------+-----------------------------------------------------------------------------------------------------+----------------------+----------------------+

.. note::

   -  VARCHAR and CHAR data is stored in STRING type on DLI. Therefore, the string that exceeds the specified length will not be truncated.
   -  FLOAT data is stored as DOUBLE data on DLI.

INT
---

Signed integer with a storage space of 4 bytes. Its value ranges from -2147483648 to 2147483647. If this field is NULL, value 0 is used by default.

STRING
------

String.

FLOAT
-----

Single-precision floating point with a storage space of 4 bytes. If this field is NULL, value 0 is used by default.

Due to the limitation of storage methods of floating point data, do not use the formula a==b to check whether two floating point values are the same. You are advised to use the formula: absolute value of (a-b) <= EPSILON. EPSILON indicates the allowed error range which is usually 1.19209290E-07F. If the formula is satisfied, the compared two floating point values are considered the same.

DOUBLE
------

Double-precision floating point with a storage space of 8 bytes. If this field is NULL, value 0 is used by default.

Due to the limitation of storage methods of floating point data, do not use the formula a==b to check whether two floating point values are the same. You are advised to use the formula: absolute value of (a-b) <= EPSILON. EPSILON indicates the allowed error range which is usually 2.2204460492503131E-16. If the formula is satisfied, the compared two floating point values are considered the same.

DECIMAL
-------

Decimal(p,s) indicates that the total digit length is **p**, including **p - s** integer digits and **s** fractional digits. **p** indicates the maximum number of decimal digits that can be stored, including the digits to both the left and right of the decimal point. The value of **p** ranges from 1 to 38. **s** indicates the maximum number of decimal digits that can be stored to the right of the decimal point. The fractional digits must be values ranging from 0 to **p**. The fractional digits can be specified only after significant digits are specified. Therefore, the following inequality is concluded: 0 <= **s** <= **p**. For example, decimal (10,6) indicates that the value contains 10 digits, in which there are four integer digits and six fractional digits.

BOOLEAN
-------

Boolean, which can be **TRUE** or **FALSE**.

SMALLINT/SHORT
--------------

Signed integer with a storage space of 2 bytes. Its value ranges from -32768 to 32767. If this field is NULL, value 0 is used by default.

TINYINT
-------

Signed integer with a storage space of 1 byte. Its value ranges from -128 to 127. If this field is NULL, value 0 is used by default.

BIGINT/LONG
-----------

Signed integer with a storage space of 8 bytes. Its value ranges from -9223372036854775808 to 9223372036854775807. It does not support scientific notation. If this field is NULL, value 0 is used by default.

TIMESTAMP
---------

Legacy UNIX TIMESTAMP is supported, providing the precision up to the microsecond level. **TIMESTAMP** is defined by the difference between the specified time and UNIX epoch (UNIX epoch time: 1970-01-01 00:00:00) in seconds. The data type **STRING** can be implicitly converted to **TIMESTAMP**, but it must be in the **yyyy-MM-dd HH:mm:SS[.ffffff]** format. The precision after the decimal point is optional.)

CHAR
----

String with a fixed length. In DLI, the STRING type is used.

VARCHAR
-------

**VARCHAR** is declared with a length that indicates the maximum number of characters in a string. During conversion from **STRING** to **VARCHAR**, if the number of characters in **STRING** exceeds the specified length, the excess characters of **STRING** are automatically trimmed. Similar to **STRING**, the spaces at the end of **VARCHAR** are meaningful and affect the comparison result. In DLI, the STRING type is used.

DATE
----

**DATE** supports only explicit conversion (cast) with **DATE**, **TIMESTAMP**, and **STRING**. For details, see :ref:`Table 2 <dli_08_0058__en-us_topic_0093946969_t15e381680c464657923c440b88e59cb9>`.

.. _dli_08_0058__en-us_topic_0093946969_t15e381680c464657923c440b88e59cb9:

.. table:: **Table 2** cast function conversion

   +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Explicit Conversion     | Conversion Result                                                                                                                                                                                 |
   +=========================+===================================================================================================================================================================================================+
   | cast(date as date)      | Same as value of **DATE**.                                                                                                                                                                        |
   +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cast(timestamp as date) | The date (yyyy-mm-dd) is obtained from **TIMESTAMP** based on the local time zone and returned as the value of **DATE**.                                                                          |
   +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cast(string as date)    | If the STRING is in the **yyyy-MM-dd** format, the corresponding date (yyyy-mm-dd) is returned as the value of **DATE**. If the STRING is not in the **yyyy-MM-dd** format, **NULL** is returned. |
   +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cast(date as timestamp) | Timestamp that maps to the zero hour of the date (yyyy-mm-dd) specified by **DATE** is generated based on the local time zone and returned as the value of **DATE**.                              |
   +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cast(date as string)    | A STRING in the **yyyy-MM-dd** format is generated based on the date (yyyy-mm-dd) specified by **DATE** and returned as the value of **DATE**.                                                    |
   +-------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
