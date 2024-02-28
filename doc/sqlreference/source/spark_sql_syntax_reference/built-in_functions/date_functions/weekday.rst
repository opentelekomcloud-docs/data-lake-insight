:original_name: dli_spark_weekday.html

.. _dli_spark_weekday:

weekday
=======

This function is used to return the day of the current week.

Syntax
------

.. code-block::

   weekday (string date)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------------+-----------------+-----------------+--------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                          |
   +=================+=================+=================+======================================+
   | date            | Yes             | DATE or STRING  | Date that needs to be processed      |
   |                 |                 |                 |                                      |
   |                 |                 |                 | The following formats are supported: |
   |                 |                 |                 |                                      |
   |                 |                 |                 | -  yyyy-mm-dd                        |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss               |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss.ff3           |
   +-----------------+-----------------+-----------------+--------------------------------------+

Return Values
-------------

The return value is of the INT type.

.. note::

   -  If Monday is used as the first day of a week, the value **0** is returned. For other weekdays, the return value increases in ascending order. For Sunday, the value **6** is returned.
   -  If the value of **date** is not of the DATE or STRING type, the error message "data type mismatch" is displayed.
   -  If the value of **date** is of the DATE or STRING type but is not in one of the supported formats, **NULL** is returned.
   -  If the value of **date** is **NULL**, **NULL** is returned.

Example Code
------------

The value **2** is returned.

.. code-block::

   select weekday ('2023-08-16 10:54:36');

The value **NULL** is returned.

.. code-block::

   select weekday (null);
