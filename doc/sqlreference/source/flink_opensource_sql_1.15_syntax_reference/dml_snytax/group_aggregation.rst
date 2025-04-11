:original_name: dli_08_15075.html

.. _dli_08_15075:

Group Aggregation
=================

An aggregate function computes a single result from multiple input rows. For example, there are aggregates to compute the **COUNT**, **SUM**, **AVG** (average), **MAX** (maximum) and **MIN** (minimum) over a set of rows.

For streaming queries, the required state for computing the query result might grow infinitely. State size depends on the number of groups and the number and type of aggregation functions. For example MIN/MAX are heavy on state size while COUNT is cheap. You can provide a query configuration with an appropriate state time-to-live (TTL) to prevent excessive state size. Note that this might affect the correctness of the query result.

For more information, see `Group Aggregation <https://nightlies.apache.org/flink/flink-docs-release-1.15/zh/docs/dev/table/sql/queries/group-agg/>`__.

DISTINCT Aggregation
--------------------

Distinct aggregates remove duplicate values before applying an aggregation function. The following example counts the number of distinct order_ids instead of the total number of rows in the **Orders** table.

.. code-block::

   SELECT COUNT(DISTINCT order_id) FROM Orders

GROUPING SETS
-------------

Grouping sets allow for more complex grouping operations than those describable by a standard **GROUP BY**. Rows are grouped separately by each specified grouping set and aggregates are computed for each group just as for simple **GROUP BY** clauses.

Each sublist of **GROUPING SETS** may specify zero or more columns or expressions and is interpreted the same way as though used directly in the **GROUP BY** clause. An empty grouping set means that all rows are aggregated down to a single group, which is output even if no input rows were present.

References to the grouping columns or expressions are replaced by null values in result rows for grouping sets in which those columns do not appear.

.. code-block::

   SELECT supplier_id, rating, COUNT(*) AS total
   FROM (VALUES
       ('supplier1', 'product1', 4),
       ('supplier1', 'product2', 3),
       ('supplier2', 'product3', 3),
       ('supplier2', 'product4', 4))
   AS Products(supplier_id, product_id, rating)
   GROUP BY GROUPING SETS ((supplier_id, rating), (supplier_id), ())

ROLLUP
------

**ROLLUP** is a shorthand notation for specifying a common type of grouping set. It represents the given list of expressions and all prefixes of the list, including the empty list.

.. code-block::

   SELECT supplier_id, rating, COUNT(*)
   FROM (VALUES
       ('supplier1', 'product1', 4),
       ('supplier1', 'product2', 3),
       ('supplier2', 'product3', 3),
       ('supplier2', 'product4', 4))
   AS Products(supplier_id, product_id, rating)
   GROUP BY ROLLUP (supplier_id, rating)

CUBE
----

**CUBE** is a shorthand notation for specifying a common type of grouping set. It represents the given list and all of its possible subsets - the power set.

For example, the following two queries are equivalent.

.. code-block::

   SELECT supplier_id, rating, product_id, COUNT(*)
   FROM (VALUES
       ('supplier1', 'product1', 4),
       ('supplier1', 'product2', 3),
       ('supplier2', 'product3', 3),
       ('supplier2', 'product4', 4))
   AS Products(supplier_id, product_id, rating)
   GROUP BY CUBE (supplier_id, rating, product_id)

   SELECT supplier_id, rating, product_id, COUNT(*)
   FROM (VALUES
       ('supplier1', 'product1', 4),
       ('supplier1', 'product2', 3),
       ('supplier2', 'product3', 3),
       ('supplier2', 'product4', 4))
   AS Products(supplier_id, product_id, rating)
   GROUP BY GROUPING SET (
       ( supplier_id, product_id, rating ),
       ( supplier_id, product_id         ),
       ( supplier_id,             rating ),
       ( supplier_id                     ),
       (              product_id, rating ),
       (              product_id         ),
       (                          rating ),
       (                                 )
   )

HAVING
------

**HAVING** eliminates group rows that do not satisfy the condition. **HAVING** is different from **WHERE**: **WHERE** filters individual rows before the **GROUP BY** while **HAVING** filters group rows created by **GROUP BY**. Each column referenced in condition must unambiguously reference a grouping column unless it appears within an aggregate function.

The presence of **HAVING** turns a query into a grouped query even if there is no **GROUP BY** clause. It is the same as what happens when the query contains aggregate functions but no **GROUP BY** clause. The query considers all selected rows to form a single group, and the **SELECT** list and **HAVING** clause can only reference table columns from within aggregate functions. Such a query will emit a single row if the **HAVING** condition is true, zero rows if it is not true.

.. code-block::

   SELECT SUM(amount)
   FROM Orders
   GROUP BY users
   HAVING SUM(amount) > 50
