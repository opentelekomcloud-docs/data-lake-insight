:original_name: dli_08_0107.html

.. _dli_08_0107:

Configuring Time Models
=======================

Flink provides two time models: processing time and event time.

DLI allows you to specify the time model during creation of the source stream and temporary stream.

Configuring Processing Time
---------------------------

Processing time refers to the system time, which is irrelevant to the data timestamp.

**Syntax**

::

   CREATE SOURCE STREAM stream_name(...) WITH (...)
   TIMESTAMP BY proctime.proctime;
   CREATE TEMP STREAM stream_name(...)
   TIMESTAMP BY proctime.proctime;

**Description**

To set the processing time, you only need to add proctime.proctime following TIMESTAMP BY. You can directly use the proctime field later.

**Precautions**

None

**Example**

::

   CREATE SOURCE STREAM student_scores (
     student_number STRING, /* Student ID */
     student_name STRING, /* Name */
     subject STRING, /* Subject */
     score INT /* Score */
   )
   WITH (
     type = "dis",
     region = "",
     channel = "dliinput",
     partition_count = "1",
     encode = "csv",
     field_delimiter=","
   )TIMESTAMP BY proctime.proctime;

   INSERT INTO score_greate_90
   SELECT student_name, sum(score) over (order by proctime RANGE UNBOUNDED PRECEDING)
   FROM student_scores;

Configuring Event Time
----------------------

Event Time refers to the time when an event is generated, that is, the timestamp generated during data generation.

**Syntax**

::

   CREATE SOURCE STREAM stream_name(...) WITH (...)
   TIMESTAMP BY {attr_name}.rowtime
   SET WATERMARK (RANGE {time_interval} | ROWS {literal}, {time_interval});

**Description**

To set the event time, you need to select a certain attribute in the stream as the timestamp and set the watermark policy.

Out-of-order events or late events may occur due to network faults. The watermark must be configured to trigger the window for calculation after waiting for a certain period of time. Watermarks are mainly used to process out-of-order data before generated events are sent to DLI during stream processing.

The following two watermark policies are available:

-  By time interval

   ::

      SET WATERMARK(range interval {time_unit}, interval {time_unit})

-  By event quantity

   ::

      SET WATERMARK(rows literal, interval {time_unit})

.. note::

   Parameters are separated by commas (,). The first parameter indicates the watermark sending interval and the second indicates the maximum event delay.

**Precautions**

None

**Example**

-  Send a watermark every 10s the **time2** event is generated. The maximum event latency is 20s.

   ::

      CREATE SOURCE STREAM student_scores (
        student_number STRING, /* Student ID */
        student_name STRING, /* Name */
        subject STRING, /* Subject */
        score INT, /* Score */
        time2 TIMESTAMP
      )
      WITH (
        type = "dis",
        region = "",
        channel = "dliinput",
        partition_count = "1",
        encode = "csv",
        field_delimiter=","
      )
      TIMESTAMP BY time2.rowtime
      SET WATERMARK (RANGE interval 10 second, interval 20 second);

      INSERT INTO score_greate_90
      SELECT student_name, sum(score) over (order by time2 RANGE UNBOUNDED PRECEDING)
      FROM student_scores;

-  Send the watermark every time when 10 pieces of data are received, and the maximum event latency is 20s.

   ::

      CREATE SOURCE STREAM student_scores (
        student_number STRING, /* Student ID */
        student_name STRING, /* Name */
        subject STRING, /* Subject */
        score INT, /* Score */
        time2 TIMESTAMP
      )
      WITH (
        type = "dis",
        region = "",
        channel = "dliinput",
        partition_count = "1",
        encode = "csv",
        field_delimiter=","
      )
      TIMESTAMP BY time2.rowtime
      SET WATERMARK (ROWS 10, interval 20 second);

      INSERT INTO score_greate_90
      SELECT student_name, sum(score) over (order by time2 RANGE UNBOUNDED PRECEDING)
      FROM student_scores;
