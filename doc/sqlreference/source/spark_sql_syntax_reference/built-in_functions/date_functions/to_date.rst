:original_name: dli_spark_to_date.html

.. _dli_spark_to_date:

to_date
=======

This function is used to return the year, month, and day in a time.

Similar function: :ref:`to_date1 <dli_spark_to_date1>`. The **to_date1** function is used to convert a string in a specified format to a date value. The date format can be specified.

Syntax
------

.. code-block::

   to_date(string timestamp)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------------+-----------------+-----------------+--------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                          |
   +=================+=================+=================+======================================+
   | timestamp       | Yes             | DATE            | Time to be processed                 |
   |                 |                 |                 |                                      |
   |                 |                 | STRING          | The following formats are supported: |
   |                 |                 |                 |                                      |
   |                 |                 |                 | -  yyyy-mm-dd                        |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss               |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss.ff3           |
   +-----------------+-----------------+-----------------+--------------------------------------+

Return Values
-------------

The return value is of the DATE type, in the **yyyy-mm-dd** format.

.. note::

   -  If the value of **timestamp** is not of the DATE or STRING type, the error message "data type mismatch" is displayed.
   -  If the value of **timestamp** is of the DATE or STRING type but is not in one of the supported formats, **NULL** is returned.

Example Code
------------

The value **2023-08-16** is returned.

.. code-block::

   select to_date('2023-08-16 10:54:36');

The value **NULL** is returned.

.. code-block::

   select to_date(null);
