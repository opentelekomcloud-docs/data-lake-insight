:original_name: dli_08_0423.html

.. _dli_08_0423:

Deduplication
=============

Function
--------

Deduplication removes rows that duplicate over a set of columns, keeping only the first one or the last one.

Syntax
------

.. code-block::

   SELECT [column_list]
   FROM (
      SELECT [column_list],
        ROW_NUMBER() OVER ([PARTITION BY col1[, col2...]]
          ORDER BY time_attr [asc|desc]) AS rownum
      FROM table_name)
   WHERE rownum = 1

Description
-----------

-  ROW_NUMBER(): Assigns a unique, sequential number to each row, starting with one.
-  PARTITION BY col1[, col2...]: Specifies the partition columns, i.e. the deduplicate key.
-  ORDER BY time_attr [asc|desc]: Specifies the ordering column, it must be a time attribute. Currently Flink supports proctime only. Ordering by ASC means keeping the first row, ordering by DESC means keeping the last row.
-  WHERE rownum = 1: The rownum = 1 is required for Flink to recognize this query is deduplication.

Precautions
-----------

None

Example
-------

The following examples show how to remove duplicate rows on **order_id**. The proctime is an event time attribute.

.. code-block::

   SELECT order_id, user, product, number
     FROM (
        SELECT *,
            ROW_NUMBER() OVER (PARTITION BY order_id ORDER BY proctime ASC) as row_num
        FROM Orders)
     WHERE row_num = 1;
