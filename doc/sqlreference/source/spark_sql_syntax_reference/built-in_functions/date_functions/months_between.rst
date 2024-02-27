:original_name: dli_spark_months_between.html

.. _dli_spark_months_between:

months_between
==============

This function returns the month difference between **date1** and **date2**.

Syntax
------

.. code-block::

   months_between(string date1, string date2)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+--------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                          |
   +=================+=================+=================+======================================+
   | date1           | Yes             | DATE            | Minuend                              |
   |                 |                 |                 |                                      |
   |                 |                 | or              | The following formats are supported: |
   |                 |                 |                 |                                      |
   |                 |                 | STRING          | -  yyyy-mm-dd                        |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss               |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss.ff3           |
   +-----------------+-----------------+-----------------+--------------------------------------+
   | date2           | Yes             | DATE            | Subtrahend                           |
   |                 |                 |                 |                                      |
   |                 |                 | or              | The following formats are supported: |
   |                 |                 |                 |                                      |
   |                 |                 | STRING          | -  yyyy-mm-dd                        |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss               |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss.ff3           |
   +-----------------+-----------------+-----------------+--------------------------------------+

Return Values
-------------

The return value is of the DOUBLE type.

.. note::

   -  If the values of **date1** and **date2** are not of the DATE or STRING type, the error message "data type mismatch" is displayed.
   -  If the values of **date1** and **date2** are of the DATE or STRING type but are not in one of the supported formats, **NULL** is returned.
   -  If **date1** is later than **date2**, the return value is positive. If **date2** is later than **date1**, the return value is negative.
   -  If **date1** and **date2** correspond to the last day of two different months, an integer month is returned. Otherwise, the calculation is based on the number of days between **date1** and **date2** divided by 31.
   -  If the value of **date1** or **date2** is **NULL**, **NULL** is returned.

Example Code
------------

The value **0.0563172** is returned.

.. code-block::

   select months_between('2023-08-16 10:54:00', '2023-08-14 17:00:00');

The value **0.06451613** is returned.

.. code-block::

   select months_between('2023-08-16','2023-08-14');

The value **NULL** is returned.

.. code-block::

   select months_between('2023-08-16',null);
