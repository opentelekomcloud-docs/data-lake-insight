:original_name: dli_spark_lastday.html

.. _dli_spark_lastday:

lastday
=======

This function is used to return the last day of the month a date belongs to. The hour, minute, and second part is 00:00:00.

Similar function: :ref:`last_day <dli_spark_last_day>`. The **last_day** function is used to return the last day of the month a date belongs to. The return value is in the **yyyy-mm-dd** format.

Syntax
------

.. code-block::

   lastday(string date)

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

The return value is of the STRING type, in the **yyyy-mm-dd hh:mi:ss** format.

.. note::

   -  If the value of **date** is not of the DATE or STRING type, the error message "data type mismatch" is displayed.
   -  If the value of **date** is of the DATE or STRING type but is not in one of the supported formats, **NULL** is returned.
   -  If the value of **date** is **NULL**, **NULL** is returned.

Example Code
------------

The value **2023-08-31** is returned.

.. code-block::

    select lastday('2023-08-10');

The value **2023-08-31 00:00:00** is returned.

.. code-block::

    select lastday ('2023-08-10 10:54:00');

The value **NULL** is returned.

.. code-block::

    select lastday (null);
