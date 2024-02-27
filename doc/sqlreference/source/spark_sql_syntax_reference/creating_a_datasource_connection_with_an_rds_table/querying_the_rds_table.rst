:original_name: dli_08_0199.html

.. _dli_08_0199:

Querying the RDS Table
======================

This statement is used to query data in an RDS table.

Syntax
------

::

   SELECT * FROM table_name LIMIT number;

Keywords
--------

LIMIT is used to limit the query results. Only INT type is supported by the **number** parameter.

Precautions
-----------

The table to be queried must exist. Otherwise, an error is reported.

Example
-------

Query data in the **test_ct** table.

::

   SELECT * FROM dli_to_rds limit 100;
