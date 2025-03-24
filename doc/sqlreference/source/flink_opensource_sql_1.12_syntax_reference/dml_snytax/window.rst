:original_name: dli_08_0419.html

.. _dli_08_0419:

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

   .. caution::

      In streaming mode, the **time_attr** argument of the group window function must refer to a valid time attribute that specifies the processing time or event time of rows.

      -  **event-time**: The type is timestamp(3).
      -  **processing-time**: No need to specify the type.

      In batch mode, the **time_attr** argument of the group window function must be an attribute of type timestamp.

-  Window helper functions

   You can use the following helper functions to select the start and end timestamps, as well as the time attribute, for grouping windows.

   .. table:: **Table 2** Window helper functions

      +---------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Helper Function                             | Description                                                                                                                                                                                                                                                                            |
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

   Note: When calling helper functions, it is important to use the same parameters as those used in the **GROUP BY** clause for grouping window functions.

**Example**

::

   // Calculate the SUM every day (event time).
   insert into temp SELECT name,
       TUMBLE_START(ts, INTERVAL '1' DAY) as wStart,
       SUM(amount)
       FROM Orders
       GROUP BY TUMBLE(ts, INTERVAL '1' DAY), name;

   // Calculate the SUM every day (processing time).
   insert into temp SELECT name,
       SUM(amount)
       FROM Orders
       GROUP BY TUMBLE(proctime, INTERVAL '1' DAY), name;

   // Calculate the SUM over the recent 24 hours every hour (event time).
   insert into temp SELECT product,
       SUM(amount)
       FROM Orders
       GROUP BY HOP(ts, INTERVAL '1' HOUR, INTERVAL '1' DAY), product;

   // Calculate the SUM of each session and an inactive interval every 12 hours (event time).
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

-  A tumbling window is triggered periodically to reduce latency.

   Before the tumbling window ends, the window can be periodically triggered based on the configured frequency. The compute result from the start to the current time is output, which does not affect the final output. The latest result can be viewed in each period before the window ends.

-  Data accuracy is improved.

   You can set a latency for the end of the window. The output of the window is updated according to the configured latency each time a piece of late data reaches.

**Precautions**

-  If you use the INSERT statement to write results to a sink, it must support the upsert mode. Ensure that the result table supports upsert operations and the primary key is defined.

-  Latency settings only take effect for event time and not for proctime.

-  When calling helper functions, it is important to use the same parameters as those used in the **GROUP BY** clause for grouping window functions.

-  If event time is used, watermark must be used. The code is as follows (**order_time** is identified as the event time column and watermark is set to 3 seconds):

   .. code-block::

      CREATE TABLE orders (
        order_id string,
        order_channel string,
        order_time timestamp(3),
        pay_amount double,
        real_pay double,
        pay_time string,
        user_id string,
        user_name string,
        area_id string,
        watermark for order_time as order_time - INTERVAL '3' SECOND
      ) WITH (
        'connector' = 'kafka',
        'topic' = '<yourTopic>',
        'properties.bootstrap.servers' = '<yourKafka>:<port>',
        'properties.group.id' = '<yourGroupId>',
        'scan.startup.mode' = 'latest-offset',
        'format' = 'json'
      );

-  If the proctime is used, you need to use the computed column. The code is as follows (**proc** is the processing time column):

   .. code-block::

      CREATE TABLE orders (
        order_id string,
        order_channel string,
        order_time timestamp(3),
        pay_amount double,
        real_pay double,
        pay_time string,
        user_id string,
        user_name string,
        area_id string,
        proc as proctime()
      ) WITH (
        'connector' = 'kafka',
        'topic' = '<yourTopic>',
        'properties.bootstrap.servers' = '<yourKafka>:<port>',
        'properties.group.id' = '<yourGroupId>',
        'scan.startup.mode' = 'latest-offset',
        'format' = 'json'
      );

**Syntax**

.. code-block::

   TUMBLE(time_attr, window_interval, period_interval, lateness_interval)

**Example**

The current time attribute column is **testtime**, the window interval is 10 seconds, and the latency is 10 seconds.

.. code-block::

   TUMBLE(testtime, INTERVAL '10' SECOND, INTERVAL '10' SECOND, INTERVAL '10' SECOND)

**Description**

.. table:: **Table 3** Parameters

   +-----------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------+
   | Parameter             | Description                                                                                                                                                                                                                                                                                        | Format                                                                    |
   +=======================+====================================================================================================================================================================================================================================================================================================+===========================================================================+
   | time_attr             | Event time or processing time attribute column                                                                                                                                                                                                                                                     | ``-``                                                                     |
   |                       |                                                                                                                                                                                                                                                                                                    |                                                                           |
   |                       | -  **event-time**: The type is timestamp(3).                                                                                                                                                                                                                                                       |                                                                           |
   |                       | -  **processing-time**: No need to specify the type.                                                                                                                                                                                                                                               |                                                                           |
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

**Helper Functions**

.. table:: **Table 4** Helper functions

   +------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+
   | Helper Function                                                              | Description                                                                              |
   +==============================================================================+==========================================================================================+
   | TUMBLE_START(time_attr, window_interval, period_interval, lateness_interval) | Returns the timestamp of the inclusive lower bound of the corresponding tumbling window. |
   +------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+
   | TUMBLE_END(time_attr, window_interval, period_interval, lateness_interval)   | Returns the timestamp of the exclusive upper bound of the corresponding tumbling window. |
   +------------------------------------------------------------------------------+------------------------------------------------------------------------------------------+

