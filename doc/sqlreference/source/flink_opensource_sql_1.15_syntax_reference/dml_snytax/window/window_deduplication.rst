:original_name: dli_08_15073.html

.. _dli_08_15073:

Window Deduplication
====================

Function
--------

Window Deduplication is a special Deduplication which removes rows that duplicate over a set of columns, keeping the first one or the last one for each window and partitioned keys.

For streaming queries, unlike regular Deduplicate on continuous tables, Window Deduplication does not emit intermediate results but only a final result at the end of the window. Moreover, window Deduplication purges all intermediate state when no longer needed. Therefore, Window Deduplication queries have better performance if users do not need results updated per record. Usually, Window Deduplication is used with Windowing TVF directly. Besides, Window Deduplication could be used with other operations based on Windowing TVF, such as Window Aggregation, Window TopN and Window Join.

Window Top-N can be defined in the same syntax as regular Top-N, see Top-N documentation for more information. Besides that, Window Deduplication requires the **PARTITION BY** clause contains **window_start** and **window_end** columns of the relation. Otherwise, the optimizer will not be able to translate the query.

Flink uses **ROW_NUMBER()** to remove duplicates, just like the way of Window Top-N query. In theory, Window Deduplication is a special case of Window Top-N in which the N is one and order by the processing time or event time.

For more information, see `Window Deduplication <https://nightlies.apache.org/flink/flink-docs-release-1.15/zh/docs/dev/table/sql/queries/window-deduplication/>`__.

Syntax
------

.. code-block::

   SELECT [column_list]
   FROM (
      SELECT [column_list],
        ROW_NUMBER() OVER (PARTITION BY window_start, window_end [, col_key1...]
          ORDER BY time_attr [asc|desc]) AS rownum
      FROM table_name) -- relation applied windowing TVF
   WHERE (rownum = 1 | rownum <=1 | rownum < 2) [AND conditions]

Parameter description:

-  **ROW_NUMBER()**: Assigns an unique, sequential number to each row, starting with one.
-  **PARTITION BY window_start, window_end [, col_key1...]**: Specifies the partition columns which contain **window_start**, **window_end** and other partition keys.
-  **ORDER BY time_attr [asc|desc]**: Specifies the ordering column, it must be a time attribute. Currently Flink supports processing time attribute and event time attribute. Ordering by ASC means keeping the first row, ordering by DESC means keeping the last row.
-  **WHERE (rownum = 1 \| rownum <=1 \| rownum < 2)**: The **rownum = 1 \| rownum <=1 \| rownum < 2** is required for the optimizer to recognize the query could be translated to Window Deduplication.

Caveats
-------

-  Flink can only perform window deduplication on window table value functions that are based on tumble, hop, or cumulate windows.
-  Window deduplication is only supported when sorting based on the event time attribute.

Example
-------

The following example shows how to keep last record for every 10 minutes tumbling window.

.. code-block::

   -- tables must have time attribute, e.g. `bidtime` in this table
   Flink SQL> DESC Bid;
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

   Flink SQL> SELECT *
     FROM (
       SELECT bidtime, price, item, supplier_id, window_start, window_end,
         ROW_NUMBER() OVER (PARTITION BY window_start, window_end ORDER BY bidtime DESC) AS rownum
       FROM TABLE(
                  TUMBLE(TABLE Bid, DESCRIPTOR(bidtime), INTERVAL '10' MINUTES))
     ) WHERE rownum <= 1;
   +------------------+-------+------+-------------+------------------+------------------+--------+
   |          bidtime | price | item | supplier_id |     window_start |       window_end | rownum |
   +------------------+-------+------+-------------+------------------+------------------+--------+
   | 2020-04-15 08:09 |  5.00 |    D |   supplier4 | 2020-04-15 08:00 | 2020-04-15 08:10 |      1 |
   | 2020-04-15 08:17 |  6.00 |    F |   supplier5 | 2020-04-15 08:10 | 2020-04-15 08:20 |      1 |
   +------------------+-------+------+-------------+------------------+------------------+--------+
