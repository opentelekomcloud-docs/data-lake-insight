:original_name: dli_spark_date_sub.html

.. _dli_spark_date_sub:

date_sub
========

This function is used to calculate the number of days in which **start_date** is subtracted by **days**.

To obtain the date with a specified change range based on the current date, use this function together with the :ref:`current_date <dli_spark_current_date>` or :ref:`getdate <dli_spark_getdate>` function.

Note that the logic of this function is opposite to that of the :ref:`date_add <dli_spark_date_add>` function.

Syntax
------

.. code-block::

   date_sub(string startdate, int days)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                               |
   +=================+=================+=================+===========================================================================+
   | start_date      | Yes             | DATE            | Start date                                                                |
   |                 |                 |                 |                                                                           |
   |                 |                 | or              | The following formats are supported:                                      |
   |                 |                 |                 |                                                                           |
   |                 |                 | STRING          | -  yyyy-mm-dd                                                             |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss                                                    |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss.ff3                                                |
   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------+
   | days            | Yes             | BIGINT          | Number of days to be reduced                                              |
   |                 |                 |                 |                                                                           |
   |                 |                 |                 | -  If the value is greater than 0, the number of days is increased.       |
   |                 |                 |                 | -  If the value of days is less than 0, the number of days is subtracted. |
   |                 |                 |                 | -  If the value is 0, the date does not change.                           |
   |                 |                 |                 | -  If the value is **NULL**, **NULL** is returned.                        |
   +-----------------+-----------------+-----------------+---------------------------------------------------------------------------+

Return Values
-------------

The return value is of the DATE type.

.. note::

   -  If the value of **start_date** is not of the DATE or STRING type, the error message "data type mismatch" is displayed.
   -  If the value of **start_date** is of the DATE or STRING type but is not in one of the supported formats, NULL is returned.
   -  If the value of **date** is **NULL**, **NULL** is returned.
   -  If the value of **format** is **NULL**, **NULL** is returned.

Example Code
------------

The value **2023-08-12** is returned after two days are subtracted.

.. code-block::

   select date_sub('2023-08-14 17:00:00', 2);

The value **2023-08-15** is returned after one day is added.

.. code-block::

   select date_sub(date'2023-08-14', -1);

If the current time is **2023-08-14 17:00:00**, **2022-08-13** is returned.

.. code-block::

   select date_sub(getdate(),1);

The value **NULL** is returned.

.. code-block::

   select date_sub('2023-08-14 17:00:00', null);
