:original_name: dli_08_0324.html

.. _dli_08_0324:

Window
======

GROUP WINDOW
------------

**Description**

Group Window is defined in GROUP BY. One record is generated from each group. Group Window involves the following functions:

-  Array functions

   .. table:: **Table 1** Array functions

      +------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Grouping Window Function           | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
      +====================================+=========================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
      | TUMBLE(time_attr, interval)        | Defines a tumbling time window. A tumbling time window assigns rows to non-overlapping, continuous windows with a fixed duration (interval). For example, a tumbling window of 5 minutes groups rows in 5 minutes intervals. Tumbling windows can be defined on event-time (stream + batch) or processing-time (stream).                                                                                                                                                                                                                                                                                                                |
      +------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | HOP(time_attr, interval, interval) | Defines a hopping time window (called sliding window in the Table API). A hopping time window has a fixed duration (second interval parameter) and hops by a specified hop interval (first interval parameter). If the hop interval is smaller than the window size, hopping windows are overlapping. Thus, rows can be assigned to multiple windows. For example, a hopping window of 15 minutes size and 5 minute hop interval assigns each row to 3 different windows of 15 minute size, which are evaluated in an interval of 5 minutes. Hopping windows can be defined on event-time (stream + batch) or processing-time (stream). |
      +------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | SESSION(time_attr, interval)       | Defines a session time window. Session time windows do not have a fixed duration but their bounds are defined by a time interval of inactivity, i.e., a session window is closed if no event appears for a defined gap period. For example a session window with a 30 minute gap starts when a row is observed after 30 minutes inactivity (otherwise the row would be added to an existing window) and is closed if no row is added within 30 minutes. Session windows can work on event-time (stream + batch) or processing-time (stream).                                                                                            |
      +------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   Notes:

   In streaming mode, the **time_attr** argument of the group window function must refer to a valid time attribute that specifies the processing time or event time of rows.

   In batch mode, the **time_attr** argument of the group window function must be an attribute of type TIMESTAMP.

-  Window auxiliary functions

   The start and end timestamps of group windows as well as time attributes can be selected with the following auxiliary functions.

   .. table:: **Table 2** Window auxiliary functions

      +---------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Auxiliary Function                          | Description                                                                                                                                                                                                                                                                            |
      +=============================================+========================================================================================================================================================================================================================================================================================+
      | TUMBLE_START(time_attr, interval)           | Returns the timestamp of the inclusive lower bound of the corresponding tumbling, hopping, or session window.                                                                                                                                                                          |
      |                                             |                                                                                                                                                                                                                                                                                        |
      | HOP_START(time_attr, interval, interval)    |                                                                                                                                                                                                                                                                                        |
      |                                             |                                                                                                                                                                                                                                                                                        |
      | SESSION_START(time_attr, interval)          |                                                                                                                                                                                                                                                                                        |
      +---------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | TUMBLE_END(time_attr, interval)             | Returns the timestamp of the **exclusive** upper bound of the corresponding tumbling, hopping, or session window.                                                                                                                                                                      |
      |                                             |                                                                                                                                                                                                                                                                                        |
      | HOP_END(time_attr, interval, interval)      | Note: The exclusive upper bound timestamp **cannot** be used as a rowtime attribute in subsequent time-based operations, such as interval joins and group window or over window aggregations.                                                                                          |
      |                                             |                                                                                                                                                                                                                                                                                        |
      | SESSION_END(time_attr, interval)            |                                                                                                                                                                                                                                                                                        |
      +---------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | TUMBLE_ROWTIME(time_attr, interval)         | Returns the timestamp of the inclusive upper bound of the corresponding tumbling, hopping, or session window. The resulting attribute is a rowtime attribute that can be used in subsequent time-based operations such as interval joins and group window or over window aggregations. |
      |                                             |                                                                                                                                                                                                                                                                                        |
      | HOP_ROWTIME(time_attr, interval, interval)  |                                                                                                                                                                                                                                                                                        |
      |                                             |                                                                                                                                                                                                                                                                                        |
      | SESSION_ROWTIME(time_attr, interval)        |                                                                                                                                                                                                                                                                                        |
      +---------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | TUMBLE_PROCTIME(time_attr, interval)        | Returns a proctime attribute that can be used in subsequent time-based operations such as interval joins and group window or over window aggregations.                                                                                                                                 |
      |                                             |                                                                                                                                                                                                                                                                                        |
      | HOP_PROCTIME(time_attr, interval, interval) |                                                                                                                                                                                                                                                                                        |
      |                                             |                                                                                                                                                                                                                                                                                        |
      | SESSION_PROCTIME(time_attr, interval)       |                                                                                                                                                                                                                                                                                        |
      +---------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   Note: Auxiliary functions must be called with exactly same arguments as the group window function in the GROUP BY clause.

**Example**

::

   // Calculate the SUM every day (event time).
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

TUMBLE WINDOW Extension
-----------------------

**Function**

