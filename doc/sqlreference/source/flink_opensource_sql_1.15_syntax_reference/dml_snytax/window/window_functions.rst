:original_name: dli_08_15070.html

.. _dli_08_15070:

Window Functions
================

.. _dli_08_15070__section3516193316120:

Windowing Table-Valued Functions (Windowing TVFs)
-------------------------------------------------

Windows are at the heart of processing infinite streams. Windows split the stream into "buckets" of finite size, over which we can apply computations.

Apache Flink provides several **window table-valued functions (TVF)** to divide the elements of your table into windows, including:

-  Tumble Windows
-  Hop Windows
-  Cumulate Windows

Note that each element can logically belong to more than one window, depending on the windowing table-valued function you use. For example, HOP windowing creates overlapping windows wherein a single element can be assigned to multiple windows.

Windowing TVFs are Flink defined Polymorphic Table Functions (abbreviated PTF). PTF is part of the SQL 2016 standard, a special table-function, but can have a table as a parameter.

Windowing TVFs is a replacement of legacy Grouped Window Functions. Windowing TVFs is more SQL standard compliant and more powerful to support complex window-based computations, e.g. Window TopN, Window Join. However, Grouped Window Functions can only support Window Aggregation.

For more information, see `Window Functions <https://nightlies.apache.org/flink/flink-docs-release-1.15/zh/docs/dev/table/sql/queries/window-tvf/>`__.


Window Functions
----------------

Apache Flink provides 3 built-in windowing TVFs: **TUMBLE**, **HOP** and **CUMULATE**.

The return value of windowing TVF is a new relation that includes all columns of original relation as well as additional 3 columns named "window_start", "window_end", "window_time" to indicate the assigned window.

In batch mode, the "window_time" field is an attribute of type **TIMESTAMP** or **TIMESTAMP_LTZ** based on input time field type. The "window_time" field can be used in subsequent time-based operations, e.g. another windowing TVF, or interval joins, over aggregations. The value of window_time always equal to window_end - 1 ms.

TUMBLE
------

-  **Function**

   The **TUMBLE** function assigns each element to a window of specified window size. Tumbling windows have a fixed size and do not overlap.

   For example, suppose you specify a tumbling window with a size of 5 minutes. In that case, Flink will evaluate the current window, and a new window started every five minutes.


   .. figure:: /_static/images/en-us_image_0000001870733085.png
      :alt: **Figure 1** Tumbling window

      **Figure 1** Tumbling window

-  **Description**

   The **TUMBLE** function assigns a window for each row of a relation based on a time attribute field. In streaming mode, the time attribute field must be either event or processing time attributes. In batch mode, the time attribute field of window table function must be an attribute of type **TIMESTAMP** or **TIMESTAMP_LTZ**.

   The return value of **TUMBLE** is a new relation that includes all columns of original relation as well as additional 3 columns named "window_start", "window_end", "window_time" to indicate the assigned window. The original time attribute "timecol" will be a regular timestamp column after window TVF.

   .. code-block::

      TUMBLE(TABLE data, DESCRIPTOR(timecol), size [, offset ])

   .. table:: **Table 1** TUMBLE function parameters

      +-----------+-----------+-----------------------------------------------------------------------------------------------------------+
      | Parameter | Mandatory | Description                                                                                               |
      +===========+===========+===========================================================================================================+
      | data      | Yes       | A table parameter that can be any relation with a time attribute column.                                  |
      +-----------+-----------+-----------------------------------------------------------------------------------------------------------+
      | timecol   | Yes       | A column descriptor indicating which time attributes column of data should be mapped to tumbling windows. |
      +-----------+-----------+-----------------------------------------------------------------------------------------------------------+
      | size      | Yes       | A duration specifying the width of the tumbling windows.                                                  |
      +-----------+-----------+-----------------------------------------------------------------------------------------------------------+
      | offset    | No        | Offset which window start would be shifted by.                                                            |
      +-----------+-----------+-----------------------------------------------------------------------------------------------------------+

