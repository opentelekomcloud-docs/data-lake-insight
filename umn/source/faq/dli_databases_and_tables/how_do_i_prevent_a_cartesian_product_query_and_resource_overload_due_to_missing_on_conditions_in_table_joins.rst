:original_name: dli_03_0187.html

.. _dli_03_0187:

How Do I Prevent a Cartesian Product Query and Resource Overload Due to Missing "ON" Conditions in Table Joins?
===============================================================================================================

Symptom
-------

The on clause was not added to the SQL statement for joining tables. As a result, the Cartesian product query occurs due to multi-table association, and the queue resources were used up. Job execution fails on the queue.

For example, the following SQL statement left-joins three tables without the on clause.

.. code-block::

   select
        case
           when to_char(from_unixtime(fs.special_start_time), 'yyyy-mm-dd') < '2018-10-12' and row_number() over(partition by fg.goods_no order by fs.special_start_time asc) = 1 then 1
           when to_char(from_unixtime(fs.special_start_time), 'yyyy-mm-dd') >= '2018-10-12' and fge.is_new = 1 then 1
           else 0 end as is_new
   from testdb.table1 fg
   left join testdb.table2 fs
   left join testdb.table3  fge
   where to_char(from_unixtime(fs.special_start_time), 'yyyymmdd') = substr('20220601',1,8)

Solution
--------

When you use join to perform multi-table query, you must use the on clause to reduce the data volume.

The following example uses the on clause for the table join, which greatly reduces the result set of associated query and improves the query efficiency.

.. code-block::

   select
        case
           when to_char(from_unixtime(fs.special_start_time), 'yyyy-mm-dd') < '2018-10-12' and row_number() over(partition by fg.goods_no order by fs.special_start_time asc) = 1 then 1
           when to_char(from_unixtime(fs.special_start_time), 'yyyy-mm-dd') >= '2018-10-12' and fge.is_new = 1 then 1
           else 0 end as is_new
   from testdb.table1 fg
   left join testdb.table2 fs on fg.col1 = fs.col2
   left join testdb.table3  fge on fg.col3 = fge.col4
   where to_char(from_unixtime(fs.special_start_time), 'yyyymmdd') = substr('20220601',1,8)
