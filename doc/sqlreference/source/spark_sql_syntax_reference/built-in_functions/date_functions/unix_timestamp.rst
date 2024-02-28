:original_name: dli_spark_unix_timestamp.html

.. _dli_spark_unix_timestamp:

unix_timestamp
==============

This function is used to convert a date value to a numeric date value in UNIX format.

The function returns the first ten digits of the timestamp in normal UNIX format.

Syntax
------

.. code-block::

   unix_timestamp(string timestamp, string pattern)

Parameters
----------

.. table:: **Table 1** Parameters

   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------+
   | Parameter       | Mandatory       | Type            | Description                                                                                                 |
   +=================+=================+=================+=============================================================================================================+
   | timestamp       | No              | DATE or STRING  | Date to be converted                                                                                        |
   |                 |                 |                 |                                                                                                             |
   |                 |                 |                 | The following formats are supported:                                                                        |
   |                 |                 |                 |                                                                                                             |
   |                 |                 |                 | -  yyyy-mm-dd                                                                                               |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss                                                                                      |
   |                 |                 |                 | -  yyyy-mm-dd hh:mi:ss.ff3                                                                                  |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------+
   | pattern         | No              | STRING          | Format to be converted                                                                                      |
   |                 |                 |                 |                                                                                                             |
   |                 |                 |                 | If this parameter is left blank, the default format **yyyy-mm-dd hh:mm:ss** is used.                        |
   |                 |                 |                 |                                                                                                             |
   |                 |                 |                 | The value is a combination of the time unit (year, month, day, hour, minute, and second) and any character. |
   |                 |                 |                 |                                                                                                             |
   |                 |                 |                 | -  **YYYY** or **yyyy** indicates the year.                                                                 |
   |                 |                 |                 | -  **MM** indicates the month.                                                                              |
   |                 |                 |                 | -  **mm** indicates the minute.                                                                             |
   |                 |                 |                 | -  **dd** indicates the day.                                                                                |
   |                 |                 |                 | -  **HH** indicates the 24-hour clock.                                                                      |
   |                 |                 |                 | -  **hh** indicates the 12-hour clock.                                                                      |
   |                 |                 |                 | -  **mi** indicates the minute.                                                                             |
   |                 |                 |                 | -  **ss** indicates the second.                                                                             |
   |                 |                 |                 | -  **SSS** indicates the millisecond.                                                                       |
   +-----------------+-----------------+-----------------+-------------------------------------------------------------------------------------------------------------+

Return Values
-------------

The return value is of the BIGINT type.

.. note::

   -  If the value of **timestamp** is **NULL**, **NULL** is returned.
   -  If both **timestamp** and **pattern** are left blank, the timestamp represented by the number of seconds since 1970-01-01 00:00:00 is returned.

Example Code
------------

The value **1692149997** is returned.

.. code-block::

   select unix_timestamp('2023-08-16 09:39:57')

If the current system time is **2023-08-16 10:23:16**, **1692152596** is returned.

.. code-block::

   select unix_timestamp();

The value **1692115200** (2023-08-16 00:00:00) is returned.

.. code-block::

   select unix_timestamp("2023-08-16 10:56:45", "yyyy-MM-dd");

Example table data

.. code-block::

   select timestamp1, unix_timestamp(timestamp1) as date1_unix_timestamp, timestamp2, unix_timestamp(datetime1) as date2_unix_timestamp, timestamp3, unix_timestamp(timestamp1) as date3_unix_timestamp from database_t; output:
   +------------+-------------------------+-----------------------+---------------------- --+------------------------------------+----------------------------+
   | timestamp1| date1_unix_timestamp | timestamp2              | date2_unix_timestamp | timestamp3                                  | date3_unix_timestamp      |
   +------------+-------------------------+-----------------------+-------------------------+------------------------------------+----------------------------+
   | 2023-08-02 | 1690905600000           | 2023-08-02 11:09:14 | 1690945754793           | 2023-01-11 00:00:00.123456789 | 1673366400000                |
   | 2023-08-03 | 1690992000000           | 2023-08-02 11:09:31 | 1690945771994           | 2023-02-11 00:00:00.123456789 | 1676044800000                |
   | 2023-08-04 | 1691078400000           | 2023-08-02 11:09:41 | 1690945781270           | 2023-03-11 00:00:00.123456789 | 1678464000000                |
   | 2023-08-05 | 1691164800000           | 2023-08-02 11:09:48 | 1690945788874           | 2023-04-11 00:00:00.123456789 | 1681142400000                |
   | 2023-08-06 | 1691251200000           | 2023-08-02 11:09:59 | 1690945799099           | 2023-05-11 00:00:00.123456789 | 1683734400000                |
   +------------+-------------------------+-----------------------+--------------------------+-----------------------------------+----------------------------+
