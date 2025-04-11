:original_name: en-us_topic_0000001873107668.html

.. _en-us_topic_0000001873107668:

Reusing Results of Subqueries
=============================

Function
--------

To improve query performance, caching the results of a subquery and reusing them in different parts of the query can be done to avoid redundant calculations of the same subquery during the execution of a SELECT statement.

Syntax
------

::

   WITH cte_name AS (
       SELECT ...
       FROM table_name
       WHERE ...
   )
   SELECT ...
   FROM cte_name
   WHERE ...

Keywords
--------

.. table:: **Table 1** Keywords

   ========== ======================================================
   Parameter  Description
   ========== ======================================================
   cte_name   Custom subquery name
   table_name Name of the table where subquery commands are executed
   ========== ======================================================

Example
-------

-  Example 1: Define the **sales_data** subquery based on the **sales** table, query all items with sales amounts greater than 100, and retrieve all data from the subquery.

   ::

      WITH sales_data AS (
        SELECT product_name, sales_amount
        FROM sales
        WHERE sales_amount > 100
      )
      SELECT * FROM sales_data;

-  Example 2: Define two subqueries, **sales_data1** and **sales_data2**, based on the **sales** table, where **sales_data1** retrieves all items with sales amounts greater than 100 and **sales_data2** retrieves all items with sales amounts less than 20. Finally, merge the data from both subqueries.

   ::

      WITH sales_data1 AS (
        SELECT product_name, sales_amount
        FROM sales
        WHERE sales_amount > 100
      ),
      sales_data2 AS (
        SELECT product_name, sales_amount
        FROM sales
        WHERE sales_amount < 20
      )
      SELECT * FROM sales_data1
      UNION ALL
      SELECT * FROM sales_data2;
