:original_name: dli_spark_datediff.html

.. _dli_spark_datediff:

datediff
========

This function is used to calculate the difference between **date1** and **date2**.

Similar function: :ref:`datediff1 <dli_spark_datediff1>`. The **datediff1** function is used to calculate the difference between **date1** and **date2** and return the difference in a specified datepart.

Syntax
------

.. code-block::

   datediff(string date1, string date2)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+--------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                        |
   +=================+=================+=================+====================================================================+
   | date1           | Yes             | DATE            | Minuend of the date difference between **date1** and **date2**.    |
   |                 |                 |                 |                                                                    |
   |                 |                 | or              | The following formats are supported:                               |
   |                 |                 |                 |                                                                    |
   |                 |                 | STRING          | -  yyyy-mm-dd                                                      |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss                                             |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss.ff3                                         |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------+
   | date2           | Yes             | DATE            | Subtrahend of the date difference between **date1** and **date2**. |
   |                 |                 |                 |                                                                    |
   |                 |                 | or              | The following formats are supported:                               |
   |                 |                 |                 |                                                                    |
   |                 |                 | STRING          | -  yyyy-mm-dd                                                      |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss                                             |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss.ff3                                         |
   +-----------------+-----------------+-----------------+--------------------------------------------------------------------+

Return Values
-------------

The return value is of the BIGINT type.

.. note::

   -  If the values of **date1** and **date2** are not of the DATE or STRING type, the error message "data type mismatch" is displayed.
   -  If the values of **date1** and **date2** are of the DATE or STRING type but are not in one of the supported formats, **NULL** is returned.
   -  If the value of **date1** is smaller than that of **date2**, the return value is a negative number.
   -  If the value of **date1** or **date2** is **NULL**, **NULL** is returned.

Example Code
------------

The value **10** is returned.

.. code-block::

   select datediff('2023-06-30 00:00:00', '2023-06-20 00:00:00');

The value **11** is returned.

.. code-block::

   select datediff(date '2023-05-21', date '2023-05-10');

The value **NULL** is returned.

.. code-block::

   select datediff(date '2023-05-21', null);
