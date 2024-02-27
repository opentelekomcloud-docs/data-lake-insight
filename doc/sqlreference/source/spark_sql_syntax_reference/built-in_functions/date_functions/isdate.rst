:original_name: dli_spark_isdate.html

.. _dli_spark_isdate:

isdate
======

This function is used to determine whether a date string can be converted into a date value based on a specified format.

Syntax
------

.. code-block::

   isdate(string date , string format)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                                             |
   +=================+=================+=================+=========================================================================================================================================+
   | date            | Yes             | DATE            | String to be checked                                                                                                                    |
   |                 |                 |                 |                                                                                                                                         |
   |                 |                 | or              | If the value is of the BIGINT, DOUBLE, DECIMAL, or DATETIME type, the value is implicitly converted to the STRING type for calculation. |
   |                 |                 |                 |                                                                                                                                         |
   |                 |                 | STRING          | The value can be any string.                                                                                                            |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------+
   | format          | Yes             | STRING          | Format of the date to be converted                                                                                                      |
   |                 |                 |                 |                                                                                                                                         |
   |                 |                 |                 | Constant of the STRING type. Extended date formats are not supported.                                                                   |
   |                 |                 |                 |                                                                                                                                         |
   |                 |                 |                 | The value is a combination of the time unit (year, month, day, hour, minute, and second) and any character.                             |
   |                 |                 |                 |                                                                                                                                         |
   |                 |                 |                 | -  **YYYY** or **yyyy** indicates the year.                                                                                             |
   |                 |                 |                 | -  **MM** indicates the month.                                                                                                          |
   |                 |                 |                 | -  **mm** indicates the minute.                                                                                                         |
   |                 |                 |                 | -  **dd** indicates the day.                                                                                                            |
   |                 |                 |                 | -  **HH** indicates the 24-hour clock.                                                                                                  |
   |                 |                 |                 | -  **hh** indicates the 12-hour clock.                                                                                                  |
   |                 |                 |                 | -  **mi** indicates the minute.                                                                                                         |
   |                 |                 |                 | -  **ss** indicates the second.                                                                                                         |
   |                 |                 |                 | -  **SSS** indicates millisecond.                                                                                                       |
   +-----------------+-----------------+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the BOOLEAN type.

.. note::

   If the value of **date** or **format** is **NULL**, **NULL** is returned.

Example Code
------------

The value **true** is returned.

.. code-block::

    select isdate('2023-08-10','yyyy-mm-dd');

The value **false** is returned.

.. code-block::

    select isdate(123456789,'yyyy-mm-dd');
