:original_name: dli_spark_datediff1.html

.. _dli_spark_datediff1:

datediff1
=========

This function is used to calculate the difference between **date1** and **date2** and return the difference in a specified datepart.

Similar function: :ref:`datediff <dli_spark_datediff>`. The **datediff** function is used to calculate the difference between **date1** and **date2** but does not return the difference in a specified datepart.

Syntax
------

.. code-block::

   datediff1(string date1, string date2, string datepart)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                     |
   +=================+=================+=================+=================================================================================================+
   | date1           | Yes             | DATE            | Minuend of the date difference between **date1** and **date2**.                                 |
   |                 |                 |                 |                                                                                                 |
   |                 |                 | or              | The following formats are supported:                                                            |
   |                 |                 |                 |                                                                                                 |
   |                 |                 | STRING          | -  yyyy-mm-dd                                                                                   |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss                                                                          |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss.ff3                                                                      |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------+
   | date2           | Yes             | DATE            | Subtrahend of the date difference between **date1** and **date2**.                              |
   |                 |                 |                 |                                                                                                 |
   |                 |                 | or              | The following formats are supported:                                                            |
   |                 |                 |                 |                                                                                                 |
   |                 |                 | STRING          | -  yyyy-mm-dd                                                                                   |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss                                                                          |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss.ff3                                                                      |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------+
   | datepart        | Yes             | STRING          | Unit of the time to be returned                                                                 |
   |                 |                 |                 |                                                                                                 |
   |                 |                 |                 | This parameter supports the following extended date formats: year, month or mon, day, and hour. |
   |                 |                 |                 |                                                                                                 |
   |                 |                 |                 | -  **YYYY** or **yyyy** indicates the year.                                                     |
   |                 |                 |                 | -  **MM** indicates the month.                                                                  |
   |                 |                 |                 | -  **mm** indicates the minute.                                                                 |
   |                 |                 |                 | -  **dd** indicates the day.                                                                    |
   |                 |                 |                 | -  **HH** indicates the 24-hour clock.                                                          |
   |                 |                 |                 | -  **hh** indicates the 12-hour clock.                                                          |
   |                 |                 |                 | -  **mi** indicates the minute.                                                                 |
   |                 |                 |                 | -  **ss** indicates the second.                                                                 |
   |                 |                 |                 | -  **SSS** indicates millisecond.                                                               |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the BIGINT type.

.. note::

   -  If the values of **date1** and **date2** are not of the DATE or STRING type, the error message "data type mismatch" is displayed.
   -  If the values of **date1** and **date2** are of the DATE or STRING type but are not in one of the supported formats, **NULL** is returned.
   -  If the value of **date1** is smaller than that of **date2**, the return value is a negative number.
   -  If the value of **date1** or **date2** is **NULL**, **NULL** is returned.
   -  If the value of **datepart** is **NULL**, **NULL** is returned.

Example Code
------------

The value **14400** is returned.

.. code-block::

   select datediff1('2023-06-30 00:00:00', '2023-06-20 00:00:00', 'mi');

The value **10** is returned.

.. code-block::

   select datediff1(date '2023-06-21', date '2023-06-11', 'dd');

The value **NULL** is returned.

.. code-block::

   select datediff1(date '2023-05-21', date '2023-05-10', null);

The value **NULL** is returned.

.. code-block::

   select datediff1(date '2023-05-21', null, 'dd');
