:original_name: dli_08_0420.html

.. _dli_08_0420:

JOIN
====

Equi-join
---------

**Syntax**

::

   FROM tableExpression INNER | LEFT | RIGHT | FULL JOIN tableExpression
     ON value11 = value21 [ AND value12 = value22]

**Precautions**

-  Currently, only equi-joins are supported, for example, joins that have at least one conjunctive condition with an equality predicate. Arbitrary cross or theta joins are not supported.
-  Tables are joined in the order in which they are specified in the FROM clause. Make sure to specify tables in an order that does not yield a cross join (Cartesian product), which are not supported and would cause a query to fail.
-  For streaming queries the required state to compute the query result might grow infinitely depending on the type of aggregation and the number of distinct grouping keys. Provide a query configuration with valid retention interval to prevent excessive state size.

**Example**

.. code-block::

   SELECT *
   FROM Orders INNER JOIN Product ON Orders.productId = Product.id;

   SELECT *
   FROM Orders LEFT JOIN Product ON Orders.productId = Product.id;

   SELECT *
   FROM Orders RIGHT JOIN Product ON Orders.productId = Product.id;

   SELECT *
   FROM Orders FULL OUTER JOIN Product ON Orders.productId = Product.id;

Time-windowed Join
------------------

**Function**

Each piece of data in a stream is joined with data in different time zones in another stream.

**Syntax**

.. code-block::

   from t1 JOIN t2 ON t1.key = t2.key AND TIMEBOUND_EXPRESSIO

**Description**

TIMEBOUND_EXPRESSION can be in either of the following formats:

-  L.time between LowerBound(R.time) and UpperBound(R.time)
-  R.time between LowerBound(L.time) and UpperBound(L.time)
-  Comparison expression with the time attributes (L.time/R.time)

**Precautions**

A time window join requires at least one equi join predicate and a join condition that limits the time of both streams.

For example, use two range predicates (<, <=, >=, or >), a BETWEEN predicate, or an equal predicate that compares the same type of time attributes (such as processing time and event time) in two input tables.

For example, the following predicate is a valid window join condition:

-  ltime = rtime
-  ltime >= rtime AND ltime < rtime + INTERVAL '10' MINUTE
-  ltime BETWEEN rtime - INTERVAL '10' SECOND AND rtime + INTERVAL '5' SECOND

**Example**

Join all orders shipped within 4 hours with their associated shipments.

.. code-block::

   SELECT *
   FROM Orders o, Shipments s
   WHERE o.id = s.orderId AND
         o.ordertime BETWEEN s.shiptime - INTERVAL '4' HOUR AND s.shiptime;

Expanding arrays into a relation
--------------------------------

**Precautions**

This clause is used to return a new row for each element in the given array. Unnesting WITH ORDINALITY is not yet supported.

**Example**

.. code-block::

   SELECT users, tag
   FROM Orders CROSS JOIN UNNEST(tags) AS t (tag);

User-Defined Table Functions
----------------------------

**Function**

This clause is used to join a table with the results of a table function. ach row of the left (outer) table is joined with all rows produced by the corresponding call of the table function.

**Precautions**

A left outer join against a lateral table requires a TRUE literal in the ON clause.

**Example**

The row of the left (outer) table is dropped, if its table function call returns an empty result.

.. code-block::

   SELECT users, tag
   FROM Orders, LATERAL TABLE(unnest_udtf(tags)) t AS tag;

If a table function call returns an empty result, the corresponding outer row is preserved, and the result padded with null values.

.. code-block::

   SELECT users, tag
   FROM Orders LEFT JOIN LATERAL TABLE(unnest_udtf(tags)) t AS tag ON TRUE;

Join Temporal Table Function
----------------------------

**Function**

**Precautions**

Currently only inner join and left outer join with temporal tables are supported.

**Example**

Assuming Rates is a temporal table function, the join can be expressed in SQL as follows:

.. code-block::

   SELECT
     o_amount, r_rate
   FROM
     Orders,
     LATERAL TABLE (Rates(o_proctime))
   WHERE
     r_currency = o_currency;

Join Temporal Tables
--------------------

**Function**

This clause is used to join the Temporal table.

**Syntax**

.. code-block::

   SELECT column-names
   FROM table1  [AS <alias1>]
   [LEFT] JOIN table2 FOR SYSTEM_TIME AS OF table1.proctime [AS <alias2>]
   ON table1.column-name1 = table2.key-name1

**Description**

-  **table1.proctime** indicates the processing time attribute (computed column) of **table1**.
-  **FOR SYSTEM_TIME AS OF table1.proctime** indicates that when the records in the left table are joined with the dimension table on the right, only the snapshot data is used for matching the current processing time dimension table.

**Precautions**

Only inner and left joins are supported for temporal tables with processing time attributes.

**Example**

LatestRates is a dimension table (such as HBase table) that is materialized with the latest rate.

.. code-block::

   SELECT
     o.amout, o.currency, r.rate, o.amount * r.rate
   FROM
     Orders AS o
     JOIN LatestRates FOR SYSTEM_TIME AS OF o.proctime AS r
     ON r.currency = o.currency;
