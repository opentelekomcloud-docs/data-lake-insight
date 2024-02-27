:original_name: dli_spark_date_add.html

.. _dli_spark_date_add:

date_add
========

This function is used to calculate the number of days in which **start_date** is increased by **days**.

To obtain the date with a specified change range based on the current date, use this function together with the :ref:`current_date <dli_spark_current_date>` or :ref:`getdate <dli_spark_getdate>` function.

Note that the logic of this function is opposite to that of the :ref:`date_sub <dli_spark_date_sub>` function.

Syntax
------

.. code-block::

   date_add(string startdate, int days)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+---------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                         |
   +=================+=================+=================+=====================================================================+
   | start_date      | Yes             | DATE            | Start date                                                          |
   |                 |                 |                 |                                                                     |
   |                 |                 | or              | The following formats are supported:                                |
   |                 |                 |                 |                                                                     |
   |                 |                 | STRING          | -  yyyy-mm-dd                                                       |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss                                              |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss.ff3                                          |
   +-----------------+-----------------+-----------------+---------------------------------------------------------------------+
   | days            | Yes             | BIGINT          | Number of days to be added                                          |
   |                 |                 |                 |                                                                     |
   |                 |                 |                 | -  If the value is greater than 0, the number of days is increased. |
   |                 |                 |                 | -  If the value is less than 0, the number of days is subtracted.   |
   |                 |                 |                 | -  If the value is 0, the date does not change.                     |
   |                 |                 |                 | -  If the value is **NULL**, **NULL** is returned.                  |
   +-----------------+-----------------+-----------------+---------------------------------------------------------------------+

Return Values
-------------

The return value is of the DATE type, in the **yyyy-mm-dd** format.

.. note::

   -  If the value of **start_date** is not of the DATE or STRING type, the error message "data type mismatch" is displayed.
   -  If the value of **start_date** is of the DATE or STRING type but is not in one of the supported formats, **NULL** is returned.
   -  If the value of **start_date** is **NULL**, **NULL** is returned.
   -  If the value of **days** is **NULL**, **NULL** is returned.

Example Code
------------

The value **2023-03-01** is returned after one day is added.

.. code-block::

   select date_add('2023-02-28 00:00:00', 1);

The value **2023-02-27** is returned after one day is subtracted.

.. code-block::

   select date_add(date '2023-02-28', -1);

The value **2023-03-20** is returned.

.. code-block::

   select date_add('2023-02-28 00:00:00', 20);

If the current time is **2023-08-14 16:00:00**, **2023-08-13** is returned.

.. code-block::

   select date_add(getdate(),-1);

The value **NULL** is returned.

.. code-block::

   select date_add('2023-02-28 00:00:00', null);