The extension functions of the DLI tumbling window are as follows:

-  Periodical tumbling windows for lower latency

   Before the tumbling window ends, the window can be periodically triggered based on the configured frequency. The compute result from the start to the current time is output, which does not affect the final output. The latest result can be viewed in each period before the window ends.

-  Custom latency for higher data accuracy

   You can set a latency for the end of the window. The output of the window is updated according to the configured latency each time a piece of late data reaches.

**Precautions**

If you use **insert** to write results into the sink, the sink must support the upsert mode.

**Syntax**

.. code-block::

   TUMBLE(time_attr, window_interval, period_interval, lateness_interval)

Example

If the current **time_attr** attribute column is **testtime** and the window interval is 10 seconds, the statement is as follows:

.. code-block::

   TUMBLE(testtime, INTERVAL '10' SECOND, INTERVAL '10' SECOND, INTERVAL '10' SECOND)

**Description**

.. table:: **Table 3** Parameter description

   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------+
   | Parameter             | Description                                                                                                                                                                                                                                                                                        | Format                                                                    |
   +=======================+====================================================================================================================================================================================================================================================================================================+===========================================================================+
   | time_attr             | Event time or processing time attribute column                                                                                                                                                                                                                                                     | ``-``                                                                     |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------+
   | window_interval       | Duration of the window                                                                                                                                                                                                                                                                             | -  Format 1: **INTERVAL** '10' **SECOND**                                 |
   |                       |                                                                                                                                                                                                                                                                                                    |                                                                           |
   |                       |                                                                                                                                                                                                                                                                                                    |    The window interval is 10 seconds. You can change the value as needed. |
   |                       |                                                                                                                                                                                                                                                                                                    |                                                                           |
   |                       |                                                                                                                                                                                                                                                                                                    | -  Format 2: **INTERVAL** '10' **MINUTE**                                 |
   |                       |                                                                                                                                                                                                                                                                                                    |                                                                           |
   |                       |                                                                                                                                                                                                                                                                                                    |    The window interval is 10 minutes. You can change the value as needed. |
   |                       |                                                                                                                                                                                                                                                                                                    |                                                                           |
   |                       |                                                                                                                                                                                                                                                                                                    | -  Format 3: **INTERVAL** '10' **DAY**                                    |
   |                       |                                                                                                                                                                                                                                                                                                    |                                                                           |
   |                       |                                                                                                                                                                                                                                                                                                    |    The window interval is 10 days. You can change the value as needed.    |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------+
   | period_interval       | Frequency of periodic triggering within the window range. That is, before the window ends, the output result is updated at an interval specified by **period_interval** from the time when the window starts. If this parameter is not set, the periodic triggering policy is not used by default. |                                                                           |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------+
   | lateness_interval     | Time to postpone the end of the window. The system continues to collect the data that reaches the window within **lateness_interval** after the window ends. The output is updated for each data that reaches the window within **lateness_interval**.                                             |                                                                           |
   |                       |                                                                                                                                                                                                                                                                                                    |                                                                           |
   |                       | .. note::                                                                                                                                                                                                                                                                                          |                                                                           |
   |                       |                                                                                                                                                                                                                                                                                                    |                                                                           |
   |                       |    If the time window is for processing time, **lateness_interval** does not take effect.                                                                                                                                                                                                          |                                                                           |
   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------+

.. note::

   Values of **period_interval** and **lateness_interval** cannot be negative numbers.

   -  If **period_interval** is set to **0**, periodic triggering is disabled for the window.
   -  If **lateness_interval** is set to **0**, the latency after the window ends is disabled.
   -  If neither of the two parameters is set, both periodic triggering and latency are disabled and only the regular tumbling window functions are available .
   -  If only the latency function needs to be used, set period_interval **INTERVAL '0' SECOND**.

OVER WINDOW
-----------

The difference between Over Window and Group Window is that one record is generated from one row in Over Window.

**Syntax**

::

   SELECT agg1(attr1) OVER (
     [PARTITION BY partition_name]
     ORDER BY proctime|rowtime
     ROWS
    BETWEEN (UNBOUNDED|rowCOUNT) PRECEDING AND CURRENT ROW FROM TABLENAME

   SELECT agg1(attr1) OVER (
     [PARTITION BY partition_name]
     ORDER BY proctime|rowtime
     RANGE
     BETWEEN (UNBOUNDED|timeInterval) PRECEDING AND CURRENT ROW FROM TABLENAME

**Description**

.. table:: **Table 4** Parameter description

   +--------------+-----------------------------------------------------------------------------------------------+
   | Parameter    | Parameter Description                                                                         |
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

-  All aggregates must be defined in the same window, that is, in the same partition, sort, and range.
-  Currently, only windows from PRECEDING (unbounded or bounded) to CURRENT ROW are supported. The range described by FOLLOWING is not supported.
-  ORDER BY must be specified for a single time attribute.

**Example**

::

   // Calculate the count and total number from syntax rules enabled to now (in proctime).
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
