:original_name: dli_spark_month.html

.. _dli_spark_month:

month
=====

This function is used to return the month (from January to December) of a specified time.

Syntax
------

.. code-block::

   month(string date)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                          |
   +=================+=================+=================+======================================================================================================================+
   | date            | Yes             | DATE            | Date that needs to be processed                                                                                      |
   |                 |                 |                 |                                                                                                                      |
   |                 |                 | or              | If the value is of the STRING type, the value must contain at least yyyy-mm-dd and cannot contain redundant strings. |
   |                 |                 |                 |                                                                                                                      |
   |                 |                 | STRING          | The following formats are supported:                                                                                 |
   |                 |                 |                 |                                                                                                                      |
   |                 |                 |                 | -  yyyy-mm-dd                                                                                                        |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss                                                                                               |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss.ff3                                                                                           |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the INT type.

.. note::

   -  If the value of **date** is not of the DATE or STRING type, the error message "data type mismatch" is displayed.
   -  If the value of **date** is of the DATE or STRING type but is not in one of the supported formats, **NULL** is returned.
   -  If the value of **date** is **NULL**, **NULL** is returned.

Example Code
------------

The value **8** is returned.

.. code-block::

   select month('2023-08-10 10:54:00');

The value **NULL** is returned.

.. code-block::

   select month('20230810');

The value **NULL** is returned.

.. code-block::

   select month(null);
