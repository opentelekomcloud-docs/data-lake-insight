:original_name: dli_08_0422.html

.. _dli_08_0422:

Top-N
=====

Function
--------

Top-N queries ask for the N smallest or largest values ordered by columns. Both smallest and largest values sets are considered Top-N queries. Top-N queries are useful in cases where the need is to display only the N bottom-most or the N top- most records from batch/streaming table on a condition.

Syntax
------

.. code-block::

   SELECT [column_list]
   FROM (
      SELECT [column_list],
        ROW_NUMBER() OVER ([PARTITION BY col1[, col2...]]
          ORDER BY col1 [asc|desc][, col2 [asc|desc]...]) AS rownum
      FROM table_name)
   WHERE rownum <= N [AND conditions]

Description
-----------

-  ROW_NUMBER(): Allocate a unique and consecutive number to each line starting from the first line in the current partition. Currently, we only support ROW_NUMBER as the over window function. In the future, we will support RANK() and DENSE_RANK().
-  PARTITION BY col1[, col2...]: Specifies the partition columns. Each partition will have a Top-N result.
-  ORDER BY col1 [asc|desc][, col2 [asc|desc]...]: Specifies the ordering columns. The ordering directions can be different on different columns.
-  WHERE rownum <= N: The rownum <= N is required for Flink to recognize this query is a Top-N query. The N represents the N smallest or largest records will be retained.
-  [AND conditions]: It is free to add other conditions in the where clause, but the other conditions can only be combined with rownum <= N using AND conjunction.

Precautions
-----------

-  The TopN query is Result Updating.
-  Flink SQL will sort the input data stream according to the order key,
-  so if the top N records have been changed, the changed ones will be sent as retraction/update records to downstream.
-  If the top N records need to be stored in external storage, the result table should have the same unique key with the Top-N query.

Example
-------

This is an example to get the top five products per category that have the maximum sales in realtime.

.. code-block::

   SELECT *
     FROM (
        SELECT *,
            ROW_NUMBER() OVER (PARTITION BY category ORDER BY sales DESC) as row_num
        FROM ShopSales)
     WHERE row_num <= 5;
