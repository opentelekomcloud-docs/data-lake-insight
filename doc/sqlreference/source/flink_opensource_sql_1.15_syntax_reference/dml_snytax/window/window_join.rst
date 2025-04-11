:original_name: dli_08_15074.html

.. _dli_08_15074:

Window Join
===========

A window join adds the dimension of time into the join criteria themselves. By doing so, the window join joins the elements of two streams that share a common key and are in the same window. The semantic of window join is same to the **DataStream window join**.

For streaming queries, unlike other joins on continuous tables, window join does not emit intermediate results but only emits final results at the end of the window. Moreover, window join purge all intermediate state when no longer needed. Usually, Window Join is used with Windowing TVF. Besides, Window Join could follow after other operations based on Windowing TVF, such as Window Aggregation, Window TopN and Window Join. Currently, Window Join requires the join on condition contains window starts equality of input tables and window ends equality of input tables. Window Join supports **INNER**/**LEFT**/**RIGHT**/**FULL OUTER**/**ANTI**/**SEMI JOIN**.

For more information, see `Window Join <https://nightlies.apache.org/flink/flink-docs-release-1.15/zh/docs/dev/table/sql/queries/window-join/>`__.

Caveats
-------

-  Currently, the window join requires the join on condition contains window starts equality of input tables and window ends equality of input tables.
-  Currently, the windowing TVFs must be the same of left and right inputs.
-  Currently, if Window Join follows after Windowing TVF, the Windowing TVF has to be with Tumble Windows, Hop Windows or Cumulate Windows instead of Session windows.

INNER/LEFT/RIGHT/FULL OUTER
---------------------------

The syntax of INNER/LEFT/RIGHT/FULL OUTER WINDOW JOIN are very similar with each other, we only give an example for FULL OUTER JOIN here. When performing a window join, all elements with a common key and a common tumbling window are joined together. We only give an example for a Window Join which works on a Tumble Window TVF. By scoping the region of time for the join into fixed five-minute intervals, we chopped our datasets into two distinct windows of time: [12:00, 12:05) and [12:05, 12:10). The L2 and R2 rows could not join together because they fell into separate windows.

**Syntax**

.. code-block::

   SELECT ...
   FROM L [LEFT|RIGHT|FULL OUTER] JOIN R -- L and R are relations applied windowing TVF
   ON L.window_start = R.window_start AND L.window_end = R.window_end AND ...

**Example**

When performing a window join, all elements with a common key and a common tumbling window are joined together. We only give an example for a Window Join which works on a Tumble Window TVF. By scoping the region of time for the join into fixed five-minute intervals, we chopped our datasets into two distinct windows of time: [12:00, 12:05) and [12:05, 12:10). The L2 and R2 rows could not join together because they fell into separate windows.

.. code-block::

   Flink SQL> desc LeftTable;
   +----------+------------------------+------+-----+--------+----------------------------------+
   |     name |                   type | null | key | extras |                        watermark |
   +----------+------------------------+------+-----+--------+----------------------------------+
   | row_time | TIMESTAMP(3) *ROWTIME* | true |     |        | `row_time` - INTERVAL '1' SECOND |
   |      num |                    INT | true |     |        |                                  |
   |       id |                 STRING | true |     |        |                                  |
   +----------+------------------------+------+-----+--------+----------------------------------+

   Flink SQL> SELECT * FROM LeftTable;
   +------------------+-----+----+
   |         row_time | num | id |
   +------------------+-----+----+
   | 2020-04-15 12:02 |   1 | L1 |
   | 2020-04-15 12:06 |   2 | L2 |
   | 2020-04-15 12:03 |   3 | L3 |
   +------------------+-----+----+

   Flink SQL> desc RightTable;
   +----------+------------------------+------+-----+--------+----------------------------------+
   |     name |                   type | null | key | extras |                        watermark |
   +----------+------------------------+------+-----+--------+----------------------------------+
   | row_time | TIMESTAMP(3) *ROWTIME* | true |     |        | `row_time` - INTERVAL '1' SECOND |
   |      num |                    INT | true |     |        |                                  |
   |       id |                 STRING | true |     |        |                                  |
   +----------+------------------------+------+-----+--------+----------------------------------+

   Flink SQL> SELECT * FROM RightTable;
   +------------------+-----+----+
   |         row_time | num | id |
   +------------------+-----+----+
   | 2020-04-15 12:01 |   2 | R2 |
   | 2020-04-15 12:04 |   3 | R3 |
   | 2020-04-15 12:05 |   4 | R4 |
   +------------------+-----+----+

   Flink SQL> SELECT L.num as L_Num, L.id as L_Id, R.num as R_Num, R.id as R_Id,
              COALESCE(L.window_start, R.window_start) as window_start,
              COALESCE(L.window_end, R.window_end) as window_end
              FROM (
                  SELECT * FROM TABLE(TUMBLE(TABLE LeftTable, DESCRIPTOR(row_time), INTERVAL '5' MINUTES))
              ) L
              FULL JOIN (
                  SELECT * FROM TABLE(TUMBLE(TABLE RightTable, DESCRIPTOR(row_time), INTERVAL '5' MINUTES))
              ) R
              ON L.num = R.num AND L.window_start = R.window_start AND L.window_end = R.window_end;
   +-------+------+-------+------+------------------+------------------+
   | L_Num | L_Id | R_Num | R_Id |     window_start |       window_end |
   +-------+------+-------+------+------------------+------------------+
   |     1 |   L1 |  null | null | 2020-04-15 12:00 | 2020-04-15 12:05 |
   |  null | null |     2 |   R2 | 2020-04-15 12:00 | 2020-04-15 12:05 |
   |     3 |   L3 |     3 |   R3 | 2020-04-15 12:00 | 2020-04-15 12:05 |
   |     2 |   L2 |  null | null | 2020-04-15 12:05 | 2020-04-15 12:10 |
   |  null | null |     4 |   R4 | 2020-04-15 12:05 | 2020-04-15 12:10 |
   +-------+------+-------+------+------------------+------------------+

SEMI
----

Semi Window Joins returns a row from one left record if there is at least one matching row on the right side within the common window.

.. code-block::

   Flink SQL> SELECT *
              FROM (
                  SELECT * FROM TABLE(TUMBLE(TABLE LeftTable, DESCRIPTOR(row_time), INTERVAL '5' MINUTES))
              ) L WHERE L.num IN (
                SELECT num FROM (
                  SELECT * FROM TABLE(TUMBLE(TABLE RightTable, DESCRIPTOR(row_time), INTERVAL '5' MINUTES))
                ) R WHERE L.window_start = R.window_start AND L.window_end = R.window_end);
   +------------------+-----+----+------------------+------------------+-------------------------+
   |         row_time | num | id |     window_start |       window_end |            window_time  |
   +------------------+-----+----+------------------+------------------+-------------------------+
   | 2020-04-15 12:03 |   3 | L3 | 2020-04-15 12:00 | 2020-04-15 12:05 | 2020-04-15 12:04:59.999 |
   +------------------+-----+----+------------------+------------------+-------------------------+

   Flink SQL> SELECT *
              FROM (
                  SELECT * FROM TABLE(TUMBLE(TABLE LeftTable, DESCRIPTOR(row_time), INTERVAL '5' MINUTES))
              ) L WHERE EXISTS (
                SELECT * FROM (
                  SELECT * FROM TABLE(TUMBLE(TABLE RightTable, DESCRIPTOR(row_time), INTERVAL '5' MINUTES))
                ) R WHERE L.num = R.num AND L.window_start = R.window_start AND L.window_end = R.window_end);
   +------------------+-----+----+------------------+------------------+-------------------------+
   |         row_time | num | id |     window_start |       window_end |            window_time  |
   +------------------+-----+----+------------------+------------------+-------------------------+
   | 2020-04-15 12:03 |   3 | L3 | 2020-04-15 12:00 | 2020-04-15 12:05 | 2020-04-15 12:04:59.999 |
   +------------------+-----+----+------------------+------------------+-------------------------+

ANTI
----

Anti Window Joins are the obverse of the Inner Window Join: they contain all of the unjoined rows within each common window.

.. code-block::

   Flink SQL> SELECT *
              FROM (
                  SELECT * FROM TABLE(TUMBLE(TABLE LeftTable, DESCRIPTOR(row_time), INTERVAL '5' MINUTES))
              ) L WHERE L.num NOT IN (
                SELECT num FROM (
                  SELECT * FROM TABLE(TUMBLE(TABLE RightTable, DESCRIPTOR(row_time), INTERVAL '5' MINUTES))
                ) R WHERE L.window_start = R.window_start AND L.window_end = R.window_end);
   +------------------+-----+----+------------------+------------------+-------------------------+
   |         row_time | num | id |     window_start |       window_end |            window_time  |
   +------------------+-----+----+------------------+------------------+-------------------------+
   | 2020-04-15 12:02 |   1 | L1 | 2020-04-15 12:00 | 2020-04-15 12:05 | 2020-04-15 12:04:59.999 |
   | 2020-04-15 12:06 |   2 | L2 | 2020-04-15 12:05 | 2020-04-15 12:10 | 2020-04-15 12:09:59.999 |
   +------------------+-----+----+------------------+------------------+-------------------------+

   Flink SQL> SELECT *
              FROM (
                  SELECT * FROM TABLE(TUMBLE(TABLE LeftTable, DESCRIPTOR(row_time), INTERVAL '5' MINUTES))
              ) L WHERE NOT EXISTS (
                SELECT * FROM (
                  SELECT * FROM TABLE(TUMBLE(TABLE RightTable, DESCRIPTOR(row_time), INTERVAL '5' MINUTES))
                ) R WHERE L.num = R.num AND L.window_start = R.window_start AND L.window_end = R.window_end);
   +------------------+-----+----+------------------+------------------+-------------------------+
   |         row_time | num | id |     window_start |       window_end |            window_time  |
   +------------------+-----+----+------------------+------------------+-------------------------+
   | 2020-04-15 12:02 |   1 | L1 | 2020-04-15 12:00 | 2020-04-15 12:05 | 2020-04-15 12:04:59.999 |
   | 2020-04-15 12:06 |   2 | L2 | 2020-04-15 12:05 | 2020-04-15 12:10 | 2020-04-15 12:09:59.999 |
   +------------------+-----+----+------------------+------------------+-------------------------+