-  **Example**

   .. code-block::

      -- tables must have time attribute, e.g. `bidtime` in this table
      Flink SQL> desc Bid;
      +-------------+------------------------+------+-----+--------+---------------------------------+
      |        name |                   type | null | key | extras |                       watermark |
      +-------------+------------------------+------+-----+--------+---------------------------------+
      |     bidtime | TIMESTAMP(3) *ROWTIME* | true |     |        | `bidtime` - INTERVAL '1' SECOND |
      |       price |         DECIMAL(10, 2) | true |     |        |                                 |
      |        item |                 STRING | true |     |        |                                 |
      +-------------+------------------------+------+-----+--------+---------------------------------+

      Flink SQL> SELECT * FROM Bid;
      +------------------+-------+------+
      |          bidtime | price | item |
      +------------------+-------+------+
      | 2020-04-15 08:05 |  4.00 | C    |
      | 2020-04-15 08:07 |  2.00 | A    |
      | 2020-04-15 08:09 |  5.00 | D    |
      | 2020-04-15 08:11 |  3.00 | B    |
      | 2020-04-15 08:13 |  1.00 | E    |
      | 2020-04-15 08:17 |  6.00 | F    |
      +------------------+-------+------+

      Flink SQL> SELECT * FROM TABLE(
         TUMBLE(TABLE Bid, DESCRIPTOR(bidtime), INTERVAL '10' MINUTES));
      -- or with the named params
      -- note: the DATA param must be the first
      Flink SQL> SELECT * FROM TABLE(
         TUMBLE(
           DATA => TABLE Bid,
           TIMECOL => DESCRIPTOR(bidtime),
           SIZE => INTERVAL '10' MINUTES));
      +------------------+-------+------+------------------+------------------+-------------------------+
      |          bidtime | price | item |     window_start |       window_end |            window_time  |
      +------------------+-------+------+------------------+------------------+-------------------------+
      | 2020-04-15 08:05 |  4.00 | C    | 2020-04-15 08:00 | 2020-04-15 08:10 | 2020-04-15 08:09:59.999 |
      | 2020-04-15 08:07 |  2.00 | A    | 2020-04-15 08:00 | 2020-04-15 08:10 | 2020-04-15 08:09:59.999 |
      | 2020-04-15 08:09 |  5.00 | D    | 2020-04-15 08:00 | 2020-04-15 08:10 | 2020-04-15 08:09:59.999 |
      | 2020-04-15 08:11 |  3.00 | B    | 2020-04-15 08:10 | 2020-04-15 08:20 | 2020-04-15 08:19:59.999 |
      | 2020-04-15 08:13 |  1.00 | E    | 2020-04-15 08:10 | 2020-04-15 08:20 | 2020-04-15 08:19:59.999 |
      | 2020-04-15 08:17 |  6.00 | F    | 2020-04-15 08:10 | 2020-04-15 08:20 | 2020-04-15 08:19:59.999 |
      +------------------+-------+------+------------------+------------------+-------------------------+

      -- apply aggregation on the tumbling windowed table
      Flink SQL> SELECT window_start, window_end, SUM(price)
        FROM TABLE(
          TUMBLE(TABLE Bid, DESCRIPTOR(bidtime), INTERVAL '10' MINUTES))
        GROUP BY window_start, window_end;
      +------------------+------------------+-------+
      |     window_start |       window_end | price |
      +------------------+------------------+-------+
      | 2020-04-15 08:00 | 2020-04-15 08:10 | 11.00 |
      | 2020-04-15 08:10 | 2020-04-15 08:20 | 10.00 |
      +------------------+------------------+-------+

HOP
---

-  **Function**

   The **HOP** function assigns elements to windows of fixed length. Like a **TUMBLE** windowing function, the size of the windows is configured by the window size parameter. An additional window slide parameter controls how frequently a hopping window is started. Hence, hopping windows can be overlapping if the slide is smaller than the window size. In this case, elements are assigned to multiple windows.

   For example, you could have windows of size 10 minutes that slides by 5 minutes. With this, you get every 5 minutes a window that contains the events that arrived during the last 10 minutes, as depicted by the following figure.


   .. figure:: /_static/images/en-us_image_0000001827630114.png
      :alt: **Figure 2** Hopping window

      **Figure 2** Hopping window

