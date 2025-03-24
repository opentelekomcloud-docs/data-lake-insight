:original_name: dli_08_15071.html

.. _dli_08_15071:

Window Aggregation
==================

Window TVF Aggregation
----------------------

Window aggregations are defined in the **GROUP BY** clause contains "window_start" and "window_end" columns of the relation applied :ref:`Windowing TVF <dli_08_15070__section3516193316120>`. Just like queries with regular **GROUP BY** clauses, queries with a group by window aggregation will compute a single result row per group. Unlike other aggregations on continuous tables, window aggregation do not emit intermediate results but only a final result, the total aggregation at the end of the window. Moreover, window aggregations purge all intermediate state when no longer needed.

For more information, see `Window Aggregation <https://nightlies.apache.org/flink/flink-docs-release-1.15/zh/docs/dev/table/sql/queries/window-agg/>`__.

.. note::

   The start and end timestamps of group windows can be selected with the grouped **window_start** and **window_end** columns.

-  **Windowing TVFs**

   Flink supports **TUMBLE**, **HOP** and **CUMULATE** types of window aggregations.

   -  In streaming mode, the time attribute field of a window table-valued function must be on either event or processing time attributes. See :ref:`Windowing TVF <dli_08_15070__section3516193316120>` for more windowing functions information.
   -  In batch mode, the time attribute field of a window table-valued function must be an attribute of type **TIMESTAMP** or **TIMESTAMP_LTZ**.

   .. code-block::

      -- tables must have time attribute, e.g. `bidtime` in this table
      Flink SQL> desc Bid;
      +-------------+------------------------+------+-----+--------+---------------------------------+
      |        name |                   type | null | key | extras |                       watermark |
      +-------------+------------------------+------+-----+--------+---------------------------------+
      |     bidtime | TIMESTAMP(3) *ROWTIME* | true |     |        | `bidtime` - INTERVAL '1' SECOND |
      |       price |         DECIMAL(10, 2) | true |     |        |                                 |
      |        item |                 STRING | true |     |        |                                 |
      | supplier_id |                 STRING | true |     |        |                                 |
      +-------------+------------------------+------+-----+--------+---------------------------------+

      Flink SQL> SELECT * FROM Bid;
      +------------------+-------+------+-------------+
      |          bidtime | price | item | supplier_id |
      +------------------+-------+------+-------------+
      | 2020-04-15 08:05 | 4.00  | C    | supplier1   |
      | 2020-04-15 08:07 | 2.00  | A    | supplier1   |
      | 2020-04-15 08:09 | 5.00  | D    | supplier2   |
      | 2020-04-15 08:11 | 3.00  | B    | supplier2   |
      | 2020-04-15 08:13 | 1.00  | E    | supplier1   |
      | 2020-04-15 08:17 | 6.00  | F    | supplier2   |
      +------------------+-------+------+-------------+

      -- tumbling window aggregation
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

      -- hopping window aggregation
      Flink SQL> SELECT window_start, window_end, SUM(price)
        FROM TABLE(
          HOP(TABLE Bid, DESCRIPTOR(bidtime), INTERVAL '5' MINUTES, INTERVAL '10' MINUTES))
        GROUP BY window_start, window_end;
      +------------------+------------------+-------+
      |     window_start |       window_end | price |
      +------------------+------------------+-------+
      | 2020-04-15 08:00 | 2020-04-15 08:10 | 11.00 |
      | 2020-04-15 08:05 | 2020-04-15 08:15 | 15.00 |
      | 2020-04-15 08:10 | 2020-04-15 08:20 | 10.00 |
      | 2020-04-15 08:15 | 2020-04-15 08:25 | 6.00  |
      +------------------+------------------+-------+

      -- cumulative window aggregation
      Flink SQL> SELECT window_start, window_end, SUM(price)
        FROM TABLE(
          CUMULATE(TABLE Bid, DESCRIPTOR(bidtime), INTERVAL '2' MINUTES, INTERVAL '10' MINUTES))
        GROUP BY window_start, window_end;
      +------------------+------------------+-------+
      |     window_start |       window_end | price |
      +------------------+------------------+-------+
      | 2020-04-15 08:00 | 2020-04-15 08:06 | 4.00  |
      | 2020-04-15 08:00 | 2020-04-15 08:08 | 6.00  |
      | 2020-04-15 08:00 | 2020-04-15 08:10 | 11.00 |
      | 2020-04-15 08:10 | 2020-04-15 08:12 | 3.00  |
      | 2020-04-15 08:10 | 2020-04-15 08:14 | 4.00  |
      | 2020-04-15 08:10 | 2020-04-15 08:16 | 4.00  |
      | 2020-04-15 08:10 | 2020-04-15 08:18 | 10.00 |
      | 2020-04-15 08:10 | 2020-04-15 08:20 | 10.00 |

