:original_name: dli_spark_datepart.html

.. _dli_spark_datepart:

datepart
========

This function is used to calculate the value that meets the specified **datepart** in **date**.

Syntax
------

.. code-block::

   datepart (string date, string datepart)

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
   | datepart        | Yes             | STRING          | Time unit of the value to be returned                                                           |
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

The return value is of the BIGINT type.

.. note::

   -  If the value of **date** is not of the DATE or STRING type, the error message "data type mismatch" is displayed.
   -  If the value of **date** is of the DATE or STRING type but is not in one of the supported formats, **NULL** is returned.
   -  If the value of **datepart** is **NULL**, **NULL** is returned.
   -  If the value of **datepart** is **NULL**, **NULL** is returned.

Example Code
------------

The value **2023** is returned.

.. code-block::

   select datepart(date '2023-08-14 17:00:00', 'yyyy');

The value **2023** is returned.

.. code-block::

   select datepart('2023-08-14 17:00:00', 'yyyy');

The value **59** is returned.

.. code-block::

   select datepart('2023-08-14 17:59:59', 'mi')

The value **NULL** is returned.

.. code-block::

   select datepart(date '2023-08-14', null);
