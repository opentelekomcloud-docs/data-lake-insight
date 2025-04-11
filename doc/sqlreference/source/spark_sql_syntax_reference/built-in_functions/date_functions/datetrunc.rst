:original_name: dli_spark_datetrunc.html

.. _dli_spark_datetrunc:

datetrunc
=========

This function is used to calculate the date otained through the truncation of a specified date based on a specified datepart.

It truncates the date before the specified datepart and automatically fills the remaining part with the default value. For details, see :ref:`Example Code <dli_spark_datetrunc__en-us_topic_0000001694195249_section13277192233920>`.

Syntax
------

.. code-block::

   datetrunc (string date, string datepart)

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

The return value is of the DATE or STRING type.

.. note::

   -  If the value of **date** is not of the DATE or STRING type, the error message "data type mismatch" is displayed.
   -  If the value of **date** is of the DATE or STRING type but is not in one of the supported formats, **NULL** is returned.
   -  If the value of **datepart** is **NULL**, **NULL** is returned.
   -  If the value of **datepart** is hour, minute, or second, the date is truncated to the day and returned.

.. _dli_spark_datetrunc__en-us_topic_0000001694195249_section13277192233920:

Example Code
------------

Example static data

The value **2023-01-01 00:00:00** is returned.

.. code-block::

   select datetrunc('2023-08-14 17:00:00', 'yyyy');

The value **2023-08-01 00:00:00** is returned.

.. code-block::

   select datetrunc('2023-08-14 17:00:00', 'month');

The value **2023-08-14** is returned.

.. code-block::

   select datetrunc('2023-08-14 17:00:00', 'DD');

The value **2023-01-01** is returned.

.. code-block::

   select datetrunc('2023-08-14', 'yyyy');

The value **2023-08-14 17:00:00** is returned.

.. code-block::

   select datetrunc('2023-08-14 17:11:11', 'hh');

The value **NULL** is returned.

.. code-block::

   select datetrunc('2023-08-14', null);