-  **GROUPING SETS**

   Window aggregations also support **GROUPING SETS** syntax. Grouping sets allow for more complex grouping operations than those describable by a standard **GROUP BY**. Rows are grouped separately by each specified grouping set and aggregates are computed for each group just as for simple **GROUP BY** clauses.

   Window aggregations with **GROUPING SETS** require both the **window_start** and **window_end** columns have to be in the **GROUP BY** clause, but not in the **GROUPING SETS** clause.

   .. code-block::

      Flink SQL> SELECT window_start, window_end, supplier_id, SUM(price) as price
        FROM TABLE(
          TUMBLE(TABLE Bid, DESCRIPTOR(bidtime), INTERVAL '10' MINUTES))
        GROUP BY window_start, window_end, GROUPING SETS ((supplier_id), ());
      +------------------+------------------+-------------+-------+
      |     window_start |       window_end | supplier_id | price |
      +------------------+------------------+-------------+-------+
      | 2020-04-15 08:00 | 2020-04-15 08:10 |      (NULL) | 11.00 |
      | 2020-04-15 08:00 | 2020-04-15 08:10 |   supplier2 |  5.00 |
      | 2020-04-15 08:00 | 2020-04-15 08:10 |   supplier1 |  6.00 |
      | 2020-04-15 08:10 | 2020-04-15 08:20 |      (NULL) | 10.00 |
      | 2020-04-15 08:10 | 2020-04-15 08:20 |   supplier2 |  9.00 |
      | 2020-04-15 08:10 | 2020-04-15 08:20 |   supplier1 |  1.00 |
      +------------------+------------------+-------------+-------+

   Each sublist of **GROUPING SETS** may specify zero or more columns or expressions and is interpreted the same way as though used directly in the **GROUP BY** clause. An empty grouping set means that all rows are aggregated down to a single group, which is output even if no input rows were present.

   References to the grouping columns or expressions are replaced by null values in result rows for grouping sets in which those columns do not appear. For example, **()** in **GROUPING SETS ((supplier_id), ())** in the preceding example is an empty sublist, and the **supplier_id** column in the corresponding result data is filled with **NULL**.

-  **ROLLUP**

   **ROLLUP** is a shorthand notation for specifying a common type of grouping set. It represents the given list of expressions and all prefixes of the list, including the empty list.

   For example, **ROLLUP (one,two)** is equivalent to **GROUPING SET((one,two),(one),())**.

   Window aggregations with **ROLLUP** requires both the **window_start** and **window_end** columns have to be in the **GROUP BY** clause, but not in the **ROLLUP** clause.

   For example, the following query is equivalent to the one above.

   .. code-block::

      SELECT window_start, window_end, supplier_id, SUM(price) as price
      FROM TABLE(
          TUMBLE(TABLE Bid, DESCRIPTOR(bidtime), INTERVAL '10' MINUTES))
      GROUP BY window_start, window_end, ROLLUP (supplier_id);

-  **CUBE**

   **CUBE** is a shorthand notation for specifying a common type of grouping set. It represents the given list and all of its possible subsets - the power set.

   Window aggregations with **CUBE** requires both the **window_start** and **window_end** columns have to be in the **GROUP BY** clause, but not in the **CUBE** clause.

   For example, the following two queries are equivalent.

   .. code-block::

      SELECT window_start, window_end, item, supplier_id, SUM(price) as price
        FROM TABLE(
          TUMBLE(TABLE Bid, DESCRIPTOR(bidtime), INTERVAL '10' MINUTES))
        GROUP BY window_start, window_end, CUBE (supplier_id, item);

      SELECT window_start, window_end, item, supplier_id, SUM(price) as price
        FROM TABLE(
          TUMBLE(TABLE Bid, DESCRIPTOR(bidtime), INTERVAL '10' MINUTES))
        GROUP BY window_start, window_end, GROUPING SETS (
            (supplier_id, item),
            (supplier_id      ),
            (             item),
            (                 )
      )

-  **Cascading Window Aggregation**

   The **window_start** and **window_end** columns are regular timestamp columns, not time attributes. Thus they can not be used as time attributes in subsequent time-based operations.

   To propagate time attributes, you need to additionally add **window_time** column into **GROUP BY** clause. The **window_time** is the third column produced by :ref:`Windowing Table-Valued Functions (Windowing TVFs) <dli_08_15070__section3516193316120>` which is a time attribute of the assigned window. Adding **window_time** into **GROUP BY** clause makes **window_time** also to be group key that can be selected. Then following queries can use this column for subsequent time-based operations, such as cascading window aggregations and Window TopN.

   The following shows a cascading window aggregation where the first window aggregation propagates the time attribute for the second window aggregation.

   .. code-block::

      -- tumbling 5 minutes for each supplier_id
      CREATE VIEW window1 AS
      -- Note: The window start and window end fields of inner Window TVF are optional in the select clause. However, if they appear in the clause, they need to be aliased to prevent name conflicting with the window start and window end of the outer Window TVF.
      SELECT window_start as window_5mintumble_start, window_end as window_5mintumble_end, window_time as rowtime, SUM(price) as partial_price
        FROM TABLE(
          TUMBLE(TABLE Bid, DESCRIPTOR(bidtime), INTERVAL '5' MINUTES))
        GROUP BY supplier_id, window_start, window_end, window_time;

      -- tumbling 10 minutes on the first window
      SELECT window_start, window_end, SUM(partial_price) as total_price
        FROM TABLE(
            TUMBLE(TABLE window1, DESCRIPTOR(rowtime), INTERVAL '10' MINUTES))
        GROUP BY window_start, window_end;
