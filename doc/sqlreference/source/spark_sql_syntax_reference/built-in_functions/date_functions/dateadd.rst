:original_name: dli_spark_dateadd.html

.. _dli_spark_dateadd:

dateadd
=======

This function is used to change a date based on **datepart** and **delta**.

To obtain the date with a specified change range based on the current date, use this function together with the :ref:`current_date <dli_spark_current_date>` or :ref:`getdate <dli_spark_getdate>` function.

Syntax
------

.. code-block::

   dateadd(string date, bigint delta, string datepart)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                     |
   +=================+=================+=================+=================================================================================================+
   | date            | Yes             | DATE            | Start date                                                                                      |
   |                 |                 |                 |                                                                                                 |
   |                 |                 | or              | The following formats are supported:                                                            |
   |                 |                 |                 |                                                                                                 |
   |                 |                 | STRING          | -  yyyy-mm-dd                                                                                   |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss                                                                          |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss.ff3                                                                      |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------+
   | delta           | Yes             | BIGINT          | Amplitude, based on which the date is modified                                                  |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------+
   | datepart        | Yes             | BIGINT          | Unit, based on which the date is modified                                                       |
   |                 |                 |                 |                                                                                                 |
   |                 |                 |                 | This parameter supports the following extended date formats: year, month or mon, day, and hour. |
   |                 |                 |                 |                                                                                                 |
   |                 |                 |                 | -  **yyyy** indicates the year.                                                                 |
   |                 |                 |                 | -  **MM** indicates the month.                                                                  |
   |                 |                 |                 | -  **dd** indicates the day.                                                                    |
   |                 |                 |                 | -  **hh** indicates the hour.                                                                   |
   |                 |                 |                 | -  **mi** indicates the minute.                                                                 |
   |                 |                 |                 | -  **ss** indicates the second.                                                                 |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING type.

.. note::

   -  If the value of **date** is not of the DATE or STRING type, the error message "data type mismatch" is displayed.
   -  If the value of **date** is of the DATE or STRING type but is not in one of the supported formats, **NULL** is returned.
   -  If the value of **date** is **NULL**, **NULL** is returned.
   -  If the value of **delta** or **datepart** is **NULL**, **NULL** is returned.

Example Code
------------

The value **2023-08-15 17:00:00** is returned after one day is added.

.. code-block::

   select dateadd( '2023-08-14 17:00:00', 1, 'dd');

The value **2025-04-14 17:00:00** is returned after 20 months are added.

.. code-block::

   select dateadd('2023-08-14 17:00:00', 20, 'mm');

The value **2023-09-14 17:00:00** is returned.

.. code-block::

   select dateadd('2023-08-14 17:00:00', 1, 'mm');

The value **2023-09-14** is returned.

.. code-block::

   select dateadd('2023-08-14', 1, 'mm');

If the current time is **2023-08-14 17:00:00**, **2023-08-13 17:00:00** is returned.

.. code-block::

   select dateadd(getdate(),-1,'dd');

The value **NULL** is returned.

.. code-block::

   select dateadd(date '2023-08-14', 1, null);