-  **Description**

   The **HOP** function assigns windows that cover rows within the interval of size and shifting every slide based on a time attribute field. In streaming mode, the time attribute field must be either event or processing time attributes. In batch mode, the time attribute field of window table function must be an attribute of type **TIMESTAMP** or **TIMESTAMP_LTZ**.

   The return value of **HOP** is a new relation that includes all columns of original relation as well as additional 3 columns named "window_start", "window_end", "window_time" to indicate the assigned window. The original time attribute "timecol" will be a regular timestamp column after window TVF.

   .. code-block::

      HOP(TABLE data, DESCRIPTOR(timecol), slide, size [, offset ])

   .. table:: **Table 2** HOP function parameters

      +-----------+-----------+-----------------------------------------------------------------------------------------------------------+
      | Parameter | Mandatory | Description                                                                                               |
      +===========+===========+===========================================================================================================+
      | data      | Yes       | A table parameter that can be any relation with a time attribute column.                                  |
      +-----------+-----------+-----------------------------------------------------------------------------------------------------------+
      | timecol   | Yes       | A column descriptor indicating which time attributes column of data should be mapped to tumbling windows. |
      +-----------+-----------+-----------------------------------------------------------------------------------------------------------+
      | slide     | Yes       | A duration specifying the duration between the start of sequential hopping windows.                       |
      +-----------+-----------+-----------------------------------------------------------------------------------------------------------+
      | size      | Yes       | A duration specifying the width of the hopping windows.                                                   |
      +-----------+-----------+-----------------------------------------------------------------------------------------------------------+
      | offset    | No        | Offset which window start would be shifted by.                                                            |
      +-----------+-----------+-----------------------------------------------------------------------------------------------------------+

-  **Example**

   .. code-block::

      > SELECT * FROM TABLE(
          HOP(TABLE Bid, DESCRIPTOR(bidtime), INTERVAL '5' MINUTES, INTERVAL '10' MINUTES));
      -- or with the named params
      -- note: the DATA param must be the first
      > SELECT * FROM TABLE(
          HOP(
            DATA => TABLE Bid,
            TIMECOL => DESCRIPTOR(bidtime),
            SLIDE => INTERVAL '5' MINUTES,
            SIZE => INTERVAL '10' MINUTES));
      +------------------+-------+------+------------------+------------------+-------------------------+
      |          bidtime | price | item |     window_start |       window_end |           window_time   |
      +------------------+-------+------+------------------+------------------+-------------------------+
      | 2020-04-15 08:05 |  4.00 | C    | 2020-04-15 08:00 | 2020-04-15 08:10 | 2020-04-15 08:09:59.999 |
      | 2020-04-15 08:05 |  4.00 | C    | 2020-04-15 08:05 | 2020-04-15 08:15 | 2020-04-15 08:14:59.999 |
      | 2020-04-15 08:07 |  2.00 | A    | 2020-04-15 08:00 | 2020-04-15 08:10 | 2020-04-15 08:09:59.999 |
      | 2020-04-15 08:07 |  2.00 | A    | 2020-04-15 08:05 | 2020-04-15 08:15 | 2020-04-15 08:14:59.999 |
      | 2020-04-15 08:09 |  5.00 | D    | 2020-04-15 08:00 | 2020-04-15 08:10 | 2020-04-15 08:09:59.999 |
      | 2020-04-15 08:09 |  5.00 | D    | 2020-04-15 08:05 | 2020-04-15 08:15 | 2020-04-15 08:14:59.999 |
      | 2020-04-15 08:11 |  3.00 | B    | 2020-04-15 08:05 | 2020-04-15 08:15 | 2020-04-15 08:14:59.999 |
      | 2020-04-15 08:11 |  3.00 | B    | 2020-04-15 08:10 | 2020-04-15 08:20 | 2020-04-15 08:19:59.999 |
      | 2020-04-15 08:13 |  1.00 | E    | 2020-04-15 08:05 | 2020-04-15 08:15 | 2020-04-15 08:14:59.999 |
      | 2020-04-15 08:13 |  1.00 | E    | 2020-04-15 08:10 | 2020-04-15 08:20 | 2020-04-15 08:19:59.999 |
      | 2020-04-15 08:17 |  6.00 | F    | 2020-04-15 08:10 | 2020-04-15 08:20 | 2020-04-15 08:19:59.999 |
      | 2020-04-15 08:17 |  6.00 | F    | 2020-04-15 08:15 | 2020-04-15 08:25 | 2020-04-15 08:24:59.999 |
      +------------------+-------+------+------------------+------------------+-------------------------+

      -- apply aggregation on the hopping windowed table
      > SELECT window_start, window_end, SUM(price)
        FROM TABLE(
          HOP(TABLE Bid, DESCRIPTOR(bidtime), INTERVAL '5' MINUTES, INTERVAL '10' MINUTES))
        GROUP BY window_start, window_end;
      +------------------+------------------+-------+
      |     window_start |       window_end | price |
      +------------------+------------------+-------+
      | 2020-04-15 08:00 | 2020-04-15 08:10 | 11.00 |
      | 2020-04-15 08:05 | 2020-04-15 08:15 | 15.00 |
      | 2020-04-15 08:10 | 2020-04-15 08:20 | 10.00 |
      | 2020-04-15 08:15 | 2020-04-15 08:25 |  6.00 |
      +------------------+------------------+-------+

