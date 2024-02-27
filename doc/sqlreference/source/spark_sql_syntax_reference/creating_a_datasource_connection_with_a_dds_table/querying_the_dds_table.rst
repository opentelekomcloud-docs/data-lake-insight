:original_name: dli_08_0232.html

.. _dli_08_0232:

Querying the DDS Table
======================

This statement is used to query data in a DDS table.

Syntax
------

::

   SELECT * FROM table_name LIMIT number;

Keywords
--------

LIMIT is used to limit the query results. Only INT type is supported by the **number** parameter.

Precautions
-----------

If schema information is not specified during table creation, the query result contains the **\_id** field for storing **\_id** in the DOC file.

Example
-------

Query data in the **test_table1** table.

::

   SELECT * FROM test_table1 limit 100;
