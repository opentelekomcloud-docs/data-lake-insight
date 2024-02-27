:original_name: dli_spark_to_date1.html

.. _dli_spark_to_date1:

to_date1
========

This function is used to convert a string in a specified format to a date value.

Similar function: :ref:`to_date <dli_spark_to_date>`. The **to_date** function is used to return the year, month, and day in a time. The date format cannot be specified.

Syntax
------

.. code-block::

   to_date1(string date, string format)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                 |
   +=================+=================+=================+=============================================================================================================+
   | date            | Yes             | STRING          | String to be converted                                                                                      |
   |                 |                 |                 |                                                                                                             |
   |                 |                 |                 | The following formats are supported:                                                                        |
   |                 |                 |                 |                                                                                                             |
   |                 |                 |                 | -  yyyy-mm-dd                                                                                               |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss                                                                                      |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss.ff3                                                                                  |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------+
   | format          | Yes             | STRING          | Format of the date to be converted                                                                          |
   |                 |                 |                 |                                                                                                             |
   |                 |                 |                 | Constant of the STRING type. Extended date formats are not supported.                                       |
   |                 |                 |                 |                                                                                                             |
   |                 |                 |                 | The value is a combination of the time unit (year, month, day, hour, minute, and second) and any character. |
   |                 |                 |                 |                                                                                                             |
   |                 |                 |                 | -  **YYYY** or **yyyy** indicates the year.                                                                 |
   |                 |                 |                 | -  **MM** indicates the month.                                                                              |
   |                 |                 |                 | -  **mm** indicates the minute.                                                                             |
   |                 |                 |                 | -  **dd** indicates the day.                                                                                |
   |                 |                 |                 | -  **HH** indicates the 24-hour clock.                                                                      |
   |                 |                 |                 | -  **hh** indicates the 12-hour clock.                                                                      |
   |                 |                 |                 | -  **mi** indicates the minute.                                                                             |
   |                 |                 |                 | -  **ss** indicates the second.                                                                             |
   |                 |                 |                 | -  **SSS** indicates the millisecond.                                                                       |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If the value of **date** is not of the DATE or STRING type, the error message "data type mismatch" is displayed.
   -  If the value of **date** is of the DATE or STRING type but is not in one of the supported formats, **NULL** is returned.
   -  If the value of **date** is **NULL**, **NULL** is returned.
   -  If the value of **format** is **NULL**, a date in the **yyyy-mm-dd** format is returned.

Example Code
------------

The value **NULL** is returned.

.. code-block::

   select to_date1('2023-08-16 10:54:36','yyyy-mm-dd');

The value **2023-08-16 00:00:00** is returned.

.. code-block::

   select to_date1('2023-08-16','yyyy-mm-dd');

The value **NULL** is returned.

.. code-block::

   select to_date(null);

The value **2023-08-16** is returned.

.. code-block::

   select to_date1('2023-08-16 10:54:36');
