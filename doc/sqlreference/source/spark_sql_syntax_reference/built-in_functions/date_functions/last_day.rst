:original_name: dli_spark_last_day.html

.. _dli_spark_last_day:

last_day
========

This function is used to return the last day of the month a date belongs to.

Similar function: :ref:`lastday <dli_spark_lastday>`. The **lastday** function is used to return the last day of the month a date belongs to. The hour, minute, and second part is 00:00:00.

Syntax
------

.. code-block::

   last_day(string date)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------------+-----------------+-----------------+--------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                          |
   +=================+=================+=================+======================================+
   | date            | Yes             | DATE            | Date that needs to be processed      |
   |                 |                 |                 |                                      |
   |                 |                 | or              | The following formats are supported: |
   |                 |                 |                 |                                      |
   |                 |                 | STRING          | -  yyyy-mm-dd                        |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss               |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss.ff3           |
   +-----------------+-----------------+-----------------+--------------------------------------+

Return Values
-------------

The return value is of the DATE type, in the **yyyy-mm-dd** format.

.. note::

   -  If the value of **date** is not of the DATE or STRING type, the error message "data type mismatch" is displayed.
   -  If the value of **date** is of the DATE or STRING type but is not in one of the supported formats, **NULL** is returned.
   -  If the value of **date** is **NULL**, **NULL** is returned.

Example Code
------------

The value **2023-08-31** is returned.

.. code-block::

    select last_day('2023-08-15');

The value **2023-08-31** is returned.

.. code-block::

    select last_day('2023-08-10 10:54:00');

The value **NULL** is returned.

.. code-block::

    select last_day('20230810');