CUMULATE
--------

-  Function

   Cumulating windows are very useful in some scenarios, such as tumbling windows with early firing in a fixed window interval. For example, a daily dashboard draws cumulative UVs from 00:00 to every minute, the UV at 10:00 represents the total number of UV from 00:00 to 10:00. This can be easily and efficiently implemented by CUMULATE windowing.

   The **CUMULATE** function assigns elements to windows that cover rows within an initial interval of step size and expand to one more step size (keep window start fixed) every step until the max window size. You can think **CUMULATE** function as applying **TUMBLE** windowing with max window size first, and split each tumbling windows into several windows with same window start and window ends of step-size difference. So cumulating windows do overlap and do not have a fixed size.

   For example, you could have a cumulating window for 1 hour step and 1 day max size, and you will get windows: [00:00, 01:00), [00:00, 02:00), [00:00, 03:00), …, [00:00, 24:00) for every day.


   .. figure:: /_static/images/en-us_image_0000001874189597.png
      :alt: **Figure 3** Cumulating window

      **Figure 3** Cumulating window

-  **Description**

   The **CUMULATE** functions assigns windows based on a time attribute column. In streaming mode, the time attribute field must be either event or processing time attributes. In batch mode, the time attribute field of window table function must be an attribute of type **TIMESTAMP** or **TIMESTAMP_LTZ**.

   The return value of **CUMULATE** is a new relation that includes all columns of original relation as well as additional 3 columns named "window_start", "window_end", "window_time" to indicate the assigned window. The original time attribute "timecol" will be a regular timestamp column after window TVF.

   .. code-block::

      CUMULATE(TABLE data, DESCRIPTOR(timecol), step, size)

   .. table:: **Table 3** CUMULATE function parameters

      +-----------+-----------+-------------------------------------------------------------------------------------------------------------+
      | Parameter | Mandatory | Description                                                                                                 |
      +===========+===========+=============================================================================================================+
      | data      | Yes       | A table parameter that can be any relation with a time attribute column.                                    |
      +-----------+-----------+-------------------------------------------------------------------------------------------------------------+
      | timecol   | Yes       | A column descriptor indicating which time attributes column of data should be mapped to cumulating windows. |
      +-----------+-----------+-------------------------------------------------------------------------------------------------------------+
      | step      | Yes       | A duration specifying the increased window size between the end of sequential cumulating windows.           |
      +-----------+-----------+-------------------------------------------------------------------------------------------------------------+
      | size      | Yes       | A duration specifying the width of the cumulating windows.                                                  |
      +-----------+-----------+-------------------------------------------------------------------------------------------------------------+
      | offset    | No        | Offset which window start would be shifted by.                                                              |
      +-----------+-----------+-------------------------------------------------------------------------------------------------------------+

