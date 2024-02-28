:original_name: dli_spark_from_unixtime.html

.. _dli_spark_from_unixtime:

from_unixtime
=============

This function is used to convert a timestamp represented by a numeric UNIX value to a date value.

Syntax
------

.. code-block::

   from_unixtime(bigint unixtime)

Parameters
----------

.. table:: **Table 1** Parameter

   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                |
   +=================+=================+=================+============================================================================+
   | unixtime        | Yes             | BIGINT          | Timestamp to be converted, in UNIX format                                  |
   |                 |                 |                 |                                                                            |
   |                 |                 |                 | Set this parameter to the first 10 digits of the timestamp in UNIX format. |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------+

Return Values
-------------

The return value is of the STRING type, in the **yyyy-mm-dd hh:mi:ss** format.

.. note::

   If the value of **unixtime** is **NULL**, **NULL** is returned.

Example Code
------------

The value **2023-08-16 09:39:57** is returned.

.. code-block::

   select from_unixtime(1692149997);

The value **NULL** is returned.

.. code-block::

   select from_unixtime(NULL);

Example table data

.. code-block::

   select unixdate, from_unixtime(unixdate) as timestamp_from_unixtime from database_t;
   Output:
   +------------------+------------------------------+
   | unixdate              | timestamp_from_unixtime   |
   +------------------+------------------------------+
   | 1690944759224  | 2023-08-02 10:52:39           |
   | 1690944999811  | 2023-08-02 10:56:39           |
   | 1690945005458  | 2023-08-02 10:56:45           |
   | 1690945011542  | 2023-08-02 10:56:51           |
   | 1690945023151  | 2023-08-02 10:57:03           |
   +------------------+------------------------------+
