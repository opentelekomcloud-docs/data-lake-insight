:original_name: dli_spark_hour.html

.. _dli_spark_hour:

hour
====

This function is used to return the hour (from 0 to 23) of a specified time.

Syntax
------

.. code-block::

   hour(string date)

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

The return value is of the INT type.

.. note::

   -  If the value of **date** is not of the DATE or STRING type, the error message "data type mismatch" is displayed.
   -  If the value of **date** is of the DATE or STRING type but is not in one of the supported formats, **NULL** is returned.
   -  If the value of **date** is **NULL**, **NULL** is returned.

Example Code
------------

The value **10** is returned.

.. code-block::

   select hour('2023-08-10 10:54:00');

The value **12** is returned.

.. code-block::

   select hour('12:00:00');

The value **NULL** is returned.

.. code-block::

   select hour('20230810105600');

The value **NULL** is returned.

.. code-block::

   select hour(null);
