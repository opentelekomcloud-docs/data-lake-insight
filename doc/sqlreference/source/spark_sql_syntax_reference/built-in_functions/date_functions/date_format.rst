:original_name: dli_spark_date_format.html

.. _dli_spark_date_format:

date_format
===========

This function is used to convert a date into a string based on the format specified by **format**.

Syntax
------

.. code-block::

   date_format(string date, string format)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                 |
   +=================+=================+=================+=============================================================================================================+
   | date            | Yes             | DATE            | Date to be converted                                                                                        |
   |                 |                 |                 |                                                                                                             |
   |                 |                 | or              | The following formats are supported:                                                                        |
   |                 |                 |                 |                                                                                                             |
   |                 |                 | STRING          | -  yyyy-mm-dd                                                                                               |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss                                                                                      |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss.ff3                                                                                  |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------+
   | format          | Yes             | STRING          | Format, based on which the date is converted                                                                |
   |                 |                 |                 |                                                                                                             |
   |                 |                 |                 | The value is a combination of the time unit (year, month, day, hour, minute, and second) and any character. |
   |                 |                 |                 |                                                                                                             |
   |                 |                 |                 | -  **yyyy** indicates the year.                                                                             |
   |                 |                 |                 | -  **MM** indicates the month.                                                                              |
   |                 |                 |                 | -  **dd** indicates the day.                                                                                |
   |                 |                 |                 | -  **HH** indicates the 24-hour clock.                                                                      |
   |                 |                 |                 | -  **hh** indicates the 12-hour clock.                                                                      |
   |                 |                 |                 | -  **mm** indicates the minute.                                                                             |
   |                 |                 |                 | -  **ss** indicates the second.                                                                             |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If the value of **date** is not of the DATE or STRING type, the error message "data type mismatch" is displayed.
   -  If the value of **date** is of the DATE or STRING type but is not in one of the supported formats, **NULL** is returned.
   -  If the value of **date** is **NULL**, **NULL** is returned.
   -  If the value of **format** is **NULL**, **NULL** is returned.

Example Code
------------

The value **2023-08-14** is returned.

.. code-block::

   select date_format('2023-08-14','yyyy-MM-dd');

The value **2023-08** is returned.

.. code-block::

   select date_format('2023-08-14','yyyy-MM')

The value **20230814** is returned.

.. code-block::

   select date_format('2023-08-14','yyyyMMdd')
