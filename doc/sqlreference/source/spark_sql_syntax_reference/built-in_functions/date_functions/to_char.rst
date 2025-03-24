:original_name: dli_spark_to_char.html

.. _dli_spark_to_char:

to_char
=======

This function is used to convert a date into a string in a specified format.

Syntax
------

.. code-block::

   to_char(string date, string format)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                 |
   +=================+=================+=================+=============================================================================================================+
   | date            | Yes             | DATE            | Date that needs to be processed                                                                             |
   |                 |                 |                 |                                                                                                             |
   |                 |                 | or              | The following formats are supported:                                                                        |
   |                 |                 |                 |                                                                                                             |
   |                 |                 | STRING          | -  yyyy-mm-dd                                                                                               |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss                                                                                      |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss.ff3                                                                                  |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------+
   | format          | Yes             | STRING          | Format of the date to be converted                                                                          |
   |                 |                 |                 |                                                                                                             |
   |                 |                 |                 | Constant of the STRING type. Extended date formats are not supported.                                       |
   |                 |                 |                 |                                                                                                             |
   |                 |                 |                 | The value is a combination of the time unit (year, month, day, hour, minute, and second) and any character. |
   |                 |                 |                 |                                                                                                             |
   |                 |                 |                 | -  **yyyy** indicates the year.                                                                             |
   |                 |                 |                 | -  **mm** indicates the month.                                                                              |
   |                 |                 |                 | -  **dd** indicates the day.                                                                                |
   |                 |                 |                 | -  **hh** indicates the hour.                                                                               |
   |                 |                 |                 | -  **mi** indicates the minute.                                                                             |
   |                 |                 |                 | -  **ss** indicates the second.                                                                             |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If the value of **date** is not of the DATE or STRING type, the error message "data type mismatch" is displayed.
   -  If the value of **date** is of the DATE or STRING type but is not in one of the supported formats, **NULL** is returned.
   -  If the value of **format** is **NULL**, **NULL** is returned.

Example Code
------------

The static data **2023-08*16** is returned.

.. code-block::

   select to_char('2023-08-16 10:54:36','Example static data yyyy-mm*dd');

The value **20230816** is returned.

.. code-block::

   select to_char('2023-08-16 10:54:36', 'yyyymmdd');

The value **NULL** is returned.

.. code-block::

   select to_char('Example static data 2023-08-16','Example static data yyyy-mm*dd');

The value **NULL** is returned.

.. code-block::

   select to_char('20230816', 'yyyy');

The value **NULL** is returned.

.. code-block::

   select to_char('2023-08-16 10:54:36', null);