**Example**

1. The Kafka is used as the data source table containing the order information, and the JDBC is used as the data result table for statistics on the number of orders settled by a user within 30 seconds. The order ID and window opening time are used as primary keys to collect result statistics in real time to JDBC.

#. Create a datasource connection for the communication with the VPC and subnet where MySQL and Kafka locate and bind the connection to the queue. Set an inbound rule for the security group to allow access of the queue, and test the connectivity of the queue using the MySQL and Kafka addresses. If the connection is successful, the datasource is bound to the queue. Otherwise, the binding fails.

#. Run the following statement to create the **order_count** table in the MySQL Flink database:

   .. code-block::

      CREATE TABLE `flink`.`order_count` (
          `user_id` VARCHAR(32) NOT NULL,
          `window_start` TIMESTAMP NOT NULL,
          `window_end` TIMESTAMP NULL,
          `total_num` BIGINT UNSIGNED NULL,
          PRIMARY KEY (`user_id`, `window_start`)
      )   ENGINE = InnoDB
          DEFAULT CHARACTER SET = utf8mb4
          COLLATE = utf8mb4_general_ci;

#. Create a Flink OpenSource SQL job and submit the job. In this example, the window size is 30 seconds, the triggering period is 10 seconds, and the latency is 5 seconds. That is, if the result is updated before the window ends, the intermediate result will be output every 10 seconds. After the watermark is reached and the window ends, the data whose event time is within 5 seconds of the watermark will still be processed and counted in the current window. If the event time exceeds 5 seconds of the watermark, the data will be discarded.

   .. code-block::

      CREATE TABLE orders (
        order_id string,
        order_channel string,
        order_time timestamp(3),
        pay_amount double,
        real_pay double,
        pay_time string,
        user_id string,
        user_name string,
        area_id string,
        watermark for order_time as order_time - INTERVAL '3' SECOND
      ) WITH (
        'connector' = 'kafka',
        'topic' = '<yourTopic>',
        'properties.bootstrap.servers' = '<yourKafka>:<port>',
        'properties.group.id' = '<yourGroupId>',
        'scan.startup.mode' = 'latest-offset',
        'format' = 'json'
      );

      CREATE TABLE jdbcSink (
        user_id string,
        window_start timestamp(3),
        window_end timestamp(3),
        total_num BIGINT,
        primary key (user_id, window_start) not enforced
      ) WITH (
        'connector' = 'jdbc',
        'url' = 'jdbc:mysql://<yourMySQL>:3306/flink',
        'table-name' = 'order_count',
        'username' = '<yourUserName>',
        'password' = '<yourPassword>',
        'sink.buffer-flush.max-rows' = '1'
      );

      insert into jdbcSink select
          order_id,
          TUMBLE_START(order_time, INTERVAL '30' SECOND, INTERVAL '10' SECOND, INTERVAL '5' SECOND),
          TUMBLE_END(order_time, INTERVAL '30' SECOND, INTERVAL '10' SECOND, INTERVAL '5' SECOND),
          COUNT(*) from orders
          GROUP BY user_id, TUMBLE(order_time, INTERVAL '30' SECOND, INTERVAL '10' SECOND, INTERVAL '5' SECOND);

#. Insert data to Kafka. Assume that orders are settled at different time and the order data at 10:00:13 arrives late.

   .. code-block::

      {"order_id":"202103241000000001", "order_channel":"webShop", "order_time":"2021-03-24 10:00:00", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      {"order_id":"202103241000000002", "order_channel":"webShop", "order_time":"2021-03-24 10:00:20", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      {"order_id":"202103241000000003", "order_channel":"webShop", "order_time":"2021-03-24 10:00:33", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

      {"order_id":"202103241000000004", "order_channel":"webShop", "order_time":"2021-03-24 10:00:13", "pay_amount":"100.00", "real_pay":"100.00", "pay_time":"2021-03-24 10:02:03", "user_id":"0001", "user_name":"Alice", "area_id":"330106"}

#. Run the following statement in the MySQL database to view the output result. The final result is displayed as follows because the periodic output result cannot be collected:

   .. code-block::

      select * from order_count

   .. code-block::

      user_id      window_start         window_end        total_num
      0001      2021-03-24 10:00:00  2021-03-24 10:00:30    3
      0001      2021-03-24 10:00:30  2021-03-24 10:01:00    1

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

.. table:: **Table 5** Parameters

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

   // Calculate the count and total number of the recent four records (in proctime).
   insert into temp SELECT name,
       count(amount) OVER (PARTITION BY name ORDER BY proctime ROWS BETWEEN 4 PRECEDING AND CURRENT ROW) as cnt1,
       sum(amount) OVER (PARTITION BY name ORDER BY proctime ROWS BETWEEN 4 PRECEDING AND CURRENT ROW) as cnt2
       FROM Orders;

   // Calculate the count and total number last 60s (in eventtime). Process the events based on event time, which is the timeattr field in Orders.
   insert into temp SELECT name,
       count(amount) OVER (PARTITION BY name ORDER BY timeattr RANGE BETWEEN INTERVAL '60' SECOND PRECEDING AND CURRENT ROW) as cnt1,
       sum(amount) OVER (PARTITION BY name ORDER BY timeattr RANGE BETWEEN INTERVAL '60' SECOND PRECEDING AND CURRENT ROW) as cnt2
       FROM Orders;
