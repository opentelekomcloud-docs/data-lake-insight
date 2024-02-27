:original_name: dli_08_0218.html

.. _dli_08_0218:

Window
======

GROUP WINDOW
------------

**Description**

Group Window is defined in GROUP BY. One record is generated from each group. Group Window involves the following functions:

.. note::

   -  **time_attr** can be **processing-time** or **event-time**.

      -  **event-time**: Specify the data type to **bigint** or **timestamp**.
      -  **processing-time**: No need to specify the type.

   -  **interval** specifies the window period.

-  Array functions

   .. table:: **Table 1** Array functions

      +------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
      | Function Name                      | Description                                                                                                                                 |
      +====================================+=============================================================================================================================================+
      | TUMBLE(time_attr, interval)        | Indicates the tumble window.                                                                                                                |
      +------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
      | HOP(time_attr, interval, interval) | Indicates the extended tumble window (similar to the datastream sliding window). You can set the output triggering cycle and window period. |
      +------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
      | SESSION(time_attr, interval)       | Indicates the session window. A session window will be closed if no response is returned within a duration specified by **interval**.       |
      +------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+

-  Window functions

   .. table:: **Table 2** Window functions

      +------------------------------------------+--------------------------------------------------------------------------------------------------------+
      | Function Name                            | Description                                                                                            |
      +==========================================+========================================================================================================+
      | TUMBLE_START(time_attr, interval)        | Indicates the start time of returning to the tumble window. The parameter is a UTC time zone.          |
      +------------------------------------------+--------------------------------------------------------------------------------------------------------+
      | TUMBLE_END(time_attr, interval)          | Indicates the end time of returning to the tumble window. The parameter is a UTC time zone.            |
      +------------------------------------------+--------------------------------------------------------------------------------------------------------+
      | HOP_START(time_attr, interval, interval) | Indicates the start time of returning to the extended tumble window. The parameter is a UTC time zone. |
      +------------------------------------------+--------------------------------------------------------------------------------------------------------+
      | HOP_END(time_attr, interval, interval)   | Indicates the end time of returning to the extended tumble window. The parameter is a UTC time zone.   |
      +------------------------------------------+--------------------------------------------------------------------------------------------------------+
      | SESSION_START(time_attr, interval)       | Indicates the start time of returning to the session window. The parameter is a UTC time zone.         |
      +------------------------------------------+--------------------------------------------------------------------------------------------------------+
      | SESSION_END(time_attr, interval)         | Indicates the end time of returning to the session window. The parameter is a UTC time zone.           |
      +------------------------------------------+--------------------------------------------------------------------------------------------------------+

**Example**

::

   //Calculate the SUM every day (event time).
   insert into temp SELECT name,
       TUMBLE_START(ts, INTERVAL '1' DAY) as wStart,
       SUM(amount)
       FROM Orders
       GROUP BY TUMBLE(ts, INTERVAL '1' DAY), name;

   //Calculate the SUM every day (processing time).
   insert into temp SELECT name,
       SUM(amount)
       FROM Orders
       GROUP BY TUMBLE(proctime, INTERVAL '1' DAY), name;

   //Calculate the SUM over the recent 24 hours every hour (event time).
   insert into temp SELECT product,
       SUM(amount)
       FROM Orders
       GROUP BY HOP(ts, INTERVAL '1' HOUR, INTERVAL '1' DAY), product;

   //Calculate the SUM of each session and an inactive interval every 12 hours (event time).
   insert into temp SELECT name,
       SESSION_START(ts, INTERVAL '12' HOUR) AS sStart,
       SESSION_END(ts, INTERVAL '12' HOUR) AS sEnd,
       SUM(amount)
       FROM Orders
       GROUP BY SESSION(ts, INTERVAL '12' HOUR), name;

OVER WINDOW
-----------

The difference between Over Window and Group Window is that one record is generated from one row in Over Window.

**Syntax**

::

   OVER (
     [PARTITION BY partition_name]
     ORDER BY proctime|rowtime(ROWS number PRECEDING) |(RANGE (BETWEEN INTERVAL '1' SECOND PRECEDING AND CURRENT ROW | UNBOUNDED preceding))
   )

**Description**

.. table:: **Table 3** Parameters

   +--------------+-----------------------------------------------------------------------------------------------+
   | Parameter    | Description                                                                                   |
   +==============+===============================================================================================+
   | PARTITION BY | Indicates the primary key of the specified group. Each group separately performs calculation. |
   +--------------+-----------------------------------------------------------------------------------------------+
   | ORDER BY     | Indicates the processing time or event time as the timestamp for data.                        |
   +--------------+-----------------------------------------------------------------------------------------------+
   | ROWS         | Indicates the count window.                                                                   |
   +--------------+-----------------------------------------------------------------------------------------------+
   | RANGE        | Indicates the time window.                                                                    |
   +--------------+-----------------------------------------------------------------------------------------------+

**Precautions**

-  In the same SELECT statement, windows defined by aggregate functions must be the same.
-  Currently, Over Window only supports forward calculation (preceding).
-  The value of **ORDER BY** must be specified as **processing time** or **event time**.
-  Constants do not support aggregation, such as sum(2).

**Example**

::

   //Calculate the count and total number from syntax rules enabled to now (in proctime).
   insert into temp SELECT name,
       count(amount) OVER (PARTITION BY name ORDER BY proctime RANGE UNBOUNDED preceding) as cnt1,
       sum(amount) OVER (PARTITION BY name ORDER BY proctime RANGE UNBOUNDED preceding) as cnt2
       FROM Orders;

   //Calculate the count and total number of the recent four records (in proctime).
   insert into temp SELECT name,
       count(amount) OVER (PARTITION BY name ORDER BY proctime ROWS BETWEEN 4 PRECEDING AND CURRENT ROW) as cnt1,
       sum(amount) OVER (PARTITION BY name ORDER BY proctime ROWS BETWEEN 4 PRECEDING AND CURRENT ROW) as cnt2
       FROM Orders;

   //Calculate the count and total number last 60s (in eventtime). Process the events based on event time, which is the timeattr field in Orders.
   insert into temp SELECT name,
       count(amount) OVER (PARTITION BY name ORDER BY timeattr RANGE BETWEEN INTERVAL '60' SECOND PRECEDING AND CURRENT ROW) as cnt1,
       sum(amount) OVER (PARTITION BY name ORDER BY timeattr RANGE BETWEEN INTERVAL '60' SECOND PRECEDING AND CURRENT ROW) as cnt2
       FROM Orders;
