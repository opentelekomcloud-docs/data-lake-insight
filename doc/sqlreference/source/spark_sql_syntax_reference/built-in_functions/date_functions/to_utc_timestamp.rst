:original_name: dli_spark_to_utc_timestamp.html

.. _dli_spark_to_utc_timestamp:

to_utc_timestamp
================

This function is used to convert a timestamp in a given time zone to a UTC timestamp.

Syntax
------

.. code-block::

   to_utc_timestamp(string timestamp, string timezone)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                        |
   +=================+=================+=================+====================================================================================================+
   | timestamp       | Yes             | DATE            | Time to be processed                                                                               |
   |                 |                 |                 |                                                                                                    |
   |                 |                 | STRING          | Date value of the DATE or STRING type, or timestamp of the TINYINT, SMALLINT, INT, or BIGINT type. |
   |                 |                 |                 |                                                                                                    |
   |                 |                 | TINYINT         | The following formats are supported:                                                               |
   |                 |                 |                 |                                                                                                    |
   |                 |                 | SMALLINT        | -  yyyy-mm-dd                                                                                      |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss                                                                             |
   |                 |                 | INT             | -  yyyy-mm-dd hh:mi:ss.ff3                                                                         |
   |                 |                 |                 |                                                                                                    |
   |                 |                 | BIGINT          |                                                                                                    |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+
   | timezone        | Yes             | STRING          | Time zone where the time to be converted belongs                                                   |
   +-----------------+-----------------+-----------------+----------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the BIGINT type.

.. note::

   -  If the value of **timestamp** is not of the DATE or STRING type, the error message "data type mismatch" is displayed.
   -  If the value of **timestamp** is of the DATE or STRING type but is not in one of the supported formats, **NULL** is returned.
   -  If the value of **timestamp** is **NULL**, **NULL** is returned.
   -  If the value of **timezone** is **NULL**, **NULL** is returned.

Example Code
------------

The value **1692028800000** is returned.

.. code-block::

   select to_utc_timestamp('2023-08-14 17:00:00','PST');

The value **NULL** is returned.

.. code-block::

   select to_utc_timestamp(null);
