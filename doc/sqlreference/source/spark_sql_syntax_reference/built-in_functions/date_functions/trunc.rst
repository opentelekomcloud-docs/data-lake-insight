:original_name: dli_spark_trunc.html

.. _dli_spark_trunc:

trunc
=====

This function is used to reset a date to a specific format.

Resetting means returning to default values, where the default values for year, month, and day are **01**, and the default values for hour, minute, second, and millisecond are **00**.

Syntax
------

.. code-block::

   trunc(string date, string format)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                 |
   +=================+=================+=================+=============================================================================================================+
   | date            | Yes             | DATE or STRING  | Date that needs to be processed                                                                             |
   |                 |                 |                 |                                                                                                             |
   |                 |                 |                 | The following formats are supported:                                                                        |
   |                 |                 |                 |                                                                                                             |
   |                 |                 |                 | -  yyyy-mm-dd                                                                                               |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss                                                                                      |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss.ff3                                                                                  |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------+
   | format          | Yes             | STRING          | Format of the date to be converted                                                                          |
   |                 |                 |                 |                                                                                                             |
   |                 |                 |                 | The value is a combination of the time unit (year, month, day, hour, minute, and second) and any character. |
   |                 |                 |                 |                                                                                                             |
   |                 |                 |                 | -  **yyyy** indicates the year.                                                                             |
   |                 |                 |                 | -  **MM** indicates the month.                                                                              |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the DATE type, in the **yyyy-mm-dd** format.

.. note::

   -  If the value of **date** is not of the DATE or STRING type, the error message "data type mismatch" is displayed.
   -  If the value of **date** is of the DATE or STRING type but is not in one of the supported formats, **NULL** is returned.
   -  If the value of **date** is **NULL**, **NULL** is returned.
   -  If the value of **format** is **NULL**, **NULL** is returned.

Example Code
------------

The value **2023-08-01** is returned.

.. code-block::

   select trunc('2023-08-16', 'MM');

The value **2023-08-01** is returned.

.. code-block::

   select trunc('2023-08-16 10:54:36', 'MM');

The value **NULL** is returned.

.. code-block::

    select trunc(null, 'MM');