-  **Example**

   .. code-block::

      > SELECT * FROM TABLE(
          CUMULATE(TABLE Bid, DESCRIPTOR(bidtime), INTERVAL '2' MINUTES, INTERVAL '10' MINUTES));
      -- or with the named params
      -- note: the DATA param must be the first
      > SELECT * FROM TABLE(
          CUMULATE(
            DATA => TABLE Bid,
            TIMECOL => DESCRIPTOR(bidtime),
            STEP => INTERVAL '2' MINUTES,
            SIZE => INTERVAL '10' MINUTES));
      +------------------+-------+------+------------------+------------------+-------------------------+
      |          bidtime | price | item |     window_start |       window_end |            window_time  |
      +------------------+-------+------+------------------+------------------+-------------------------+
      | 2020-04-15 08:05 |  4.00 | C    | 2020-04-15 08:00 | 2020-04-15 08:06 | 2020-04-15 08:05:59.999 |
      | 2020-04-15 08:05 |  4.00 | C    | 2020-04-15 08:00 | 2020-04-15 08:08 | 2020-04-15 08:07:59.999 |
      | 2020-04-15 08:05 |  4.00 | C    | 2020-04-15 08:00 | 2020-04-15 08:10 | 2020-04-15 08:09:59.999 |
      | 2020-04-15 08:07 |  2.00 | A    | 2020-04-15 08:00 | 2020-04-15 08:08 | 2020-04-15 08:07:59.999 |
      | 2020-04-15 08:07 |  2.00 | A    | 2020-04-15 08:00 | 2020-04-15 08:10 | 2020-04-15 08:09:59.999 |
      | 2020-04-15 08:09 |  5.00 | D    | 2020-04-15 08:00 | 2020-04-15 08:10 | 2020-04-15 08:09:59.999 |
      | 2020-04-15 08:11 |  3.00 | B    | 2020-04-15 08:10 | 2020-04-15 08:12 | 2020-04-15 08:11:59.999 |
      | 2020-04-15 08:11 |  3.00 | B    | 2020-04-15 08:10 | 2020-04-15 08:14 | 2020-04-15 08:13:59.999 |
      | 2020-04-15 08:11 |  3.00 | B    | 2020-04-15 08:10 | 2020-04-15 08:16 | 2020-04-15 08:15:59.999 |
      | 2020-04-15 08:11 |  3.00 | B    | 2020-04-15 08:10 | 2020-04-15 08:18 | 2020-04-15 08:17:59.999 |
      | 2020-04-15 08:11 |  3.00 | B    | 2020-04-15 08:10 | 2020-04-15 08:20 | 2020-04-15 08:19:59.999 |
      | 2020-04-15 08:13 |  1.00 | E    | 2020-04-15 08:10 | 2020-04-15 08:14 | 2020-04-15 08:13:59.999 |
      | 2020-04-15 08:13 |  1.00 | E    | 2020-04-15 08:10 | 2020-04-15 08:16 | 2020-04-15 08:15:59.999 |
      | 2020-04-15 08:13 |  1.00 | E    | 2020-04-15 08:10 | 2020-04-15 08:18 | 2020-04-15 08:17:59.999 |
      | 2020-04-15 08:13 |  1.00 | E    | 2020-04-15 08:10 | 2020-04-15 08:20 | 2020-04-15 08:19:59.999 |
      | 2020-04-15 08:17 |  6.00 | F    | 2020-04-15 08:10 | 2020-04-15 08:18 | 2020-04-15 08:17:59.999 |
      | 2020-04-15 08:17 |  6.00 | F    | 2020-04-15 08:10 | 2020-04-15 08:20 | 2020-04-15 08:19:59.999 |
      +------------------+-------+------+------------------+------------------+-------------------------+

      -- apply aggregation on the cumulating windowed table
      > SELECT window_start, window_end, SUM(price)
        FROM TABLE(
          CUMULATE(TABLE Bid, DESCRIPTOR(bidtime), INTERVAL '2' MINUTES, INTERVAL '10' MINUTES))
        GROUP BY window_start, window_end;
      +------------------+------------------+-------+
      |     window_start |       window_end | price |
      +------------------+------------------+-------+
      | 2020-04-15 08:00 | 2020-04-15 08:06 |  4.00 |
      | 2020-04-15 08:00 | 2020-04-15 08:08 |  6.00 |
      | 2020-04-15 08:00 | 2020-04-15 08:10 | 11.00 |
      | 2020-04-15 08:10 | 2020-04-15 08:12 |  3.00 |
      | 2020-04-15 08:10 | 2020-04-15 08:14 |  4.00 |
      | 2020-04-15 08:10 | 2020-04-15 08:16 |  4.00 |
      | 2020-04-15 08:10 | 2020-04-15 08:18 | 10.00 |
      | 2020-04-15 08:10 | 2020-04-15 08:20 | 10.00 |
      +------------------+------------------+-------+

Window Offset
-------------

