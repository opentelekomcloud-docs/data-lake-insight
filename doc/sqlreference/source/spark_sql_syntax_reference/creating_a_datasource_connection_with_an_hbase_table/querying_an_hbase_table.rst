:original_name: dli_08_0121.html

.. _dli_08_0121:

Querying an HBase Table
=======================

This statement is used to query data in an HBase table.

Syntax
------

::

   SELECT * FROM table_name LIMIT number;

Keyword
-------

LIMIT is used to limit the query results. Only INT type is supported by the **number** parameter.

Precautions
-----------

The table to be queried must exist. Otherwise, an error is reported.

Example
-------

Query data in the **test_ct** table.

::

   SELECT * FROM test_hbase limit 100;

Query Pushdown
--------------

Query pushdown implements data filtering using HBase. Specifically, the HBase Client sends filtering conditions to the HBase server, and the HBase server returns only the required data, speeding up your Spark SQL queries. For the filter criteria that HBase does not support, for example, query with the composite row key, Spark SQL performs data filtering.

-  Scenarios where query pushdown is supported

   -  Query pushdown can be performed on data of the following types:

      -  Int
      -  boolean
      -  short
      -  long
      -  double
      -  string

      .. note::

         Data of the float type does not support query pushdown.

   -  Query pushdown is not supported for the following filter criteria:

      -  >, <, >=, <=, =, !=, and, or

         The following is an example:

         ::

            select * from tableName where (column1 >= value1 and column2<= value2) or column3 != value3

      -  The filtering conditions are **like** and **not like**. The prefix, suffix, and inclusion match are supported.

         The following is an example:

         ::

            select * from tableName where column1 like "%value" or column2 like "value%" or column3 like "%value%"

      -  IsNotNull()

         The following is an example:

         ::

            select * from tableName where IsNotNull(column)

      -  in and not in

         The following is an example:

         ::

            select * from tableName where column1 in (value1,value2,value3)  and column2 not in (value4,value5,value6)

      -  between \_ and \_

         The following is an example:

         ::

            select * from tableName where column1 between value1 and value2

      -  Filtering of the row sub-keys in the composite row key

         For example, to perform row sub-key query on the composite row key **column1+column2+column3**, run the following statement:

         ::

            select * from tableName where column1= value1

-  Scenarios where query pushdown is not supported

   -  Query pushdown can be performed on data of the following types:

      Except for the preceding data types where query pushdown is supported, data of other types does not support query pushdown.

   -  Query pushdown is not supported for the following filter criteria:

      -  Length, count, max, min, join, groupby, orderby, limit, and avg

      -  Column comparison

         The following is an example:

         ::

            select * from tableName where column1 > (column2+column3)
