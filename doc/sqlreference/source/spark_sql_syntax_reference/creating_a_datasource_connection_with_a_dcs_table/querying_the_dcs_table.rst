:original_name: dli_08_0228.html

.. _dli_08_0228:

Querying the DCS Table
======================

This statement is used to query data in a DCS table.

Syntax
------

::

   SELECT * FROM table_name LIMIT number;

Keyword
-------

LIMIT is used to limit the query results. Only INT type is supported by the **number** parameter.

Example
-------

Query data in the **test_redis** table.

::

   SELECT * FROM test_redis limit 100;