**Offset** is an optional parameter which could be used to change the window assignment. It could be positive duration and negative duration. Default values for window offset is **0**. The same record maybe assigned to the different window if set different offset value. For example, which window would be assigned to for a record with timestamp 2021-06-30 00:00:04 for a Tumble window with 10 MINUTE as size?

-  If **offset** value is **-16** MINUTE, the record assigns to window [2021-06-29 23:54:00, 2021-06-30 00:04:00).
-  If **offset** value is **-6** MINUTE, the record assigns to window [2021-06-29 23:54:00, 2021-06-30 00:04:00).
-  If **offset** is **-4** MINUTE, the record assigns to window [2021-06-29 23:56:00, 2021-06-30 00:06:00).
-  If **offset** is **0**, the record assigns to window [2021-06-30 00:00:00, 2021-06-30 00:10:00).
-  If **offset** value is **4** MINUTE, the record assigns to window [2021-06-29 23:54:00, 2021-06-30 00:04:00).
-  If **offset** is **6** MINUTE, the record assigns to window [2021-06-29 23:56:00, 2021-06-30 00:06:00).
-  If **offset** is **16** MINUTE, the record assigns to window [2021-06-29 23:56:00, 2021-06-30 00:06:00). We could find that, some windows offset parameters may have same effect on the assignment of windows. In the above case, **-16** MINUTE, **-6** MINUTE and **4** MINUTE have same effect for a Tumble window with 10 MINUTE as size.

.. note::

   The effect of window offset is just for updating window assignment, it has no effect on Watermark.

.. code-block::

   -- NOTE: Currently Flink doesn't support evaluating individual window table-valued function,
   --  window table-valued function should be used with aggregate operation,
   --  this example is just used for explaining the syntax and the data produced by table-valued function.
   Flink SQL> SELECT * FROM TABLE(
      TUMBLE(TABLE Bid, DESCRIPTOR(bidtime), INTERVAL '10' MINUTES, INTERVAL '1' MINUTES));
   -- or with the named params
   -- note: the DATA param must be the first
   Flink SQL> SELECT * FROM TABLE(
      TUMBLE(
        DATA => TABLE Bid,
        TIMECOL => DESCRIPTOR(bidtime),
        SIZE => INTERVAL '10' MINUTES,
        OFFSET => INTERVAL '1' MINUTES));
   +------------------+-------+------+------------------+------------------+-------------------------+
   |          bidtime | price | item |     window_start |       window_end |            window_time  |
   +------------------+-------+------+------------------+------------------+-------------------------+
   | 2020-04-15 08:05 |  4.00 | C    | 2020-04-15 08:01 | 2020-04-15 08:11 | 2020-04-15 08:10:59.999 |
   | 2020-04-15 08:07 |  2.00 | A    | 2020-04-15 08:01 | 2020-04-15 08:11 | 2020-04-15 08:10:59.999 |
   | 2020-04-15 08:09 |  5.00 | D    | 2020-04-15 08:01 | 2020-04-15 08:11 | 2020-04-15 08:10:59.999 |
   | 2020-04-15 08:11 |  3.00 | B    | 2020-04-15 08:11 | 2020-04-15 08:21 | 2020-04-15 08:20:59.999 |
   | 2020-04-15 08:13 |  1.00 | E    | 2020-04-15 08:11 | 2020-04-15 08:21 | 2020-04-15 08:20:59.999 |
   | 2020-04-15 08:17 |  6.00 | F    | 2020-04-15 08:11 | 2020-04-15 08:21 | 2020-04-15 08:20:59.999 |
   +------------------+-------+------+------------------+------------------+-------------------------+

   -- apply aggregation on the tumbling windowed table
   Flink SQL> SELECT window_start, window_end, SUM(price)
     FROM TABLE(
       TUMBLE(TABLE Bid, DESCRIPTOR(bidtime), INTERVAL '10' MINUTES, INTERVAL '1' MINUTES))
     GROUP BY window_start, window_end;
   +------------------+------------------+-------+
   |     window_start |       window_end | price |
   +------------------+------------------+-------+
   | 2020-04-15 08:01 | 2020-04-15 08:11 | 11.00 |
   | 2020-04-15 08:11 | 2020-04-15 08:21 | 10.00 |
   +------------------+------------------+-------+
