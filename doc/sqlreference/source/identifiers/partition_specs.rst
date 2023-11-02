:original_name: dli_08_0036.html

.. _dli_08_0036:

partition_specs
===============

Syntax
------

partition_specs : (partition_col_name = partition_col_value, partition_col_name = partition_col_value, ...);

Description
-----------

Table partition list, which is expressed by using key=value pairs, in which **key** represents **partition_col_name**, and **value** represents **partition_col_value**. If there is more than one partition field, separate every two key=value pairs by using a comma (,).
